For a secure communication and to make sure that payload is not altered, HTTP messages are signed and signature is sent in header. 
For more details: https://justinsecurity.medium.com/signing-http-messages-962510d65895

https://developer.apple.com/documentation/apple_news/apple_news_api_tutorial/signing_the_http_request

Details about how signatuires will be communiocated and used:  
https://developers.becknprotocol.io/docs/infrastructure-layer-specification/authentication/gateway-signing/
https://developers.becknprotocol.io/docs/infrastructure-layer-specification/authentication/signature-verification/
https://developers.becknprotocol.io/docs/infrastructure-layer-specification/authentication/subscriber-signing/




Sample code below for signing the request.  Its taken from https://github.com/beckn/gtfs-to-beckn

```ts
import _sodium, { base64_variants } from 'libsodium-wrappers';
import { Request, Response, NextFunction } from "express";

//to create header 
export const createAuthorizationHeader = async (message: any) => {
    const { signing_string, expires, created } = await createSigningString(JSON.stringify(message)); //creates  hashed request details 
    console.log(message?.context?.transaction_id, "Signing string: ", signing_string);
    const signature = await signMessage(signing_string, process.env.sign_private_key || ""); // signs the message 
    const subscriber_id = config.bpp_id;
  //creates header data to be sent as Authorization key 
    const header = `Signature keyId="${subscriber_id}|${config.unique_key_id}|ed25519",algorithm="ed25519",created="${created}",expires="${expires}",headers="(created) (expires) digest",signature="${signature}"`
    return header;
}


//creates  hashed request details 
export const createSigningString = async (message: string, created?: string, expires?: string) => {
    if (!created) created = Math.floor(new Date().getTime() / 1000).toString();
    if (!expires) expires = (parseInt(created) + (1 * 60 * 60)).toString(); //Add required time to create expired
   
    await _sodium.ready;
    const sodium = _sodium;
    const digest = sodium.crypto_generichash(64, sodium.from_string(message));
    const digest_base64 = sodium.to_base64(digest, base64_variants.ORIGINAL);
    const signing_string =
        `(created): ${created}
(expires): ${expires}
digest: BLAKE-512=${digest_base64}`
    return { signing_string, expires, created }
}


// sign the message 

export const signMessage = async (signing_string: string, privateKey: string) => {
    await _sodium.ready;
    const sodium = _sodium;
    const pk=sodium.from_base64(privateKey, base64_variants.ORIGINAL); // ed25519 private key
    const signedMessage = sodium.crypto_sign(signing_string, pk);
    return sodium.to_base64(signedMessage, base64_variants.ORIGINAL);
}


```
