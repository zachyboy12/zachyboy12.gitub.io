# How to make a web framework with python
# Part 1

In this first part in this series we will be looking at how to make a web framework.  
Before we dive in, however, we must look at how the web works.  
While the web page is loading when you first go here, your browser sends an HTTP Request. Next, the server 
processes that request and returns an HTTP Response, which is the web page  
you are looking at. Ok, so you know how the web works, now let's dive  
in to WSGI.
The Python Web Server Gateway Interface, or WSGI for short, is basically something to mix and match  
web frameworks and web servers.  
You can find more about WSGI [here](https://peps.python.org/pep-0333/).
So, WSGI is all well and good, but how do you actually DO it?
Well, let's code first and then explain.  
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
start_response is a function, to start the response, hence the name. It takes a status param and a headers  
param.  
make_server makes a simple WSGI server that serves on HOST:PORT and only serves one WSGI app, as mentioned.  
