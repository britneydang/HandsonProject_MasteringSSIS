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
- Merge Join Transformation: it combines both sorted two data sources (2 sources have one similar column as primary key which will be used to join) into one using a different type of joins. 2 OLE DB Source component, 2 sort components, a merge join, a flat file destination component -> salesorderID is the primary key -> sort by it in both sources -> Merge Join Editor -> select type of joins, select which columns I want to see in the new table (destination) -> 

![image](https://user-images.githubusercontent.com/110323703/216878467-8d5a04c4-5b51-4daa-862b-fb44e735c141.png)
![image](https://user-images.githubusercontent.com/110323703/216880303-bb1b6bb2-bb82-45e0-937f-a5b71fab70c8.png)

- Lookup Transformation: it joins additional columns to the data flow by looking up values in a table.

![image](https://user-images.githubusercontent.com/110323703/216881964-96d8ab92-d048-4686-98e2-da461f189814.png)
![image](https://user-images.githubusercontent.com/110323703/216882902-d551d18d-b2fa-420c-a5c0-08c32c120225.png)

- Cache Transform: Cache serves as a buffer memory. Once all records are available in the cache and whenever the lookup has a search for the productID, it sarches for that product ID into the buffer memory rather than requesting in SQL Server so it is much faster. I need to create two data flow tasks. One data flow contains a source and cache transform. One data flow will contain a source, a lookup, a destination. In the Cache connection manager editor, make sure that the common field (which is used to looked up table), in this case is ProductID, = 1. In the Lookup Transformation Editor, select Full Cache/Cache connection manager

![image](https://user-images.githubusercontent.com/110323703/217612390-1f995ee5-e7fa-4d2d-be56-dcf55bcc30ed.png)

![image](https://user-images.githubusercontent.com/110323703/217608405-6b31fa2b-2e6d-472f-a46d-f7a880b32238.png)
![image](https://user-images.githubusercontent.com/110323703/217611549-5b71e93c-5fbf-4c5d-849a-43ab2a7d7982.png)
![image](https://user-images.githubusercontent.com/110323703/217612562-52adc2a9-1e33-4037-bc12-68c29aa4e834.png)
- Fuzzy Lookup Transformation: What if the lookup column is not an integer but it's an alphabetical data. An example of Fuzzy: Standard is "Customer Service Representative" and Look up column is "Cust Service Rep." or "Cutsomer Serviec Representative". Human error typo even though they are all the same but normal lookup will not matched. In this case, Fuzzy Lookup will be used. Flat file that contain manual typing is my source, lookup, fuzzy lookup, DB Destination. Lookup will be able to match things when spelling is perfect; when spelling is not perfect, lookup will put the no match output to the Fuzzy Lookup. And then use Union All to add the Lookup and Fuzzy Lookup before sending to the Destination. In the Lookup Editor, select Redirect rows to no match output -> column: connect the common field in the flat file to the Lookup table -> connect Lookup to Union All with Match output and No Match with Fuzzy Lookup. In Fuzzy Editor, need to specify the threshold and the token delimiter (look at the flat file, see that some typo like "." represent a continuity or space) -> Output of Fuzzy Lookup is to Union All

![image](https://user-images.githubusercontent.com/110323703/217622872-0269a9bb-9511-47f6-99de-2306c35037d3.png)
![image](https://user-images.githubusercontent.com/110323703/217623689-a9838029-237a-4ba3-9bc9-74758c923893.png)
![image](https://user-images.githubusercontent.com/110323703/217624830-31725445-8ea4-46a8-8c9e-462f4f345c0b.png)
![image](https://user-images.githubusercontent.com/110323703/217625517-ab8f2f63-0281-499e-9770-bcdc2c6bba1c.png)
![image](https://user-images.githubusercontent.com/110323703/217625950-0c682fc2-c9db-4ae7-b8fe-870a04876cc6.png)
![image](https://user-images.githubusercontent.com/110323703/217636842-9a107c55-83cb-4914-86c7-b80595c817e1.png)







