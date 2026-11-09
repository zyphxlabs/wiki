# Lecture 8 Web Servers and curl

I do two things in this lecture. First I start a website on Linux with one command using Python. Second I talk to websites using the command line with tools like curl and wget. These are essential for every IT person and especially hackers. I start the lab and open the Linux Fundamentals module and launch the terminal. I can do this on my own Kali or Parrot VM too.

## Why a Quick Website

It is cool and it is a very useful tool when hacking or doing IT stuff. It gives another way to transfer files from a Linux box besides SSH and FTP and Netcat. If those are not available I can quickly spin up a website and browse the files.

## Python Web Server

```bash
python3 -m http.server
```

That is it. A web server is running on my Linux box. To visit it I opened Firefox and went to localhost. localhost means this computer I am on. It is hosting the website locally. The server doesnt run on a normal website port. The normal port for http is 80 and for https it is 443. Here it runs on 8000. So I put a colon and the port.

```
localhost:8000
```

In the terminal it logged that someone came. In the browser I got a directory listing of the files. I clicked into Documents and it was empty. Desktop had a file with credentials. I clicked it and it opened. It is a file on the desktop but I accessed it through a web server.

To stop the server I press Ctrl+C. I can give another port after the command.

```bash
python3 -m http.server 7600
```

Now it runs on `localhost:7600`.

## Making My Own Page

It lists files because there is no `index.html` in the folder. Websites use that file to bring up pages. So I made one.

```bash
mkdir website
cd website
nano index.html
```

I wrote a line like hi welcome to my website. Then Ctrl+X then Y then Enter to save. Then I ran the server again in that folder and refreshed the browser. Now my page showed. The server runs in whatever directory I am in. That is why it didnt list the files from before because they were one directory up.

## PHP and Node Servers

I kept the Python one running and opened another terminal. I started a PHP server. The address `127.0.0.1` is the loopback address. It is a special IP address that always loops back to my own network card. This machine is virtual and the virtual network card is the same. `localhost` is like the DNS name for `127.0.0.1`.

```bash
php -S 127.0.0.1:8085
```

Then in a third terminal I used node. `npx` runs packages from npm which is the node package manager.

```bash
npx http-server -p 8086
```

`-p` is for the port. Now I had three web servers running. Python is my favorite but I might not have it in a given situation so I should be ready with other tools. PHP gave nothing to show at 8085 because there was nothing for it to serve.

## Apache

Apache is a very popular web server and I usually already have it installed. It is a service so I use what I learned.

```bash
sudo systemctl start apache2
```

It asked for sudo and the password. In the lab I took the password from the credentials file on the desktop. It still failed. Apache wants port 80 by default but the lab already uses port 80. So I changed the port. I edited one file.

```bash
sudo nano /etc/apache2/ports.conf
```

I went down to the line that says `Listen` and changed 80 to 8080. Listen means the web server only hears requests on that port. Then Ctrl+X and Y and Enter. And I started it again.

```bash
sudo systemctl start apache2
```

In the browser `localhost:8080` showed the Apache2 default page. Now I had four web servers on one machine.

## curl

Now I talk to websites without a browser. `curl` stands for client URL. It is used in so many things and it comes pre installed almost everywhere. My main use is downloading files. It works with HTTP and HTTPS and FTP and FTPS and SFTP. But I can also communicate with websites and test them and send them things and hack them.

```bash
curl localhost:7600
curl localhost:8080
```

I browsed the website from the command line. I get the source code of the page because I am downloading it. It is all HTML code.

To save it to a file I use lowercase `-o` and a file name.

```bash
curl -o cool-website localhost:8080
ls
cat cool-website
```

`cat` shows what I downloaded. I could use nano or vim but cat is quick.

## Headers

I can analyze how a website talks to its client. Capital `-I` shows only the headers.

```bash
curl -I localhost:8080
```

This is a response header. Think of a letter. The contents are the message. The envelope has the mailing details like where it is from. That is the header. It tells who is sending and what kind of data. Here it told me the server is Apache and gave the content length and content type. There is a status like `200 OK` and that is the best message when dealing with websites. It means yes it is here. I can check and troubleshoot and manipulate this and I will use it as a hacker.

## Request Headers

Now I want to see what I sent. A conversation has two sides. I use `-v` which means verbose and it is very chatty.

```bash
curl -v localhost:8080
```

I scrolled to the very top. The arrows tell the direction. An arrow pointing right `>` is a request header. An arrow pointing left `<` is a response header.

The request was a `GET` because I was trying to get something. Other methods exist. A `POST` request sends something and updates data. Adding something to something could be handy for hacking. There are others like `PUT`. Most of the time a browser sends GET to ask for a web page. The site answers with a response header usually `200 OK` and what it sends and then the contents of the response.

## wget

`wget` does the same thing as curl for one job and that is downloading.

```bash
wget localhost:7600
ls
```

It downloaded `index.html` the code of the first page. It also showed HTTP headers and the request sent and the response. The response was `200 OK`. A code I never want to see is `404` which means not found. It means the web address doesnt exist on that server.

## Recap

- I can start a quick web server with Python or PHP or npx and I can also start Apache with systemctl
- Those give me a way to transfer files and browse them
- `curl` lets me talk to websites from the command line. `-o` saves a file and `-I` shows the response header and `-v` shows the request and response
- `wget` downloads files
- The curl tool has much more to learn and I can always search for it
