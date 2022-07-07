# Make a web framework with python
# Part 4
[Part 1](https://zachyboy12.github.io/zachyboy12.gitub.io/blog-posts/make-a-web-framework-with-python-part-1)
[Part 2](https://zachyboy12.github.io/zachyboy12.gitub.io/blog-posts/make-a-web-framework-with-python-part-2)
Part 3's features were:
- Raising error when routing existing route
In part 4, we will add one feature:  
- Making our own database manager for our framework
Now, let's create a class called Database:  
```
class Database:
  def __init__(self, database_file):
    self.__file = database_file
```
