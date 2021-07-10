
  **Frontend**
  ----------------------------------------------

**React:**
A JavaScript library for building user interfaces.React.js is an open-source JavaScript library that is used for building user interfaces specifically for single-page applications. It’s used for handling the view layer for web and mobile apps. React also allows us to create reusable UI components. React was first created by Jordan Walke, a software engineer working for Facebook. React first deployed on Facebook’s newsfeed in 2011 and on Instagram.com in 2012.

**Axios:**
Axios is a library that helps us make http requests to external resources In our React applications we often need to retrieve data from external APIs so it can be displayed in our web pages.
One way to build this feature is to use the Javascript Fetch API. Fetch is quite capable of retrieving external data, but it has some limitations.
A more popular way of performing this operation is to use the Axios library. Axios is designed to handle http requests and responses.
It's used more often than Fetch because it has a larger set of features and it supports older browsers.

**Nginx:**
Nginx, pronounced like “engine-ex”, is an open-source web server that, since its initial success as a web server, is now also used as a reverse proxy, HTTP cache, and load balancer.
Some high-profile companies using Nginx include Autodesk, Atlassian, Intuit, T-Mobile, GitLab, DuckDuckGo, Microsoft, IBM, Google, Adobe, Salesforce, VMWare, Xerox, LinkedIn, Cisco, Facebook, Target, Citrix Systems, Twitter, Apple, Intel, and many more (source).

**Docker:**
Docker is an open source platform for building, deploying, and managing containerized applications.It enables developers to package applications into containers—standardized executable components combining application source code with the operating system (OS) libraries and dependencies required to run that code in any environment. Containers simplify delivery of distributed applications, and have become increasingly popular as organizations shift to cloud-native development and hybrid multicloud environments.

**Create a React App:**
--------------------------------
$ npx create-react-app react-docker  ###create package.json(Package Dependency) using npm init command.  
$ cd react-docker  
$ npm install axios  
$ npm start   

**Create Todolist.js file**  
**Code:**  
-----------------------------
```
import React from 'react';
import axios from 'axios';

export default class TodoList extends React.Component{
    state = {
        todos: []
    }
    componentDidMount(){
        axios.get(`https://jsonplaceholder.typicode.com/users`)
      .then(res => {
        const todos = res.data;
        this.setState({ todos });
      })
    }

    render() {
        return (
          <ul>
            { this.state.todos.map(todos => <li>{todos.name}</li>)}
          </ul>
        )
      }
}
```     
        
**Update App.js file**
```
//import logo from './logo.svg';
import './App.css';
import TodoList from './Todolist.js'

function App() {
  return (
    <div className="App">
     <TodoList></TodoList> 
    </div>
  );
}

export default App;
```

I am using nginx for web server. Dockerfile will install nginx. So here is the Nginx configuration file:

**Create conf/conf.d directory in react-docker directory:**  
$ mkdir conf/conf.d -p  
$ vim conf/conf.d/default.conf  
```
server {
  listen 80;
  location / {
    root   /usr/share/nginx/html;
    index  index.html index.htm;
    try_files $uri $uri/ /index.html;
  }
  error_page   500 502 503 504  /50x.html;
  location = /50x.html {
    root   /usr/share/nginx/html;
  }
}
```
        
Create Dockerfile:
```
FROM node:latest as builder
WORKDIR /usr/src/app
COPY package.json .
RUN npm install 
COPY . .
RUN npm run build

FROM nginx:1.19.4-alpine

RUN rm -rf /etc/nginx/conf.d
COPY conf /etc/nginx
COPY --from=builder /usr/src/app/build /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Using Docker:**
```
$ docker build -t <usrname>/<image_name>:version .  
$ docker run -p <outer_port>:<inner_port> <usrname>/<image_name>:version  
```
**Command:**  
$ docker build -t sujit/react-docker:v1.0.1 .  
$ docker run -p 8080:80 sujit/react-docker:v1.0.1 
    
