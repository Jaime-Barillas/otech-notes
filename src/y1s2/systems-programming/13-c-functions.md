# Good to Know Functions

## Memory

|Function|Header|Notes|
|:------:|------|-----|
|malloc|stdlib.h||
|free|stdlib.h||
|memcmp|string.h||
|memset|string.h||

## Strings

|Function|Header|Notes|
|:------:|------|-----|
|strcat|string.h|Concatenate two strings|
|strcmp|string.h|Compare two strings|
|strcpy|string.h|Copy a string|
|strdup|string.h|Duplicate a string|
|strlen|string.h|Calculate the length of a string|
|strstr|string.h|Locate a substring|
|strtok|string.h|Extract tokens from strings|
|strto{d,f}|stdlib.h|Convert a string to a double/float|
|ato{i,l}|stdlib.h|Convert a string to an integer/long|
|toupper|ctype.h|Operates on a single character, convert input to unsigned char before use|
|tolower|ctype.h|Operates on a single character, convert input to unsigned char before use|

## File I/O

|Function|Header|Notes|
|:------:|------|-----|
|fopen|stdio.h||
|fclose|stdio.h||
|fread|stdio.h|Read n bytes|
|fwrite|stdio.h|Write n bytes|
|fscanf|stdio.h|Formatted read|
|fprintf|stdio.h|Formatted write|
|fgets|stdio.h|Read line-by-line|
|fseek|stdio.h|Set file position|
|getcwd|unistd.h|Current working directory|
|stat|sys/stat.h|File attributes|
|chmod|sys/stat.h|Change file permissions|
|opendir|dirent.h|Open a directory for listing contents|
|readdir|dirent.h|Get next directory entry|
|closedir|dirent.h|Close directory|

Plus some others...

## Processes/Threads

|Function|Header|Notes|
|:------:|------|-----|
|fork|sys/types.h, unistd.h||
|exec\*|unistd.h|Family of functions (see execvp)|
|waitpid|sys/types.h, sys/wait.h|wait for process to change state|
||||
|pthread\_create|pthread.h||
|pthread\_exit|pthread.h|It is not generally required to explicitly call this function|
|pthread\_join|pthread.h||
|pthread\_self|pthread.h||
|pthread\_mutex\_init|pthread.h||
|pthread\_mutex\_destroy|pthread.h||
|pthread\_mutex\_lock|pthread.h||
|pthread\_mutex\_unlock|pthread.h||

## Networking

Required functions to start a TCP server:
```c
#include <sys/socket.h> // included by arpa/inet, socket, bind, etc.
#include <netinet/in.h> // included by arpa/inet, struct sockaddr_in
#include <arpa/inet.h> // inet_pton, htons
#include <unistd.h> // close

void server() {
    // First, setup the address.
    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(8080);
    inet_pton(AF_INET, "127.0.0.1", &saddr.sin_addr);

    // Socket setup
    int s_fd = socket(AF_INET, SOCK_STREAM, 0);
    bind(s_fd, (struct sockaddr*)&saddr, sizeof(saddr));
    listen(s_fd, 1);

    // Client connections
    int c_fd;
    struct sockaddr_in caddr;
    unsigned int caddr_len = sizeof(caddr);
    c_fd = accept(s_fd, (struct sockaddr*)&caddr, &caddr_len);
    recv(c_fd, <buf>, <size>, 0);
    send(c_fd, <buf>, <size>, 0);

    close(c_fd);
    close(s_fd);
}

void client() {
    // Address
    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(8080);
    inet_pton(AF_INET, "127.0.0.1", &saddr.sin_addr);

    // Socket setup
    int s_fd = socket(AF_INET, SOCK_STREAM, 0);
    connect(s_fd, (struct sockaddr*)&saddr, sizeof(saddr));

    // Communication
    recv(s_fd, <buf>, <size>, 0);
    send(s_fd, <buf>, <size>, 0);

    close(s_fd);
}
```

For UDP communication:
```c

void server() {
    // First, setup the address.
    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(8080);
    inet_pton(AF_INET, "127.0.0.1", &saddr.sin_addr);

    // Socket setup
    int s_fd = socket(AF_INET, SOCK_DGRAM, 0);
    bind(s_fd, (struct sockaddr*)&saddr, sizeof(saddr));

    // Client connections
    struct sockaddr_in caddr;
    unsigned int caddr_len = sizeof(caddr);
    recvfrom(s_fd, <buf>, <size>, 0, (struct sockaddr*)&caddr, &caddr_len);
    sendto(c_fd, <buf>, <size>, 0, (struct sockaddr*)&caddr, sizeof(caddr));

    close(c_fd);
    close(s_fd);
}

void client() {
    // Address
    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(8080);
    inet_pton(AF_INET, "127.0.0.1", &saddr.sin_addr);
    unsigned int saddr_len = sizeof(saddr);

    // Socket setup
    int s_fd = socket(AF_INET, SOCK_STREAM, 0);

    // Communication
    recv(s_fd, <buf>, <size>, 0, (struct sockaddr*)&saddr, &saddr_len);
    send(s_fd, <buf>, <size>, 0, (struct sockaddr*)&saddr, sizeof(saddr));

    close(s_fd);
}
```
