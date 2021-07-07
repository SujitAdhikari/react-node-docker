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

**npm init:**
npm init <initializer> can be used to set up a new or existing npm package. initializer in this case is an npm package named create-<initializer>, which will be installed by npm-exec, and then have its main bin executed -- presumably creating or updating package.json and running any other initialization-related operations.

**npm-start:**
This runs a predefined command specified in the "start" property of a package's "scripts" object. If the "scripts" object does not define a "start" property, npm will run node server.js.

**Create a React App:**
--------------------------------
$ npx create-react-app react-docker  ###create package.json(Package Dependency) using npm init command.  
$ cd react-docker  
$ npm install axios  
$ npm start   #nodejs server start  

**Create Todolist.js file**  
**Code:  **
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
            { this.state.todos.map(person => <li>{person.name}</li>)}
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

I am using nginx for web server. Dockerfile will install nginx. So here is the Nginx configuration:

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
  location /api/v1/ {
      proxy_pass      http://backend:8080;
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
    
**Without using Docker**
$ git clone https://github.com/SujitAdhikari/react-docker.git 
$ cd react-docker  
$ code .    
$ npm install  
$ npm start  

$ npm build  
or
$ npm run build #create build dir and index.html  
