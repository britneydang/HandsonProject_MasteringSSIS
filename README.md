# Hands on Project 3: Mastering SSIS
Project description: This project will be about building several (SQL Server Integration Services) SSIS Packages from scratch. AdventureWorks2019 data will be used.

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

Working with Data Flow Tasks:
- Character Map transformation: it is used to converted data to upper case or lower case. In data flow task, drag Character Map in between Source and Destination components -> Editor: select column that need to be changed -> Destination: add a new column for the change (new column) or replace the old with the change (in-place change)? Select Operation (uppercase, lowercase, etc.), add Alias name for new column -> Ok

![image](https://user-images.githubusercontent.com/110323703/215317189-070d9bdf-fbb7-4a03-abd8-49c9e217f1f3.png)
![image](https://user-images.githubusercontent.com/110323703/215317324-5dcf8ded-21e4-48bf-a0e2-1cfb0354a09e.png)

- Copy Column transformation: copy the whole column from Source and put into Destination. In data flow task, drag Copy Column in between Source and Destination components -> Editor: select column that need to be copied over -> Ok

![image](https://user-images.githubusercontent.com/110323703/215317852-adc1d553-16d9-4d02-ae39-ed71ae0a98ea.png)

- Derived Column transformation: it is like a computed column. For ex: can use it to concatenate firstname, middlename, lastname into fullname. In data flow task, drag Derived Column in between Source and Destination components -> Editor: add concatenate formular in Expression -> Fullname will be passed over to Destination.

![image](https://user-images.githubusercontent.com/110323703/215318303-ee612638-37f3-4f33-a1ac-5094e46a295f.png)

- Percentage Sampling transformation: it picks up randomly records for its sampling. In the Editor, choose percentage of rows. For ex = 20%, sampling selected output and sampling unselected output can be sent to different Destination components. 
- Row Sampling Transformation
- Sort transformation: it is used to sort column(s) in asc or desc order
- Union All transformation: it combines records from multiple data flows without sorting and passes the information to one destination
- Multicast transformation:it passes the information to multiple destination components
- Conditional Split transformation: I want to look at sale order detail and split into poor, average, good, excellent orders base on the line total quantity. If line total < 100 then it is rated poor. The last one in the conditional split will be excellent order. After adding the link from conditonal split to Destination, it will ask to select an input and an output to connect the components

![image](https://user-images.githubusercontent.com/110323703/215386561-e42a3026-c793-449e-b9bc-e36f0deab620.png)
![image](https://user-images.githubusercontent.com/110323703/215386924-da0dcd5a-a8c8-42bd-89d9-2ec0545df561.png)

- Aggregate transformation: I want to group by SalesOrderID and have a count/sum/average of line total. Click on Advanced if I want to have a different/multiple group by. They can be passed into different Destination components.

![image](https://user-images.githubusercontent.com/110323703/215387775-5dd6816d-f05c-4d0b-96ef-70e58a47f4ec.png)
![image](https://user-images.githubusercontent.com/110323703/215388586-0d567ad7-3933-4339-8532-80e4e9f0afd9.png)

- Audit transformation: it saves other information about the environment such as package name, username, machine name, time execution, task name, etc.
- Export Column transformation: it reads data from a data flow and insert data into a file. For this transformation, I need Source, Data Conversion, and Export Column components.  
- Import Column transformation
- Merge transformation
- Merge Join transformation: it combines both sorted two data sources (2 sources have one similar column as primary key which will be used to join) into one using a different type of joins. 2 OLE DB Source component, 2 sort components, a merge join, a flat file destination component -> salesorderID is the primary key -> sort by it in both sources -> Merge Join Editor -> select type of joins, select which columns I want to see in the new table (destination) -> 

![image](https://user-images.githubusercontent.com/110323703/216878467-8d5a04c4-5b51-4daa-862b-fb44e735c141.png)
![image](https://user-images.githubusercontent.com/110323703/216880303-bb1b6bb2-bb82-45e0-937f-a5b71fab70c8.png)

- Lookup transformation: it joins additional columns to the data flow by looking up values in a table.

![image](https://user-images.githubusercontent.com/110323703/216881964-96d8ab92-d048-4686-98e2-da461f189814.png)
![image](https://user-images.githubusercontent.com/110323703/216882902-d551d18d-b2fa-420c-a5c0-08c32c120225.png)

- Cache transformation: Cache serves as a buffer memory. Once all records are available in the cache and whenever the lookup has a search for the productID, it sarches for that product ID into the buffer memory rather than requesting in SQL Server so it is much faster. I need to create two data flow tasks. One data flow contains a source and cache transform. One data flow will contain a source, a lookup, a destination. In the Cache connection manager editor, make sure that the common field (which is used to looked up table), in this case is ProductID, = 1. In the Lookup Transformation Editor, select Full Cache/Cache connection manager

![image](https://user-images.githubusercontent.com/110323703/217612390-1f995ee5-e7fa-4d2d-be56-dcf55bcc30ed.png)
![image](https://user-images.githubusercontent.com/110323703/217608405-6b31fa2b-2e6d-472f-a46d-f7a880b32238.png)
![image](https://user-images.githubusercontent.com/110323703/217611549-5b71e93c-5fbf-4c5d-849a-43ab2a7d7982.png)
![image](https://user-images.githubusercontent.com/110323703/217612562-52adc2a9-1e33-4037-bc12-68c29aa4e834.png)
- Fuzzy Lookup transformation: What if the lookup column is not an integer but it's an alphabetical data. An example of Fuzzy: Standard is "Customer Service Representative" and Look up column is "Cust Service Rep." or "Cutsomer Serviec Representative". Human error typo even though they are all the same but normal lookup will not matched. In this case, Fuzzy Lookup will be used. Flat file that contain manual typing is my source, lookup, fuzzy lookup, DB Destination. Lookup will be able to match things when spelling is perfect; when spelling is not perfect, lookup will put the no match output to the Fuzzy Lookup. And then use Union All to add the Lookup and Fuzzy Lookup before sending to the Destination. In the Lookup Editor, select Redirect rows to no match output -> column: connect the common field in the flat file to the Lookup table -> connect Lookup to Union All with Match output and No Match with Fuzzy Lookup. In Fuzzy Editor, need to specify the threshold and the token delimiter (look at the flat file, see that some typo like "." represent a continuity or space) -> Output of Fuzzy Lookup is to Union All

![image](https://user-images.githubusercontent.com/110323703/217622872-0269a9bb-9511-47f6-99de-2306c35037d3.png)
![image](https://user-images.githubusercontent.com/110323703/217623689-a9838029-237a-4ba3-9bc9-74758c923893.png)
![image](https://user-images.githubusercontent.com/110323703/217624830-31725445-8ea4-46a8-8c9e-462f4f345c0b.png)
![image](https://user-images.githubusercontent.com/110323703/217625517-ab8f2f63-0281-499e-9770-bcdc2c6bba1c.png)
![image](https://user-images.githubusercontent.com/110323703/217625950-0c682fc2-c9db-4ae7-b8fe-870a04876cc6.png)
![image](https://user-images.githubusercontent.com/110323703/217636842-9a107c55-83cb-4914-86c7-b80595c817e1.png)
- Fuzzy Grouping transformation: it is used to remove duplicates. In the flat file (same file used as above), there are variation of typo and one clean version of them. I want fuzzy grouping to walk through all the records and pick up the unique/perfect titles and put the title into a blank table as a destination. In this data flow, it contains a flat file source, a fuzzy grouping, a conditional split, and a DB destination -> Fuzzy Grouping editor, only select one column that has variation of typos. Threshold can be adjust based on the result on SSMS (0.8 threshold will get 12 records but not all are exact match. 0.5 threshold will get 6 records with better match). The reason to use conditional split is only the records where keyin = keyout will be passed on to destination. The blank table on ssms finally has 6 records in it. 

![image](https://user-images.githubusercontent.com/110323703/217663254-7027bbe7-b14d-4a32-bcf8-a8a6e6cb1a40.png)
![image](https://user-images.githubusercontent.com/110323703/217666143-1c82226b-3d52-4014-ba04-bfd6a11d35b5.png)
![image](https://user-images.githubusercontent.com/110323703/217664732-4f6c7ca1-a71a-46ab-aabe-83cd74b0c179.png)
![image](https://user-images.githubusercontent.com/110323703/217664919-ead0c930-8304-48f0-bd28-5aed76e903d4.png)
![image](https://user-images.githubusercontent.com/110323703/217666071-210a7d92-c6b8-4083-a3f8-a0e92841847f.png)
![image](https://user-images.githubusercontent.com/110323703/217666415-d4be63c6-a96e-483d-8a09-3022b53b6a18.png)
- Row Count Transformation with Variable at package level: Row Count counts how many rows are ready to be passed through the pipeline. My data flow contains a OLE DB source, a row count, and a variable at the control flow level -> in control flow tab, R-click blank space, choose Variables -> create new variable name "RowCounter" -> in data flow tab, double click Row Count component and select the newly added variable. 

![image](https://user-images.githubusercontent.com/110323703/217948810-b1c4eb65-cb5a-43b6-9bcf-bd60e50390d0.png)
![image](https://user-images.githubusercontent.com/110323703/217949543-bf150eb8-69ff-4a82-a6f1-6406b8915a28.png)

- OLE DB Command transformation: it uses to manipulate data using SQL commands like Insert, Update, or Delete in a row wise manner. I want to run an insert command using the OLE DB Command to add data from one table into a different blank table. In the OLE DB Command Editor, select the Connection Manager -> Component Properties: SQLCommand: type in the command for the new table (Insert data from OccupationTitle column in Occupation table into NewOccupations table and VALUES(?) mean one parameter). No need to have a Destination component.

![image](https://user-images.githubusercontent.com/110323703/218130748-ff5ba0b0-98b6-43d8-ac71-764dfcc1e389.png)
![image](https://user-images.githubusercontent.com/110323703/218130913-956c1297-aa26-4df8-8b11-bd49ff0bff62.png)
![image](https://user-images.githubusercontent.com/110323703/218133561-062b0d44-180e-42e0-b00b-951a0d367342.png)
![image](https://user-images.githubusercontent.com/110323703/218133893-6dd7122e-0604-446c-868f-78058cd36271.png)

- Pivot transformation: it pivot data on a column value, making it less normalized. For Ex: I want to transform Text file A into B. The data flow contains a Flat text file with unpivot data, a pivot component, a flat file destination -> In the Pivot, select appropriate value for Pivot Key, Set Key, Pivot Value. Type the pivot output column -> click generate column now -> Advanced Editor for Pivot, I can update output column names

![image](https://user-images.githubusercontent.com/110323703/218136439-20c1365b-7bdf-44a5-9b19-764a4165e0c2.png) (Text file A)
![image](https://user-images.githubusercontent.com/110323703/218136582-1e032e70-8d3a-4925-931e-1496f782a1af.png) (Text file B)
![image](https://user-images.githubusercontent.com/110323703/218150576-9d9af888-f87a-43e8-a074-0d2d72b89053.png)
![image](https://user-images.githubusercontent.com/110323703/218151704-e84d9f2d-8642-4d34-87fe-3748391a2f90.png)
![image](https://user-images.githubusercontent.com/110323703/218154441-3f8ce27c-9a4f-49ff-a6fc-f70588a818d2.png)
- Unpivot transformation: it expands an un-normalized data flow into a more normalized version. I want to transform Text file B into A. The data flow contains a Flat text file with pivot data, a unpivot component, a multicast, a flat file destination. In (B) pivot version, there are not as many columns as in (A) -> Unpivot Editor, select individual names (not title) that will be converted into rows, add name for Destination Column (new column that will be created after unpivoting), add Pivot key value column name.

![image](https://user-images.githubusercontent.com/110323703/218136582-1e032e70-8d3a-4925-931e-1496f782a1af.png) (Text file B)
![image](https://user-images.githubusercontent.com/110323703/218136439-20c1365b-7bdf-44a5-9b19-764a4165e0c2.png) (Text file A)
![image](https://user-images.githubusercontent.com/110323703/218157445-939b2aa2-0659-4c7d-95db-efd177a7aaa8.png)
![image](https://user-images.githubusercontent.com/110323703/218158723-05137fc3-3274-458b-917b-8c13efdf01f1.png)
- Term Extraction transformation: it extracts frequent used English-only terms from an input data flow and written to output data columns. Data flow contains a Flat file, a term extraction, a OLE DB destination. Create a blank table to store -> Advanced Flat file editor, make sure the data type between SSMS table matched to data type in Flat file Editor (NVARCHAR = DT_WSTR) and update output colume width if needed (edit Flat File Connection Manager and modify character type ) -> Term Extraction Editor, select the column that I want to find out the duplicate terms. 

![image](https://user-images.githubusercontent.com/110323703/218162501-3c09d8f5-207b-48f8-b233-e96525913139.png)
![image](https://user-images.githubusercontent.com/110323703/218169251-a8361f96-b589-46d0-99d8-88894b92a534.png)

- Term Lookup transformation: it determines how frequently a specific term occur in a data flow/a sentence. Use the same flat file as above for source -> Term Lookup Editor, select the newly created TermExtraction table as Reference table -> mapping avialble input to avaialble destination

![image](https://user-images.githubusercontent.com/110323703/218177137-fe29bd81-e8e0-4d27-9a71-76ab8bc325d0.png)
![image](https://user-images.githubusercontent.com/110323703/218177313-a4bff22c-9d89-4ae8-9f83-3ffef0f47dbf.png)
![image](https://user-images.githubusercontent.com/110323703/218178489-82fb2807-e0a9-4758-b930-d057671e2229.png)

Working with Control Flow Tasks:
The major differences between data flow and control flow is the data flow activities are mostly meant for transformations, which get executed for every row that gets processed; the control flow activities will run one time (unless put them into ForLoop container)

- Script task: it brings the result into the screen like a pop-up table. It is meant for writing VB .NET or C# scripts. In Control Flow tab, it contains a data flow task and a script task ->  Script Task editor, select appropriate Variable -> edit Script... -> a script file will be opened -> edit script: inside the Main () function, MessageBox.Show("My Row Count Result is:" + Dts.Variables["User::RowCounter"].Value.ToString()); -> save script and close.

![image](https://user-images.githubusercontent.com/110323703/217950701-84121a0d-9997-4915-a3fc-4ad033d7aa17.png)
![image](https://user-images.githubusercontent.com/110323703/217953017-1f66ba47-4082-4f00-8df8-bfbcb89e06d4.png)
![image](https://user-images.githubusercontent.com/110323703/217953177-2ce20009-e7a0-4b0c-a45b-87fcd4fbdc24.png)

- Bulk Insert task: it inserts multiple records into the destination in a bulk manner. I want to insert data from an existing csv file (with delimiter ",") into a table on SSMS -> Bulk Insert Editor, in Connection tab, select appropriate info for Connection, DestinationTable, Delimiter, File. In Options tab, Options:
  - Check constraint: if the check constraint fails, then the record insert will fail and everything will fail.
  - Keep nulls: if the source file doesnt have any data, the the nulls will be retained in the destination database.
  - Enable indentity insert: if I want to supply the values for the auto identity fields by myself.
  - Table lock: if I want to keep the table in a locked state while it is processing
  - Fire triggers: if I have written any triggers on this table and while that record is getting inserted into this table, the triggers will also fire.

![image](https://user-images.githubusercontent.com/110323703/218189828-35162041-e982-4cbf-8971-1647d70c0f32.png)
![image](https://user-images.githubusercontent.com/110323703/218194013-f5d85866-b6a0-4f89-8143-73bfbc09f97c.png)
- Sequence Container task: it groups and organizes many tasks/activities in one unit/block in a readable way.
- Execute Process task: it runs any executable file or batch file from SSIS package. I want to execute copy a specific folder from one drive to a different drive -> Create a destination new folder -> In the Execute Process Task Editor, on the Process tab, specify the executable file I want to run in the Executable property. In the Arguments property, specify any command-line arguments that need to be passed to the executable file. In the WorkingDirectory property, specify the working directory for the executable file. All files inside Folder A will be copied into Folder B.

![image](https://user-images.githubusercontent.com/110323703/218207140-c124bb13-eb5a-4ce9-a92d-848832e7c8fb.png)
- Execute Package task: it calls another SSIS package (Child) as a step within a Parent SSIS package. It's useful when I want to break up a complex process into smaller, reusable packages, or when I want to organize packages into a logical grouping of tasks. Here I have 2 packages Child and Parent. Execute Package Task Editor, select reference type: Project Reference and External Reference 
  - Project Reference: both Parent and Child packages are in the same project/ same solution -> select ReferenceType and PackageName -> in Child package, add a Script task to create a message box saying that "I am in the Child package right now" and add a Script task to create a message box saying that "I am back to the Parent package!" in Parent package. Use this to prove that my Execute Package task working well. If I click start the Parent package, the Child package will run first, then go back to the Parent package and run.

![image](https://user-images.githubusercontent.com/110323703/218244344-9214a18f-7c5e-49d3-8f3a-b1b7777506bf.png)
![image](https://user-images.githubusercontent.com/110323703/218244783-a3aeeeb6-3a15-44e5-b211-2911fe682f4b.png)
![image](https://user-images.githubusercontent.com/110323703/218245077-1e68e2b0-70fa-414a-a8c0-bbd71c37da68.png)
  - External Reference: Child package is not in the solution, not in this project and it is saved somewhere else like on desktop/file system or in SQL Server MSDB Database.

![image](https://user-images.githubusercontent.com/110323703/218245141-411ac4b5-b2b2-4e0d-80a4-546a63748c7f.png)
- Passing Parameters between Packages: it passes parameters from one package to another package. I can pass some data from the Parent package to the Child package so that the Child can use that data as an input to do something. Child parameter = 20 in the Child package and Parent parameter = 10 in the Parent package. If I run from the Child package, the Child value is 20. If I run from the Parent package, the Child value is 10. In Parent package, Execute package task -> parameter bindings -> add 

![image](https://user-images.githubusercontent.com/110323703/218245826-d963523d-ae11-4bb4-a143-8a2eb4ab1d3c.png)
![image](https://user-images.githubusercontent.com/110323703/218245837-3f03d221-638e-4d02-b089-bab8dc5884b2.png)
![image](https://user-images.githubusercontent.com/110323703/218245882-d305b3e2-28d7-4de7-87fb-32aff37778c5.png)
![image](https://user-images.githubusercontent.com/110323703/218246241-e9c31137-c5e8-4b29-a5f3-81ef3ae0a7c6.png)
- File System task: it performs operations on files and directories in the file system such as create directory, remove directory, copy files, remove files, etc. In File System Task editor, select the operation, add source connection, and specify destination connection. For Copy file operation, I can do a different way by creating 2 variables.
- Web Service task: (learn later)
- XML task: (learn later) it work with XML data. There are many Operation types:
  - XPATH: it is used to query XML file with the expression
- Data Profiling task: it creates graphical report after analyzing data. In this example, I want to have a report on a count of characters on Firstname column in Person table; a count of NULL on Title column; a count of not NULL on Title column. Data Profiling Editor -> in general tab, destination: create new connection and location -> profile requests tab, select profile type -> under Data, select connection manager, select a specific table -> After running, click Editor and click open Profile Viewer

![image](https://user-images.githubusercontent.com/110323703/218296681-9226bdac-6dce-454c-a8cc-da1eef00a040.png)
![image](https://user-images.githubusercontent.com/110323703/218296920-12baea3a-91c2-4495-b71f-63fa590d94ba.png)
![image](https://user-images.githubusercontent.com/110323703/218297027-4e22a3d5-23b6-414c-a7ad-7073b6613ded.png)
![image](https://user-images.githubusercontent.com/110323703/218297921-318162c5-6072-4963-97b0-e72545a75993.png)
![image](https://user-images.githubusercontent.com/110323703/218297965-2f9dd478-9368-408b-b74f-fb4e51ae4050.png)
![image](https://user-images.githubusercontent.com/110323703/218297987-b65ed4ac-87c7-4042-a141-8880a29604de.png)
- Transfer Database task: it transfers database from one instance of SQL Server to another SQL Server
- Transfer SQL Server Objects task: it transfers SQL Server objects between two instances of SQL Server
- Transfer Master Stored Procedures task: it copies stored procedure in a master database from one instance of SQL Server to another SQL Server
- Transfer Jobs task: it transfer Jobs from one instance of SQL Server to another SQL Server. Before starting, ensure SQL Server Agent Service is enable on both SQL Servers in ssms -> Job, create new job -> Steps tab, add command, space each command with "GO" 

![image](https://user-images.githubusercontent.com/110323703/218580299-0244cc5c-bd76-49ee-8ab4-f5ea5724121b.png)

- Transfer Logins task: In ssms, create new log in -> it transfer Login from one instance of SQL Server to another SQL Server

![image](https://user-images.githubusercontent.com/110323703/218621408-4bb6122b-2efc-46f5-a080-2afa2fe8a13f.png)
- Transfer Error Messages task: it transfers the user defined error messages from one instance of SQL Server to another instance.
- For Loop Container: it repeats tasks in a package until it meets the condition
- For Loop Container in a Fixed Manner: In this example, I add a Script task inside the For Loop Container -> Create a variable -> For Loop Editor, in Properties, InitExpression means variable starting from (usually variable = 0), EvalExpression variable means I will run until variable is reach this number, AssignExpression means how I want to incremeant or decreament the value -> Ok. The script task will print the value of the variable -> Script task editor, select user::VariableName -> Edit stript. This loop is running in a Fixed manner because of AssignExpression set variable = variable + 1

![image](https://user-images.githubusercontent.com/110323703/218625142-2d0fbc13-d619-432d-86de-c3210d8642c2.png)
![image](https://user-images.githubusercontent.com/110323703/218624750-64143797-e3b9-40f7-80bb-59349fc74575.png)
![image](https://user-images.githubusercontent.com/110323703/218634108-f80b374c-b591-4c47-9cef-1504c95b0670.png)
![image](https://user-images.githubusercontent.com/110323703/218634405-361813cc-db3a-44c2-adc2-abad88577296.png)
- For Loop Container in a Variant Manner: For Loop Container -> Create a variable -> For Loop Editor, leave AssignExpression blank because this loop is running in a Variant manner. Add Execute SQL Task inside the container. I have a table that currently has 6 records and EvalExpression variable set less than or equal 10. SSIS package will run continuously until the chosen table gets more than 10 records. While the package is running and user is entering data into the chosen table. The moment the table gets more than 10 records, the loop will end and the package execution will end. In Execute SQL Task Editor, under General tab, Resultset = SingleRow, select appropriate Connection Type, Connection, input SQLStatement. Under Result Set, update Result Name to the name I had given in the script 'RowCount'

![image](https://user-images.githubusercontent.com/110323703/218640772-a394d01e-7212-4643-a854-04f5e9cf034e.png)
![image](https://user-images.githubusercontent.com/110323703/218646899-08c431e8-c882-45f1-8b64-06e68bc96bb4.png)
- ForEach Loop Container: it repeats a set of tasks for each element in a specified enumerator, such as a file, folder, or table. Foreach Loop Editor, under Collection tab, select Enumerator: 
  - For Each File Enumerator: iterating through a list of files. For this example, I have 3 text files and a table in SSMS. I want to create a SSIS package that is able to look into the folder, find as many text files as possible, and export records of each text file into a designation table currently empty) in SQL Server. Under Variable Mapping, create a variable to hold the value of the file. Add a Data Flow Task inside the container -> Drag in Flat file as Source and OLE DB destination -> In flat file source editor, connect to one of the files inside folder -> In OLE DB destination editor, connect to the chosen table -> go to Properties of Flat file connection manager, in Expressions -> ... -> select Connection string and drag [@User::SourceFileName] into the Expression -> erase the previous Connectionstring in the Properties. Make sure the data type is matched between source and destination. 

![image](https://user-images.githubusercontent.com/110323703/218807282-f9c90bea-ea82-4914-a836-66b602071da7.png)

![image](https://user-images.githubusercontent.com/110323703/218808597-eb873be5-9feb-4061-88e5-4dc9d373f08f.png)
![image](https://user-images.githubusercontent.com/110323703/218808823-a6bb7f30-83c2-4959-994f-9c8522ae97e2.png)
![image](https://user-images.githubusercontent.com/110323703/218872389-954c673b-7005-4ea4-b9cd-cdb1f831c8dd.png)
![image](https://user-images.githubusercontent.com/110323703/218875717-4fee9bd9-3799-4eef-80ad-319bb4277590.png)
![image](https://user-images.githubusercontent.com/110323703/218876641-cc02aa63-48fe-424b-8e79-2ad406cf5d81.png)
  - For Each Item Enumerator: 
  - For Each From Variable Enumerator:
  - For Each Node List Enumerator:
  - For Each SMO Enumerator:
  - For Each ADO Enumerator:

Working with Configuration Types:
- Configuration Files:
- Configuration Tables:

Creating Dynamic Packages:

Event Handling Techniques:

Working with Log Providers:
- Log Provider for Text Files:
- Log Provider for XML Files:
- Log Provider for Windows Event Log:

Working with WMI:
- WMI Data Reader task:
- WMI Event Watcher task:

Using Message Queing techniques:

Working with Maintenance Planning:
- Backup Database task:
- SQL Server Agent Job task:
- Execute T-SQL task:
- Update Statistics task:
- History Cleanup task:
- Shrink Database task:
- Rebuild Indexes task:
- Reorganize Index task:
- Check Database Integrity task:

Working with CheckPoints:

Working with different sources and Destinations:
- XML Source and RawFile Destination:
- RawFile Source and SQLServer Destination:
- ADO.NET Source and ADO.NET Destination:
- RecordSet Destination:
- DataReader Destination: 

Incremental Data Loading Techniques:

Working with CDC Components:

Deployment:






