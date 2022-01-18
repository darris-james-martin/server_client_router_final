# server_client_router_final
This is part 2 of the project for CS4504 - Parallel and Distributed Computing</br>
https://www.youtube.com/watch?v=RQm_A099Wac&ab_channel=frostbyte000jm

## The Goal
The goal of this project was to establish a system where multiple clients can connect to multiple servers, but still share information between one another. 
 
 ![image](https://user-images.githubusercontent.com/93277335/148878981-41f7b6e3-3ff0-4695-8ea4-9c2b0fdc0be6.png)

My design of this project includes the following. 
There are 4 systems:</br>
**1)	The Client** – The Client(s) can display a message, send a message, send a file, and receive a file. </br>
**2)	The Router** – the Router(s) wait on the client to log in and once a connection is established, it waits for the server to give it commands. It can transfer a message to the client, transfer a message to the server, transfer a file to the client, or transfer a file to the server. </br>
**3)	The Server** – The Server(s) has multiple functions. There is an echo game where it will repeat back whatever the user types, but replaces all vowels with “oo”s. It can also store files on the server and the clients can request them. The client can also request files on Server n (where n is any server, except the Server that the client is logged into) and the Server will connect to the Server n to receive those files. Lastly, a server can send a client another client machine's information so that they can start a P2P connection with one another. </br>
**4)	The Server-Router** – This single machine acts as an operator. When a Server comes online, it will call the Server-Router and inform it of its machine information, and then hang-up. When a Server needs to connect to other Servers on the network, it will call the Server-Router and ask for a list of currently connected Servers. </br>

### Threads
When a client connects to the Router, the Router immediately creates a thread, which allows the router to wait on the next client. The server does the exact same process. Here is what the code snippet looks like for the router. 
```java
public void waitForConnection() throws IOException, ClassNotFoundException {
        //declarations
        ServerSocket serverSocket = null;
        Socket clientSocket = null;
        boolean doRun = true;

        while(doRun){
            //set up ServerSocket and wait on port ????
            try {
                serverSocket = new ServerSocket(portNum);
                System.out.println("Router: "+ machineContainer.getLocalHostName() +" is listening on port: "+ portNum);
            } catch (IOException e) {
                System.err.println("Could not listen on port: "+ portNum +".");
                System.exit(1);
            }

            // Wait for Connection
            System.out.println("Waiting for Connection...");
            try {
                clientSocket = serverSocket.accept();
                serverSocket.close();
            } catch (IOException e) {
                doRun = false;
                System.err.println("Unable to accept a connection.");
                System.exit(1);
            }
            RouterThread routerThread = new RouterThread(clientSocket,this);
            routerThread.start();
        }
    }
```
