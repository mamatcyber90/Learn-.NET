# Building WPF and ASP.NET Applications with Entity Framework

This tutorial shows how to build a WPF(desktop) application and a ASP.NET(web) application that displays data from a SQL Server database. 

# Starter Steps for Both WPF and ASP.NET Apps

### 1. Make Database with Fake Data

Create a database called 'Venme'.

Make two tables, "User" and "Transaction" with the following script:

```sql
USE Venme;

CREATE TABLE Users (
	id int Primary Key,
	name varchar(255) NOT NULL
);

CREATE TABLE Transactions (
	id int Primary Key,
	[from] int,
	[to] int,
	amount int
);
```

Insert some fake data for "Users":

```sql
USE Venme;
GO

INSERT INTO [Users] VALUES 
	(1, 'Marvin Minsky'), 
	(2, 'John McCarthy'), 
	(3, 'Edsger Dijkstra'), 
	(4, 'Donald Knuth'), 
	(5, 'Allen Newell'), 
	(6, 'Herbert Simon');
```



Some fake data for 'Transactions':

```sql
USE Venme;
GO

INSERT INTO [Transactions] VALUES 
	(1, 1, 2, 35), 
	(2, 1, 3, 24), 
	(3, 2, 4, 7), 
	(4, 3, 5, 56), 
	(5, 4, 1, 89), 
	(6, 6, 5, 45),
	(7, 2, 1, 102), 
	(8, 3, 1, 23), 
	(9, 4, 2, 65), 
	(10, 5, 3, 82), 
	(11, 1, 4, 58), 
	(12, 5, 6, 79);
```




# ASP.NET Web App

### 1. Start a new project

Open Visual Studio 2015 or 2017, click "File" > "New" > "Project." On the pop-up window, select "Web" in the left panel and then select "ASP.NET Web Application" in the middle panel. On the bottom, under "Name," rename the project to "Venme," and hit "OK."

In the new pop-up window, select "MVC" and de-select the Microsoft Azure "Host in the cloud" option if it was checked by default. Click "OK."

### 1. Generate Models from Database with Entity Framework

Assume you completed the starter step "Make Database with Fake Data," now right click on your project > click "Add" > click "New Item."

In the popup menu, select "ADO.NET Entity Data Model," and name it "VenmeContext." In the pop-up window, select "Code First from database," and click "Next." In the following window, click "New Connection." My set up is: "localhost" for "Server name" and "Venme" for "Select or enter a database name." Include all of "Tables". Click "OK" if you receive a warning message saying "this might harm your computer." It won't.

After the automatic code generation, You will see a data diagram without any link between them. We will now add a relationship between the two tables.

### 1. Migrate Data

Much like Git, we use data migration to track and sync our database models. Under "Tools" > "NuGet Package Manager" > "Package Manager Console," type in "Enable-Migrations -ContextTypeName Venme.VenmeContext" like this:

```
PM> Enable-Migrations -ContextTypeName Venme.VenmeContext
```

Add our initial model:

```
PM> Add-Migration InitialModel -IgnoreChanges
```

Apply the (empty) changes:

```
PM> Update-Database
```

It should succeed doing nothing!

### 1. Add Reference Type 

Follow [this article](https://stackoverflow.com/questions/13650257/adding-a-foreign-key-with-code-first-migration)

### 1. (Optional) Add Primary Key

**Important**: declare primary key for `id`. Otherwise the following steps will break.

Insert some fake data using the following:

If you cannot change the table design

### 1. Move model files into the Models folder



### 1. Add Controllers


### 1. Edit Views to Display Data


# WPF App

### 1. Start Project

	Under "file" > "new" > "project," select "WPF Application" and name it venmeWPF. 
	
### 1. Add Models Entity Framework

	In the Solution Explorer, right click the project "VenmeWPF" and select "Add" > New Item. Then under "Data" in the left panel, select "ADO.NET Entity Data Model." Name it "VenmeContext" and select "Code First from Database." Then use "localhost" for database url and "Venme" as database. Then check the box for all of "Tables." Finish.
	
	You must now build your solution. Otherwise your new models will not be included in your project! This means that the data binding step will fail when the Data Source Wizard will not find your VenmoWPF models.
	
	
### 1. Add DataGrid in XAML

	Double-click the MainWindow.xaml in the Solution Explorer. In the "Toolbox" explorer on the left, search for "datagrid." Drag it into the UI. 

### 1. Bind Our Model to DataGrid 

	In the Data Source explorer, drag our VenmeContext into the DataGrid. This will generate some code.
	
### 1. Display Data On Load

	First, we must manually set the Source property to the Transactions.Local property of an instance of VenmeContext. The reason for saying Local is that query-able objects such as our DbSets cannot execute on load. If you don't use Load, but simply venmeContext.Transactions, starting the project will crash on load.
	
	Also, because Entity Framework uses lazy loading by default, we need to specify its loading behavior as `_venmeContext.Transactions.Load();`
