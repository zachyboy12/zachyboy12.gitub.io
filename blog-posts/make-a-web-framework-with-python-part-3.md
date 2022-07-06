# Make a web framework with python
# Part 3
[Part 1](https://zachyboy12.github.io/zachyboy12.gitub.io/blog-posts/make-a-web-framework-with-python-part-1)
In [part 2](https://zachyboy12.github.io/zachyboy12.gitub.io/blog-posts/make-a-web-framework-with-python-part-2), we added the following features:  
- Configuring exception handlers
- Configuring 404 handlers
- Made routing function that's not a decorator
- Placed a message in runserver() before serving

This part will have less features and we will do:  
- Raising error when routing an existing route
- Simple Template engine

Right now, our framework lets us route a path multiple times. Of course,  
this is unacceptable. We must raise an error. This is very simple:  
```
...
def connect_route(self, route: str):
    def wrapper(app):
      def ignore_favicon(request): # /favicon.ico stands for favorite icon
        return HTTPResponse('')
      if self.routes is None:
        self.routes = {route: app, '/favicon.ico': ignore_favicon}
      else:
        assert route in self.routes, "Cannot route a path twice."
        self.routes[route] = app
    return wrapper
...
```  
Ok. Now let's make a Template engine. I named mine Templater:  
```
```  
