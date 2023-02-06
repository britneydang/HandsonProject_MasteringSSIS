# Hands on Project 3: Mastering SSIS
Project description: This project will build (SQL Server Integration Services) SSIS Packages from scratch. I will use AdventureWorks2019 data for this project.

Required installation:
- SQL Server Management Studio
- Visual Studio 2022 (add-on Data Storage and Processing)

Step by step:
Initial set-up:
- Set up a database follow instruction how to restore .bak file to SQL SERVER (https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms) 

Data Migration Techniques:
- SQL Server Import and Export Wizard: R-click database -> Task -> Import Data -> Wizard pop-up -> SQL Server Native Client -> Add Server name, Authentication, Database -> select Flat File Destination and choose location to save file -> select option Copy data from one or more tables or views -> choose Source file that I want to import -> select options: run immediately or save package -> choose location to save package -> Finish
- Export data from SQL Server to Excel File using SQL Server Import and Export Wizard: R-click database -> Task -> Export Data -> Wizard pop-up -> SQL Server Native Client -> Add Server name, Authentication, Database -> select MS Destination and choose location to save file -> select option Copy data from one or more tables or views -> choose Source file that I want to export -> select options: run immediately or save package -> choose location to save package -> Finish
- in Visual Studio -> create new project -> Integration Service Project -> create -> R-clich SSIS Package -> SSIS Import and Export Wizard -> steps are similar as above. Instead of select option Copy data from one or more tables or views, I can write my own query -> Finish -> In VS, click Start/Stop -> new file will be created in the chosen destination folder.

![image](https://user-images.githubusercontent.com/110323703/215315853-4c19f3b1-27a6-49e6-9778-1e2bc96c7a32.png)

Data Flow Tasks - Transformations:
- Character Map Transformation: it is used to converted data to upper case or lower case. In data flow task, drag Character Map in between Source and Destination components -> Editor: select column that need to be changed -> Destination: add a new column for the change (new column) or replace the old with the change (in-place change)? Select Operation (uppercase, lowercase, etc.), add Alias name for new column -> Ok

![image](https://user-images.githubusercontent.com/110323703/215317189-070d9bdf-fbb7-4a03-abd8-49c9e217f1f3.png)
![image](https://user-images.githubusercontent.com/110323703/215317324-5dcf8ded-21e4-48bf-a0e2-1cfb0354a09e.png)

- Copy Column Transformation: copy the whole column from Source and put into Destination. In data flow task, drag Copy Column in between Source and Destination components -> Editor: select column that need to be copied over -> Ok

![image](https://user-images.githubusercontent.com/110323703/215317852-adc1d553-16d9-4d02-ae39-ed71ae0a98ea.png)

- Derived Column Transformation: it is like a computed column. For ex: can use it to concatenate firstname, middlename, lastname into fullname. In data flow task, drag Derived Column in between Source and Destination components -> Editor: add concatenate formular in Expression -> Fullname will be passed over to Destination.

![image](https://user-images.githubusercontent.com/110323703/215318303-ee612638-37f3-4f33-a1ac-5094e46a295f.png)

- Percentage Sampling Transformation: it picks up randomly records for its sampling. In the Editor, choose percentage of rows. For ex = 20%, sampling selected output and sampling unselected output can be sent to different Destination components. 
- Row Sampling Transformation
- Sort Transformation
- Union All Transformation: it combines records from multiple data flows without sorting and passes the information to one destination
- Multicast Transformation:it passes the information to multiple destination components
- Conditional Split Transformation: I want to look at sale order detail and split into poor, average, good, excellent orders base on the line total quantity. If line total < 100 then it is rated poor. The last one in the conditional split will be excellent order. After adding the link from conditonal split to Destination, it will ask to select an input and an output to connect the components

![image](https://user-images.githubusercontent.com/110323703/215386561-e42a3026-c793-449e-b9bc-e36f0deab620.png)
![image](https://user-images.githubusercontent.com/110323703/215386924-da0dcd5a-a8c8-42bd-89d9-2ec0545df561.png)

- Aggregate Transformation: I want to group by SalesOrderID and have a count/sum/average of line total. Click on Advanced if I want to have a different/multiple group by. They can be passed into different Destination components.

![image](https://user-images.githubusercontent.com/110323703/215387775-5dd6816d-f05c-4d0b-96ef-70e58a47f4ec.png)
![image](https://user-images.githubusercontent.com/110323703/215388586-0d567ad7-3933-4339-8532-80e4e9f0afd9.png)

- Audit Transformation: it saves other information about the environment such as package name, username, machine name, time execution, task name, etc.
- Export Column Transformation: it reads data from a data flow and insert data into a file. For this transformation, I need Source, Data Conversion, and Export Column components.  
- Import Column Transformation
- Merge Transformation
- Merge Join Transformation: 




