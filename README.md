# BAP BPP Integration-to-Gateway
Step wise guide to integrate your BAP or BPP to beckn gateway. 

For starter please have a look at this URL https://beckn-registry.readthedocs.io/en/latest/index.html 

What you will need: 

- A working API where request can reach
- ED25519 key pair for singing the request  
- An account on https://registry.becknprotocol.io/ (sign up via google).  
- BG Gateway url : https://gateway.becknprotocol.io/bg/


Regsiter to registry and and add yourself as network participant. A unique ID is needed hence your applications FQDN domain is suggested, you can use any text for ID as long as its uniquue on network at that moment. 


<img width="1122" alt="image" src="https://user-images.githubusercontent.com/1314582/190950127-39ed50ba-bb75-4e90-b492-25716ffa2dc6.png">


Once participant is added, details about participant need to be filled. 
Please visit https://registry.becknprotocol.io/network_domains/index to see available domain, only these domain will be used to commmunicate on beckn network. 

Navigate to network participant and click on edit button on your entry. 
Click on Network role tab. Here you can register as BAP or BPP 

One example where one network participant has registerred as BAP and BPP both.
<img width="1755" alt="image" src="https://user-images.githubusercontent.com/1314582/190951130-45b7840c-e9eb-4ee8-95b5-60145a93e512.png">


Make sure status is subscribed while filling the network role information to be able to send and recieve request. 

<img width="1755" alt="image" src="https://user-images.githubusercontent.com/1314582/190951412-fb58ecc4-28a1-42ac-877c-fb794635f1f9.png">


make sure to add the operaing region.

<img width="1761" alt="image" src="https://user-images.githubusercontent.com/1314582/190951814-b5fcf057-a39a-4e12-8558-b51e5a3bf603.png">

Participant key : 

Signing public key and encryption key need to be added to veify the legitimate requesters.

Generate a pair of private key , publick key using ED25519 
```shell
ssh-keygen -t ed25519
```

OR
Use for testing use any of the online tool. 
one working example as of now is: https://ed25519.herokuapp.com/

other URLS: 
https://registry.becknprotocol.io/crypto_keys/generate/ed25519:256
https://registry.becknprotocol.io/crypto_keys/generate/x25519:256

Once you have the keys, navigate to participant key tab on network participant information page. 
and add the information
 
Once added,  your entry on network is complete and you should be able to send request given that you have signed the requests. 

