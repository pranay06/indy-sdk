# Onboarding

Here I have tried to encompass what is happening and what should (and need not) happen in the onboarding process (This is also a function in gettingStarted.js which part of sample nodejs code of indy-sdk).

# Let's dive in

The sole purpose of the onboarding process is to generate generate pairwise dids and keys at both the ends (onboarder as well as new user), exchanges this data between two parties and also create wallet of the new user (at it's end.)

## Steps

### At onboarder's end
- Pairwise DID and key of the onboarder (steward) is created (using wallet) which will be used to communicate with the new user.

- Send these pairwise DID and key of the onboarder to the ledger (Not neccessary as this are private credentials and need not to be stored inside ledger)

- Onboarder sends pairwise did and nonce as payload as part of connection request to new user
>**with this pairwise verkey can also be sent in the request so that the step to store the pairwise did and verkey into the ledger could be ommitted. 

### At New user's end
- Wallet creation (and the wallet gets opened) of the new user using the it's wallet credentials.


- Pairwise DID and key of the new user  is created (using the wallet which just got created) which will be used to communicate with the onboarder.

- Make connection response for the sender with the pairwise did and key which we just created and the nonce which we recieved in the connection request.

-Encrypts this connection response message by anonymous-encryption scheme (cryptoAnonCrypt) using the pairwise DID of the sender

>**Please note this pairwise DID of the sender could be retireved from the ledger (if stored in ledger by the sender) or could be retreived by the connection request sent by sender (if sender attached it in the connection request)

### At Onboarder's end
- Once the connection response sent by new user is recieved by the the onboarder, it decrypts the it using it's wallet and pairwise did (corresponding to the new user)

- Athenticate the connection response by checking the nonce.

- Once authenticated, pairwise did and verkey of the new user is stored in the ledger by the onboarder.

>**This step might not be neccessary since in reality we are not supposed to store pairwise dids and keys into the ledger.
 
## Notes:
I have emphasised on pairwise DIDs and verkeys not to be stored on public ledger to support both privacy by Design architecture as well as its ability to scale.

These are not my views but what is being documented by evernym in one of their documents.

>A DID added directly to the Sovrin public ledger is called a public DID, whereas a pairwise pseudonymous DID shared and stored privately “off-ledger” between the agents for two identity holders is called a private DID. The ability for Sovrin infrastructure to support both is fundamental to both its Privacy by Design architecture as well as its ability to scale.

Document link: https://www.evernym.com/wp-content/uploads/2017/07/What-Goes-On-The-Ledger.pdf


