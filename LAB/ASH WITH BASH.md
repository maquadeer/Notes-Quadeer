---
dg-publish: true
---
## CN LAB
- [[#1. Write aim & Program to demonstrate connectionless Using Well Known / Standard Port with expected output & actual output (2a)|1. Write aim & Program to demonstrate connectionless Using Well Known / Standard Port with expected output & actual output (2a)]]
- [[#2. Write aim & Program to demonstrate connection-less time service using well known port with expected output & actual output (2d)|2. Write aim & Program to demonstrate connection-less time service using well known port with expected output & actual output (2d)]]
- [[#3. Write aim & Program Write a Program to Demonstrate Connection oriented (TCP) Echo Service Using Well Known / Standard Port with expected output & actual output|3. Write aim & Program Write a Program to Demonstrate Connection oriented (TCP) Echo Service Using Well Known / Standard Port with expected output & actual output]]
- [[#4. Write aim & Program to demonstrate Connection-oriented time service using well known port with expected output & actual output (3d)|4. Write aim & Program to demonstrate Connection-oriented time service using well known port with expected output & actual output (3d)]]
- [[#5. Write aim & Program to demonstrate Connectionless(UDP) Echo Service Using User Defined Port with expected output & actual output|5. Write aim & Program to demonstrate Connectionless(UDP) Echo Service Using User Defined Port with expected output & actual output]]
- [[#6. Write aim & Program to demonstrate Connectionless(UDP) Date & Time Service Using User Defined Port with expected output & actual output|6. Write aim & Program to demonstrate Connectionless(UDP) Date & Time Service Using User Defined Port with expected output & actual output]]
- [[#7. Write aim & Program to demonstrate Connection-oriented(TCP) Echo Service Using User Defined Port with expected output & actual output|7. Write aim & Program to demonstrate Connection-oriented(TCP) Echo Service Using User Defined Port with expected output & actual output]]
- [[#8. Write aim & Program to demonstrate Connection-oriented (TCP) Date & Time Service Using User Defined Port with expected output & actual output|8. Write aim & Program to demonstrate Connection-oriented (TCP) Date & Time Service Using User Defined Port with expected output & actual output]]
- [[#9. Write aim & Program to demonstrate Connection-oriented (TCP) Iterative Echo Service using user defined port with expected output & actual output|9. Write aim & Program to demonstrate Connection-oriented (TCP) Iterative Echo Service using user defined port with expected output & actual output]]
- [[#10. Write aim & Program to demonstrate Connection-oriented(TCP) Concurrent Echo Service using user defined port with expected output & actual output|10. Write aim & Program to demonstrate Connection-oriented(TCP) Concurrent Echo Service using user defined port with expected output & actual output]]
##### 1. Write aim & Program to demonstrate connectionless Using Well Known / Standard Port with expected output & actual output (2a)
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<sys/types.h>
#include<string.h>
main(int argc, char * argv[])
{
	struct sockaddr_in s;
	int rval,sockid,slen;
	char m1[20],m2[20];
	system("clear");
	if(argc<3)
	{
		printf("\n USAGE : %s IP_Address port \n",argv[0]);
		exit(1);
	}
	sockid=socket(AF_INET,SOCK_DGRAM,0);
	if(sockid ==-1)
	{
		perror("SOCK-CRE-ERR:");
		exit(1);
	}
	s.sin_family=AF_INET;
	s.sin_addr.s_addr=inet_addr(argv[1]);
	s.sin_port=htons(atoi(argv[2]));
	printf("\n 	ENTER THE REQUEST MESSAGE ::");
	scanf("%s",m1);
	slen=sizeof(s);
	rval=sendto(sockid,m1,sizeof(m1),0,(struct sockaddr *)&s,slen);
	if(rval==-1)
	{
		perror("MSG-SEND-ERR:");
		exit(1);
	}
	printf("\n message sent sucessfully \n");
	strncpy(m2," ",20);
	rval=recvfrom(sockid,m2,sizeof(m2),0,(struct sockaddr *)&s,&slen);
	if(rval==-1)
	{
		perror("MSG-RCV-ERR:");
		exit(1);
	}
	printf("\n message received is %s \n",m2);
	close(sockid);
}
```

>[!danger] INFO
>Same program can be referred for ==UDP-CLIENT==

---
##### 2. Write aim & Program to demonstrate connection-less time service using well known port with expected output & actual output (2d)

```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<sys/types.h>
#include<string.h>
main(int argc, char * argv[])
{
	unsigned long timeval,tempval;
	struct sockaddr_in s;
	int rval,sockid,slen;
	char m1[20],m2[100];
	system("clear");
	if(argc<3){
		printf("\nUSAGE :%s IP-Address Port #\n", argv[0]);
		exit(1);
	}
	sockid = socket(AF_INET, SOCK_DGRAM,0);
	if(sockid ==  -1){
		perror("SOCK-CRE-ERR:");
		exit(1);
	}
	s.sin_family= AF_INET;
	s.sin_addr.s_addr = inet_addr(argv[1]);
	s.sin_port = htons(atoi(argv[2]));
	printf("\nSocket created");
	slen = sizeof(s);
	rval = sendto(sockid,m1,sizeof(m1),0,(struct sockaddr *)&s,slen);
	if(rval == -1){
		perror("\nMSG-SEND-ERR:");
		exit(1);
	}
	printf("\nMessage sent successfully\n");
	rval = recvfrom(sockid,&tempval,sizeof(tempval),0, (struct sockaddr *)&s,&slen);
	if(rval == -1){
	perror("MSG-RCV-ERR");
	exit(1);
	}
	timeval = htonl(tempval);
	printf("\nMessage received is :%u\n",timeval);
	close(sockid);
}
```
##### 3. Write aim & Program Write a Program to Demonstrate Connection oriented (TCP) Echo Service Using Well Known / Standard Port with expected output & actual output
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
main(int argc, char *argv[])
{
    struct sockaddr_in s;
    int sockid, rval;
    char m1[20], m2[20];
    sockid = socket(AF_INET, SOCK_STREAM, 0);
    if (sockid == -1)
    {
        perror("SOCK-CRE-ERR");
        exit(1);
    }
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDR PORT#\n", argv[0]);
        exit(0);
    }
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
    rval = connect(sockid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("CONN-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nEnter the request message : ");
    scanf("%s", m1);
    rval = send(sockid, m1, sizeof(m1), 0);
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nMessage sent successfully\n");
    rval = recv(sockid, m2, sizeof(m2), 0);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nServer response is : %s\n", m2);
    close(sockid);
}
```

>[!TIP] INFO
>Same program can be referred for ==TCP-CLIENT==

---
##### 4. Write aim & Program to demonstrate Connection-oriented time service using well known port with expected output & actual output (3d)

```c

#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
main(int argc, char *argv[])
{
    unsigned long timeval, tempval;
    int sockid, rval;
    char m1[20], m2[20];
    sockid = socket(AF_INET, SOCK_STREAM, 0);
    if (sockid == -1)
    {
        perror("SOCK-CRE-ERR");
        exit(1);
    }
    struct sockaddr_in s;
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDR PORT#\n", argv[0]);
        exit(0);
    }
    s.sin_family = AF_INET;
    s.sin_port = htons(atoi(argv[2]));
    s.sin_addr.s_addr = inet_addr(argv[1]);
    rval = connect(sockid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("CONN-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nEnter the request message : ");
    scanf("%s", m1);
    rval = send(sockid, m1, sizeof(m1), 0);
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nMessage sent successfully\n");
    rval = recv(sockid, &tempval, sizeof(tempval), 0);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        close(sockid);
        exit(1);
    }
    timeval = htonl(tempval);
    printf("\nServer response is : %u\n", timeval);
    close(sockid);
}
```
##### 5. Write aim & Program to demonstrate Connectionless(UDP) Echo Service Using User Defined Port with expected output & actual output
>[!danger] Note
>for ==UDP-CLIENT== program refer Q.no:1

==UDP-SERVER:==
```c
// UDP-SERVER
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<string.h>
main(int argc,char * argv[])
{
	int sockid,rval,clen;
	char buffer[20];
	struct sockaddr_in s,c;
	if(argc<3){
		printf("\nUSAGE:%s IP_ADDRESS PORT #\n",argv[0]);
		exit(0);
	}
	sockid = socket(AF_INET,SOCK_DGRAM,0);
	if(sockid == -1){
		perror("SOCK-CRE-ERR");
		exit(1);
	}
	s.sin_family = AF_INET;
	s.sin_addr.s_addr = inet_addr(argv[1]);
	s.sin_port = htons(atoi(argv[2]));
	printf("socket created\n");
	rval = bind(sockid,(struct sockaddr *)&s,sizeof(s));
	if(rval == -1){
		perror("BIND-ERR");
		close(sockid);
		exit(1);
	}
	printf("socket binded\n");
	clen = sizeof(c);
	rval = recvfrom(sockid,buffer,sizeof(buffer),0,(struct sockaddr *)&c,&clen);
	if(rval == -1){
		perror("MSG-RCV-ERR");
		exit(1);
	}
	printf("\nRequest received\nRequest message is :%s\n",buffer);
	strcat(buffer," Quadeer");
	rval = sendto(sockid,buffer,sizeof(buffer),0,(struct sockaddr *)&c,clen);
	if(rval == -1){
		perror("MSG-SND-ERR");
		exit(1);
	}
	printf("\nResponse sent successfully\n");
	close(sockid);
}
```
##### 6. Write aim & Program to demonstrate Connectionless(UDP) Date & Time Service Using User Defined Port with expected output & actual output
>[!tip] Note
>for ==UDP-CLIENT== program refer Q.no:1

==UDP-SERVER-DTS:==
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<time.h>
#include<string.h>
main(int argc, char *argv[])
{
	int sockid, rval, clen;
	char buffer[20], smsg[30];
	time_t t;
	struct sockaddr_in s,c;
	if(argc<3){
		printf("\nUSAGE :%s IP-Address port#\n",argv[0]);
	}
	sockid = socket(AF_INET,SOCK_DGRAM,0);
	if(sockid == -1){
		perror("SOCK-CRE-ERR:");
		exit(1);
	}
	s.sin_family = AF_INET;
	s.sin_addr.s_addr = inet_addr(argv[1]);
	s.sin_port = htons(atoi(argv[2]));
	rval = bind(sockid,(struct sockaddr *)&s,sizeof(s));
	if(rval == -1){
		perror("BIND-ERR");
		close(sockid);
		exit(1);
	}
	clen = sizeof(c);
	rval = recvfrom(sockid,buffer,sizeof(buffer),0,(struct sockaddr *)&c,&clen);
	if(rval == -1){
		perror("MSG-RCV-ERR");
		exit(1);
	}
	printf("\nRequest received \nRequest message is :%s\n",buffer);
	t = time(0);
	strcpy(smsg,ctime(&t));
	rval = sendto(sockid,smsg,sizeof(smsg),0,(struct sockaddr*)&c,sizeof(c));
	if(rval == -1){
		perror("MSG-SND-ERR:");
		exit(1);
	}
	printf("\nResponse sent successfully\n");
	close(sockid);
}
```

##### 7. Write aim & Program to demonstrate Connection-oriented(TCP) Echo Service Using User Defined Port with expected output & actual output
>[!DANGER] Note
>for ==TCP-CLIENT== program refer Q.no:3

==TCP-SERVER:==
```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
main(int argc, char *argv[])
{
    int sid, sid1, rval;
    struct sockaddr_in s, c;
    char buffer[20];
    int clen;
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDRESS PORT#\n", argv[0]);
        exit(0);
    }
    sid = socket(AF_INET, SOCK_STREAM, 0);
    if (sid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
    /*DEFINING NAME OF THE SERVICE*/
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
    /*BIND SOCKET- indicates the process that is listening*/
    rval = bind(sid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("BIND-ERR:");
        close(sid);
        exit(1);
    }
    rval = listen(sid, 5); // range : 1-5
    if (rval == -1)
    {
        perror("LISTEN-ERR:");
        close(sid);
        exit(1);
    }
    clen = sizeof(c);
    sid1 = accept(sid, (struct sockaddr *)&c, &clen);
    rval = recv(sid1, buffer, sizeof(buffer), 0);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
    }
    else
    {
        printf("\nClient request is %s\n", buffer);
    }
    rval = send(sid1, buffer, sizeof(buffer), 0);
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
    }
    else
    {
        printf("\nResponse sent\n");
    }
    close(sid);
    close(sid1);
}
```

##### 8. Write aim & Program to demonstrate Connection-oriented (TCP) Date & Time Service Using User Defined Port with expected output & actual output
>[!FAQ] NOTE 
>for ==TCP-CLIENT== program refer Q.no:3

==TCP-SERVER-DTS:==
```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <time.h>
#include <string.h>
main(int argc, char *argv[])
{
    int sid, sid1, rval;
    time_t t = time(0);
    struct sockaddr_in s, c;
    char smsg[30];
    strcpy(smsg, ctime(&t));
    int clen;
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDRESS PORT#\n", argv[0]);
        exit(0);
    }
    sid = socket(AF_INET, SOCK_STREAM, 0);
    if (sid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }   /*DEFINING NAME OF THE SERVICE*/
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
    /*BIND SOCKET- indicates the process that is listening*/
    rval = bind(sid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("BIND-ERR:");
        close(sid);
        exit(1);
    }
    rval = listen(sid, 5); // range : 1-5
    if (rval == -1)
    {
        perror("LISTEN-ERR:");
        close(sid);
        exit(1);
    }
    clen = sizeof(c);
    sid1 = accept(sid, (struct sockaddr *)&c, &clen);
    strcpy(smsg, ctime(&t)); // const time_t* if error
    rval = send(sid1, smsg, sizeof(smsg), 0);
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
    }
    else
    {
        printf("\nResponse sent\n");
    }
    close(sid);
    close(sid1);
}
```

##### 9. Write aim & Program to demonstrate Connection-oriented (TCP) Iterative Echo Service using user defined port with expected output & actual output
>[!EXAMPLE] NOTE 
>for ==TCP-CLIENT== program refer Q.no:3

==TCP-ITR-SERVER:==
```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
main(int argc, char *argv[])
{
    int sid, sid1, rval, itr, i; // sid is half association. sid1 is full association
    struct sockaddr_in s, c;
    char buffer[20];
    int clen; // accept() uses value-result parameter
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDRESS PORT#\n", argv[0]);
        exit(0);
    }
        printf("\nEnter the number of clients to serve/ server iterations : ");
    scanf("%d", &itr);
    sid = socket(AF_INET, SOCK_STREAM, 0);
    if (sid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
    /*DEFINING NAME OF THE SERVICE*/
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
    /*BIND SOCKET- indicates the process that is listening*/
    rval = bind(sid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("BIND-ERR:");
        close(sid);
        exit(1);
    }
    rval = listen(sid, 5); // range : 1-5
    if (rval == -1)
    {
        perror("LISTEN-ERR:");
        close(sid);
        exit(1);
    }
    for (i = 1; i <= itr; i++)
    {
        clen = sizeof(c);
        sid1 = accept(sid, (struct sockaddr *)&c, &clen);
        /*sid1 is a full association tuple and has information of client,server and communication
        protocol i.e serving socket*/
        rval = recv(sid1, buffer, sizeof(buffer), 0);
        if (rval == -1)
        {
            perror("MSG-RCV-ERR:");
        }
        else
        {
            printf("\nClient request is %s\n", buffer);
        }
        rval = send(sid1, buffer, sizeof(buffer), 0);
        if (rval == -1)
        {
            perror("MSG-SND-ERR:");
        }
        else
        {
            printf("\nResponse sent\n");
        }
        close(sid1); // closing the serving socket
    }
    close(sid); // closing the listening socket
}
```

##### 10. Write aim & Program to demonstrate Connection-oriented(TCP) Concurrent Echo Service using user defined port with expected output & actual output
>[!TIP] NOTE 
>for ==TCP-CLIENT== program refer Q.no:3

==TCP-CONC-SERVER:==
```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
main(int argc, char *argv[])
{
    int sid, sid1, rval, itr, i, pid; // sid is half association. sid1 is full association
    struct sockaddr_in s, c;
    char buffer[20];
    int clen; // accept() uses value-result parameter
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDRESS PORT#\n", argv[0]);
        exit(0);
    }
    printf("\nEnter the number of clients to serve/ server iterations : ");
    scanf("%d", &itr);
    sid = socket(AF_INET, SOCK_STREAM, 0);
    if (sid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
    /*DEFINING NAME OF THE SERVICE*/
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
    /*BIND SOCKET- indicates the process that is listening*/
    rval = bind(sid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("BIND-ERR:");
        close(sid);
        exit(1);
    }
    rval = listen(sid, 5); // range : 1-5
    if (rval == -1)
    {
        perror("LISTEN-ERR:");
        close(sid);
        exit(1);
    }
    for (i = 1; i <= itr; i++)
    {
        clen = sizeof(c);
        sid1 = accept(sid, (struct sockaddr *)&c, &clen);
        if (sid1 == -1)
        {
            perror("ACCEPT-ERR:");
            close(sid);
            exit(1);
        }
        pid = fork();
        if (pid == -1)
        {
            perror("FRK-ERR:");
            close(sid1);
            close(sid);
            exit(1);
        }
        /*sid1 is a full association tuple and has information of client,server and communication
        protocol i.e serving socket*/
        if (pid == 0) // CHILD
        {
            rval = recv(sid1, buffer, sizeof(buffer), 0);
            if (rval == -1)
            {
                perror("MSG-RCV-ERR:");
            }
            else
            {
                printf("\nClient request is %s\n", buffer);
            }
            rval = send(sid1, buffer, sizeof(buffer), 0);
            if (rval == -1)
            {
                perror("MSG-SND-ERR:");
            }
            else
            {
                printf("\nResponse sent\n");
            }
            close(sid1); // closing the serving socket
            exit(0);
        }
        else             // PARENT
            close(sid1); /*parent also has a copy of the serving socket. So close it here.*/
    }
    close(sid); // closing the listening socket
    exit(0);
}
```