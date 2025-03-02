---
title: "Example: Specifying the HIDE Directive"
description: View an example of using the HIDE directive in an SQL query.
ms.custom: ""
ms.date: 05/05/2022
ms.service: sql
ms.reviewer: randolphwest
ms.subservice: xml
ms.topic: conceptual
helpviewer_keywords:
  - "HIDE directive"
author: MikeRayMSFT
ms.author: mikeray
---
# Example: Specify the HIDE directive

[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

This example illustrates the use of the **HIDE** directive. This directive is useful when you want the query to return an attribute for ordering the rows in the universal table that is returned by the query, but you don't want that attribute in the final resulting XML document.

This query constructs this XML:

```xml
<ProductModel ProdModelID="19" Name="Mountain-100">
  <Summary>
    <SummaryDescription>
           <Summary> element from XML stored in CatalogDescription column
    </SummaryDescription>
  </Summary>
</ProductModel>
```

This query generates the XML you want. The query identifies two column groups having 1 and 2 as Tag values in the column names.

This query uses the [query() Method (xml Data Type)](../../t-sql/xml/query-method-xml-data-type.md) of the **xml** data type to query the CatalogDescription column of **xml** type in order to retrieve the summary description. The query also uses the [value() Method (xml Data Type)](../../t-sql/xml/value-method-xml-data-type.md) of the **xml** data type to retrieve the ProductModelID value from the CatalogDescription column. This value isn't required in the resulting XML, but is required to sort the resulting rowset. Therefore, the column name, `[Summary!2!ProductModelID!HIDE]`, includes the **HIDE** directive. If this column isn't included in the SELECT statement, you'll have to sort the rowset by `[ProductModel!1!ProdModelID]` and `[Summary!2!SummaryDescription]` that is **xml** type and you can't use the **xml** type column in ORDER BY. Therefore, the extra `[Summary!2!ProductModelID!HIDE]` column is added and is then specified in the ORDER BY clause.

```sql
USE AdventureWorks2012;
GO
SELECT  1 as Tag,
        0 as Parent,
        ProductModelID     as [ProductModel!1!ProdModelID],
        Name               as [ProductModel!1!Name],
        NULL               as [Summary!2!ProductModelID!hide],
        NULL               as [Summary!2!SummaryDescription]
FROM    Production.ProductModel
WHERE   CatalogDescription is not null
UNION ALL
SELECT  2 as Tag,
        1 as Parent,
        ProductModelID,
        Name,
        CatalogDescription.value('
         declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";
       (/PD:ProductDescription/@ProductModelID)[1]', 'int'),
        CatalogDescription.query('
         declare namespace pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";
         /pd:ProductDescription/pd:Summary')
FROM    Production.ProductModel
WHERE   CatalogDescription is not null
ORDER BY [ProductModel!1!ProdModelID],[Summary!2!ProductModelID!hide]
FOR XML EXPLICIT;
GO
```

This is the result:

```xml
<ProductModel ProdModelID="19" Name="Mountain-100">
  <Summary>
    <SummaryDescription>
      <pd:Summary xmlns:pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription" xmlns="">
        <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">Our top-of-the-line competition mountain bike. Performance-enhancing options include the innovative HL Frame, super-smooth front suspension, and traction for all terrain. </p1:p>
      </pd:Summary>
    </SummaryDescription>
  </Summary>
</ProductModel>
```

## See also

- [Use EXPLICIT Mode with FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)
