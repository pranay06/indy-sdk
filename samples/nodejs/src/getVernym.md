# getVernym

Here I have tried to encompass what is happening and what should (and need not) happen in the getVernym process (This is also a function in gettingStarted.js which is  part of sample nodejs code of indy-sdk).

# Let's dive in

Once the [Onboarding process](https://github.com/pranay06/indy-sdk/blob/master/samples/nodejs/src/onboarding.md) gets completed, self sovereign identity (it's own DID and Key, not the pairwise) of the user needs to be created and logged into ledger by the onboarder. This is what has been achieved in this function. 


*** At new user's end

- Create a new pair of DID and key using it's wallet and also store it in the wallet

- Create a DIDInfo JSON object with the newly created DID and verkey

- Encrypt this object using it's wallet, pairwise key of onboarder (wrt new user) and of new user (wrt onboarder)
> This encrypts the message using authenticated-encryption scheme unlike anonymous-encryption scheme which was used in onboarding function.
- Send this encrypted object to onboarder

*** At onboarder's end

- Once the encrypted json object gets recieved at onboarder's end, this object gets decrypted. This decryption process returns sender's verkey as well as decrypted DID info object sent to it.

- Onboarder retrieves it's pairwise did (wrt new user) from the ledger
> Although this key shouldn't have been stored in the ledger at first place.

- It compares it's just retrieved did from the ledger with the did it received from the decryption process.

- Once this comparison returns true, it gives the confirms the authenticity of the message.

- Now onboarder stores the DID and Key of the new user in the ledger. 

> Note that this DID and key will act as self sovereign identity of the new user (It is not the pairwise did and key) and this information is supposed to be stored in the ledger.