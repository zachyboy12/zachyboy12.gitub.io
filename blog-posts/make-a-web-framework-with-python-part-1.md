# How to make a web framework with python
# Part 1

In this first part in this series we will be looking at how to make a web framework.  
Before you start this, you must have a basic understanding of how the web works and  
a basic understanding of Python, the language we are going to use.  
Ok, before we get here, you MUST know about WSGI.  
The Python Web Server Gateway Interface, or WSGI for short, is basically something to mix and match  
web frameworks and web servers.  
You can find more about WSGI [here](https://peps.python.org/pep-0333/).
So, WSGI is all well and good, but how do you actually DO it?  
Open your terminal to create a folder named your framework. I named mine apiwsgi:  
```
$ mkdir apiwsgi
```  
Then create a file named __init__.py:  
```
$ touch __init__.py
```  
Then create another file named test.py:  
```
$ touch test.py
```  
Now, let's open the file editor of your choice, code in test.py, and then explain.  
```
from wsgiref.simple_server import make_server

def app(environ, start_response):
  response = b"Hello world"
  status = "200 OK"
  response_headers = []
  start_response(status, response_headers)
  return [response]

make_server('localhost', 8000, app).serve_forever()
```  
So, here's the explaination:  
wsgiref.simple_server helps serve one WSGI application.  
environ is basically CGI-style variables, which is what I call the request. It is a Python dictionary.  
start_response is a function to start the response, hence the name. It takes a status  
parameter and a headers parameter.  
make_server makes a simple WSGI server that serves on HOST:PORT and only serves one WSGI app, as mentioned.  
After this, delete everything in test.py.  
Now, let's convert this to a class, since it is much more convenient that way:  
```
class API:  # or whatever you want
  def app(self, request, response):
    response("200 OK", [])
    return [b"Hello, world!"]
```  
Note: I changed environ to request, and start_response to just response.  
Now, to run this, just write this code in test.py:
```
from __init__ import API
from wsgiref.simple_server import make_server
api = API()
server = make_server('localhost', 8000, api.app)
server.serve_forever()
```  
There's a catch. It's not perfect. Found it? It handles every request the same way: It always says  
"Hello, world!". What if you want to implement routing?  
Let's explain first, then code.  
So, in the request dictionary, there is a key called PATH_INFO, which is the requested path. You can  
play around with this.  
Let's do this!  
```
class API:  # or whatever you want
  def app(self, request, response):
    response("200 OK", [])
    if request["PATH_INFO"] == '/':  # if the path is the home page
      return [b"Hello, world!"]
```  
Ok, run test.py and see what happens. Did you see it? Cool. Let's move on.  
So how are we going to let the user route custom routes with custom handlers? Simple. Let's do it!  
Our plan is to use a Python dictionary; The key for the route and the value for the handler.  
Over here, we are going to use a decorator to route, and create a runserver function.  
The handler is gonna have a request param.  
```
class API:  # or whatever you want
  def __init__(self):
    self.routes = None
    
    
  def route(self, route: str):
    def wrapper(app):
      if self.routes is None:
        self.routes = {route: app}
      else:
        self.routes[route] = app
    return wrapper


  def app(self, request, response):
    response("200 OK", [])
    return [bytes(str(self.routes[request['PATH_INFO']](request)).encode())]
    
    
  def runserver(self, host='localhost', port=8000):
    from wsgiref.simple_server import make_server
    server = make_server(host, port, self.app)
    try:
      server.serve_forever()
    except KeyboardInterrupt:
      server.shutdown()
```  
Awesome. Now delete everything inside test.py and add the following:  
```
from __init__ import API
api = API()
@api.route('/')
def home(request):
  return request
api.runserver()
```  
Does this seem familiar? Yes, it's Flask! Let's make this a little more  
original by adding a class called HTTPResponse:  
```
class HTTPResponse:
  def __init__(self, body):
    self.body = body


class API:  # or whatever you want
  def __init__(self):
    self.routes = None
    
    
  def route(self, route: str):
    def wrapper(app):
      if self.routes is None:
        self.routes = {route: app}
      else:
        self.routes[route] = app
    return wrapper


  def app(self, request, response):
    response("200 OK", [])
    return [bytes(str(self.routes[request['PATH_INFO']](request).body).encode())]
    
    
  def runserver(self, host='localhost', port=8000):
    from wsgiref.simple_server import make_server
    server = make_server(host, port, self.app)
    try:
      server.serve_forever()
    except KeyboardInterrupt:
      server.shutdown()
```  
Once again, it is quite simple: we simply made a simple class called HTTPResponse,  
and added .body after calling the handler.  
After this, we should delete everything from test.py and replace it with:  
```
from __init__ import API, HTTPResponse
api = API()
@api.route('/')
def home(request):
  return HTTPResponse(request)
api.runserver()
```  
Now, before we do this, please read this one chapter: [environ Variables](https://peps.python.org/pep-0333/#environ-variables). Did you read it? Let's move on.  
So, as you recall, there are variables called CONTENT_TYPE, SERVER_PROTOCOL, and CONTENT_LENGTH.  
Let's add these attributes to the HTTPResponse class, and change this app() class so as to  
change the CONTENT_TYPE, SERVER_PROTOCOL, and CONTENT_LENGTH variables.  
```
class HTTPResponse:
  def __init__(self, body, content_type='text/html', HTTP_version='HTTP/1.1', charset='utf-8'):
    self.body = body
    self.content_type = f"{content_type}; charset={charset}"
    self.HTTP_version = HTTP_version
    self.content_length = len(body)


class API:  # or whatever you want
  def __init__(self):
    self.routes = None
    
    
  def route(self, route: str):
    def wrapper(app):
      if self.routes is None:
        self.routes = {route: app}
      else:
        self.routes[route] = app
    return wrapper


  def app(self, request, response):
    response("200 OK", [])
    request['CONTENT_TYPE'] = self.routes[route](request).content_type
    request['CONTENT_LENGTH'] = self.routes[route](request).content_length
    request['SERVER_PROTOCOL'] = self.routes[route](request).HTTP_version
    return [bytes(str(self.routes[request['PATH_INFO']](request).body).encode())]
    
    
  def runserver(self, host='localhost', port=8000):
    from wsgiref.simple_server import make_server
    server = make_server(host, port, self.app)
    try:
      server.serve_forever()
    except KeyboardInterrupt:
      server.shutdown()
```  
This is amazing! We just have the building blocks of a web framework!  
# Conclusion
That was amazing. If you like this blog post, be sure to check out two days after this blog  
post. In the next blog post, I think I am going to do exception and 404 handlers.  
