# Sqltxt
## __init__.py (The main file)

#### *def createfile(name: str)*

Creates a file.  

name - The name of the file.  

#### *def editfile(thefile: str, new_text: str)*

Edits a file.  

thefile - The file to edit.  
new_text - The new text of the file.  

#### *def resetall(thefile: str)*

Resets the file.  

thefile - The file to reset.  

#### *def getdata(thefile: str, take_off_list_format=False)*

Gets the data of the file.  

thefile - The file to get the data.  
take_off_list_format - If set to False, no square brackets will appear.  

#### *def appendtext(thefile: str, appending_text: str)*

Appends some text to the file contents.  

thefile - The file to append the data to.  
appending_text - The text to append.  

#### *def removefile(thefile: str)*

Removes the file.  

thefile - The file to remove.  

#### *def renamefile(old_name: str, new_name: str)*

Renames the file.  

old_name - The old name of the file.  
new_name - The new name of the file.  

Note: did not document savetime() and version().
