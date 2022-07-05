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
Create a folder named your framework. I named mine apiwsgi:  
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
Now, let's code in test.py first and then explain.  
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
start_response is a function, to start the response, hence the name. It takes a status  
parameter and a headers parameter.  
make_server makes a simple WSGI server that serves on HOST:PORT and only serves one WSGI app, as mentioned.  
After this, delete everything in test.py.  
Now, let's convert this to a class, since it is much more convenient that way:  
```
class API:  # or whatever you want
  def app(request, response):
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
"Hello, world!". What if you want to route
