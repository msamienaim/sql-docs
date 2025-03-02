---
title: "Azure SQL Connection Type | Microsoft Docs"
description: The Azure SQL Connection data extension supports multivalued parameters, server aggregates, and credentials managed separately from the connection string.
author: maggiesMSFT
ms.author: maggies
ms.service: reporting-services
ms.subservice: report-data
ms.topic: conceptual
ms.date: 02/15/2019
monikerRange: ">= sql-server-2016"
---

# Azure SQL Connection Type (SSRS)

[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] is a cloud-based, hosted relational database built on [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] technologies. To include data from [!INCLUDE[ssSDS](../../includes/sssds-md.md)] in your report, you must have a dataset that is based on a report data source of type [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. This built-in data source type is based on the [!INCLUDE[ssSDS](../../includes/sssds-md.md)] data extension. Use this data source type to connect to and retrieve data from [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
This data extension supports multivalued parameters, server aggregates, and credentials managed separately from the connection string.  
  
[!INCLUDE[ssSDS](../../includes/sssds-md.md)] is similar to an instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] on your premises and getting data from [!INCLUDE[ssSDS](../../includes/sssds-md.md)] is, with a few exceptions, identical to getting data from [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
> When opening a connection to a [!INCLUDE[ssSDS](../../includes/sssds-md.md)], set the connection timeout to 30 seconds.
  
For more information, see [Microsoft Azure SQL Database](/azure/sql-database/).  
  
Use the information in this topic to build a data source. For step-by-step instructions, see [Add and Verify a Data Connection &#40;Report Builder and SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
## <a name="Connection"></a> Connection String

When you connect to [!INCLUDE[ssSDS](../../includes/sssds-md.md)], you're connecting to a database object in the cloud. Just like onsite databases, the hosted database might have multiple schemas that have multiple tables, views, and stored procedures. You specify the database object to use in the query designer. If you don't specify a database in the connection string, you connect to the default database that the administrator assigned to you.  
  
Contact your database administrator for connection information and for the credentials to use to connect to the data source. The following connection string example specifies a hosted sample database named AdventureWorks.  
  
```
Data Source=<host>;Initial Catalog=AdventureWorks; Encrypt=True;  
```
  
Also, you use the **Data Sources Properties** dialog box to provide credentials such as user name and password. The `User Id` and `Password` options are automatically appended to the connection string; you don't need to type them as part of the connection string.  
  
For more information and connection string examples, see [Create data connection strings - Report Builder & SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
## <a name="Credentials"></a> Credentials

Windows Authentication (integrated security) isn't supported. If you attempt to connect to [!INCLUDE[ssSDS](../../includes/sssds-md.md)] using Windows Authentication an error occurs. [!INCLUDE[ssSDS](../../includes/sssds-md.md)] supports only SQL Server Authentication (user name and password) and users must provide credentials (login and password) every time they connect to [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
Credentials must be sufficient to access the database. Depending on your query, you might need other permissions, such as sufficient permissions to run stored procedures and access tables and views. The owner of the external data source must configure credentials that are sufficient to provide read-only access to the database objects that you need.  
  
From a report authoring client, the following options are available to specify credentials:  
  
- Use a stored user name and password. To negotiate the double hop that occurs when the database that contains the report data is different than the report server, select options to use credentials as Windows credentials. You can also choose to impersonate the authenticated user after connecting to the data source.  
  
- No credentials are required. To use this option, you must have the unattended execution account configured on the report server. For more information, see [Configure the Unattended Execution Account &#40;Report Server Configuration Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
For more information, see [Create data connection strings - Report Builder & SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) or [Specify Credential and Connection Information for Report Data Sources](specify-credential-and-connection-information-for-report-data-sources.md).  
  
## <a name="Query"></a> Queries

A query specifies which data to retrieve for a report dataset. The columns in the result set for a query populate the field collection for a dataset. If the query returns multiple result sets, the report processes only the first result set that the query retrieves. Although there are some differences between [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] and [!INCLUDE[ssSDS](../../includes/sssds-md.md)]s such as the sizes of databases supported, writing queries against [!INCLUDE[ssSDS](../../includes/sssds-md.md)]s is similar to writing queries against [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] databases. Some [!INCLUDE[tsql](../../includes/tsql-md.md)] statements such as BACKUP aren't supported in [!INCLUDE[ssSDS](../../includes/sssds-md.md)], but they aren't ones that you use in report queries. For more information, see [SQL Server Connection Type &#40;SSRS&#41;](../../reporting-services/report-data/sql-server-connection-type-ssrs.md).  
  
By default, if you create a new query or open an existing query that can be represented in the graphical query designer, the relational query designer is available. You can specify a query in the following ways:  
  
- Build a query interactively. Use the relational query designer that displays a hierarchical view of tables, views, stored procedures, and other database items, organized by the database schema. Select columns from tables or views, or specify stored procedures or table-valued functions. Limit the number of rows of data to retrieve by specifying filter criteria. Customize the filter when the report runs by setting the parameter option.  
  
- Type or paste a query. Use the text-based query designer to enter [!INCLUDE[tsql](../../includes/tsql-md.md)] text directly, to paste query text from another source, to enter complex queries that cannot be built by using the relational query designer or to enter query-based expressions.  
  
- Import an existing query from a file or report. Use the **Import** query button from either query designer to browse to a .sql file or .rdl file and import a query.  
  
The text-based query designer supports the following two modes:  
  
- [Text](#QueryText) Type [!INCLUDE[tsql](../../includes/tsql-md.md)] commands that select data from the data source.  
  
- [Stored Procedure](#QueryStoredProcedure) Choose from a list of stored procedures.  
  
For more information, see [Relational Query Designer User Interface &#40;Report Builder&#41;](../../reporting-services/report-data/relational-query-designer-user-interface-report-builder.md) and [Text-based Query Designer User Interface &#40;Report Builder&#41;](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
The graphical query designer that [!INCLUDE[ssSDS](../../includes/sssds-md.md)] uses provides built-in support for grouping and aggregates to help you write queries that retrieve only summary data. The [!INCLUDE[tsql](../../includes/tsql-md.md)] language features are: the GROUP BY clause, DISTINCT keyword, and aggregates such as SUM and COUNT. The text-based query designer provides full support for the [!INCLUDE[tsql](../../includes/tsql-md.md)] language, including grouping and aggregates. For more information about [!INCLUDE[tsql](../../includes/tsql-md.md)], see [Transact-SQL Reference &#40;Database Engine&#41;](../../t-sql/language-reference.md).  
  
### <a name="QueryText"></a> Using Query Type Text

In the text-based query designer, you type [!INCLUDE[tsql](../../includes/tsql-md.md)] commands to define the data in a dataset. For example, the following [!INCLUDE[tsql](../../includes/tsql-md.md)] query selects the names of all employees who are marketing assistants:

```sql
SELECT  
  HumanResources.Employee.BusinessEntityID  
  ,HumanResources.Employee.JobTitle  
  ,Person.Person.FirstName  
  ,Person.Person.LastName  
FROM  
  Person.Person  
  INNER JOIN HumanResources.Employee  
    ON Person.Person.BusinessEntityID = HumanResources.Employee.BusinessEntityID  
WHERE HumanResources.Employee.JobTitle = 'Marketing Assistant'   
```

Click the **Run** button (**!**) on the toolbar to run the query and display a result set.  
  
To parameterize this query, add a query parameter. For example, change the WHERE clause to the following:  

```sql
WHERE HumanResources.Employee.JobTitle = (@JobTitle)  
```

When you run the query, report parameters that correspond to query parameters are automatically created. For more information, see [Query Parameters](#Parameters) later in this topic.  
  
### <a name="QueryStoredProcedure"></a> Using Query Type StoredProcedure

You can specify a stored procedure for a dataset query in one of the following ways:  
  
- In the **Dataset Properties** dialog box, set the **Stored Procedure** option. Choose from the drop-down list of stored procedures and table-valued functions.  
  
- In the relational query designer, in the Database view pane, select a stored procedure or table-valued function.  
  
- In the text-based query designer, select **StoredProcedure** from the toolbar.  
  
After you select a stored procedure or table-valued function, you can run the query. You then are prompted for input parameter values. When you run the query, report parameters that correspond to input parameters are automatically created. For more information, see [Query Parameters](#Parameters) later in this topic.  
  
Only the first result set that is retrieved for a stored procedure is supported. If a stored procedure returns multiple result sets, only the first one is used.  
  
If a stored procedure has a parameter that has a default value, you can access that value by using the DEFAULT keyword as a value for the parameter. If the query parameter is linked to a report parameter, the user can type or select the word DEFAULT in the input box for the report parameter.  
  
For more information about stored procedures, see [Stored Procedures (Database Engine)](../../relational-databases/stored-procedures/stored-procedures-database-engine.md).  
  
## <a name="Parameters"></a> Parameters

When query text contains query variables or stored procedures that have input parameters, the corresponding query parameters for the dataset and report parameters for the report are automatically generated. The query text must not include the DECLARE statement for each query variable.  
  
 For example, the following SQL query creates a report parameter named **EmpID**:  

```sql
SELECT FirstName, LastName FROM HumanResources.Employee E INNER JOIN  
       Person.Contact C ON  E.ContactID=C.ContactID   
WHERE EmployeeID = (@EmpID)  
```

By default, each report parameter has data type Text and an automatically created dataset to provide a drop-down list of available values. After the report parameters are created, you might have to change default values. For more information, see [Report Parameters &#40;Report Builder and Report Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  

## <a name="Remarks"></a> Remarks
  
###### Alternate Data Extensions

You can also retrieve data from a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database by using an ODBC data source type. Connecting to [!INCLUDE[ssSDS](../../includes/sssds-md.md)] by using OLE DB isn't supported.  
  
For more information, see [ODBC Connection Type &#40;SSRS&#41;](../../reporting-services/report-data/odbc-connection-type-ssrs.md).  
  
###### Platform and Version Information

For more information about platform and version support, see [Data Sources Supported by Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  

::: moniker range=">=sql-server-2016"

## Azure SQL Database and AAD

You can use the Azure SQL database with Azure Active Directory (AAD).

This scenario is supported when you set up the following items properly:

- Active Directory Authentication Library for SQL Server (ADALSQL) is installed on the report server.
- [Active Directory Federation Services (ADFS)](/windows-server/identity/active-directory-federation-services) is configured to federate across on-premises Active Directory (AD) and AAD.
- [Kerberos Constrained Delegation (KCD)](/windows-server/security/kerberos/kerberos-constrained-delegation-overview) is configured from the report server to the ADFS server.
- Configure the report/data source to authenticate to [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) as the user viewing the report.

::: moniker-end

## <a name="HowTo"></a> How-To Topics

This section contains step-by-step instructions for working with data connections, data sources, and datasets.  
  
[Add and Verify a Data Connection &#40;Report Builder and SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
[Create a Shared Dataset or Embedded Dataset &#40;Report Builder and SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
[Add a Filter to a Dataset &#40;Report Builder and SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
## <a name="Related"></a> Related Sections

These sections of the documentation provide in-depth conceptual information about report data, and procedural information about how to define, customize, and use parts of a report that are related to data.  
  
[Report Datasets &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
Provides an overview of accessing data for your report.  
  
[Create data connection strings - Report Builder & SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
Provides information about data connections and data sources.  
  
[Report Embedded Datasets and Shared Datasets &#40;Report Builder and SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
Provides information about embedded and shared datasets.  
  
[Dataset Fields Collection &#40;Report Builder and SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
Provides information about the dataset field collection generated by the query.  
  
[Data Sources Supported by Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
Provides in-depth information about platform and version support for each data extension.  
  
## See Also

[Microsoft Azure SQL Database](/azure/sql-database/)  
[Report Parameters &#40;Report Builder and Report Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
[Filter, Group, and Sort Data &#40;Report Builder and SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
[Expressions &#40;Report Builder and SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)
