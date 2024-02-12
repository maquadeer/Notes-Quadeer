---
dg-publish: true
---
> [!TIP]-  Note
> Make sure to execute `server` program before sending a `client` request.

```c
#include<stdio.h> //Standard Input/Output functions.
#include<stdlib.h> //Std Lib functions, including mem allocation.
#include<sys/types.h> //System data types.
#include<sys/socket.h> //Socket programming functions and structures.
#include<netinet/in.h> //Internet address family structures and functions.
#include<string.h> //String manipulation functions.
```

```c
main(int argc, char * argv[])
```

This is the entry point of the program, taking command-line arguments (`argc` and `argv`).

```c
{
    int sockid, rval, clen;
    char buffer[20];
    struct sockaddr_in s, c;
```

Here, variables are declared, including `sockid` for the socket identifier, `rval` for return values, `clen` for the length of the client structure, `buffer` to store messages, and `s` and `c` for server and client address structures.

```c
    if(argc<3){
        printf("\nUSAGE:%s IP_ADDRESS PORT #\n",argv[0]);
        exit(0);
    }
```

This block checks if there are less than three command-line arguments. If so, it prints a usage message and exits the program.

```c
    sockid = socket(AF_INET, SOCK_DGRAM, 0);
    if(sockid == -1){
        perror("SOCK-CRE-ERR");
        exit(1);
    }
```

A UDP socket is created, and if the creation fails, an error message is printed, and the program exits.

```c
    s.sin_family = AF_INET; //IPV4
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
```

Server address structure is set up with `IP address` and `port number` obtained from command-line arguments.

```c
    printf("socket created\n");
    rval = bind(sockid, (struct sockaddr *)&s, sizeof(s));
    if(rval == -1){
        perror("BIND-ERR");
        close(sockid);
        exit(1);
    }
    printf("socket binded\n");
```

The socket is bound to the server address structure. If binding fails, an error message is printed, and the program exits.

```c
    clen = sizeof(c);
    rval = recvfrom(sockid, buffer, sizeof(buffer), 0, (struct sockaddr *)&c, &clen);
    if(rval == -1){
        perror("MSG-RCV-ERR");
        exit(1);
    }
    printf("\nRequest received\nRequest message is :%s\n", buffer);
```

The server waits to receive a message from the client using `recvfrom`. If an error occurs during the receiving process, an error message is printed, and the program exits.

```c
    strcat(buffer, " ash");
    rval = sendto(sockid, buffer, sizeof(buffer), 0, (struct sockaddr *)&c, clen);
    if(rval == -1){
        perror("MSG-SND-ERR");
        exit(1);
    }
    printf("\nResponse sent successfully\n");
    close(sockid);
}
```

The server modifies the received message and sends it back to the client using `sendto`. If sending fails, an error message is printed, and the program exits. Finally, the socket is closed.