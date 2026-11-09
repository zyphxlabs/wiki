# Web and Networking

## Terms

A web server is a program that sends web pages to whoever asks. The client is the one asking like my browser. A port is a numbered door and normal websites use 80 for http and 443 for https. localhost means this computer. It points to the IP address `127.0.0.1` which is called the loopback address. It always loops back to my own machine. HTTP is the protocol websites use to talk.

## Quick Web Server

One command starts a website that serves the files of the directory I am in. This is useful to move files when the usual ways like ssh or FTP are not available.

```bash
python3 -m http.server            # port 8000
python3 -m http.server 7600       # my own port
php -S 127.0.0.1:8085
npx http-server -p 8086
```

I open it in the browser at `localhost:8000`. I need the port after the colon when it is not 80. If the folder has no `index.html` the server shows a list of files. If there is one it shows that page. Ctrl+C stops the server.

## Apache

Apache is a very common web server and it is often already installed. It is a service so I start it with systemctl.

```bash
sudo systemctl start apache2
```

By default it wants port 80. If that port is taken the start fails. I fix it by editing `/etc/apache2/ports.conf` and changing the `Listen` line to another port like 8080. Listen means the server only hears requests on that port.

## curl

curl means client URL. It talks to websites from the command line. It downloads files and also tests and sends things.

```bash
curl localhost:7600                 # prints the page source
curl -o page.html localhost:8080    # saves it to a file
curl -I localhost:8080              # response headers only
curl -v localhost:8080              # chatty. Shows request and response
```

A header is information about the message and not the message. It is like the envelope of a letter. The request header goes from me to the server and the response header comes back. In the `-v` output an arrow `>` marks the request and `<` marks the response.

A request has a method. GET asks for data and it is the normal one. POST sends data. There are others like PUT.

The response has a status code. 200 means OK and it is the healthy one. 404 means not found.

## wget

wget also downloads from a website. It saves the page as a file.

```bash
wget localhost:7600
```