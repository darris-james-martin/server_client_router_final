# server_client_router_final
This is part 2 of the project for CS4504 - Parallel and Distributed Computing</br>
https://www.youtube.com/watch?v=RQm_A099Wac&ab_channel=frostbyte000jm

## The Goal
The goal of this project was to establish a system where multiple clients can connect to multiple servers, but still share information between one another. 
 
My design of this system was the following. 
There are 4 systems:
1)	The Clients – They can display a message, send a message, send a file, and receive a file. 
2)	The Router – They wait on the client to log in and once a connection is established, it waits for the server to give it commands. It can transfer a message to the client, transfer a message to the server, transfer a file to the client, or transfer a file to the server. 
3)	The Server – The server has multiple functions. It has an echo game where it will repeat back whatever the user types, but replaces all vowels with “oo”s. It can also store files on the server and the clients can request them. The client can also request files on other servers and the server will connect to the other servers to receive them. Last a server can send two client machines information so they can start a P2P connection with one another. 
4)	The Server-Router – This system waits for a server to connect to it and let it know that it has logged onto the network. It then hangs up. A server can also log into the Server Router and ask for a list of other Servers. This allows the servers to communicate and connect with one another. 

### Threads
When a client connects to the Router, the Router immediately creates a thread, this allows the router to wait on the next client. The server does the exact same process. This is what the code snippet looks like for the router. 
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

