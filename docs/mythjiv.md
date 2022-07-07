# Mythjiv

## __init__.py (The main file)  

### *class HTTPResponse(self, body, content_type='text/html', HTTP_version='HTTP/1.1', charset='utf-8')*

Creates an HTTP response.  

body - The body of the HTTP response.  
content_type - The file type of the HTTP response body.  
HTTP_version - The version of HTTP.  
charset - The character set of the file.  

### *class Myth():*  

#### *def connect_route(self, route: str, app: function)*  

Adds a route, with the handler as the app.  

route - The path in which to call the handler.  
app - The handler.  

#### *def route(self, route: str)*

The decorator version of connect_route.  

route - The route to the app.  

#### *def default_exception_handler(self, request: dict, exception='?')*

The default exception handler, if there's an internal server error.  

request - The request information.
exception - The exception message.  

#### *def default_404_handler(self, request: dict)*

The default 404 not found handler.  

request - The request information.  

#### *def configure_exception_handler(self, new_handler: function)*

Changes the default exception handler to the new handler.  

new_handler - The new handler to handle internal server errors.  

#### *def configure_404_handler(self, new_handler: function)*

Changes the default 404 handler to the new handler.  

new_handler - The new handler to handle 404 errors.  

#### *def wsgi_app(self, request: dict, response: function)*

The main WSGI app.  

request - The request information.  
response - Starts the response.  

#### *def runserver(self, host='localhost', port=8000, poll_seconds=0)*

Runs a simple WSGI server, which is __not__ for production.  

host - The host of the server.  
port - The port of the server.  
poll_seconds - Amount of time the server rests???  

### *class DISHED(self, database_file: str)*

Creates a DISHED object for storing and getting data from a database.

database_file - The database file path.  

#### *def get_layer(self, layer_number: int)*

Gets a line in the database file.  

layer_number - The line number.  

#### *def get_item(self, row: int, column: int)*

Gets a column in the database file.  

row - The line number.  
column - The column number.  

#### *def add_layer(self, layer_number: int, layer: list)*

Adds a line in the database file.  

layer_number - The line number.  
layer - The line contents as a list.  

#### *def add_item(self, layer_number: int, column: int, item: Any)*

Adds an item to an existing line.  

layer_number - The line number.  
column - The column number.  
item - The column value to add.  

#### *def remove_layer(self, layer_number: int)*

Removes a line.  

layer_number - The line number.  

#### *def remove_item(self, layer_number: int, column: int)*

Removes an item in a line.  

layer_number - The line number.  
column - The column number.  

#### *def get(self)*

Gets the unsaved changes.  

#### *def empty_DISHED_object(self)*

Empties the unsaved changes and the database.  

#### *def get_database(self)*

Gets the database contents.  

#### *def save_changes(self, replace_db_contents=True)*

Saves the changes.  

replace_db_contents - A parameter asking if the unsaved changes is to be added to the database contents or to replace the database contents.  

### *def render_values(filepath: str, names_and_values: dict)*

Renders the values to the given file path.  

filepath - The path to the template.  
names_and_values - The names of the values to be replaced and the values to replace the names.  

### *def for_loop(filepath: str, beginning_line_to_forloop: int, ending_line_to_forloop: int, line_to_insert: int, times: int)*

Does a for loop.  

filepath - The path to the template.  
beginning_line_to_forloop - The line number to begin for for looping.
ending_line_to_forloop - The ending line number to begin for for looping.  
line_to_insert - The beginning line number to insert results of for loop.  
times - How many times it is supposedly for looped.  
