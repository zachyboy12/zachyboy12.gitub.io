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
        assert route in self.routes, "Cannot route a path twice."  # if the given statement (route in self.routes) isn't true, raise an AssertionError, with a message saying "Cannot route a path twice."
        self.routes[route] = app
    return wrapper
...
```  
Ok. Now let's make a Template engine. I named mine Templater:  
```
...
class Templater:
    def __init__(self, filepath):
        self.__file = filepath  # Making variable private
...
```  
Cool, right? Now let's add some methods.  
The first method we are gonna do is to render values to the file, and to do this we are gonna use a dictionary:  
```
class Templater:
    def __init__(self, filepath):
        self.__file = filepath  # Making variable private (two underscores at the beginning)
        
    def render_values(self, names_and_values):  # names_and_values is the context of the template
        filecontents = open(self.__file).read()
        new_contents = ''
        for key in names_and_values:
            new_contents = filecontents.replace('[% ' + key + ' %]', names_and_values.get(key))
        open(self.__file, 'w').write(new_contents)
```  
Simple, wasn't it? We simply iterated over names_and_values, made new_contents equal to the  
original file contents, and replaced the beginning tag plus the key plus the end tag with the value of  
the key in the dictionary. Finally, we opened the file and wrote new_contents to the file.  
Ok, so now we got this method, now let's add a for loop method:  
```
class Templater:
    def __init__(self, filepath):
        self.__file = filepath  # Making variable private (two underscores at the beginning)
        
    def render_values(self, names_and_values):  # names_and_values is the context of the template
        filecontents = open(self.__file).read()
        new_contents = ''
        for key in names_and_values:
            new_contents = filecontents.replace('[% ' + key + ' %]', names_and_values.get(key))
        open(self.__file, 'w').write(new_contents)
        
    def for_loop(self, beginning_line_forloop, ending_line_forloop, line_insert, times):
        list_of_forloop_lines = open(self.__file).read().split('\n')[beginning_line_forloop - 1:ending_line_forloop]  # Opens the file, reads it, splits the result by a new line, and gets the items in the list from beginning_line_forloop - 1 (for indexing) to ending_line_forloop (why didn't I put a - 1 on it? Because the ending index is automatically "subtracted"!)
        forlooplines = ''  # The lines to repeat, which is a string
        for forloop_line in list_of_forloop_lines:
            forlooplines += forloopline + '\n'
        line_to_insert = line_insert - 1  # - 1 for indexing
        forlist = [filecontent.split(' ') for filecontent in open(filepath).read().split('\n')]  # Makes a for list so that a line is represented by another list and a column is represented by an item in an inside list
        for i in range(times):
            forlist.insert(line_to_insert, [forlooplines + '\n'])  # Inserts the string version of the for loop lines
            line_to_insert += 1  # Adding the line to insert by 1 to add more lines
        new_contents = ''
        # In the bottom for loop we are making new_contents equal to the string version of forlist
        for line in forlist:
            for column in line:
                new_contents += column + ' '
            new_contents += '\n'  # After finishing the columns, add a newline to seperate the other lines
        open(self.__file, 'w').write(''.join(new_contents))
```  
Whew! That was a tough ride!  
I have an exercise for you.  
Make an if_statement method for if statements in the Templater (or whatever you called it) class.  
Done? Cool. You just made your first simple Template engine!  
# Conclusion
That was short but I loved it. If you liked this blog post, be sure to stay tuned, because  
in my final part in this series, we are going to build a Database class to store and get data.  