**Without using Docker**  
$ git clone https://github.com/SujitAdhikari/react-docker.git  
$ cd react-docker Note: ($ code . #open vscode)  
$ npm install  
$ npm start  

--------------------------Note-----------------------------  
**npm init:**
npm init <initializer> can be used to set up a new or existing npm package. initializer in this case is an npm package named create-<initializer>, which will be installed by npm-exec, and then have its main bin executed -- presumably creating or updating package.json and running any other initialization-related operations.

**npm start:**
This runs a predefined command specified in the "start" property of a package's "scripts" object. If the "scripts" object does not define a "start" property, npm will run node server.js.

**npm build:**
npm run build: runs the build field from the package.json scripts field. npm run build creates a build directory with a production build of your app. Inside the build/static directory will be your JavaScript and CSS files. Each filename inside of build/static will contain a unique hash of the file contents. This hash in the file name enables long term caching techniques.

 
$ npm build  
or  
$ npm run build #Short Note: create build dir and index.html  

----------------------------------------------------
  **Backend**
----------------------------------------------------
  **Node.js:**
Node.js is a platform built on Chrome's JavaScript runtime for easily building fast and scalable network applications.
Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive
real-time applications that run across distributed devices.

Node.js is an open source, cross-platform runtime environment for developing server-side and networking applications.
Node.js applications are written in JavaScript, and can be run within the Node.js runtime on OS X, Microsoft Windows, and Linux.

  **Express.js:**
Express.js is a Node js web application server framework, which is specifically designed for building single-page, multi-page, 
and hybrid web applications. It has become the standard server framework for node.js. Express is the backend part of 
something known as the MEAN stack. The Express.js framework makes it very easy to develop an application which 
can be used to handle multiple types of requests like the GET, PUT, and POST and DELETE requests.

**Cors:**
CORS is shorthand for Cross-Origin Resource Sharing. It is a mechanism to allow or restrict requested resources on a web server depend on where the HTTP request was initiated. It allows us to relax the security applied to an API. This is done by bypassing the Access-Control-Allow-Origin headers, which specify which origins can access the API.
In other words, CORS is a browser security feature that restricts cross-origin HTTP requests with other servers and specifies which domains access your resources.
 
**Docker Compose:**
Docker Compose is a tool for running multi-container applications as a single service on Docker defined using the Compose file format. A Compose file is used to define how the one or more containers that make up your application are configured. Once you have a Compose file, you can create and start your application with a single command: docker-compose up.
   
Each of the containers here run in isolation but can interact with each other when required. Docker Compose files are very easy to write in a scripting language called YAML, which is an XML-based language that stands for Yet Another Markup Language. 
 

 
** Packages:**
```
  $ npm i cors
  $ npm i nodemon
  $ nodemon server.js  
```
 or
```
  $ npm i cors express nodemon
```
**Create a backend directory: **
 ```
  $ mkdir backend
 ```
 **Initializer**
 ```
  $ npm init
 ```
** Make a js file:**
   $ vim index.js
 ```
const express = require("express");

const app = express();

const users = [
    {
      "id": 1,
      "name": "Leanne Graham",
      "username": "Bret",
      "email": "Sincere@april.biz",
      "address": {
        "street": "Kulas Light",
        "suite": "Apt. 556",
        "city": "Gwenborough",
        "zipcode": "92998-3874",
        "geo": {
          "lat": "-37.3159",
          "lng": "81.1496"
        }
      },
      "phone": "1-770-736-8031 x56442",
      "website": "hildegard.org",
      "company": {
        "name": "Romaguera-Crona",
        "catchPhrase": "Multi-layered client-server neural-net",
        "bs": "harness real-time e-markets"
      }
    }
  ]

// app.use(cors)
app.use(express.json());

app.get("/api/v1/healthz",(req,res)=>{
    console.log("hello");
    res.send({"msg":"hi"})
})

app.get("/api/v1/users",(req,res)=>{
    console.log(req);
    res.send(users);
})

app.post("/api/v1/add",async(req,res)=>{
  res.send(users);
})

app.listen(8080,()=>{
    console.log("The server is running")
})
 ```
 
** Create a Dockerfile:**
   $ vim Dockerfile
 ```
 FROM node:14

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "index.js" ]
 ```
 
** Create a docker-compose file under react-node-docker directory:**
   $ vim docker-compose
 ```
 
version: '3'

services:
  backend:
    build:
      context: ./backend
      dockerfile: ./Dockerfile
    image: "devops2015/backend"
    ports:
      - "8081:8080"
  frontend:
    build:
      context: ./frontend
      dockerfile: ./Dockerfile
    image: "devops2015/frontend"
    ports:
      - "3030:80"
    depends_on: 
      - "backend" 
  # db:

** Run and build containers using docker-compose:**
   $ docker-compose build
   $ docker-compose up
 
 
 ```
  **Check Running Docker container:**
  ubuntu@srv:~$ sudo docker ps
  CONTAINER ID   IMAGE                 COMMAND                  CREATED       STATUS          PORTS                                       NAMES
  f9f66fb34904   devops2015/frontend   "/docker-entrypoint.…"   9 hours ago   Up 34 seconds   0.0.0.0:3030->80/tcp, :::3030->80/tcp       react-node-docker_frontend_1
  5377751e0674   devops2015/backend    "docker-entrypoint.s…"   9 hours ago   Up 35 seconds   0.0.0.0:8081->8080/tcp, :::8081->8080/tcp   react-node-docker_backend_1

**Verification Frontend:**
  $ curl localhost:3030
  Or
**Use Browser to check:**
 ![image](https://user-images.githubusercontent.com/82542326/125155080-6534fe00-e17f-11eb-93c2-90c5c4b039e9.png)

  Request URL:  https://jsonplaceholder.typicode.com/users
**Backend Verification:**
 
![image](https://user-images.githubusercontent.com/82542326/125155093-74b44700-e17f-11eb-8dc9-e4a0e03a385e.png)

Check frontend:  
![image](https://user-images.githubusercontent.com/82542326/125155102-80a00900-e17f-11eb-8587-ad5d28bf5e94.png)
 

Request URL:  https://jsonplaceholder.typicode.com/users
Now we want to load the backend api from frontend:

**Update react-node-docker/frontend/components/Todolist.js**
 ```
    // axios.get(`https://jsonplaceholder.typicode.com/users`)
      axios.get(`http://localhost:8081/api/v1/users`)
```
I am using // for comment for above line and add backed api http://localhost:8081/api/v1/users`) .
But it is not working, I am checking this error.

**Configure Reverse Proxy:**
 ```
server {
	  listen 80;
	  location / {
	    root   /usr/share/nginx/html;
	    index  index.html index.htm;
	    try_files $uri $uri/ /index.html;
	  }
	  location /api/v1/ {
	      proxy_pass      http://backend:8080;
	  }
	  error_page   500 502 503 504  /50x.html;
	  location = /50x.html {
	    root   /usr/share/nginx/html;
	  }
	}
```

**Verification reverse proxy:**
![image](https://user-images.githubusercontent.com/82542326/125155114-91e91580-e17f-11eb-854a-2d9712fb9560.png)
 
Reverse proxy is working, I want to check running container and IP address of frontend & backend container. 
 ![image](https://user-images.githubusercontent.com/82542326/125155145-b9d87900-e17f-11eb-89f2-f608e83d5859.png)


Here we are getting dns server:  127.0.0.11 





Now I am checking port reachability from frontend container to backend inner port 8080. Install busybox-extras for telnet.

![image](https://user-images.githubusercontent.com/82542326/125155158-cd83df80-e17f-11eb-9fbc-2f04c530c552.png)
 

Log in to Backend and install net-tools for ifconfig command:

![image](https://user-images.githubusercontent.com/82542326/125155167-d96fa180-e17f-11eb-8dfc-29a2e295ef06.png)
 



I am checking react-node-docker_default bridge inspect the bridge to get container IP network range and other.
![image](https://user-images.githubusercontent.com/82542326/125155184-e7252700-e17f-11eb-85fe-49a82fd22a21.png)
 
Here, Frontend is running on 3030 port and backend is running on port 8080, I want to run tcpdump on the port.
![image](https://user-images.githubusercontent.com/82542326/125155185-eee4cb80-e17f-11eb-99a0-33f775ef367c.png)
 

-----------------------Notes-------------------------  
  YAML: A dictionary is represented in a simple key: value form (the colon must be followed by a space). 
 ```
 # An employee record
martin:
  name: Martin D'vloper
  job: Developer
  skill: Elite
 ```
 More complicated data structures are possible, such as lists of dictionaries, dictionaries whose values are lists or a mix of both:
```
# Employee records
- martin:
    name: Martin D'vloper
    job: Developer
    skills:
      - python
      - perl
      - pascal
- tabitha:
    name: Tabitha Bitumen
    job: Developer
    skills:
      - lisp
      - fortran
      - erlang 
```
