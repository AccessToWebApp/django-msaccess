# Changelog

All notable changes to this project will be documented in this file.


# [0.2.0] - Jan 19, 2026

## Fixed

### "UserModel.objects.all().delete() in Standalone execution" command could not be executed due to an error.
The following program failed to run with an error in "v0.1.1":  
```
import os  
imort django  
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'YourPoject.settings')  
django.setup()  
from YourApplication.models import YourModel  
YourModel.objects.all().delete()  
```
The reason it didn't work was that "class DatabaseWrapper" needed to include "get_connection_params", "get_new_connection", and "_set_autocommit".  
I added those commands.  

### If a column is declared with a default value when creating a table, "python manage.py sqlmigrate YourApp xxxx" command  will fail.

```
Yourfield = models.CharField(db_column='xxxx', max_length=30, db_default='DefaultValue')
```
  
The console will display "ValueError: *** ToDo ***" and the command execution will fail. The cause of the error was that the function "quote_value" in the program was incomplete.  
To address this issue, I reused the function from mssql-django program.


### Creating a table fails when a column has a default value.
When you register a default value for a column, the following SQL statement is generated:  
```
CREATE TABLE [tableName] ([id] COUNTER NOT NULL PRIMARY KEY, [columnName] varchar(30)  DEFAULT ? NOT NULL)
params=('ABC',)
```
This is the format of a parameterized SQL statement.
Microsoft Access SQL statements do not recognize parameter queries in table creation SQL.
The DEFAULT statement can be executed only through the Access OLE DB provider and ADO, not through ODBC.
Remove the 'DEFAULT ?' clause.
Reference: https://learn.microsoft.com/en-us/office/vba/access/concepts/structured-query-language/modify-a-table-s-design-using-access-sql




### Added truncation of constraint names.
(1) "CREATE INDEX" statement.  
(2) "CREATE UNIQUE INDEX" statement.  

### Added On a trial basis, we have enabled connection to Excel files.
See the readme.


  
# [0.1.1] - Jan 06, 2026
  django-msaccess  
  https://github.com/AccessToWebApp/django-msaccess  
  https://pypi.org/project/django-msaccess/  

  Initial Release.


# [0.1.0] - 2012 ?
  django-pyodbc-access  
  https://github.com/18F/django-pyodbc-access/tree/develop/access

  This is the original product before I patched it.

  