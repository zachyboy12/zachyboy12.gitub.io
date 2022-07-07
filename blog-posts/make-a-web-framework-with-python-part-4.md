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
What if you want to get the unsaved changes, as a string?  
Let's do just that!  
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
    
  def get_unsaved_changes(self):
    unsaved_changes = ''
    for line in self.__database:
      unsaved_changes += ' '.join(line)  # Joining line items with a space in between them
    return full
```  
Wouldn't it be easier to just call a method to get the current contents of the database,  
instead of having to do the built-in open function all over again?  
We will do this:  
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
    
  def get_unsaved_changes(self):
    unsaved_changes = ''
    for line in self.__database:
      unsaved_changes += ' '.join(line)  # Joining line items with a space in between them
    return full
    
  def get(self):
    return open(self.__file).read()
```  
Oh, two more methods before the last method!  
Let's make these awesome methods to empty the database, and another to empty unsaved changes!  
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
    
  def get_unsaved_changes(self):
    unsaved_changes = ''
    for line in self.__database:
      unsaved_changes += ' '.join(line)  # Joining line items with a space in between them
    return full
    
  def get(self):
    return open(self.__file).read()
    
  def empty_database(self):
    open(self.__file, 'w').write('')  # We changed all file contents to ''
    
  def empty_unsaved_changes(self):
    self.__database = []  # We changed self.__database to an empty list
```  
Now, our last method will be save_changes: To save the unsaved changes to the database.  
Let's discuss and then do.  
So, our plan is to loop through self.__database, and inside the loop we will join it to the  
contents to add.  
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
    
  def get_unsaved_changes(self):
    unsaved_changes = ''
    for line in self.__database:
      unsaved_changes += ' '.join(line)  # Joining line items with a space in between them
    return full
    
  def get(self):
    return open(self.__file).read()
    
  def empty_database(self):
    open(self.__file, 'w').write('')  # We changed all file contents to ''
    
  def empty_unsaved_changes(self):
    self.__database = []  # We changed self.__database to an empty list
    
  def save_changes(self, replace_db_contents=True):
    new_contents = ''
    for line in self.__database:
      new_contents += ' '.join(line) + '\n'
    contents_before = ''
    if replace_db_contents is True:
      pass
    else:
      contents_before = self.get()
    open(self.__file, 'w').write(contents_before + new_contents)
```  
🥳 YAY! We just made a web framework---with no 3rd-party libraries---and it's from scratch! This  
must be a day of celebration!  
# Conclusion
This is the ending part for this series. I hope you liked it!
