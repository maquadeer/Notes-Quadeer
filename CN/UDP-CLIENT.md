---
dg-publish: true
---
> [!Danger] Note
> Same program can be referred for `UDP Services` Using Well Known / Standard Ports.

>[!info]
> can also refer [[UDP-CLIENT.md#Llama generated response|Llama2 70B generated response]]


```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<sys/types.h>
#include<string.h>
```
These are the include statements for various standard C libraries used in the program. `stdio.h` for input/output operations, `stdlib.h` for memory allocation and other general utilities, `sys/socket.h` and `netinet/in.h` for socket programming, `sys/types.h` for various data types, and `string.h` for string manipulation.

```c
main(int argc, char * argv[])
```
This is the entry point of the program. It takes command-line arguments: `argc` is the count of arguments, and `argv` is an array of strings containing the arguments.

```c
{
	struct sockaddr_in s;
    int rval, sockid, slen;
    char m1[20], m2[100];
```
Here, a structure `sockaddr_in` is defined to store information about the server's address. `rval` is a variable to store return values, `sockid` is the socket identifier, `slen` is the length of the server structure, `m1` and `m2` are character arrays to store messages.

```c
    system("clear");
```
This line clears the console screen.

```c
if(argc<3){
    printf("\nUSAGE :%s IP-Address Port #\n", argv[0]);
    exit(1);
}
```
This block checks if there are less than three command-line arguments. If so, it prints a usage message and exits the program.

```c
    sockid = socket(AF_INET, SOCK_DGRAM, 0);
    if(sockid ==  -1){
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
```
Here, a UDP socket is created using the `socket` function. If the socket creation fails, an error message is printed, and the program exits.

```c
    s.sin_family= AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
```
These lines set up the server address structure. `AF_INET` specifies the address family (IPv4), `inet_addr` converts the IP address from string to binary format, and `htons` converts the port number from host byte order to network byte order.

```c
    printf("\nEnter the request message:");
    scanf("%s", m1);
```
The user is prompted to enter a request message, which is then read and stored in the `m1` array.

```c
    slen = sizeof(s);
    rval = sendto(sockid, m1, sizeof(m1), 0, (struct sockaddr *)&s, slen);
    if(rval == -1){
        perror("\nMSG-SEND-ERR:");
        exit(1);
    }
```
The size of the server address structure is determined, and the `sendto` function is used to send the message to the server. If an error occurs during the sending process, an error message is printed, and the program exits.

```c
    printf("\nMessage sent successfully\n");
    strncpy(m2, "", 20);
    rval = recvfrom(sockid, m2, sizeof(m2), 0, (struct sockaddr *)&s, &slen);
    if(rval == -1){
        perror("MSG-RCV-ERR");
        exit(1);
    }
```
A success message is printed, and the `recvfrom` function is used to receive a message from the server. If an error occurs during the receiving process, an error message is printed, and the program exits.

```c
    printf("\nMessage received is: %s\n", m2);
    close(sockid);
}
```
The received message is printed, and the socket is closed using the `close` function.

Now, let's discuss some networking-related keywords and functions:

- `socket`: Creates a socket for communication.
- `sockaddr_in`: A structure representing an Internet socket address.
- `AF_INET`: Address family for IPv4.
- `inet_addr`: Converts a string representation of an IPv4 address to a binary form.
- `htons`: Converts a 16-bit quantity from host byte order to network byte order.
- `sendto`: Sends data to a specific destination using a socket.
- `recvfrom`: Receives data from a socket and stores the source address.
- `close`: Closes a file descriptor, in this case, the socket.

These functions and structures are crucial for basic UDP socket programming in C.

## Llama generated response:

This code is a simple network client that sends a message to a server and receives a response. Here's a breakdown of the code:

1. The first section of the code includes various header files, which provide functions and data types for working with sockets, IP addresses, and other network-related tasks.

2. The `main()` function is defined, which is the entry point of the program. It takes two arguments: `argc`, which represents the number of command-line arguments passed to the program, and `argv`, which is an array of character pointers that represents the command-line arguments themselves.

3. The program initializes a socket using the `socket()` function, with the address family `AF_INET` (Internet) and the socket type `SOCK_DGRAM` (datagram). The socket ID is stored in the variable `sockid`.

4. The program checks if the socket creation failed by checking the value of `sockid`. If the socket ID is -1, it means that the creation failed, and the program will print an error message and exit.

5. The program sets the address family of the socket address structure `s` to `AF_INET`.

6. The program sets the IP address of the socket address structure `s` to the address specified in the command-line argument `argv[1]`. The `inet_addr()` function is used to convert the IP address string to a binary format.

7. The program sets the port number of the socket address structure `s` to the port number specified in the command-line argument `argv[2]`.

8. The program prompts the user to enter a request message and reads the message into a buffer `m1` using the `scanf()` function.

9. The program calculates the length of the socket address `s` and stores it in the variable `slen`.

10. The program sends the request message to the server using the `sendto()` function, which takes the socket ID, the message buffer, the message length, and the socket address as arguments. The `sendto()` function sends the message to the server at the specified IP address and port number.

11. The program checks if the message sending failed by checking the return value of `sendto()`. If the return value is -1, it means that the sending failed, and the program will print an error message and exit.

12. The program prints a success message indicating that the message has been sent successfully.

13. The program allocates a buffer `m2` to store the response message from the server.

14. The program uses the `recvfrom()` function to receive the response message from the server. The `recvfrom()` function takes the socket ID, the response buffer, the buffer length, and the socket address as arguments. The function receives the message from the server at the specified IP address and port number.

15. The program checks if the message receiving failed by checking the return value of `recvfrom()`. If the return value is -1, it means that the receiving failed, and the program will print an error message and exit.

16. The program prints the received message in a success message.

17. The program closes the socket using the `close()` function.

In summary, this code creates a socket, sends a request message to a server, receives a response message from the server, and prints the received message.
