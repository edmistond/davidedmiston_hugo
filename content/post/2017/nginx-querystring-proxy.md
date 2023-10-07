---
title: "Routing requests through nginx by querystring"
date: 2017-11-16
tags: [nginx, devops, docker]
---

I'm writing this up as much for my own future reference as I am to help others, since I found the documentation a bit lacking in this area.

For reasons not worth getting into, I needed to build a proof-of-concept to transparently proxy requests to different servers based on a querystring value, using nginx as a reverse proxy.

<!-- more -->

The TL;DR solution:

```nginx nginx.conf
worker_processes 2;

events { worker_connections 1024; }

http {

  upstream server1 {
    server web1:8080;
  }

  upstream server2 {
    server web2:8080;
  }

  map $arg_queryval $node {
    default       server2;
    "abcd1234"    server1;
  }

  server {

    listen 80;

    location / {
      proxy_pass http://$node;
    }
  }
}
```

This transparently routes any request to the proxy with a querystring argument of `queryval=abcd1234` to server 1. All other requests would go to server 2. It should also automatically include all of the headers and the full querystring, too.

This was a pain in the butt to figure out, and I ended up having to call on a friend more experienced than I am with nginx for the final critical detail that got this working -- basically, each server (or collection of servers, since you could theoretically load balance across them) needs to be in its own upstream block.

Testing this was interesting. The straightforward way to do it was to set up a quick test environment using Docker and Docker Compose, which are tools I've been interested in but haven't really worked with yet. Fortunately, there's a lot of good information out there on how to set up these sorts of environments. Particularly helpful for me was [Anand Mani Sankar's article](http://anandmanisankar.com/posts/docker-container-nginx-node-redis-example/) covering sample Docker Compose workflows using nginx and node.js, specifically.

So let's delve into this a little! First we have our Express app. Nothing complicated, I just want something that traps all GET and POST requests and dumps the querystring, body, and headers to the console for inspection. There's some repetition in this code, but that's fine for quick-and-dirty.

```javascript index.js
const express = require("express");
const bodyParser = require("body-parser");
const app = express();
const os = require("os");

const PORT = 8080;
const HOST = "0.0.0.0";

app.use(bodyParser.json());

app.post("*", (req, res) => {
  const dirOpts = { showHidden: true, depth: null };
  console.dir(req.body, dirOpts);
  console.dir(req.headers, dirOpts);
  console.dir(req.query, dirOpts);
  res.send("thanks");
});

app.get("*", (req, res) => {
  console.log(process.env.worker_name);
  res.send("it worked!");
});

app.listen(PORT, HOST, () => console.log(`app listening on ${HOST}:${PORT}`));
```

Pretty standard. Nothing of note in package.json other than that you want to have one, and the way I set it up, you do want to have a script for starting the app:

```json package.json
  "scripts": {
    "start": "node index.js"
  }
```

That'll do for both of our server nodes. Now we need to Dockerize it, which is pretty straightforward.

```Dockerfile Dockerfile (node)
FROM node:9.2.0
WORKDIR /usr/src/app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

Use the current version of node, copy package.json first to install deps, copy the app, expose a port, run the app.

Now, nginx. We already have the config file above, so let's just deal with its Dockerfile.

```Dockerfile Dockerfile (nginx)
FROM nginx:latest

RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/nginx.conf

WORKDIR /etc/nginx

CMD ["nginx", "-g", "daemon off;"]

EXPOSE 80
```

Use the latest nginx image, remove the default config, copy in our config, run it. Also pretty straightforward. Not sure if removing the default config is absolutely necessary here. One thing I found was a pain was nginx 'completing' successfully and terminating when running Docker Compose interactively, so I had to set a few additional options. This was fine since I didn't want to run Docker Compose in a detached mode -- I wanted to see the logs immediately in the console. Probably not the ideal production configuration, but for testing, just fine.

Finally, we need to put all the pieces together with a Docker Compose file.

```yaml docker-compose.yml
version: "3"
services:
  load_bal:
    build: ./nginx
    ports:
      - "8080:80"
    links:
      - web1:web1
      - web2:web2
    depends_on:
      - web1
      - web2
  web1:
    build: ./node1
    ports:
      - "8080"
  web2:
    build: ./node1
    ports:
      - "8080"
```

Pretty straightforward. You can see we're creating two instances of the node app. They're identical, but one will handle all the requests that have the specified querystring argument, and we'll be able to see this in the console logs while docker-compose is running. We also create names for the two worker services and link them to the nginx (load_bal) container, which creates internal hostnames for them (you can see nginx.conf is referring to them as well). Finally, we map port 8080 on the host machine to port 80 in the nginx service.

You might also note that web1 and web2 are exposing port 8080, whereas in our Dockerfile for the node app, we expose port 3000. This is partly because I changed it after writing the node app and its Dockerfile, but before creating the compose file. It also seems that options set in the compose file will override ones set in the Dockerfile -- though I wouldn't take my word for it here as I've not gone digging to confirm that.

And the results:

```
web2_1      | { foo: 'bar' }
web2_1      | { host: 'server2',
web2_1      |   connection: 'close',
web2_1      |   'content-length': '13',
web2_1      |   'content-type': 'application/json; charset=utf-8',
web2_1      |   'user-agent': 'Paw/3.1.5 (Macintosh; OS X/10.13.1) GCDHTTPRequest' }
web2_1      | { queryval: 'abcd' }
load_bal_1  | 172.18.0.1 - - [16/Nov/2017:21:32:54 +0000] "POST /?queryval=abcd HTTP/1.1" 200 6 "-" "Paw/3.1.5 (Macintosh; OS X/10.13.1) GCDHTTPRequest"

web1_1      | { foo: 'bar' }
web1_1      | { host: 'server1',
web1_1      |   connection: 'close',
web1_1      |   'content-length': '13',
web1_1      |   'content-type': 'application/json; charset=utf-8',
web1_1      |   'user-agent': 'Paw/3.1.5 (Macintosh; OS X/10.13.1) GCDHTTPRequest' }
web1_1      | { queryval: 'abcd1234' }
load_bal_1  | 172.18.0.1 - - [16/Nov/2017:21:33:05 +0000] "POST /?queryval=abcd1234 HTTP/1.1" 200 6 "-" "Paw/3.1.5 (Macintosh; OS X/10.13.1) GCDHTTPRequest"
```

As you can see from the logs, anything with the specified queryval is getting routed to web1; everything else goes to web2.

This really isn't a production configuration in itself (of nginx _or_ Docker), but for spiking out a quick proof of concept, it was pretty awesome. Setting up a fleet of virtual machines on my laptop would've taken way longer.
