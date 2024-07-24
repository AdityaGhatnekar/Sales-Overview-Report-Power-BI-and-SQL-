## Sales Overview

### Business Task

The business request for this data analyst project was an executive sales report for sales team. Below are variables for the project: 

Operating Costs: Understand and recognise the total various costs involved in running the business.

Best-Selling Products: Identify and highlight the products that generate the most revenue.

Country-wise Sales Performance: Analyze and compare sales data across different countries.

Net Profit: Calculate and display the net profit, taking into account revenue and expenses.

Budget: Monitor and compare the actual sales against the budget.

Top Customers: Manage and understand top customers for the team to keep following up.

Customer Demographics: Understanding top demographics of customers to help team approach similar demographics.

### Data Cleaning and Transformation

To create data model neccessary tables were cleaned and transformed with SQL. The 'Sales Budget' table was created on excel and connected later on Power BI on a later step.

Below are SQL queries for data cleaning and transforming.

#### Date Table

```--Data Cleaning for Date Table
SELECT [DateKey]
      ,[FullDateAlternateKey] AS Date
      --,[DayNumberofWeek]
      ,[EnglishDayNameOfWeek] AS Day
      --,[SpanishDayNameOfWeek]
      --,[FrenchDayNameOfWeek]
      --,[DayNumberOfMonth]
      --,[DayNumberOfYear]
      --,[WeekNumberOfYear]
      ,[EnglishMonthName] AS MonthName
	  ,LEFT ([EnglishMonthName], 3) AS MonthShort
      --,[SpanishMonthName]
      --,[FrenchMonthName]
      ,[MonthNumberOfYear] AS MonthNumber
      ,[CalendarQuarter] AS Quater
      ,[CalendarYear] AS Year
      --,[CalendarSemester]
      --,[FiscalQuarter]
      --,[FiscalYear]
      --,[FiscalSemester]
  FROM [AdventureWorksDW2022].[dbo].[DimDate]
  WHERE [CalendarYear] >= 2021
```

#### Customer Table

```-- Data Cleaning for Customer Table
SELECT 
       [CustomerKey]
      --,[GeographyKey]
      --,[CustomerAlternateKey]
      --,[Title]
      ,[FirstName]
      --,[MiddleName]
      ,[LastName]
	  ,CONCAT ([FirstName], ' ', [LastName]) AS FullName
      --,[NameStyle]
      ,[BirthDate]
      --,[MaritalStatus]
      --,[Suffix]
      ,CASE [Gender] WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender
      --,[EmailAddress]
      --,[YearlyIncome]
      --,[TotalChildren]
      --,[NumberChildrenAtHome]
      --,[EnglishEducation]
      --,[SpanishEducation]
      --,[FrenchEducation]
      --,[EnglishOccupation]
      --,[SpanishOccupation]
      --,[FrenchOccupation]
      --,[HouseOwnerFlag]
      --,[NumberCarsOwned]
      --,[AddressLine1]
      --,[AddressLine2]
      --,[Phone]
      ,[DateFirstPurchase]
      --,[CommuteDistance]
	  ,[DimGeography].[City]		-- Joined City from Dim.Geography Table
	  ,[DimGeography].[EnglishCountryRegionName] AS Country		-- Joined Country from Dim.Geography Table
  FROM 
	  [AdventureWorksDW2022].[dbo].[DimCustomer]
  LEFT JOIN 
      [AdventureWorksDW2022].[dbo].[DimGeography] ON [DimCustomer].[GeographyKey] = [DimGeography].[GeographyKey]
  WHERE
      [DateFirstPurchase] >= '2021-01-01'
```

#### Products Table

