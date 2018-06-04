#!/usr/local/bin/python3

'''This code is to create a website where the user can enter
a name and remove a name from a list. The list appears 
under the fiels. The list should remain the same when the 
user comes back on the website later.'''

import cgi
form = cgi.FieldStorage()
add = form.getvalue('add_name', '')
remove = form.getvalue('remove_name', '')

'''Create a list with names that were saved in a file 
before or an empty list if the file doesn't exist. The 
list will be edited when the user enter/remove a name.'''

try:
  with open('list_of_name.txt', 'r') as file1:
    list_of_name = [line.strip() for line in file1]
except:
    list_of_name = []

'''If the user enter a name, this name will append to the 
list, if the user removes a name the name will be remove 
from the list.  '''  

if add != '':
  list_of_name.append(add)
for name in list_of_name:
  if name == remove:
     list_of_name.remove(remove)
     break;

'''This method is to list the name in the website under 
the field. Between each name it should have a <br/>(new line)'''

def get_name(list_of_name):
 list = ''
 if len(list_of_name) >1:
   for name in list_of_name[0:-1]:
     list += name + '\n'  + '<br/>' + '\n'
   for name in list_of_name[-1]:
     list += name
 elif len(list_of_name) == 1:
    for name in list_of_name:
      list += name
 return list
names = get_name(list_of_name)

'''The list is written in a file so the list is saved for 
the next time the user enters or removes a name.'''

with open('list_of_name.txt', 'w') as file2:
  for name in list_of_name:
    file2.write(name + '\n')

print('''Content-Type: text/html
<!DOCTYPE html>
<html>
    <body>
        <form action="project5.cgi" method="POST">
            Add a name: <input type="text" name="add_name" />
            Remove a name : <input type="text" name="remove_name" />
            <br/>
            <input type="submit" />
        </form>
        <p>
         <div>
           {0}
         </div>
        <p/>
    </body>
</html>
'''.format(names))
