# Chapter 1 - Inital Set Up

## The Command Line

unlike the book approach, i used [cmder](https://cmder.app/)

s the computer name/username :

```bash
$ whoami
desktop-6aark50\taha
```

print hello world :

```bash
$ echo "hello world"
"hello world"
```

print working directory

```bash
$ pwd
C:\Users\Taha\Documents\GitHub\cheat-sheets\Django for apis book
```

change directory

```bash
$ cd onedrive\desktop
```

To make a new directory use the command mkdir followed by the name

```bash
$ mkdir new_directory
$ cd new_directory
```

to list the contents of a directory use the command ls

```bash
$ ls
DjangoForAPIs.md  new_directory/
```

to exit the terminal use the mouse or the command exit

```bash
$ exit
```

## Installing Python 3

To check if python is installed on your computer, open the terminal and type python3. If you see something like this, you have python installed:

```bash
$ python
Python 3.11.2 (tags/v3.11.2:878ead1, Feb  7 2023, 16:38:35) [MSC v.1934 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

or you can use the command python --version

```bash
$ python3 --version
Python 3.11.2
```

if not installed, download it from [here](https://www.python.org/downloads/)

## Virtual Environments

A virtual environment is a tool that helps to keep dependencies required by different projects separate by creating isolated python virtual environments for them. It solves the “Project X depends on version 1.x but, Project Y needs 4.x” dilemma, and keeps your global site-packages directory clean and manageable.

To create a virtual environment, go to the directory where you want to create it and run the command:

```bash
$ python -m venv .venv
```

To activate the virtual environment, run the command:

```bash
$ .venv\Scripts\activate.bat
```

To deactivate and leave a virtual environment type deactivate.

```bash
$ deactivate
```

to delete the virtual environment, delete the .venv folder

```bash
$ rm -rf .venv
```

## Installing Django and Django REST Framework

To install Django run the command:

```bash
$ pip install django
```

To install Django REST Framework run the command:

```bash
$ pip install djangorestframework
```

to output the installed packages, run the command:

```bash
$ pip freeze
asgiref==3.6.0
Django==4.2
djangorestframework==3.14.0
pytz==2023.3
sqlparse==0.4.3
tzdata==2023.3
```

It is a standard practice to output the contents of a virtual environment to a file called requirements.txt. This is a way to keep track of installed packaged and also lets other developers
recreate the virtual environment on different computers.

```bash
$ pip freeze > requirements.txt
```

# Chapter 2 - Web APIs

## World Wide Web

The World Wide Web (WWW) is a system of interlinked hypertext documents accessed via the Internet. It is a collection of documents and other web resources, linked by hyperlinks and URLs. Each resource is identified by a Uniform Resource Identifier (URI), and may be a web page, image, video, or other type of file.

## URLs

is the address of a resource on the internet. For example, the Google homepage lives at `https://www.google.com`

This request and response pattern is the basis of all web communication. A client (typically a web browser but also a native app or really any internet-connected device) requests information and a server responds with a response.

Since web communication occurs via HTTP these are known more formally as HTTP requests
and HTTP responses.

The google url is made up of three parts:

- scheme: `https` | `http` | `ftp` (for files) | `stmp` (for email)
- hostname: `www.google.com`: the actual name of the site
- path (optional) : `https://www.python.org/about/`. The `/about/` piece is the path.

## Internet Protocol Suite

The Internet Protocol Suite (TCP/IP) is a set of communication protocols used to interconnect network devices on the internet. It is commonly known as TCP/IP because the foundational protocols in the suite are the Transmission Control Protocol (TCP) and the Internet Protocol (IP).

when you type `www.google.com` in your browser, your computer sends a request to the DNS server (domain name service) to find the IP address of the server that hosts the google website. The DNS server then returns the IP address to your computer.

After your computer receives the IP address, it need a way to set up a consistent connection with the desired server. This happens via the Transmission Control Protocol (TCP) which provides reliable, ordered, and error-checked delivery of bytes between two application.

To establish a TCP connection between two computers, a three-way “handshake” occurs between the client and server :

1. The client sends a SYN (synchronize) packet to the server.
2. The server responds with a SYN-ACK (synchronize-acknowledge) packet amd passing a connection parameter
3. The client responds with an ACK (acknowledge) packet confirming the connection.

Once the TCP connection is established, the two computers can start communicating via HTTP.

## HTTP verbs

HTTP verbs are used to describe the action that is being performed on a resource. The most common HTTP verbs are:

- GET: Read a resource
- POST: Create a resource
- PUT: Update a resource
- DELETE: Delete a resource

## Endpoints

An endpoints contains data, typically in the `JSON` format, and also a list of available actions (HTTP verbs)

for example, the endpoint `https://api.github.com/users/` contains a list of all github users. The endpoint `https://api.github.com/users/{username}` contains the data for a specific user.

## HTTP

HTTP is the protocol that is used to send and receive data ol between two computers that have an existing TCP connection. It is a request-response protocol, which means that a client (typically a web browser) sends a request to a server and the server responds with a response.

Every HTTP message consists of

```bash
Response/request line
Headers...

(optional) Body
```

for example, the following is a HTTP request:

```bash
GET / HTTP/1.1
Host: www.google.com
Accept_Language: en-US
```

The top line is the status line, which contains the HTTP verb, the path, and the HTTP version.

The next line is the header, `Host` is the domain name and `Accept_Language` is
the language to use

example of a HTTP response:

```bash
HTTP/1.1 200 OK
Date: Mon, 24 Jan 2022 23:26:07 GMT
Server: gws
Accept-Ranges: bytes
Content-Length: 13
Content-Type: text/html; charset=UTF-8
Hello, world!
```

The top line is the status line, which contains the HTTP version, the status code `200 ok`

The next lines are the headers, after it is the body of the response. `Hello, world!`

## Status Codes

HTTP status codes are used to indicate the success or failure of an HTTP request. The most common status codes are:

- `2xx Success` - the action requested by the client was received, understood, and accepted
- `3xx Redirection` - the requested URL has moved
- `4xx Client Error` - there was an error, typically a bad URL request by the client
- `5xx Server Error` - the server failed to resolve a request

for example:

```
- 200: OK
- 201: Created
- 204: No Content
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 405: Method Not Allowed
- 500: Internal Server Error
- 502: Bad Gateway
- 503: Service Unavailable
```

## Statelessness

HTTP is a stateless protocol, which means that each request/response pair is completely independent of the previous one. There is no stored
memory of past interactions

## REST

It is an architecture approach to building APIs on top of the web, which means on top of the HTTP protocol.

For an api to be RESTful, it must follow the REST constraints:

- stateless like HTTP
- supports common HTTP verbs (GET, POST, PUT, DELETE, etc.)
- returns data in either the JSON or XML format
