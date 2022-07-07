# How to make a web framework with python
# Part 2

[Part 1](https://zachyboy12.github.io/zachyboy12.gitub.io/blog-posts/make-a-web-framework-with-python-part-1)  
In this second part of How to Make a Web Framework with Python, we did:  
- Made a WSGI server function
- Made a router

In this second part, we are going to do:  
- Make a function to configure an exception handler
- Make a function to configure a 404 not found handler
- Make an alternative function to route, which is not a decorator
- Make runserver print where the server is serving at

Let's dive in!  
So, let's remember our original code in __init__.py:  
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
      def ignore_favicon(request): # /favicon.ico stands for favorite icon
        return HTTPResponse('')
      if self.routes is None:
        self.routes = {route: app, '/favicon.ico': ignore_favicon}
      else:
        self.routes[route] = app
    return wrapper


  def app(self, request, response):
    response("200 OK", [])
    request['CONTENT_TYPE'] = self.routes[request['PATH_INFO']](request).content_type
    request['CONTENT_LENGTH'] = self.routes[request['PATH_INFO']](request).content_length
    request['SERVER_PROTOCOL'] = self.routes[request['PATH_INFO']](request).HTTP_version
    return [bytes(str(self.routes[request['PATH_INFO']](request).body).encode())]
    
    
  def runserver(self, host='localhost', port=8000):
    from wsgiref.simple_server import make_server
    server = make_server(host, port, self.app)
    try:
      server.serve_forever()
    except KeyboardInterrupt:
      server.shutdown()
```  
Let's remember the file contents of test.py:  
```
from __init__ import API, HTTPResponse
api = API()
@api.route('/')
def home(request):
  return HTTPResponse(request)
api.runserver()
```  
Now, if you run test.py and go to [http://localhost:8000](http://localhost:8000), and  
you try a nonexistant route, it will raise an exception and exit.  
Let's make our framework handle things gracefully.  
Explain first and then do.  
So, we are gonna create two functions: one is the default exception handler, and  
another is for configuring an exception handler.  
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
      def ignore_favicon(request): # /favicon.ico stands for favorite icon
        return HTTPResponse('')
      if self.routes is None:
        self.routes = {route: app, '/favicon.ico': ignore_favicon}
      else:
        self.routes[route] = app
    return wrapper
    
    
  def default_exception_handler(self, request, exception_message):
    return HTTPResponse(f"An exception occured: {str(exception_message)}")
    
    
  def configure_exception_handler(self, new_handler):
    self.default_exception_handler = new_handler


  def app(self, request, response):
    response("200 OK", [])
    request['CONTENT_TYPE'] = self.routes[request['PATH_INFO']](request).content_type
    request['CONTENT_LENGTH'] = self.routes[request['PATH_INFO']](request).content_length
    request['SERVER_PROTOCOL'] = self.routes[request['PATH_INFO']](request).HTTP_version
    return [bytes(str(self.routes[request['PATH_INFO']](request).body).encode())]
    
    
  def runserver(self, host='localhost', port=8000):
    from wsgiref.simple_server import make_server
    server = make_server(host, port, self.app)
    try:
      server.serve_forever()
    except KeyboardInterrupt:
      server.shutdown()
```  
Simple, wasn't it?  
Ok, now let's actually use it:  
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
      def ignore_favicon(request): # /favicon.ico stands for favorite icon
        return HTTPResponse('')
      if self.routes is None:
        self.routes = {route: app, '/favicon.ico': ignore_favicon}
      else:
        self.routes[route] = app
    return wrapper
    
    
  def default_exception_handler(self, request, exception_message):
    return HTTPResponse(f"An exception occured: {exception_message}")
    
    
  def configure_exception_handler(self, new_handler):
    self.default_exception_handler = new_handler


  def app(self, request, response):
    response("200 OK", [])
    try:
      request['CONTENT_TYPE'] = self.routes[request['PATH_INFO']](request).content_type
      request['CONTENT_LENGTH'] = self.routes[request['PATH_INFO']](request).content_length
      request['SERVER_PROTOCOL'] = self.routes[request['PATH_INFO']](request).HTTP_version
      return [bytes(str(self.routes[request['PATH_INFO']](request).body).encode())]
    except Exception as exception:
      request['CONTENT_TYPE'] = self.default_exception_handler(request, exception).content_type
      request['CONTENT_LENGTH'] = self.default_exception_handler(request, exception).content_length
      request['SERVER_PROTOCOL'] = self.default_exception_handler(request, exception).HTTP_version
      return [bytes(str(self.default_exception_handler(request, exception).body).encode())]
    
    
  def runserver(self, host='localhost', port=8000):
    from wsgiref.simple_server import make_server
    server = make_server(host, port, self.app)
    try:
      server.serve_forever()
    except KeyboardInterrupt:
      server.shutdown()
```  
Once again, simple. We just did a try-except statement, caught the exception message,  
and use default_exception_handler to show the exception page.  
Now, here's an exercise for you:  
Add a 404 handler function, a 404 handler configure function, and refractor  
app() to catch a 404 not found error (Hint: except KeyError:).  
You did it? Ok. Here's the solution:  
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
      def ignore_favicon(request): # /favicon.ico stands for favorite icon
        return HTTPResponse('')
      if self.routes is None:
        self.routes = {route: app, '/favicon.ico': ignore_favicon}
      else:
        self.routes[route] = app
    return wrapper
    
    
  def default_exception_handler(self, request, exception_message):
    return HTTPResponse(f"An exception occured: {exception_message}")
    
    
  def configure_exception_handler(self, new_handler):
    self.default_exception_handler = new_handler
    
    
  def default_404_handler(self, request):
    return HTTPResponse(f"URL NOT FOUND: {request['PATH_INFO']}")
    
    
  def configure_404_handler(self, new_handler):
    self.default_404_handler = new_handler


  def app(self, request, response):
    response("200 OK", [])
    try:
      request['CONTENT_TYPE'] = self.routes[request['PATH_INFO']](request).content_type
      request['CONTENT_LENGTH'] = self.routes[request['PATH_INFO']](request).content_length
      request['SERVER_PROTOCOL'] = self.routes[request['PATH_INFO']](request).HTTP_version
      return [bytes(str(self.routes[request['PATH_INFO']](request).body).encode())]
    except KeyError:
      request['CONTENT_TYPE'] = self.default_404_handler(request, exception).content_type
      request['CONTENT_LENGTH'] = self.default_exception_handler(request, exception).content_length
      request['SERVER_PROTOCOL'] = self.default_exception_handler(request, exception).HTTP_version
      return [bytes(str(self.default_exception_handler(request, exception).body).encode())]
    except Exception as exception:
      request['CONTENT_TYPE'] = self.default_404_handler(request).content_type
      request['CONTENT_LENGTH'] = self.default_404_handler(request).content_length
      request['SERVER_PROTOCOL'] = self.default_404_handler(request).HTTP_version
      return [bytes(str(self.default_404_handler(request).body).encode())]
    
    
  def runserver(self, host='localhost', port=8000):
    from wsgiref.simple_server import make_server
    server = make_server(host, port, self.app)
    try:
      server.serve_forever()
    except KeyboardInterrupt:
      server.shutdown()
```  
And there you have it.  
Now, let's make a router function that's __not__ a function:  
```
...
def connect_route(self, route: str):
    def wrapper(app):
      def ignore_favicon(request): # /favicon.ico stands for favorite icon
        return HTTPResponse('')
      if self.routes is None:
        self.routes = {route: app, '/favicon.ico': ignore_favicon}
      else:
        self.routes[route] = app
    return wrapper
...
```  
Hey, isn't this the same code in route()?  
Let's follow the [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) principle and do this:  
```
...
def route(self, route: str):
    def wrapper(app):
      self.connect_route(self, route)
    return wrapper
...
```  
Good job, everyone!  
Finally, let's add something to runserver(), a print message saying "Serving on http://localhost:8000 ...":  
```
...
def runserver(self, host='localhost', port=8000):
    from wsgiref.simple_server import make_server
    print(f"Serving on http://{host}:{port} ...")
    server = make_server(host, port, self.app)
    try:
      server.serve_forever()
    except KeyboardInterrupt:
      server.shutdown()
...
```  
# Conclusion
That was an awesome ride, I love it.  
In my next post, I'll probably go into either making a simple Template engine, or a Database class  
for data storage.  
If you like it, please wait two days for my other blog posts.
