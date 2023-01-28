# Metadata & iFrames

<center>
<iframe src="https://regionh-my.sharepoint.com/personal/nicolai_schmidt_01_regionh_dk1/_layouts/15/Doc.aspx?sourcedoc={e61ce2ee-cd72-4689-813d-6beefb41a13d}&amp;action=embedview&amp;wdAr=1.7777777777777777&amp;wdEaaCheck=0&wdAllowInteractivity=FALSE" width="476" height="288" frameborder="0"></iframe>
</center>
	
	
## Tabeller

<center>
<iframe width="100%" height="700" frameborder="0" scrolling="no" src="https://regionh-my.sharepoint.com/personal/nicolai_schmidt_01_regionh_dk1/_layouts/15/Doc.aspx?sourcedoc={c7c4140c-dc3a-4d83-955c-b6ae4c7ba5db}&action=embedview&wdAllowInteractivity=FALSE&Item=tbl_tabeller&&wdHideGridlines=TRUE&wdHideHeaders=TRUE&wdInConfigurator=TRUE&wdInConfigurator=TRUE&edesNext=TRUE&edrtees6=FALSE&resen=FALSE&ed1JS=FALSE&wdHideSheetTabs=TRUE&ActiveCell=A1000"></iframe>
</center>
<br>


## Measures
<center>
<iframe width="100%" height="700" frameborder="0" scrolling="no" src="https://regionh-my.sharepoint.com/personal/nicolai_schmidt_01_regionh_dk1/_layouts/15/Doc.aspx?sourcedoc={c7c4140c-dc3a-4d83-955c-b6ae4c7ba5db}&action=embedview&wdAllowInteractivity=FALSE&wdHideGridlines=TRUE&wdHideHeaders=TRUE&wdInConfigurator=TRUE&wdInConfigurator=TRUE&edesNext=TRUE&edrtees6=FALSE&resen=FALSE&ed1JS=FALSE&wdHideSheetTabs=TRUE&Item=tbl_measures&ActiveCell=A1000"></iframe>
</center>
<br>


## Kode til træk af metadata

<!-- TABELLER -->
{::options parse_block_html="true" /}
<details><summary markdown="span">Tabeller: SQL-query til træk af metadata om tabeller i SSMS</summary>
```sql
USE [Flis2_LønHR_v2];

SELECT
   col.TABLE_SCHEMA AS 'Skema'
   ,col.TABLE_NAME AS 'Tabel'
   ,col.ORDINAL_POSITION as ' '
   ,COALESCE(LEFT(keys.CONSTRAINT_NAME,1), NULL) AS '_Key'
   ,col.COLUMN_NAME AS 'Kolonne'
   ,DATA_TYPE AS 'Type'
   --,CHARACTER_MAXIMUM_LENGTH AS 'CharMaxLength'
   --,NUMERIC_PRECISION AS 'NumPrec'
   --,DATETIME_PRECISION AS 'dtPrec'
   ,COALESCE(DATETIME_PRECISION, NUMERIC_PRECISION, CHARACTER_MAXIMUM_LENGTH, NULL ) AS 'Len/Prec'
   ,CASE WHEN IS_NULLABLE = 'YES' THEN 'Y' ELSE 'N' END AS 'NULLs'
   ,COALESCE(colDesc.columnDescription, NULL) AS '_Beskrivelse'
  FROM INFORMATION_SCHEMA.COLUMNS col
INNER JOIN information_schema.TABLES tbl 
   ON col.table_name = tbl.table_name
LEFT JOIN INFORMATION_SCHEMA.KEY_COLUMN_USAGE keys ON 1=1
   AND keys.TABLE_SCHEMA = col.TABLE_SCHEMA
   AND keys.TABLE_NAME = col.TABLE_NAME
   AND keys.COLUMN_NAME = col.COLUMN_NAME			
LEFT JOIN (
	SELECT 
		sc.object_id
		,sc.column_id
		,sc.name
		,colProp.[value] AS 'ColumnDescription'
      FROM sys.columns sc
	INNER JOIN sys.extended_properties colProp ON 1=1
		AND colProp.major_id = sc.object_id
        AND colProp.minor_id = sc.column_id
        AND colProp.name = 'MS_Description' 
   ) colDesc
   ON 1=1 
   AND colDesc.object_id = object_id(tbl.table_schema + '.' + tbl.table_name)
   AND colDesc.name = col.COLUMN_NAME
WHERE 1=1
   AND col.TABLE_SCHEMA in ('chru_cube', 'DM_FL_HR')
ORDER BY Skema asc, Tabel ASC, ' ' ASC  
```
</details>
<br/>
{::options parse_block_html="false" /}



{::options parse_block_html="true" /}
<!-- MEASURES -->
<details><summary markdown="span">Measures: DMV-query til træk af metadata om measures i DaxStudio</summary>
```sql
SELECT
	[DisplayFolder] AS [Mappe]
	,[Name] AS [Measure]
	--,[DataType] AS [Type]
	,[FormatString] AS [Format]
	,[Expression] AS [DAX]
	,[Description] AS [Beskrivelse]
	,[ModifiedTime] AS [Redigeret]
  FROM $SYSTEM.TMSCHEMA_MEASURES
ORDER BY [Name]
```
</details>
<br>
{::options parse_block_html="false" /}


## Christians guide til bestilling af dataadgange

<center>
<iframe src="https://regionh-my.sharepoint.com/personal/stefan_sajin-henningsen_regionh_dk/_layouts/15/Doc.aspx?sourcedoc={c652f92d-8025-4f11-9b4c-3e0f0e0dadba}&amp;action=embedview&amp;wdEmbedCode=0&amp;wdPrint=0" width="100%" height="700" frameborder="0"></iframe>
</center>

<!--
<iframe src="https://regionh-my.sharepoint.com/personal/stefan_sajin-henningsen_regionh_dk/_layouts/15/Doc.aspx?sourcedoc={c652f92d-8025-4f11-9b4c-3e0f0e0dadba}&amp;action=embedview&amp;wdEmbedCode=0&amp;wdPrint=0" width="476px" height="288px" frameborder="0">This is an embedded <a target="_blank" href="https://office.com">Microsoft Office</a> document, powered by <a target="_blank" href="https://office.com/webapps">Office</a>.</iframe>
-->









<!--
&action=embedview
&wdAllowInteractivity=FALSE
&Item=measures
&wdHideGridlines=TRUE
&wdHideHeaders=TRUE
&wdInConfigurator=TRUE
&wdInConfigurator=TRUE
&edesNext=TRUE
&edrtees6=FALSE
&resen=FALSE
&ed1JS=FALSE
&wdHideSheetTabs=TRUE
&ActiveCell=A1000
-->
