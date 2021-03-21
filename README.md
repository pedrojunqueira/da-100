
# DA-100 Notes

This are my notes as I cover the DA-100 exam curriculum

## Prepare the Data (20-25%)

### Get data from different data sources 

Azure Doc: [Get Data](https://docs.microsoft.com/en-gb/learn/modules/get-data/)

- Documents are flat files with no Hierarchy all data is in a single file with the same structure (.csv, .txt, xlsx)
- Flat files (documents) can be imported from	
	- Local
	- OnDrive for business. If any changes PBI automatically updates
	- OnDrive personal. To update regularly need to opt to "Keep me signed". Also check if it allowed by the organization Sys Admin
	- SharePoint - Team Sites. Works  similar to OneDrive business but connects to the URL or Root Folder

- Importing data options
	- Load the data: straight into PBI desktop
	- Transform the data. Take you to Power Query
	
- Connecting to database offer options
	- Windows credentials
	- Database credentials
	- Microsoft Account
- To use a query statement to import data use the advanced options

- To change data source settings can be done from the PBI Menu (Home>Query>Transform Data> Data Source Settings
		or from within the PowerQuery (Data Source >Data Source Settings)

- Importing folder and combining file (binary)
	-  If you have multiple files that have the same schema, combine them into a single logical table
	[Azure Docs](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-combine-binaries) 

- Using SQL statements. Do not use the * (wildcard) as you may be importing redundant data
- Only import the data that you need for the report
- Better than creating a SQL statement is to create a view in the DB and let Power query do the query folding

- Connecting to a CosmoDB (Azure)
- Select connection (Others)
- If you are connecting to an endpoint for the first time, as you are in this example, make sure that you enter your account key.

- Importing Json
- When importing Json the file needs to be formatted in Power Query to a tabular format before go to PBI desktop
- A Record in a document database need to use the "Expander" then data will be in a tabular format ready to be imported for modelling

- Selecting a storage mode. 
[Azure Docs](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-storage-mode)

- Import : Data loaded and resides in the PBI file (.pbix). Data refresh as manually done.
- DirectQuery: allows you to query the data in the data source directly and not import a copy into Power BI. 
  data gets refreshed as you interact with visualizations 
  -	When in direct query mode there is no support for parent child relationship in DAX
  - Other DirectQuery [limitations](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-directquery-about#modeling-limitations)
	- No Calculated tables
	- No scalar Dax functions such as `left()` for calculated columns
	- Max length of text in columns 32,764
	- No Built in date hierarchy
Always get the latest data
- Dual: 

- Mode Change: Is possible to change import to Direct Query but not from Direct Query to Import. 
- If a import mode is changed to Query mode this is irreversible

Analysis Services vs. SQL Server

Similarities

- Authenticate to the server.
- Pick the cube you want to use.
- Select which tables you need.

Differences

- Analysis Services cubes have calculations already in the cube, which will be discussed in more detail later.
- If you don’t need an entire table, you can query the data directly. 
- Instead of using Transact-SQL (T-SQL) to query the data,
 like you would in SQL Server, you can use multi-dimensional expressions (MDX) 
 or data analysis expressions (DAX).

- Azure Analysis Services can be connected. 
[Azure Docs](https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-connect-pbi)
	- Import or
	- Connect live : Using the Connect live option helps you keep the data and DAX calculations in their original location, 
				without having to import them all into Power BI. Azure Analysis Services can have a fast refresh schedule , 
				which means that when data is refreshed in the service, Power BI reports will immediately be updated, 
				without the need to initiate a Power BI refresh schedule.
- Analysis Services Multidimensional can only connect life. It does not import. [Reference Docs](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-ssas-multidimensional)


- Query folding. 
	- [Query Folding Docs](https://docs.microsoft.com/en-us/power-bi/guidance/power-query-folding)
	- [Power Query Folding Docs](https://docs.microsoft.com/en-us/power-query/power-query-folding)
	- Query folding is the process by which the transformations and edits that you make 
		in Power Query Editor are simultaneously tracked as native queries, 
		or simple Select SQL statements.
	- Whe transformation are done in power query it gets translated in a native query (SQL)
	- It is possible to many transformations like deleting column or filtering,  GROUP BY a column for example but does not work for
		- Adding an index column
		- Merging and appending columns of different tables with two different sources
		- Changing the data type of a column
		- Running complex DAX functions
		
- Optimization tips
	- Process as much data as possible in the original data source
	- Use native SQL queries.  SP or CTEs
	- Separate date and time, if bound together
	

- [Power Query ](https://docs.microsoft.com/en-gb/learn/modules/clean-data-power-bi/2-shape-data)
	- All steps are recorded 
	- Promote headers can use the promote header in the Home Ribbon session or click the table icon next to first column (use first row as header)
	- Can remove top N rows
	- Can delete columns by selecting the ones you need or you can select the ones you want to keep then delete the others
	- You can unpivot columns by selecting the columns to unpivot and the column names will be attributes and the numeric value will be value
	- Pivot the columns is the opposite of unpivot and used to summarize and aggregate data
	- Visualize query dependency
		- Tab "View" -> Query dependency

- Improving data
	- Rename columns to more meaningful names
	- Replace values to meaningful and fix data issues, spelling or errors. Replace nulls with 0 (zeros)
	- Remove duplicates

- Columns name conventions
	- Replace underscore with space
	- Remove prefixes and suffixes
	
- Evaluate and change data types
	- Can be changed in power query and in power bi desktop reporting view by using columns tools
	- Data types in [PBI]https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-data-types
	
- Combine multiple tables
	- Merging: Need to have a columns to join
		- Can use left, inner and full outer
	- Appending: Need to have same number of columns and types

- [Use or Create a PBIDS file](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-data-sources#how-to-create-a-pbids-connection-file)
	- PBIDS files are files that you can provide to another user with already the connections setting configured.
	- This will save time for the user to having to go through the process of Get Data> Select Source > Enter source connections details and etc
	- Once you open the file it takes you straight to the source tables for instance if the connection is a database.
	- To create the PBIDS file:
		- Select File > Options and settings > Data source settings
		- Select Export PBIDS.
		- Save file (Rename if you want but it will give you a default name based on your connection name)
	- The saved file is a text file is a JSON like structure that you actually can edit or create manually if you know the structure
	- Below an example of connecting to SQL server.
	```
	{
	"version": "0.1",
	"connections": [
		{
		"details": {
			"protocol": "tds",
			"address": {
			"server": "Server_Name",
			"database": "AdventureWorks2019"
			},
			"authentication": null,
			"query": null
		},
		"options": {},
		"mode": null
		}
	]
	}
	```
	- If mode is missing/null in the file, the user who opens the file in Power BI Desktop is prompted to select DirectQuery or Import.
	- [Examples](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-data-sources#pbids-file-examples)

- Use or create a data flow
	- [Documentation Create](https://docs.microsoft.com/en-us/power-bi/transform-model/dataflows/dataflows-create)
	- [Documentation Create and use](https://docs.microsoft.com/en-us/power-query/dataflows/create-use)
	- A Dataflow in other words is Power Query running in the cloud(PowerBI Service)
	- A dataflow is a collection of tables that are created and managed in workspaces in the Power BI service. 
	- Create a DataFlow
		- To create a dataflow, launch the Power BI service in a browser then select a workspace (dataflows are not available in my-workspace in the Power BI service)
		- There are a multiple of ways to create or build on top of a new dataflow:
			- Create a dataflow using define new tables: Connecting to a new source and "creating a table(query) in data flow. E.g. connecting to a SQL database and importing some data into dataflow
			- Create a dataflow using linked tables: Enables you to reference an existing table, defined in another dataflow, in a read-only fashion.
				- Linked tables are available only with Power BI Premium.
			- Create a dataflow using a computed table: Creating a dataflow using a computed table allows you to reference a linked table and perform operations on top of it in a write-only fashion. 
				- The result is a new table, which is part of the dataflow. To convert a linked table into a computed table, you can either create a new query from a merge operation, or if you want to edit or transform the table, create a reference or duplicate of the table.
			- Create a dataflow using import/export: Creating a dataflow using import/export lets you import a dataflow from a file. This is useful if you want to save a dataflow copy offline, or move a dataflow from one workspace to another.
		- Configuring a Dataflow
			- You can configure Dataflow schedule refresh and define how often you want it refreshed
			- In the service find the dataflow and select the More menu (the ellipsis) and select Settings.
			- In configuration you can set
				- Gateway Connection: What gateway dataflow connects to
				- Data Source Credentials: Data source credential dataflow connects to
				- Sensitivity Label: Define the sensitivity of the data
				- Take ownership: If you are not the owner will be prompted to it
				- Scheduled Refresh
				- Enhanced Compute Engine settings: Here you can define whether the dataflow is stored inside the compute engine.
					- The compute engine allows subsequent dataflows, which reference this dataflow, to perform merges and joins and other transformations much faster than you would otherwise.
					- It also allows DirectQuery to be performed over the dataflow
		- Consuming a dataflow
			- A dataflow can be consumed in the following three ways:
				- Create a linked table from the dataflow to allow another dataflow author to use the data
				- Create a dataset from the dataflow to allow a user to utilize the data to create reports
				- Create a connection from external tools that can read from the CDM (Common Data Model) format
		- Dataflow [best practices](https://docs.microsoft.com/en-us/power-bi/transform-model/dataflows/dataflows-best-practices)
		
- Connect Microsoft Dataverse Power BI
	- [Documentation](https://docs.microsoft.com/en-us/powerapps/maker/data-platform/data-platform-powerbi-connector#connect-to-dataverse-using-the-connector)
	- Common Data Service has been renamed to Microsoft Dataverse.
	- Microsoft Dataverse allows you to connect directly to your data using Power BI Desktop to create reports and publish them to Power BI.
	- Prerequisites
		- Dataverse environment with maker permissions to access the portal and read permissions to access data within tables.
		- You must have the appropriate Power BI license
		- To use the Dataverse connector, the Enable TDS endpoint setting must be enabled in your environment
		- Find your Dataverse environment URL
			- Open Power Apps, select the environment you're going to connect to, select Settings in the top-right corner, and then select Session details.
			- The URL will be in the format: https://yourenvironmentid.crm.dynamics.com/. Make sure you remove https:// and the trailing / from the URL before pasting it to connect to your environment.
	- In Power BI to connect to Dataverse just select the dataverse connector and enter the environment URL as described above
	- Select the tables data you want to import and import

- Connect to a dataset using the XMLA endpoint
	- [Documentation](https://docs.microsoft.com/en-us/power-bi/admin/service-premium-connect-tools)
	- This feature is only available with PBI Premium, Premium per user and Embeded
	- What's an XMLA endpoint
		- Power BI Premium uses the XML for Analysis (XMLA) protocol for communications between client applications and the engine that manages your Power BI workspaces and datasets.
		-  XMLA is the same communication protocol used by the Microsoft Analysis Services engine
		- By default, **read-only** connectivity using the endpoint is enabled for the Datasets workload in a capacity
		- With read-only, data visualization applications and tools can query dataset model data, metadata, events, and schema.
			- Read-write operations using the endpoint can be enabled providing additional dataset management, governance, advanced semantic modeling, debugging, and monitoring
			- With read-write enabled, Power BI Premium datasets have more parity with Azure Analysis Services and SQL Server Analysis Services enterprise grade tabular modeling tools and processes.
	- These are some client tools that can connect to a XMLA end point
		- Visual Studio with Analysis Services projects - SSDT
		- SQL Server Management Studio (SSMS) - Supports DAX, MDX, and XMLA queries.
		- SQL Server Profiler – Installed with SSMS
		- Analysis Services Deployment Wizard – Installed with SSMS, 
		- PowerShell cmdlets 
		- Power BI Report Builder
		- Tabular Editor - An open-source tool for creating, maintaining, and managing tabular models using an intuitive, lightweight editor.**XMLA read-write is required** for metadata operations
		- DAX Studio – An open-source tool for DAX authoring, diagnosis, performance tuning, and analysis. Features include object browsing, integrated tracing, query execution breakdowns with detailed statistics, DAX syntax highlighting and formatting. **XMLA read-only is required**
		- ALM Toolkit: An open-source schema compare tool for Power BI datasets, most often used for application lifecycle management (ALM) scenarios. **XMLA read-write is required** for metadata operations.
		- Microsoft Excel – Excel PivotTables are one of the most common tools used to summarize, analyze, explore, and present summary data from Power BI datasets
	- To enable read-write for a capacity
		- In the Admin portal, select Capacity settings > Power BI Premium > capacity name.
		- Expand Workloads. In the XMLA Endpoint setting, select Read Write.
	- Connecting to a Premium workspace
		- in workspace Settings > Premium > Workspace Connection, select Copy.
	- Security
		- In addition to the XMLA Endpoint property being enabled read-write by the capacity admin, the tenant-level setting Allow XMLA endpoints and Analyze in Excel with on-premises datasets must be enabled in the admin portal. If you need to generate AIXL files that connect to the XMLA Endpoint, the tenant-level setting Allow live connections should also be enabled. These settings are both enabled by default.
		- Allow XMLA endpoints and Analyze in Excel with on-premises datasets is an integration setting.

### Profile the data 

- Distribution: Histogram of count of distinct and unique values
- Quality: check empty, validity percentage and errors
- Profile: Column in depth statistics

	- Profiling data is about studying the nuances of the data
		- Anomalies
		- Structure
		- Statistics
		- Row Count
		- Distribution
	- Examine data structures
		- Before profile the data examine the model in power bi and the table relationships
		- Find data anomalies and data statistics
	- Select the View ribbon
		- Column Quality: Show % of errors, empty and valid
		- Column Profile: Detail statistics about the column
		- Column Distribution: Histogram chart of value distribution if continuos or classes (distinct)
	- Column quality and Column distribution are shown in the graphs above the columns of data
	- Columns profile statistics
		- Count
		- Error
		- Empty
		- Distinct : All values in a column, including duplicates and null values
		- Unique : Do not include duplicates or nulls
		- Empty String
		- Min
		- Max
		- Value Distribution (Bart Chart): important because it identifies outliers.


- Use editor to modify M code
		- Click in Power Query - Advanced Editor 

- Use Parameters
	- In power query click Manage Parameters in the Home Ribbon
	- This allow to load different data in the report depending on the parameter
	- Steps
		- Write a SQL query with a parameter &parameter
		- Needs to be type text and configured in the M code outside the query string: https://docs.microsoft.com/en-gb/learn/modules/manage-datasets-power-bi/2-report-parameters


### Clean, transform, and load the data

[Clean data learning](https://docs.microsoft.com/en-gb/learn/modules/clean-data-power-bi/)

- Power Query Editor
	- Query pane (left)
	- Query displays in the middle of the screen and
	- Query setting on the right
	- All steps that you take to shape your data are recorded

- Identify column headers and names 
	- Make sure columns are in the right place
	- You can promote headers or delete rows to make headers are column names
	- Promote header by clicking on the top left of the grid view and select "use first row as header"
	- Rename Columns
		- Look for spelling error and fix
		- Give meaningful names for the end user. Avoid codes, shortened, underscores and acronyms 
	- Remove top rows if empty
	- Remove columns
		- Help you to focus on the data that you need and help improve the overall performance of your Power BI Desktop datasets and reports
		- Can select what you want to keep and "remove other" or select what you want to remove and keep the rest
	- Unpivot columns 
		- often use it when importing data from Excel
		- It transform column headers in rows
		- It helps to shape the data to make aggregation and measures more efficient
	- Pivot columns 
		- Inverse o unpivot
		- You can aggregate flat data

- Simplify the data structure
	- Rename a query 
		- Usually PBI give name of the query "query1" or a name from data source file or DB name
		- Give a name that makes sense to your data model that the user is more familiar with
	- Replace values
		- Use the Replace Values feature in Power Query Editor to replace any value with another value in a selected column. 
		- Can fix spelling errors in bulk
		- Replace null values. Can replace to a numeric value to allow calculation and improve compression rate
	- Remove duplicates
		- right-clicking on the header of the column, and then selecting the Remove Duplicates option
	- Best practices for naming tables, columns, and values
		- Replace underscores ("_") with spaces
		- Consistent with abbreviation
		- Excessively short abbreviations can cause confusion
		- Removing prefixes or suffixes that you might use in table names and instead naming them in a simple format

- Evaluate and change column data types
	-  Power BI Desktop automatically starts scanning the first 1,000 rows (default setting) and tries to detect the type of data in the columns.
	- Sometimes Power BI does not detect the right data type
	- The incorrect data type will affect performance issues
	- Higher change of getting incorrect if text files (.csv/.txt) or excel
	- Having incorrect data type can cause measure calculation errors
	- Inability to create hierarchy with mixed data types (non dates)
	- Change the column data type (2  places)
		- In Power Query Editor and in the Power BI Desktop Report view by using the column tools.
		- Better to change data type in Power Query before you load the data 
		- In power query you can click the icon in the column or in "any column" Data Type

- Combine queries
	- Power BI allows you to 
		- Append or 
		- Merge different tables or queries together
	- You wanted to combine because
		- Too many tables exist and you want to simplify the model
		- Table has similar roles
		- You want to custom a column gathering columns from diff tables

	- Appending 
		- When appending you will add rows to a query or table
		- Home tab in the ribbon is the location of the transformation
		- You can append as new or append to any existing table (options)
	- Merging 
		- When merging you will be adding columns to a query or table
		- Combining the data from multiple tables into one based on a column that is common between the tables.
		- This process is similar to the JOIN clause in SQL
			- Left Outer
			- Full Outer
			- Inner

- Use Advanced Editor to modify M code
	- Each cleaning step that you made was likely created by using the graphical interface, but Power Query uses the M language behind the scenes.
	- Power Query Advanced Editor is where all the M code was written
	- You will rarely need to write M code, but it can still prove useful

- Identify and create appropriate keys for joins
	- This process is to make sure that when you are joining tables the field is appropriate
	- Ideally the join should be integer values
	- A lookup table with unique values is important (without duplicates)
	- Good video from [Patrick - Guy in a Cube](https://www.youtube.com/watch?v=Zlu99RUtMRY)

- Configure data loading
	- You can configure for each query by right click the query
		- Enable Load. If unchecked will not bring table to data model in PBI desktop
		- Include in report refresh (it will not refresh in PBI desktop. In service alwyas refresh)

- Resolve data import errors
	- Import Error can be various it comes down to troubleshooting
	- Where there is an error in import PBI will create a folder and a table query so you can analysise the errors
	- Once you fix the error you can then delete the folder and queries with errors.
	- Usually are related to
		- Data types and mixed types
		- Missing a driver to connect to a source
		- Credentials expired
		- Database engine missing [documentation](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-access-database-errors)
		- You can right click column in Power Query and remove the error... this is not proper as it will remove all rows with error and will lose data. It may be a solution if it is a row that is all with error

- Splitting queries into parts
	- If you want to split the query parts then in Query Setting right click in any step and select "Extract previous", then:
		- A new query is created with the current step and the following and the previous steps will be referenced to the original query.

- Languages that can be used in PBI. [source](https://community.powerbi.com/t5/Community-Blog/The-Languages-of-Power-BI/ba-p/69104#:~:text=Python%20can%20also%20be%20used,More%20%7C%20Other%20%7C%20Python%20Script.)
	- Power BI integrates a number of different data languages including DAX, "M", SQL, MDX and R. 
	- Depending on what you want to do and where one of those languages can be use in PBI
	
## Model the Data (25-30%)

### Design a data model

[Azure learning](https://docs.microsoft.com/en-gb/learn/modules/design-model-power-bi)

- The best practice for a data model in Power BI is a Star Schema
- The reason is that a Star Schema optimise performance and make dax code simpler to filter on dimension
- The granularity of the model need be at the level it is required as it can increase cardinality and reduce performance
- A data model need to as much as possible de-normalize tom reduce the number of tables and follow a Start Schema and avoid snow flake patterns
- Power BI allows relationships to be built from tables with different data sources, a powerful function that enables you to pull one table from Microsoft Excel and another from a relational database

- Star Schema
	- Fact tables contain observational or event data values
	- Dimension tables contain the details about the data in fact tables

- Model Relationships
	- Can be created using the Model View by dragging and dropping
	- On the the Ribbon Home > Relationships > Manage Relationships

- Configure table Property Pane
	- In model view you can configure columns using the Property Pane
	- The following fields
		- Description
		- Synonyms (useful for Q&A search)
		- Display Folder. To organize column in folder groups
		- Hide/Show toggle
		- Format Data type
		- Advanced
			- Sort column
			- Assign to category
			- Summarize data
			- If contain nullable values
	- To configure multiple columns simultaneously hold control key and select various columns

- Create a Common date table there are the methods
	- Source (e.g. EDW)
	- DAX
		- `CALENDAR()` OR `CALENDARAUTO()` Functions
		- From a base Date columns can create other calculated columns using DAX functions
			- `Year = YEAR(Dates[Date])`
			- `MonthNum = MONTH(Dates[Date])`
			- `WeekNum = WEEKNUM(Dates[Date])`
			- `DayoftheWeek = FORMAT(Dates[Date].[Day], "DDDD")`
			- DAX [date and time functions](https://docs.microsoft.com/en-us/dax/date-and-time-functions-dax) 
	- Power Query
		- You can use M-language, the development language that is used to build queries in Power Query
		- ``` DAX
			= List.Dates(#date(2011,05,31) // start date
			, 365*10 // every day for 10 years
			, #duration(1,0,0,0) // duration of period (Day, Hour, Minute, Second)
			```
		- to add columns to your new table to see dates in terms of year, month, week, and day so that you can build a hierarchy in your visual.
		- Add column and then on the drop down under date select what column you want e.g. year
	- Once the date table is added to PBI you need to mark the table as date table
	- Specify which column is the date column
	- Power BI will validate if the table is valid checking if there is no gaps and if the dates are valid

- Creating Hierarchy
	- 2 ways
		- right click column and create hierarchy then drag and drop
		- in model view in the property pane click on fields to add to hierarchy
	- Once created hierarchy can add to visual axis and drill down on visuals

- Parent Child Hierarchy
	- Used for chart of accounts or organizational staff hierarchy
	- Power BI has a function to flatten parent child hierarchy
		- `Path = PATH(Employee[Employee ID], Employee[Manager ID])`
		- The calculated columns above find the path from employee to manager`
		- The flatten path is divided with a pipe operator `|`
		- From the flatten it can be split in levels using the function `PATHITEM()`

- Role-playing dimensions
	-  Meaning that the same dimension can be used to filter multiple columns or tables of data
	- You can filter data differently depending on what information you need to retrieve
	- This happens when when a a dimension table makes reference to more than one fact table
		- Example a date dimension has relationship to 2 fact tables (e.g. Orders and Sales)

- Define data granularity
	- Data granularity needs to be at the level you are going to report on
	- For example if you collect data every minute from a IOT device and want to do an average daily temperature you only import the aggregated data to the report to save space and improve performance
	- If Budget and Sales (actual) has different granularity
		- May need to transform budget table to add column to link table to aggregate on same granularity

- Relationships and cardinality
	- Power BI has the concept of directionality to a relationship
	- This direction indicates how the filter propagates between tables
	- The rule is that if propagates from the one to the many side of the relationship
	- Cardinalities can be
		- One-to-many or Many-to-One (same)
		- One-to-One: Unique value in both tables. Indicates that there are space to de-normalize the model and a redundant relationship exists
		- Many-to-Many: There are many value in both tables. Although possible it is not recommended as it can generate ambiguity.

- Cross-filter direction
	- The direction of a relationship cross-filter can be 
		- single or 
			- Only one side filter the other table. In a one-to-Many the one side inly filters
		- both direction
			- Both sides can filter the other table
			- You might have lower performance when using bi-directional cross-filtering with many-to-many relationships.
			- A word of caution regarding bi-directional cross-filtering.
			- Enabling it can lead to ambiguity, over-sampling, unexpected results, and potential performance degradation.
	
- Cardinality and cross-filter direction
	- one-to-one: Only Bidirectional
	- many-to-many: Single or Bidirectional
		The ambiguity that is associated with bi-directional cross-filtering is amplified in a many-to-many
	- one-to-many: Single or Bidirectional

- Resolve many-to-many relationships
	- Before relationships with a many-many cardinality became available, the relationship between two tables was defined in Power BI. 
	- A bridging table needed to be created as a work around (Before the July 2018 release)
	- With the July 2018 version of Power BI Desktop, you can directly relate tables, such as the ones we described earlier, without having to resort to similar workarounds. 
	- [Documentation reference](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-many-to-many-relationships)


- Create quick measures
	- There are 3 ways
		- Fields pane, right-click a table or a field and "new quick measure"
		- Home Ribbon button in "calculations"
		- Model Ribbon button in "calculations"
	- In the Quick Measures window, in the Calculation dropdown list chose the calculation
	- Depending on what you calculation pick it will ask for values to pick (columns)
		- Drag and drop columns
		- Click ok and the measure will be created and write the DAX for you
	- This is a good way to learn DAX (maybe not... :) )


- Design the data model to meet performance requirements
	- Throughout my notes here in various sessions I talked about that you can do to optimize model performance
	- There is [this guide]((https://docs.microsoft.com/en-us/power-bi/guidance/power-bi-optimization)) in the docs which gives some guidelines and link to resources
	- The main takes are
		- Develop a Star Schema
		- Import only data you need. E.g. remove unnecessary columns
		- Analyse performance of visuals loading and querying the data
		- Use managed aggregations 
		- Reduce cardinal levels
		- Limit visuals per report pages
			- Use drill through and tooltip visuals to avoid cramming to many visuals in one page
		- Evaluate custom vision performance before bringing it to your report
		- Manage capacity settings
		- Verify network latency
			- Tools such as [Azure Speed Test](https://azurespeedtest.azurewebsites.net/)
		- [Monitoring performance](https://docs.microsoft.com/en-us/power-bi/guidance/monitor-report-performance)
			- Use Query Diagnostic (power query)
			- Use Performance Analyzer (PBI Desktop)
			- Use SQL Server Profiler
			- Use Dax Studio
			- Monitor Premium metrics (only for premium capacity)
		

### Develop a data model

[Azure Learning](https://docs.microsoft.com/en-gb/learn/modules/model-data-power-bi/)

- Apply cross-filter direction and security filtering
	- The default filter is form the one side to the many side
	- Bidirectional cross-filtering enables them to apply filters on both sides of a table relationship
	- To enable bi-directional filtering
		- Set Cross filter direction to Both
		- Select Apply security filter in both directions
			- The selection of security filter is to make sure that RLS apply in both directions
			- This property only works for certain models and is designed when there is a pattern with dynamic row level security. 
	- [Bidirection and Security Filtering](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-bidirectional-filtering)
	- [White Paper](https://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional%20cross-filtering%20in%20Analysis%20Services%202016%20and%20Power%20BI.docx)
	

- Create Calculated Tables
	- Can create in Power Query
	- Can create in Power BI using DAX

- create calculated columns
	- Can create in Power Query (Preferred as it is optimized for compression and performance)
	- Can create in Power BI using DAX

- Assume referential integrity 
	- When creating relationships It enables queries on the data source to use INNER JOIN rather than OUTER JOIN which improves query efficiency 

- Set up the Q&A feature
	- [Azure Learning](https://docs.microsoft.com/en-us/power-bi/consumer/end-user-q-and-a)
	- Use Q&A is a natural language capabilities of PBI that allow you to receive answers in the form of charts and graphs from asking questions
	- Q&A can be used in
		- Dashboards in PBI Service
		- Power BI Mobile at the bottom of Dashboards
		- In Dashboards you are able to use Q&A but will not be able to save the charts
		- In Reports if add Q&A visual
	- On Dashboards
		- You can type in the search box to get answers
		- You can click in one of the suggested questions PBI detect automatically
	- On Reports
		- On a Q&A visual you can also ask questions and get charts in return
		- If you like the chart you can make a regular chart
	- Troubleshooting and settings
		- Go on the model view and click Q&A setup
		- Go to review sections
			- If you published in PBI Service all questions asked will be there
			- All red underlines will be terms that PBI does not understand
			- To fix the term click in fix needed and then teach PBI what the term means in the model


- Implementing Row Lever Security
	- [Azure Learning](https://docs.microsoft.com/en-gb/learn/modules/row-level-security-power-bi/)
	- Statics Method
		- Control access based on filter applied to table rows using DAX
		- Uses a fixed value in the DAX filter
		- Steps - PBI Desktop
			- Create RLS Role(s)
			- Test Role(s)
			- Deploy PBI Service
		- Steps - PBI Service
			- In Dataset (ellipsis ...) select Security
			- Add Members to the role
			- Test the roles
	- Dynamic Method
		- Control access based on the email of the used logged in
		- Uses a DAX function
		- Steps in PBI Desktop and Service 
			- Same Steps as above
		- When creating the role use the function userprincipalname() which will compare the 
			email of person signed in and the email filter in the report

### Create measures by using DAX

[Learning Page Documentation](https://docs.microsoft.com/en-gb/learn/paths/dax-power-bi/)

```python 
TODO
 use DAX to build complex measures
 use CALCULATE to manipulate filters
 implement Time Intelligence using DAX
 replace numeric columns with measures
 use basic statistical functions to enhance data
 create semi-additive measures
```

### Optimize model performance

[Azure Learning](https://docs.microsoft.com/en-gb/learn/modules/optimize-model-power-bi/1-introduction)

- Performance optimization
	- It involves making changing in the data and model to run more efficiently
	- As a data analyst, you will spend approximately 90 percent of your time working with your data
	- Poor performance is related to a bad data model or bad dax code
		- Ensuring that the correct data types are used
		- Deleting unnecessary columns and rows
		- Avoiding repeated values
		- Replacing numeric columns with measures
		- Reducing cardinalities
		- Analyzing model metadata
		- Summarizing data where possible

- Analyse performance to find bottlenecks
	- You can use Performance analyzer in Power BI Desktop to help you find out how each element is performing
	- Identify how long each visual is taking to load
	- Before doing tests clear the cache
		- Either reopen PBI desktop
		- Connect to Dax studio and clear cache
	- Start recording and reload visuals
	- The result is broken down in
		- Dax Query: How long it took to run the query. Send to source model and return results
		- Visual display: How long took to load the visual
		- Other: Time took for other background processing, wait other visuals and etc.
	- Visuals
		- Consider the number of visuals on the report page; less visuals means better performance
		- Consider the number of fields in each visual
	- DAX QUery
		- There are more efficient way to right dax to improve performance
	- Data Model
		- Relationships can be costly. For example snow flake reduce performance
		- Columns: Do not import unnecessary columns
		- Unnecessary rows: remove empty rows
		- Data Type: Date and Time columns is costly. Consider splitting them or remove time if do not need it
			Vertipaq optimize numeric types over text (Numeric versus hash encoding).
		- Auto Date/Time. Disable to improve performance as it reduces de size of the data model

- Use variables instead of numeric calculated columns
	- Reduce size of model

- Use variables in measure 	
	- Variables can make measures more efficient because they remove the need for Power BI to evaluate the same expression multiple times
	- Improve readability
	- Easier to debug
	- Reduce complexity

	#### not using variables
	```M
	Sales YoY Growth =
	DIVIDE (
    ( [Sales] - CALCULATE ( [Sales], PARALLELPERIOD ( 'Date'[Date], -12, MONTH ) ) ),
    CALCULATE ( [Sales], PARALLELPERIOD ( 'Date'[Date], -12, MONTH ) )
	)
	```
	#### using variables

	```M
	Sales YoY Growth =
	VAR SalesPriorYear =
    CALCULATE ( [Sales], PARALLELPERIOD ( 'Date'[Date], -12, MONTH ) )
	VAR SalesVariance =
    DIVIDE ( ( [Sales] - SalesPriorYear ), SalesPriorYear )
	RETURN
    SalesVariance
	```
- Reduce cardinality
	- Cardinality is a term that is used to describe the uniqueness of the values in a column.
	- Use data profiler in Power Query to analyse column distribution and unique values
	- Lots of repeated values indicated low cardinality. A GOOD THING.

- Improve performance by reducing cardinality levels
	- Power BI Desktop offers different techniques that you can use to help reduce the data that is loaded into data models, such as summarization.
	- Reducing the data that is loaded into your model will improve the relationship cardinality of the report.
	- The most effective technique to reduce a model size is to use a summary table from the data source.

[Azure Learning - Reduce import in modelling](https://docs.microsoft.com/en-us/power-bi/guidance/import-modeling-data-reduction#group-by-and-summarize/?azure-portal=true)

- Import mode
	- Import mode store the source data to disk by VertiPaq engine
	- The compression is 10x. e.g. 10Gb to 1Gb
	- Despite the high compression of the engine you should strive to reduce data imported to PBI
		for performance
	- Shared Capacity can host model up to 1Gb and Premium up to 13Gb

- Data reduction techniques
	- Remove columns
	- Remove rows
	- Group and Summarize
	- Optimize Column Data Type
	- Preference for Custom Columns
		- It is less efficient to add table columns as calculated columns than Power Query computed columns (defined in M).
	- Disable Power Query Load
	- Disable Date Time (Auto)
	- Switch to mixed Model

- Group by and summarize
	- The most effective technique to reduce a model size is to load pre-summarized data
	- For example from daily to moth aggregation of sales data

- Switch to Mixed mode
	- In Power BI Desktop, a Mixed mode design produces a Composite model
	- it allows you to determine storage mode for each table
		- Recommendation
		- Large Fact Table to Direct Query
		- Summarized Data: Import Mode

- Implications of using DirectQuery
	- It is suitable in cases where data changes frequently and near real-time reporting is required.
	- It can handle large data without the need to pre-aggregate.
	- It applies data sovereignty restrictions to comply with legal requirements.
	- It can be used with a multidimensional data source that contains measures such as SAP Business Warehouse (BW).

- Behavior of DirectQuery Connections
	- No data is imported into the Power BI Desktop, only the schema is loaded. 
	- When visual is built query is sent to source
	- Query made in source will not automatically affect. Refresh will be required.
	
- Limitations Direct Query
	- Performance: Depend on performance of underlying source
	- Security: Be aware that if multiple sources are imported understand how data can move between data sources and the associated security implications
	- Data Transformation: Within Power Query if you connect to a OLAP source no transformation can be performed so any transformation need to be do in the source.
	- Modelling: Some modelling capability are not available
	- Reporting: All reporting capability is offered but when published in the PBI service the quick insight feature are not supported for DirectQuery sources.

- Create managed aggregations
	- Why?
		- Aggregated data is cached and, therefore, uses a fraction of the resources that are required for detailed data.
		- Instead of refreshing what could be millions of rows, you would refresh a smaller amount of data instead.
		- help you reduce and maintain the size of your model.
		- anticipate your data model growing in size in the future and acting proactively
	- Creating aggregations
		- Decide the grain or what level will aggregate: Month, Category etc`
		- Where you can create aggregation
			- Source if you have access
			- Power Query after import : Use the Group By tool
	- Managing the aggregation
		- Once you created the aggregated table 
		- Open by right clicking any field and then "Manage Aggregation" in the detailed table
		- Reference the column and aggregation
		- [Azure Docs](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-aggregations)
		- aggregation tables are hidden from Report view.
		- All Power BI Import and non-multidimensional DirectQuery data sources can work with aggregations.
		- Set the storage mode of an aggregated table to Import to speed up queries
		- To work correctly for aggregations, RLS expressions should filter both the aggregation table and the detail table. (RLS- Role Level Security)
		- Once the aggregation is set up the detailed table performance will improve and load faster as it will
			use the aggregation from the aggregation

## Visualize the Data (20-25%)

### Create reports 

[Azure Docs](https://docs.microsoft.com/en-gb/learn/modules/visuals-power-bi/)

- Add visualization items to reports
	- Power BI come with a range out of the box visuals that can be selected form the visualization pane
		- To add more visuals click on the ellipses and can chose from Marketplace or Upload a file
	- Table and Matrix
		- Are grid like visualizations
		- Table is simpler and summarize measures in values (measure) by a chosen dimension (column)
		- Matrix allow to "slice" in rows and columns. Similar to a pivot table
		- Matrix can cross highlight other charts. Any chart can.
		- Matrix options: Row, Column and Value
		- Table option: Value
	- Bar and Column 
		- Can be clustered or stacked
		- Represent groups and classes
		- Axis, Legend, Values and tooltip are options
	- Line and Area chart
		- Present Trend over time
		- Options
			- Axis, Value, Legend, Secondary axis and tooltips
	- Pie charts, donut charts, and tree-maps
		- Show relationship of the part in a whole
		- Avoid presenting too many categories because it results in thin slices (or rectangles) that provide no added value to the user
		- Options
			- Legend, Details, Value and Tooltip
	- Combo charts
		- Combination of Bar and Line charts sharing same X axis
		- Compare multiple measures with different value ranges
		- Illustrate the correlation between two measures in one visual
		- Conserve space on your report page
		- Options
			- Shared axis, column values, line values and tooltips
	- Card visualization
		- Display a single value data point
	- Funnel visualization
		- Represent sequential stages representing workflow
		- Options
			- Group, value and tooltips
	- Gauge chart
		- Displays a single value that measures progress toward a goal or target
		- Options
			- Min, Max, Target, Value and tooltip
	- Waterfall 
		- Useful in displaying a series of positive and negative changes.
		- Aka bridge chart
		- Options
			- Category, Breakdown, Value and tooltips
	- Scatter Chart
		- Effective when you are comparing large numbers of data points without regard to time.
		- Analyse correlation
		- Options
			- Detail, legend, X axis, Y axis, size, play axis and tooltips
	- Maps
		- Have options of Bubble Maps (simpler) and filled maps (e.g.fill countries, states)
		- Options for data (Fill)
			- Location, legend, Lat, Lon and tool tips
		- Options for data (bubble)
			- Location, legend, Lat, Lon, SIZE and tool tips
	- Slicer visualization
		- The slicer visualization is a standalone chart that can be used to filter the other visuals on the page.
		- Slicer list have the options	
			- Single select: 
			- Multi-select: Use control to select more than on
			- Show "Select all"
	- Q&A visualization
		- The Q&A visualization allows you to ask natural language questions and get answers in the form of a visual
		- Insert question in the box or use the pre defined selections
		- Click in turn result into standard visual 

- Format and configure visualizations
	- Select the Format button (paint roller icon) to display the Format pane
	- You can format the title by changing the text, text size, font, color, background, and alignment. The subsequent section shows an example of customizing a title.
	- The format will allow to customize and configure your visual
	- Not all options you will have to all chart type but the main things to configure are
		- Background
		- Legend
		- Title
		- Axis
		- Data Color
		- Border
		- Shadow
		- Plot area

- Customize Tooltip Visuals
	- Using tooltips is a clever way of providing more contextual information and detail to data points on a visual
	- Beyond just providing the extra information of tooltip when you hove over a chart it is possible to create a chart tool tip that will pop up when you hove over a chart
	- Steps
		- Open new page
		- In the format pane select Page Size type "tooltip"
		- In page information (format pane) switch the toggle tooltip to ON
		- On the View tab, set the Page view option to Actual size
		- Add the visual you want to be displayed in the tooltip
		- Return to the report page and apply the tooltip
		- Select a visual you want the tooltip
		- In the Format pane, scroll down to the Tooltip section. Turn the tooltip option On and then select your tooltip page from the Page list.

- Import a custom visual
	- In addition to the out-of-the-box visualizations in Power BI Desktop, hundreds of other developers have created a multitude of visuals for you to choose from.
	-  Are created by Microsoft and Microsoft partners
	- Some of these custom visuals are certified and some are not
	- The certified status means that the visual meets the Microsoft Power BI team code requirements
	- An uncertified visual is not necessarily unsafe to use
	- The custom visual software development kit (SDK), which is an open-source tool based on NodeJS (JavaScript programming language)
	- To get a custom visual click in the ellipses (...) "Get more visuals"

- Add an R or Python visual
	- R
		- Before you create the R visual, you must install R
		- When you have downloaded and installed R, Power BI enables it automatically
		- Despite automatic you should verify that it has been enabled in the correct location
			- In Power BI Desktop, select File > Options and settings > Options and then select R scripting in the Global options list.
			- Verify if the path you your system where R installed is correct
		- To create a visual
			- Click in the R chart icon
			- Drag the fields to values 
			- This will behave like a table visual
				- Fields are grouped and duplicate rows appear only once.
				- The default aggregation is: do not summarize.
			- then will automatically create a (Data Frame) in the script editor
			- You will use data frame to render the chart using R
  
	- Python
		- No prerequisites exist for creating a Python visual, so you can start right away in Power BI Desktop by selecting the Python visual icon in the Visualizations pane
		- To create a visual the process is the same as describe above for R

	- Python and R visuals do not interact and are not dynamic and the out of the box chart in PBI
	- Python packages supported in PBI [service](https://docs.microsoft.com/en-us/power-bi/connect-data/service-python-packages-support)
		- matplotlib
		- numpy
		- pandas
		- skit-learn
		- scipy
		- seaborn
		- statsmodel
		- xgbost

- Configure the report page
	- The configuration of a report page is the process of adding visuals and power BI features to meet requirements

- Accessibility Configuration				
	- Alt text: To accommodate report consumers who use screen readers
		To add alt text to an object, select that object and, in the Visualizations pane, open the Format pane
	- Tab order: help keyboard users navigate your report in an order that matches the way that visual users would see it
	- Titles and labels:  provide clear, concise, descriptive titles
	- Markers : Avoid. Can bring distraction
	- Themes : Use colors with contrast

- Report Themes
	- What can be configured in themes 
		- Sequence of customer colors
		- Default properties for new visuals
	- What CANNOT configure
		- Background color
		- Background image

- Create Paginated Report
	- [Learning Documentation](https://docs.microsoft.com/en-gb/learn/modules/create-paginated-reports-power-bi/)
	- Paginated reports give a pixel-perfect view of the data. It is ideal to generate table reports that go over multiple pages. For example printing invoices, list of addresses and etc
	- Pixel perfect is the ability to control the report rendering in a page with a pixel precision
	- Power BI paginated reports are descendants of SQL Server Reporting Services (SSRS) and they have a lot in common
	- Power BI paginated reports are not created in Power BI Desktop
		- They are built by using Power BI Report Builder. 
		- Power BI paginated reports are a feature of **Power BI Premium.**
	- Get data
		- Getting data in a Power BI paginated report does not involve data cleaning steps
		- In fact, data is not stored in a Power BI paginated report dataset
		- Data is retrieved in an unaltered form from the data source
		- Data can be collected from multiple data sources, including Microsoft Excel, Oracle, SQL Server, and many more.
		- Data from different data sources **cannot be merged** into a single data model.
		- Each source must be used for a different purpose
		- Paginated reports have an [expression language](https://docs.microsoft.com/en-us/sql/reporting-services/report-design/expressions-report-builder-and-ssrs?view=sql-server-ver15) that can be used to look up data in different datasets, but it is nothing like Power Query.
		- Power BI paginated reports can use a dataset from Power BI service which can be transformed as a previous step
	- Data Source
		- A Data Source is a connection to a source where you create to a source like for example a SQL server
	- Dataset
		- Is the basis for your report which is derived to the Data Source
		- In a database a Dataset can be a text query or a Stored procedure
	- Create a paginated report
		- Once you set a Source and Created a Dataset you are ready to create a report
		- To create a report you must add one visual at least. Select the Insert Tab
		- Most common visual is a Table which can be done from scratch or use the wizard
	- Add [parameters](https://docs.microsoft.com/en-us/power-bi/paginated-reports/paginated-reports-parameters)
		- To add a parameter, right-click Parameters and select Add Parameter
		- Once you configured the parameter you can add as a variable in the Dataset with a `@` before the parameter like `@parameterName`
	- Work with charts on the report
		- In insert either select chart icon and add or right click in the canvas and insert chart
		- Once the chart is in drag fields and how you want to aggregate and categorize in chart.
		- Configure labels by clicking and using the format ribbon or de property using expressions 
	- Publish
		- Click publish and select workspace. 
		- Work the same as Power BI
	- Paginated Report in a Day [Course](https://docs.microsoft.com/en-us/power-bi/learning-catalog/paginated-reports-online-course)
	

### Create dashboards 

[Azure Docs](https://docs.microsoft.com/en-gb/learn/modules/create-dashboards-power-bi/)

- Dashboards can pin visuals from various reports and dashboards
- Dashboards can contain visuals from different datasets
- Dashboards are only available in the Power BI service
- Dashboards do not have filters like reports do
- Dashboards can only be a single page
- Source for tiles
	- Dashboards
	- Reports 
	- Datasets
	- SSRS
	- Excel

- Dashboard alerts give you alert if a visual data reach a certain threshold and it can ONLY be applied to the following visuals
	- KPI
	- Cards
	- Gauge
- Alerts can be sent to the notification center or email.
- More info about alerts (https://docs.microsoft.com/en-us/power-bi/create-reports/service-set-data-alerts)

- Alerts can be set on visuals created from streaming datasets that you pin from a report to a dashboard.
- Alerts can't be set on streaming tiles created directly on the dashboard using Add tile > Custom streaming data.

- Dashboard has Q&A feature where you can ask questions
- Q&A uses natural language processing

- It is possible to pin an entire report page to a dashboard 

[Azure Docs](https://docs.microsoft.com/en-us/power-bi/create-reports/service-dashboard-pin-live-tile-from-report)

- You can stream data to power bi connections from
[Azure Docs](https://docs.microsoft.com/en-us/power-bi/connect-data/service-real-time-streaming)
	- API
	- Azure Stream Analytics
	- PubNub
	
There are 3 types of streaming dataset
- Push
- Streaming
- PubNub

- Can set up a theme by clicking in edit. 
- Can also upload or download a Json Theme to be applied to the dashboard

- Classification of dashboards on `...` (ellipsis) settings options 
[Azure Docs](https://docs.microsoft.com/en-us/power-bi/create-reports/service-data-classification)
	- DO NOT SHARE, 
	- ASK FOR PERMISSION
	- OK TO SHARE
	
- Mobile View is to customize view for mobile
- Once publish it is possible to view on mobile but it can be further customized

### Enrich reports for usability

[Azure Docs](https://docs.microsoft.com/en-gb/learn/modules/data-driven-story-power-bi/1-introduction)

- Design a report layout
	- Key Guidelines
		- The audience is not you but the business user
		- Draw a sketch of your report layout
		- Focus on the most important information
		- Select the right background for the context of your report.
		- Consider the number of visuals and cluttering
			 - Can use drill through or tooltips
		- Even distribute and align titles and visuals. Position is important
		- Configure iterations if want to cross highlight or filter or no interactions in some cases (depend on user preference)
		- Consider hierarchy on visuals for better data navigation and drill down experience
		- Consider accessability like tab order and color for color blindness
			- PBI is compliant to  Web Content Accessibility Guidelines (WCAG)
				- Perceivable
				- Operable
				- Understandable
		- Accessibility features`
			- Keyboard navigation
			- Screen-reader compatibility
			- High contrast colors view
			- Focus mode
			- Show data table
			
- Add buttons, bookmarks, and selections
	- Bookmarks: Capture the currently configured view of a report page so you can quickly return to that view later.
	- Buttons: Create a more interactive experience for the report users.
		- Button actions options:
			- Back : Take back previous page. Used well with drill through
			- Bookmark: apply a bookmark
			- Drill through: take to a drill through of the current filter
			- Page navigation : Navigate to a specific page
			- Q&A : Open a search window
			- Web URL: Take to a URL
	- Selections: Allow you to determine what items in the report are visible and what items are hidden.
	
- Use basic interactions
	- On Format click on Edit Iteration
		- There are 3 options
			- None: when iterating nothing will happen. Visual stay the same
			- cross-filtered: Visual will filter what of selected in the other visual when iterating
			- cross-highlight: Visual will stay the same and highlight what was filtered proportionals
	- hierarchies
		- When add hierarchy in a visual you will have the ability to drill down

NOTE:
Keep in mind that the number of interactions between your visuals will impact the performance 
of your report. To optimize the performance of your report,
consider the query reduction options that are available within Power BI Desktop. 
You have the option to send fewer queries . This is configured in options > query reduction

## Analyze the Data (10-15%)

### Enhance reports to expose insights

- Add Filters
	- Filters only are available in Reports NOT Dashboards
	- Slicers : Can add to report to filter visuals
		types
		- Numerical
		- Categorical
		- Date
	Filter Pane: Filter that can filter across report and tabs 

- Visual Drill Down
	- Works if there is a hierarchy

- Bookmarks is used to save states of the data you want to come back. Can be a filter or unfilter data

- Power Bi visuals allow you to (click ellipsis)
	- add a commend
	- export data (table or underlying data: depends on sys adm permissions and settings)
	- Visual table (visualize table of the chart data)
	- Spotlight (show just selected chart and hiding all other elements
	- Sort (Sort by date or category depending on what is selected on axis)

- Drill through
	- Create a new report page (tab) and add a drill through filter
	- Automatically all visuals with the drill through filter will then take the filter and pass to drill through page (detail)
	- To access drill through right click the visuals
	
- Conditional formatting
	- Conditional formatting apply to fields and measures
	- Click on down arrow in the visual value and select option to condition format
	
- Apply slicing, filtering, and sorting
	- Filter only work in reports NOT Dashboard`
	- Slicers are added in report so end users can filter the report
	- Putting slicer next to visuals make user experience better to filter visuals
	- Slicer does not support input fields or drill through features
	- Slicer list have the options	
		- Single select: 
		- Multi-select: Use control to select more than on
		- Show "Select all"
	- If want to filter in a basic way use the filter pane
	- Sort Data can be done in visuals Ascending and Descending
		- For dates that are text need a numeric column to help sort the date text otherwise the sort will be alphabetically
	
- Publish and export
	- Publish
		- Click publish. If not saved will prompt to save
		- chose the app workspace to publish in Power BI service
	- Can export report to 
		- PPT
		- CSV
		- Excel
		- PDF
- Comments
	- Comments are also available for paginated reports, dashboards, and visuals. 
	
- Tune report performance
	- In the power Bi ribbon on Report View > View click on "Performance Analyser"
	- Click record and then as you interact with the report and PBI fire queries it will display how long eac query took
	- Results display in milliseconds
	- Reasons for bad performance
		- too many visuals
		- High cardinality
		- High Granularity
	- Make sure if granularity and only necessary data is loaded so this can improve performance
	
- Optimize for mobile use
	- Any report is already possible to access via mobile
	- However it is possible to customize and optimize layout for mobile
	
- Sync Slicer
	- It is possible that a filter slicer apply across multiple report pages.
	- You have the option to sync and also show the slicer
	- A slicer does not need to show to sync in a page
	- Configure in View ribbon in Report View


- Explore statistical summary
	- Statistics show the distribution of your data
	- Help you identify take ways and outliers
	- Provide a quick description of your data
	- PBI offers tools like
		- Dax
		- Visuals (Histograms, Bell Curves)
		- Statistical programming languages R and Python
	- In visualizations click value (right) to access aggregation functions (sum, count, average, min, max etc)
	- You can use bar chart to represent Histograms and area chart for bell curves
	- A typical bar or column chart visual in Power BI relates two data points: a measure and a dimension. A histogram differs slightly from a standard bar chart in that it only visualizes a single data point.
	- To represent a histogram create a new grouping for the x-axis. 
	- To create a group click in the fields pane on the field you want to create the group.
		- For histogram grouping pick bin
		- Define the type of bin (Number of Bins)
		- Define the number of bins
		- THe min max are gray out (defined automatically)
		- Then on the visual (clustered bar) drag the new group to the axis

- Top N analysis
	- Returns the top N rows of a specified table based on a criteria such as sales, count of orders and etc
	- It is possible to sort from bottom to top
	- Top N with DAX
		``` Dax
		Top 10 Products =
		SUMX ( TOPN ( 10, Product, Product[Product Name], ASC ), [Total Sales] )

		```
	- It returns a table with top 10 products by Total Sales.

- AI powered analysis
	- You can use the analyse feature in power BI. 
	- Say in a time series you noticed an increase or decrease in sales in a particular month and wanted to understand why
	- Right click on the month in the chart and chose analyse "Explain Increase" or "Explain Decrease"
	- PBI will the pull another chart with an explanation. Say... category X had an significant increase on that month.

- Quick insights
	-  Quick insights feature in Power BI uses machine learning algorithms to go over your entire dataset and produce insights (results) for you quickly
	- Quick insights are available in the Power BI Service
	- Go to datasets and click on the quick insight
	- Power BI will use various algorithms to search for trends in your dataset.
	- The Quick Insights page contains up to **32 separate insight cards**, and each card has a chart or graph plus a short description.
	- If you see an insight card that is particularly compelling, you can add it to your report. You can filter the insight on your report by using  filter pane and;
	
- Create reference Lines using the Analytics Pane
	- [Docs](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-analytics-pane)
	- With the Analytics pane in Power BI Desktop, you can add dynamic reference lines to visuals, and provide focus for important trends or insights
	- The Analytics pane only appears when you select a visual on the Power BI Desktop canvas
	- You can create the following types of dynamic reference lines
		- X-Axis constant line
		- Y-Axis constant line
		- Min line
		- Max line
		- Average line
		- Median line
		- Percentile line
		- Symmetry shading
	- Not all lines are available for all visual types (e.g. Not available for Maps)
	- You can add constant line with a set value or a dynamic line based on a calculation (e.g. Min, Max etc)
	- Applying forecast
		- If you have time data in your data source, you can use the forecasting feature
		- You may specify many inputs to modify the forecast, such as the Forecast length or the Confidence interval.
		- Forecasting feature is only available for line chart visuals.
		- [Limitations](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-analytics-pane#limitations)


### Perform advanced analysis

[Learning Docs](https://docs.microsoft.com/en-gb/learn/modules/perform-analytics-power-bi/?WT.mc_id=cloudskillschallenge_bc12b5d7-d7ce-4fcd-aa6a-e29ed9d1f358)

[Learning Docs 2](https://docs.microsoft.com/en-gb/learn/modules/ai-visuals-power-bi/?WT.mc_id=cloudskillschallenge_bc12b5d7-d7ce-4fcd-aa6a-e29ed9d1f358)

- Identify Outliers
	- Outliers is a type os anomaly in your data
	- Outliers are data points that are significant different from your average data point.
	- Identify Outliers with visuals
		- The process of identifying outliers involves segmenting your data into two groups
			- Contains the outliers
			- Does not contain the outliers
		- The best way to identify outliers is by using scatter chart
	- Using DAX to identify outliers
		```dax
				Outliers =
		CALCULATE (
			[Order Qty],
			FILTER (
				VALUES ( Product[Product Name] ),
				COUNTROWS ( FILTER ( Sales, [Order Qty] >= [Min Qty] ) ) > 0
			)
		)
		```
		- When you have created a new outlier measure, you can group your products into categories by using the grouping feature described above and below
	- Apply clustering techniques
		- allows you to identify a segment (cluster) of data that is similar to each other but dissimilar to the rest of the data.
		- Create a scatter chart
			- To apply clustering to your scatter chart, select More options (...) in the upper-right corner of the visual and then select Automatically find clusters.
			- In the window that pop up define the name and description of cluster and the number of bins you want in your cluster

- Conduct time series analysis
	- To conduct a time series analysis in Power BI, you need to use a visualization type that is suitable for displaying trends and changes over time, such as a line chart, area chart, or scatter chart.
	- Microsoft AppSource has an animation custom visual called Play Axis similar to the one that Hans Rosling presentation in 2007

- Group and bin data for analysis
	- When you create a visual power BI automatically aggregate data into categories and group according to your data categories
	- Grouping is used for categories of data.
	- Say you have a chart splitting your data by city and you want to display in 2 groups
		- Create a list type of groups
		- Major city
		- Minor cities

- Use the [Key Influencers](https://docs.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-influencers)
	- The Key influencers visual helps you understand the factors that are affecting a specific metric
	- The visual also helps you to contrast the relative importance of these factors
	- In the visual select the measure you want to understand the key influencer than drag the dimension column to explain by, then PBI will automatically prot a chart that explain the relationship between measure and column


- Use the [Decomposition Tree](https://docs.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-decomposition-tree) visual
	- The Decomposition Tree visual automatically aggregates your data and lets you drill down into your dimensions so that you can view your data across multiple dimensions
	- The visualization requires two types of input:
		- Analyze – the metric you would like to analyze. This has to be a measure or an aggregate.
		- Explain By – one or more dimensions you would like to drill down into.
	- AI Splits 
		- These splits appear at the top of the list and are marked with a lightbulb and help you find high and low values in the data, automatically.
	- Locking
		- A content creator can lock levels for report consumers. When a level is locked, it cannot be removed or changed
		- A consumer can explore different paths within the locked level but they cannot change the level itself.
		- Limitations
			- The maximum number of levels for the tree is 50.
			- The decomposition tree is not supported in the following scenarios
				- On-premises Analysis Services
				- Azure Analysis Services
				- Power BI Report Server
				- Publish to Web
				- Complex measures and measures from extensions schemas in 'Analyze'
				- Support inside Q&A

- Apply AI Insights
	-  AI Insights feature allows you to connect to a collection of pretrained machine learning models that you can apply to your data to enhance your data preparation efforts.
	- AI Insight is a Power Query Editor feature
	- Can be assessed by the home ribbon or the add column ribbon
	- Will be available for you to choose from: Text Analytics, Vision, and Azure Machine Learning.


## Deploy and Maintain Deliverables (10-15%)

### Manage datasets

- Managing Dataset
	- Upload to the service for reuse in multiple reports
	- Use parameter to change server and database for data source
	- Use parameters to configure incremental refresh
	- Maintain gateways
	- Create what if parameters
	
- Create dynamic reports using parameters
	- This allow to load different data in the report depending on the parameter
	- Steps
		- Write a SQL query with a parameter &parameter
		- Needs to be type text and configured in the M code outside the [query string](https://docs.microsoft.com/en-gb/learn/modules/manage-datasets-power-bi/2-report-parameters)

- What if scenario
	- Create a New Parameter in report view (modelling)
	- Define the min max boundaries and step and add as a sliced
	- Add this to a measure and then do what if analysis with it
	
	
- Configure [On-Premisses Gateway](https://docs.microsoft.com/en-us/data-integration/gateway/service-gateway-app)
	- The on premisses gateway can be in two modes
		- Personal : Installed in the local machine and used by only 1 user
		- Organizational: Installed in the same server where the data source is and can be used to multiple users in the organization
	- The gateway is a mean of confecting the power bi service (cloud) to on premisses sources
	- It allow save traffic of credentials over the internet
	- It allows for scheduled refresh of the power bi service to get data from on premisses sources
		
- Configure Schedule refresh
	-  Scheduled refresh feature in Power BI allows you to define the frequency and time slots to refresh a particular dataset
	- Saves time as you do not need to do it manually
	- Need to create a gateway connection first
	- Can refresh 48 times a day on premium and 8 times on shared capacity
	- It is possible to refresh manually as many time as you want and that does not affect the scheduled refresh
	- After four (4) consecutive failures the scheduled refresh deactivate
	
- Configure [incremental refresh settings](https://docs.microsoft.com/en-us/power-bi/admin/service-premium-incremental-refresh)
	-  allows you to refresh large datasets quickly and as often as you need, without having to reload historical data each time.
	- Incremental refresh should only be used on data sources and queries that support query folding. (e.g. SQL DB)
	- RangeStart and RangeEnd values must be the name of the parameters
	-  Type is set to Date/Time and the Suggested Value is set to Any value.
	- In power query select the column to filter (date column) and right click and pick option custom filter
	- In power BI right click in the table and select [incremental refresh](https://docs.microsoft.com/en-us/power-bi/admin/service-premium-incremental-refresh)
	
- Manage and promote datasets
	- You want governance in your data and
		direct your users to the most up-to-date and highest-quality datasets 
		in your workspaces, or you might want to restrict the reuse of datasets across your workspaces.
	- This can be done by endorsing datasets
	- Power BI provides two ways to endorse your datasets
		- Promotion: Means data is ready for use and any member of the worspace can do it
		- Certification: Certify a promoted dataset as "source of truth" and only can be done by Adm role
		
- Troubleshoot [service connectivity](https://docs.microsoft.com/en-us/power-bi/connect-data/refresh-troubleshooting-refresh-scenarios)
	- Cloud sources (Azure) does not require gateways
	- Always make sure credentials are up to date. User service accounts where password does not expire

- Query caching [(Premium Feature)](https://docs.microsoft.com/en-us/power-bi/connect-data/power-bi-query-caching)
	- With the Query Caching feature, you can use the local caching services of Power BI to process
			query results. Instead of relying on the dataset to calculate queries
	- Query Caching is a local caching feature that maintains results on a user and report basis. 
			This service is only available to users with *Power BI Premium or *Power BI Embedded.
	- To configure: Go to a dataset in your workspace and open its Settings page. 
			

- Configure row-level security group membership [source](https://docs.microsoft.com/en-us/power-bi/admin/service-admin-rls)
	- In the Power BI service, members of a workspace have access to datasets in the workspace. RLS doesn't restrict this data access.
	- You can configure RLS for data models imported into Power BI with Power BI Desktop
	- You can also configure RLS on datasets that are using DirectQuery, such as SQL Server
	- For Analysis Services or Azure Analysis Services lives connections, you configure Row-level security in the model, not in Power BI Desktop. The security option will not show up for live connection datasets.
	- To Manage  security in your model once published in the PBI Service
		- Select the More options menu for a dataset. This menu appears when you hover on a dataset name
			- Select Security
			- Add members to a role you created in Power BI Desktop
			- You can only create or modify roles within Power BI Desktop
			- Only the owners of the dataset will see Security.
			- If the dataset is in a Group, only administrators of the group will see the security option.
		- Add members
			- Add a member to the role by typing in the email address or name of the user or security group.
			- You **can't** add Groups created in Power BI
			- You can add members external to your organization.
			- You can use the following groups to set up row level security.
				- Distribution Group
				- Mail-enabled Group
				- Security Group
			- **Office 365 groups are not supported** and cannot be added to any roles

- Provide access to datasets [source](https://docs.microsoft.com/en-us/power-bi/connect-data/service-datasets-build-permissions)
	- Once a data model is published to the PBI service it also store the underlying dataset which can be reused in other PBI reports
	- If you want to allow other members of your organization to have access to a dataset that you published you can do this by providing access to it.
	- To achieve this you need to give users **Build permission**, they can build new content on your dataset, such as reports, dashboards, pinned tiles from Q&A, paginated reports, and Insights Discovery.
	- Users also need Build permissions to work with the data outside Power BI
		- To export the underlying data.
		- To build new content on the dataset such as with Analyze in Excel.
		- To access the data via the XMLA endpoint.
	- You can give access in different ways
		- Members of a workspace with at least a Contributor role automatically have Build permission
		- Members of the workspace where the dataset resides can assign the permission to specific users or security groups in the Permission center.	
			- Select More options (...) next to a dataset > Manage Permissions.
			- An admin or member of the workspace where the dataset resides can decide during app publishing that users with permission for the app also get Build permission for the underlying datasets
	- Once the Built Permission is created all the activity of that permission will be tracked in the dataset lineage so the owner of the dataset can track where the dataset has been used to create dataset in the organization.

- Configure large dataset format [source](https://docs.microsoft.com/en-us/power-bi/admin/service-premium-large-models)
	- With Premium capacities, large datasets beyond the default limit can be enabled with the Large dataset storage format setting. 
	- When enabled, dataset size is limited by the Premium capacity size or the maximum size set by the administrator.
	- While required for datasets to grow beyond 10 GB, enabling the Large dataset storage format setting has additional benefits.
		- Like If you're planning to use XMLA endpoint based tools for dataset write operations
		- Then be sure to enable the setting, even for datasets that you wouldn't necessarily characterize as a large dataset
		- When enabled, the large dataset storage format can improve XMLA write operations performance.
		- This allow the use open source tools like [ALM toolkit]https://www.sqlbi.com/tools/alm-toolkit/, which is the same tool as [BISM Normalizer](https://www.sqlbi.com/tools/bism-normalizer/) for Analysis Services.
			- Such tools allow you to connect to the model in the service and make updates of only metadata schema in a very granular way and compare schema changes.
			- This allow to change your model without having to load the full model and data that you imported to PBI desktop.
	- TO enables
		- If your dataset will become larger and progressively consume more memory, be sure to configure Incremental refresh.
		- Publish the model as a dataset to the service.
		- In the service > dataset > Settings, expand Large dataset storage format, click the slider to On, and then click Apply.
		- Invoke a refresh to load historical data based on the incremental refresh policy. The first refresh could take a while to load the history. Subsequent refreshes should be faster, depending on your incremental refresh policy
		- Chris Wade [video](https://docs.microsoft.com/en-us/power-bi/admin/service-premium-large-models) about PBI premium plan to have same capability of Analysis Services 
	- Set default storage format
		- You can set the all dataset published to premium to be large dataset format
		- in the  Settings > Premium.
		- Default storage format, select Large dataset storage format, 

### Create and manage workspaces 

[Azure Docs](https://docs.microsoft.com/en-gb/learn/modules/create-manage-workspaces-power-bi/1-introduction)

Workspaces are created in Power BI Service where you can upload and share report to visualization consumers
When you create you can chose from classic or modern ( recommended)
As default once added user they are admin but can pick from roles
	- Admin (do everything)
	- Member (All that admin does except add remove users, delete workspace and update metadata)
	- Contributor (cannot publish apps or edit app unless given ability by members or admins). Can create and publish content, can schedule refreshes
	- Viewer (Read only)

There is a 1:1 relationship to Workspaces and App
When publish the member or admin can allow users to share, download data and make copies of report (tick box options)
If any changes made in the workspace they do not automatically get pushed to the app It needs to be updates 
A Workspace acts a staging area to to an app where it is the tools to the organization wide can consume reports

Monitor Usage Performance
- Usage metrics can only be accessed by Admin, Members or Contributors

Recommend a development life cycle strategy
- The life cycles can be managed by having a staging area a workspace and production are as productions

- Deployment Pipelines https://docs.microsoft.com/en-us/power-bi/create-reports/deployment-pipelines-best-practices
If in a premium capacity you can use the Deployment Pipeline which is a premium capacity feature
It manages content in different environments (Development, Test and Production)
It increases productivity, Allow Faster Delivery of content, Lower manual human intervention

Troubleshoot data by viewing its lineage
- Data lineage refers to the path that data takes from the data source to the destination.
- The Lineage view is only accessible to Admin, Contributor, and Member roles.
- Artifacts include data sources, datasets and dataflow, reports, and dashboards. 
- By using the Lineage view feature, you can go through the different datasets in one view and 
then use the Refresh data button to refresh datasets that you determine as stale.

Sensitivity labels (https://docs.microsoft.com/en-us/power-bi/admin/service-security-apply-data-sensitivity-labels)
- Sensitivity labels specify which data can be exported.

- Share with Free License users
	- Free License users can consume reports published when admins and Pro users assign workspaces to a capacity (in a Premium Subscription).
	- Then free users can consume content without requiring to have Pro licenses. Within those workspaces, free users have elevated permissions.

- All office 365 Global admins and Power Bi admins are automatically capacity admins of both Power Bi premium capacity and Power BI embedded capacity

- Configure PBI Subscription
	- [Documentation](https://docs.microsoft.com/en-us/power-bi/collaborate-share/service-report-subscribe)
	- The basic process for subscribing your colleagues and others to report pages, dashboards, and paginated reports is the same as [subscribing yourself](https://docs.microsoft.com/en-us/power-bi/consumer/end-user-subscribe).
	- By subscribing to report pages and dashboards Power BI will email a snapshot to your inbox.
	- You tell PBI how often (Daily, weekly etc) you want it or when the data is refreshed or a specific time
	- When you receive the email, it includes a link to "go to report or dashboard
	- Creating a subscription for yourself requires a Power BI Pro or Premium per user license. 
	- Admins control subscription activity
		- In the admin portal the admin can
			- Enable email subscriptions for all members of the organization.
			- Enable specific users to send email subscriptions to external users. See Invite external users to your organization.
		- Admins can audit subscription activity by analyzing the audit logs data


