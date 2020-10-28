# A-Secure-Client-Server-Network
A simple messenger which is used to exchange messages and files through a secure tunnel

In this project, "socket" and "cryptography" libraries have been used.

## Symmetric Encryption:
1. After a successful connection between a client and server, the user decides each client to play a "sender" or a "receiver" role. In the sender mode, the user chooses a free receiver to exchange data with.

2. A master key is generated by the sender and transfered securely to the receiver. For this purpose, each side generates a (private key, public key) set first using the RSA algorithm (from cryptography library) and sends its public key to the other side. Thus the master key is encrypted in sender side using server's public key, decrypted in server side using its private key, encrypted in server side again using receiver's public key and decrypted in receiver's side using its private key.  

3. A session key is generated by the sender, encrypted using AES algorithm (from cryptography library) and the master key, transfered to the server and from there to the receiver and decrypted finally by the receiver using the master key.

4. The user decides whether to send a message or a file over the network.

5. If the user chooses to transfer a file, they will be asked for a file address (for example C:/Users/user/Desktop/book.**jpg**). The file will then be transfered byte by byte. There's also a session key expire time (5 seconds) that is particularly used for transfering large files. If transfering a file does not finish before this period, the session will get exprired and a new session key will be created. 

6. After finishing the msg / file transfer, a new master key will be created and transfered for next connections between the same sender and the same receiver. 

## Asymmetric Encryption:
This method is somewhat similar to the above method (creating and transfering public keys, private keys and a session key). 

1. Before transfering data, each client should be authenticated to the server (The server should be authenticated to each client too). For this purpose, the following algorithm is implemented:

![alt text](https://user-images.githubusercontent.com/42779113/97398713-ea81c680-1900-11eb-9c0a-8c89b4359723.png)
{ Rs is a 32 bit random number generated by the server and Rc is a 32 bit random number generated by a client. The first arrow for instance means the Rs is encrypted by the server using client's public key and then transfered to the client. }

### Note:
- First run the server code and then run then client code as many times as you wish, because if you run the client code before the server code, there would be no server to bind to that client and the client terminates with a message saying: "Failed to connect to host: 127.0.0.1 on port: 9999, because: [WinError 10061] No connection could be made because the target machine actively refused it".