```-- Data Cleaning for Products Table
SELECT 
	 A.[ProductKey]
	,A.[ProductAlternateKey] AS ItemCode
--	,[ProductSubcategoryKey]
--	,[WeightUnitMeasureCode]
--	,[SizeUnitMeasureCode]
	,A.[EnglishProductName] AS ProductName
	,C.[EnglishProductSubcategoryName] AS ProductSubCategory
 	,B.[EnglishProductCategoryName] AS ProductCategory
--	,[SpanishProductName]
--	,[FrenchProductName]
--	,A.[StandardCost]
--	,[FinishedGoodsFlag]
	,A.[Color]
--	,[SafetyStockLevel]
--	,A.[ReorderPoint]
--	,[ListPrice]
--	,[Size]
--	,[SizeRange]
--	,[Weight]
--	,[DaysToManufacture]
--	,[ProductLine]
--	,[DealerPrice]
--	,[Class]
--	,[Style]
	,A.[ModelName]
--	,[LargePhoto]
	,A.[EnglishDescription] AS Description
--	,[FrenchDescription]
--	,[ChineseDescription]
--	,[ArabicDescription]
--	,[HebrewDescription]
--	,[ThaiDescription]
--	,[GermanDescription]
--	,[JapaneseDescription]
--	,[TurkishDescription]
	,Cast ([StartDate] AS Date) AS StartDate
	,Cast ([EndDate] AS Date) AS EndDate
	,ISNULL ([Status], 'Terminate') AS Status
FROM
	[AdventureWorksDW2022].[dbo].[DimProduct] AS A
LEFT JOIN
	[AdventureWorksDW2022].[dbo].[DimProductSubCategory] AS C ON C.[ProductSubCategoryKey] = A.[ProductSubcategoryKey]
LEFT JOIN
	[AdventureWorksDW2022].[dbo].[DimProductCategory] AS B ON C.[ProductCategoryKey] = B.[ProductCategoryKey]
```

### Sales Table (Fact Table)

```-- Data Cleaning For Sales Table
SELECT [ProductKey]
      ,[OrderDateKey]
      ,[DueDateKey]
      ,[ShipDateKey]
      ,[CustomerKey]
      --,[PromotionKey]
      ,[CurrencyKey]
      ,[SalesTerritoryKey]
      ,[SalesOrderNumber]
      --,[SalesOrderLineNumber]
      --,[RevisionNumber]
      ,[OrderQuantity]
      ,[UnitPrice]
      --,[ExtendedAmount]
      --,[UnitPriceDiscountPct]
      --,[DiscountAmount]
      --,[ProductStandardCost]
      ,[TotalProductCost] AS ProductCost
      ,[SalesAmount]
      ,[TaxAmt]
      ,[Freight]
      --,[CarrierTrackingNumber]
      --,[CustomerPONumber]
      ,CAST ([OrderDate] AS Date) AS OrderDate
      ,CAST ([DueDate] AS Date) AS DueDate
      ,CAST ([ShipDate] AS Date) AS ShipDate
  FROM [AdventureWorksDW2022].[dbo].[FactInternetSales]
WHERE LEFT ([OrderDateKey], 4) >= 2021
```

### Data Model

Below is the screenshot how Sales fact table connects with other dimension tables defining relationship with other tables.

![Data Model](https://github.com/user-attachments/assets/cbc71b10-0590-4968-b19e-c2e07aa139eb)

![Data Model Screenshot  1](https://github.com/user-attachments/assets/b53b456f-9266-4d4f-90d7-9acef1dcf3e0)

### Sales Report

To further browse the report click the picture and try it out.

[![Project Screenshot](https://github.com/user-attachments/assets/df36f0dd-89a7-4278-a8e4-2118c9ec35b0)](<iframe title="Sales and Customers Overview" width="600" height="373.5" src="https://app.fabric.microsoft.com/view?r=eyJrIjoiMWZkMmVkY2QtMmMzOC00NDg0LTg5ZGYtNTRkMzQ1MjhiN2ZiIiwidCI6IjgyYTIxMTg1LTQyMGUtNDFlOS05OGQ5LTM4YzQ2YzY1ZWZjYyJ9&embedImagePlaceholder=true&pageName=76e7b7e7f32d7b61f7e1" frameborder="0" allowFullScreen="true"></iframe>)


[![Project Screenshot 1](https://github.com/user-attachments/assets/d7ee3f97-df9f-4e44-8869-3a3b10a5a306)](!https://app.fabric.microsoft.com/view?r=eyJrIjoiMWZkMmVkY2QtMmMzOC00NDg0LTg5ZGYtNTRkMzQ1MjhiN2ZiIiwidCI6IjgyYTIxMTg1LTQyMGUtNDFlOS05OGQ5LTM4YzQ2YzY1ZWZjYyJ9&embedImagePlaceholder=true&pageName=76e7b7e7f32d7b61f7e1!)

 



