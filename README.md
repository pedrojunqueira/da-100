
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
	- Sharepoint - Team Sites. Works  similar to OneDrive business but connects to the URL or Root Folder

- Importing data options
	- Load the data: straight into PBI desktop
	- Transform the data. Take you to Power Query
	
- Connecting to database offer options
	- Windows credentials
	- Database credentials
	- Microsoft Account
- To use a query statement to import data use the advanced options

- To change data source settings can be done from the PBI Menu (Home>Query>Transform Data> Data Source Settting
		or from within the PowerQuery (Data Source >Data Source Settting)

- Using SQL statements. Do not use the * (wildcard) as you may be importing redundant data
- Only import the data that you need for the report
- Better tham creating a SQL statement is to create a view in the DB and let Power query do the query folding

- Connecting to a CosmoDB (Azure)
- Select connection (Others)
- If you are connecting to an endpoint for the first time, as you are in this example, make sure that you enter your account key.

- Importing Json
- When importing Json the file needs to be formated in Power Query to a tabular format before go to PBI desktop
- A Record in a document database need to use the "Expander" then data will be in a tabular format ready to be imported for modelling

- Selecting a storage mode. 
[Azure Docs](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-storage-mode)

- Import : Data loaded and resides in the PBI file (.pbix). Data refresh as manually done.
- DirectQuery: allows you to query the data in the data source directly and not import a copy into Power BI. Always get the latest data
- Dual: 

- Mode Change: Is possible to change import to Direct Query but not from Direct Query to Import. 
- If a import mode is changed to Query mode this is ireversible

Analysis Services vs. SQL Server

Similarities

- Authenticate to the server.
- Pick the cube you want to use.
- Select which tables you need.

Diferences

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


- Query folding. 
	- [Azure Docs](https://docs.microsoft.com/en-us/power-bi/guidance/power-query-folding)
	- [Azure Docs](https://docs.microsoft.com/en-us/power-query/power-query-folding)
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
	- Promote headers can use the promote hearder in the Home Ribbon session or click the table icon next to first column (use first row as header)
	- Can remove top N rows
	- Can delete columns by selecting the ones you need or you can select the ones you want to keep then delete the others
	- You can unpivot columns by selecting the columns to unpivot and the column names will be atributes and the numeric value will be value
	- Pivot the columns is the oposite of unpivot and used to sumirize and agregate data

- Improving data
	- Rename columns to more meaningful names
	- Replace values to meaninful and fix data issues, spelling or errors. Replace nulls with 0 (zeros)
	- Remove duplicates

- Columns name conventions
	- Replace undescore with space
	- Remove prefixes and sufixes
	
- Evaluate and change data types
	- Can be changed in power query and in power bi desktop reporting view by using columns tools
	- Data types in [PBI]https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-data-types
	
- Combine multiple tables
	- Merging: Need to have a columns to join
		- Can use left, inner and full outer
	- Appending: Need to have same number of columns and types

### Shape and transform data 

	- Profile the data
	- Distribution: HIstogram of count of distinct and unique values
	- Quality: check empty, validity % and errors
	- Profile: Column in depth statistics

- Use editor to modify M code
		- Click in Power Query - Advanced Editor 


- Use Parameters
	- In power query click Manage Parameters in the Home Ribbon
	- This allow to load different data in the report depending on the parameter
	- Steps
		- Write a SQL query with a parameter &parameter
		- Needs to be type text and configured in the M code outside the query string: https://docs.microsoft.com/en-gb/learn/modules/manage-datasets-power-bi/2-report-parameters

```python 
TODO
 Microsoft Dataverse
 use or create a PBIDS file
 use or create a data flow
 connect to a dataset using the XMLA endpoint
```

### Clean, transform, and load the data

```python 
TODO
 resolve inconsistencies, unexpected or null values, and data quality issues
 apply user-friendly value replacements
 identify and create appropriate keys for joins
 evaluate and transform column data types
 apply data shape transformations to table structures
 combine queries
 apply user-friendly naming conventions to columns and queries
 leverage Advanced Editor to modify Power Query M code
 configure data loading
 resolve data import errors
```

## Model the Data (25-30%)

### Design a data model

```python 
TODO
 define the tables
 configure table and column properties
 define quick measures
 flatten out a parent-child hierarchy
 define role-playing dimensions
 define a relationship's cardinality and cross-filter direction
 design the data model to meet performance requirements
 resolve many-to-many relationships
 create a common date table
 define the appropriate level of data granularity
```

### Develop a data model

```python 
TODO
 apply cross-filter direction and security filtering
 create calculated tables
 create hierarchies
 create calculated columns
 implement row-level security roles
 set up the Q&A feature
```

### Create measures by using DAX

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

```python 
TODO
 remove unnecessary rows and columns
 identify poorly performing measures, relationships, and visuals
 improve cardinality levels by changing data types
 improve cardinality levels through summarization
 create and manage aggregations
```

## Visualize the Data (20-25%)

### Create reports 

[Azure Docs](https://docs.microsoft.com/en-gb/learn/modules/visuals-power-bi/)

```python 
TODO
 add visualization items to reports
 choose an appropriate visualization type
 format and configure visualizations
 import a custom visual
 apply slicing and filtering
 add an R or Python visual
 configure the report page
```

- Accessibility Configuration				
	- Alt text: To accommodate report consumers who use screen readers
		To add alt text to an object, select that object and, in the Visualizations pane, open the Format pane
	- Tab order: help keyboard users navigate your report in an order that matches the way that visual users would see it
	- Titles and labels:  provide clear, concise, descriptive titles
	- Markers : Avoid. Can bring distraction
	- Themes : Use colors with contrast


```python 
TODO
 create a paginated report  - 
```

[Azure Docs](https://docs.microsoft.com/en-gb/learn/modules/create-paginated-reports-power-bi/)

### Create dashboards 

[Azure Docs](https://docs.microsoft.com/en-gb/learn/modules/create-dashboards-power-bi/)

- Dashboards can pin visuals from various reports and dashboards
- Dashboards can contain visuals from different datasrts
- Dashboards are only available in the Power BI service
- Dashboards do not have filters like reports do
- Dashboards can only be a single page
- Source for tiles
	- Dashboards
	- Reports 
	- Datasets
	- SSRS
	- Excel

- Dashboard alerts give you alett if a visual data reach a certain threshold and it can ONLY be applied to the following visuals
	- KPI
	- Cards
	- Gauge
- Alerts can be sent to the notification center or email.
- More infor about alerts (https://docs.microsoft.com/en-us/power-bi/create-reports/service-set-data-alerts)

- Alerts can be set on visuals created from streaming datasets that you pin from a report to a dashboard.
- Alerts can't be set on streaming tiles created directly on the dashboard using Add tile > Custom streaming data.

-Dashboard has Q&A feature where you can ask questions
-Q&A uses natural languag processing

- It is possible to pin an entire report page to a dashboard 

[Azure Docs](https://docs.microsoft.com/en-us/power-bi/create-reports/service-dashboard-pin-live-tile-from-report)

- You can stream data to power bi connectiong from
[Azure Docs](https://docs.microsoft.com/en-us/power-bi/connect-data/service-real-time-streaming)
	- API
	- Azure Stream Analytics
	- PubNub
	
There are 3 types of streaming dataset
- Push
- Streaming
- PubNub

- Can set up a theme by clickinh in edit. 
- Can also upload or download a Json Theme to be applied to the dashboard

- Classification of dashboards on `...` (elipsis) settings options 
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
		- Consider the number of visuals and clutering
			 - Can use drill through or tooltips
		- Enven distribute and aligh titles and visuals. Position is important
		- Configure iterations if want to cross highlight or filter or no interactions in some cases (depend on user preference)
		- Consider hierachy on visuals for beter data navigatino and drill down experience
		- Consider accessbility like tab order and color for color blindness
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
			- Back : Take back previous page. Used well with drill trhough
			- Bookmark: apply a bookmark
			- Drill through: take to a drill trhough of the current filter
			- Page navigation : Navigate to a specific page
			- Q&A : Open a search window
			- Web URL: Take to a URL
	- Selections: Allow you to determine what items in the report are visible and what items are hidden.
	
- Use basic interactions
	- On Format click on Edit Iteraction
		- There are 3 options
			- None: when iteracting nothing will happen. Visual stay the same
			- cross-filtered: Visual will filter what of selected in the other visual when iteracting
			- cross-highlight: Visual will stay the same and highligh what was filtered proportionaly
	- hierarchies
		- When add hierarchy in a visual you will have the ability to drill down

NOTE:
Keep in mind that the number of interactions between your visuals will impact the performance 
of your report. To optimize the performance of your report,
consider the query reduction options that are available within Power BI Desktop. 
You have the option to send fewer queries . This is configured in options > query reduction
	
```python 
TODO
 create custom tooltips
```

## Analyze the Data (10-15%)

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

- Power Bi visuals allow you to (click elipsis)
	- add a commend
	- export data (table or underlying data: depends on sys adm permissions and settings)
	- Visual table (visualize table of charch data)
	- Spotlight (show just selected chart and hiding all other elements
	- Sort (Sort by date or category depending on what is selected on axis)

- Drill through
	- Create a new report page (tab) and add a drill trhough filter
	- Automatically all visuals with the drill through filter will then take the filter and pass to drill through page (detail)
	- To access drill trhough right click the visuals
	
- Conditional formatting
	- Conditional formatting apply to fields and measureas
	- Click on down arrow in the visual value and select option to condition format
	
- Apply slicing, filtering, and sorting
	- Filter only work in reports NOT Dashboard`
	- Slicers are added in report so end users can filter the report
	- Putting slicer next to visuals make user experience better to filter visuals
	- Slicer does not support input fields or drillthrough features
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
	- Results display in miliseconds
	- Reasons for bad performance
		- too many visuals
		- High cardinality
		- High Granularity
	- Make sure if glanularity and only necessary data is loaded so this can imporove performance
	
- Optimize for mobile use
	- Any report is already possible to access via mobile
	- However it is possible to customize and optimize layout for mobile
	
- Sync Slicer
	- It is possible that a filter slicer apply across multiple report pages.
	- You have the option to sync and also show the slicer
	- A slicer does not need to show to sync in a page
	- Configure in View ribon in Report View
	
```python 
TODO
 perform top N analysis
 explore statistical summary
 use the Q&A visual
 add a Quick Insights result to a report
 create reference lines by using Analytics pane
 use the Play Axis feature of a visualization
```

### Perform advanced analysis

```python 
TODO
 identify outliers
 conduct Time Series analysis
 use groupings and binnings
 use the Key Influencers to explore dimensional variances
 use the decomposition tree visual to break down a measure
 apply AI Insights
```


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
		- Needs to be type text and configured in the M code outside the query string: https://docs.microsoft.com/en-gb/learn/modules/manage-datasets-power-bi/2-report-parameters

- What if scenario
	- Create a New Parameter in report view (modelling)
	- Define the min max boundaries and step and add as a sliced
	- Add this to a measure and then do what if analysis with it
	
	
- Configure On-Premissies Gateway - https://docs.microsoft.com/en-us/data-integration/gateway/service-gateway-app
	- The on premisses gateway can be in two modes
		- Personal : Installed in the local machine and used by only 1 user
		- Organisational: Installed in the same server where the data source is and can be used to multiple users in the organisation
	- The gateway is a mean of conecting the power bi service (cloud) to on premisses sources
	- It allow save traffic of credentials over the internet
	- It allows for schedulled refresh of the power bi service to get data from on premisses sources
	
	
- Configure Schedule refresh
	-  Scheduled refresh feature in Power BI allows you to define the frequency and time slots to refresh a particular dataset
	- Saves time as you do not need to do it manually
	- Need to create a gateway connection first
	- Can refresh 48 times a day on premium and 8 times on shared capacity
	- It is possible to refresh manually as many time as you want and that does not affect the scheduled refresh
	- After four (4) consecutive failures the sheduled refresh deativate
	
- Configure incremental refresh settings - https://docs.microsoft.com/en-us/power-bi/admin/service-premium-incremental-refresh
	-  allows you to refresh large datasets quickly and as often as you need, without having to reload historical data each time.
	- Incremental refresh should only be used on data sources and queries that support query folding. (e.g. SQL DB)
	- RangeStart and RangeEnd values must be the name of the parameters
	-  Type is set to Date/Time and the Suggested Value is set to Any value.
	- In power query select the column to filter (date column) and right click and pick option 
			custom filter https://docs.microsoft.com/en-gb/learn/modules/manage-datasets-power-bi/6-incremental-refresh
	- In power BI right click in the table and select incremental refresh
	
- Manage and promote datasets
	- You want governance in your data and
		direct your users to the most up-to-date and highest-quality datasets 
		in your workspaces, or you might want to restrict the reuse of datasets across your workspaces.
	- This can be done by endorsing datasets
	- Power BI provides two ways to endorse your datasets
		- Promotion: Means data is ready for use and any member of the worspace can do it
		- Certification: Certify a promoted dataset as "source of truth" and only can be done by Adm role
		
- Troubleshoot service connectivity - https://docs.microsoft.com/en-us/power-bi/connect-data/refresh-troubleshooting-refresh-scenarios
	- Cloud sources (Azure) does not require gateways
	- Aways make sure credentials are upto date. User service accoutns where password does not expire

- Query caching (Prmium Feature) - https://docs.microsoft.com/en-us/power-bi/connect-data/power-bi-query-caching
	- With the Query Caching feature, you can use the local caching services of Power BI to process
			query results. Instead of relying on the dataset to calculate queries
	- Query Caching is a local caching feature that maintains results on a user and report basis. 
			This service is only available to users with *Power BI Premium or *Power BI Embedded.
	- To configure: Go to a dataset in your workspace and open its Settings page. 
				

```python 
TODO
  configure row-level security group membership 
  providing access to datasets 
  configure large dataset format
 ```

### Create and manage workspaces 

[Azure Docs](https://docs.microsoft.com/en-gb/learn/modules/create-manage-workspaces-power-bi/1-introduction)

Workspaces are created in Power BI Service where you can upload and share report to visualisation consumers
When you create you can chose from classic or modern ( recommended)
As defauld once added user they are admin but can pick from roles
	- Admin (do everything)
	- Member (All that admin does except add remove users, delete workspace and update metadata)
	- Contributor (cannot publish apps or edit app unless given ability by members or admins). Can create and publish content, can shedule refreshes
	- Viewer (Read only)

There is a 1:1 relationship to Workspaces and App
When publish the member or admin can allow users to share, download data and make copies of report (tick box options)
If any changes made in the workspace they do not automatically get pushed to the app It needs to be updates 
A Workspace acts a staging area to to an app where it is the tools to the organisationwide can consume reports

Monitor Usage Performance
- Usage metrics can only be accessed by Admin, Members or Contributors

Recommend a development life cycle strategy
- The life cycles can be managed by having a staging area a workspace and production are as productions

- Deployment Pipelines https://docs.microsoft.com/en-us/power-bi/create-reports/deployment-pipelines-best-practices
If in a premium capacity you can use the Deployment Pipeline which is a premium capacity feature
It manages content in diferent environements (Development, Test and Production)
It increases productivity, Allow Faster Delivery of content, Lower manual human intervention

Troubleshoot data by viewing its lineage
- Data lineage refers to the path that data takes from the data source to the destination.
- The Lineage view is only accessible to Admin, Contributor, and Member roles.
- Artifacts include data sources, datasets and dataflows, reports, and dashboards. 
- By using the Lineage view feature, you can go through the different datasets in one view and 
then use the Refresh data button to refresh datasets that you determine as stale.

Sensitivity labels (https://docs.microsoft.com/en-us/power-bi/admin/service-security-apply-data-sensitivity-labels)
- Sensitivity labels specify which data can be exported.


```python 
TODO
 configure subscriptions
```


