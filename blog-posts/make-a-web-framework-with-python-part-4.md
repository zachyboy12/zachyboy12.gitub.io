# Make a web framework with python
# Part 4
[Part 1](https://zachyboy12.github.io/zachyboy12.gitub.io/blog-posts/make-a-web-framework-with-python-part-1)  
[Part 2](https://zachyboy12.github.io/zachyboy12.gitub.io/blog-posts/make-a-web-framework-with-python-part-2)  
Part 3's features were:
- Raising error when routing existing route
- Making simple Template engine

In part 4, we will add one feature:  
- Making our own database manager for our framework

Now, let's create a class called Database:  
```
class Database:
  def __init__(self, database_file):
        self.__database = []
        current_line = []
        for line in open(database_file).read().split('\n'):
            for column in line.split(' '):
                current_line.append(column)
            self.__database.append(current_line)
            current_line = []
    self.__file = database_file
```  
Simple, right? We just created a variable called database, which will be representing our  
database. We then created a local variable called current_line which will be handy soon.  
Now, there is a for loop that says "for line in open(database_file).read().split('\n')", which is all  
self-explainatory. Also, there is a nested for loop which loops through the line split by a  
space, and inside this you append the column to the current line. After looping through all the columns,  
We append the current line to self.__database, and finally we reset current_line to an empty list.  
Whew! Now that we got this, let's make a get_row and a get_column method to get the rows in the  
database and to get the columns in a row in the database:  
```
class Database:
  def __init__(self, database_file):
        self.__database = []
        current_line = []
        for line in open(database_file).read().split('\n'):
            for column in line.split(' '):
                current_line.append(column)
            self.__database.append(current_line)
            current_line = []
    self.__file = database_file
    
  def get_row(self, row):
    return self.__database[row - 1]  # Why did I subtract it by one? To support indexing!
    
  def get_column(self, row, column):
    return self.get_row(row)[column - 1]  # Following the DRY principle here
```  
Great job!  
For an exercise, make some methods to add rows and columns.  
Done? Here's the solution:  
```
class Database:
  def __init__(self, database_file):
        self.__database = []
        current_line = []
        for line in open(database_file).read().split('\n'):
            for column in line.split(' '):
                current_line.append(column)
            self.__database.append(current_line)
            current_line = []
    self.__file = database_file
    
  def get_row(self, row):
    return self.__database[row - 1]  # Why did I subtract it by one? To support indexing!
    
  def get_column(self, row, column):
    return self.get_row(row)[column - 1]  # Following the DRY principle here
    
  def add_row(self, row, row_contents):
    self.__database.insert(row - 1, row_contents)
    
  def add_column(self, row, column, column_value):
    self.__database[row - 1].insert(column - 1, column_value)
```  
Ok, let's make some handy methods to remove either a row or a column:  
```
class Database:
  def __init__(self, database_file):
        self.__database = []
        current_line = []
        for line in open(database_file).read().split('\n'):
            for column in line.split(' '):
                current_line.append(column)
            self.__database.append(current_line)
            current_line = []
    self.__file = database_file
    
  def get_row(self, row):
    return self.__database[row - 1]  # Why did I subtract it by one? To support indexing!
    
  def get_column(self, row, column):
    return self.get_row(row)[column - 1]  # Following the DRY principle here
    
  def add_row(self, row, row_contents):
    self.__database.insert(row - 1, row_contents)
    
  def add_column(self, row, column, column_value):
    self.__database[row - 1].insert(column - 1, column_value)
    
  def remove_row(self, row):
    del self.__database[row - 1]
    
  def remove_column(self, row, column):
    del self.__database[row - 1][column - 1]
```  
After we do that, let's discuss.  
What if you want to get the unsaved changes?  
Let's do just that!
