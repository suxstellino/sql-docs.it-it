---
description: Validate XML with the XML Task
title: Eseguire la convalida XML con Attività XML | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- XML validation
- XML, validating
ms.assetid: 224fc025-c21f-4d43-aa9d-5ffac337f9b0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 91eb457beedb4712598b871a9325228c2309f2f2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237898"
---
# <a name="validate-xml-with-the-xml-task"></a>Validate XML with the XML Task

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  È possibile convalidare documenti XML e ottenere output avanzato degli errori abilitando la proprietà **ValidationDetails** dell'Attività XML.  
  
 L'immagine seguente illustra l'**Editor attività XML** con le impostazioni necessarie per la convalida di XML con l'output di errore completo.  
  
 ![Proprietà delle attività XML nell'Editor attività XML](../../integration-services/control-flow/media/xmltaskproperties.jpg "Proprietà delle attività XML nell'Editor attività XML")  
  
 Prima che fosse disponibile la proprietà **ValidationDetails** , la convalida XML da parte dell'Attività XML restituiva solo un risultato di tipo True o False, senza informazioni sugli errori o sulle rispettive posizioni. Attualmente, quando si imposta **ValidationDetails** su True, il file di output contiene informazioni dettagliate su ogni errore, inclusi il numero di riga e la posizione. È possibile usare questa informazione per comprendere, individuare e risolvere gli errori nei documenti XML.  
  
 La funzionalità di convalida XML è facilmente scalabile per documenti XML di grandi dimensioni e un numero elevato di errori. Dato che il file di output è in formato XML, è possibile eseguire una query e analizzare l'output. Ad esempio, se l'output contiene un numero elevato di errori, è possibile raggrupparli usando un [!INCLUDE[tsql](../../includes/tsql-md.md)] , come descritto in questo argomento.  
  
> [!NOTE]
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ([!INCLUDE[ssIS](../../includes/ssis-md.md)]) ha introdotto la proprietà **ValidationDetails** in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Service Pack 2. La proprietà è disponibile anche in [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].  
  
## <a name="sample-output-for-xml-thats-valid"></a>Esempio di output per un file XML valido  
 Ecco un file di output di esempio con i risultati della convalida per un file XML valido.  
  
```xml  
  
<root xmlns:ns="https://schemas.microsoft.com/xmltools/2002/xmlvalidation">  
    <metadata>  
        <result>true</result>  
        <errors>0</errors>  
        <warnings>0</warnings>  
        <startTime>2015-05-28T10:27:22.087</startTime>  
        <endTime>2015-05-28T10:29:07.007</endTime>  
        <xmlFile>d:\Temp\TestData.xml</xmlFile>  
        <xsdFile>d:\Temp\TestSchema.xsd</xsdFile>  
    </metadata>  
    <messages />  
</root>  
```  
  
## <a name="sample-output-for-xml-thats-not-valid"></a>Esempio di output per un file XML non valido  
 Ecco un file di output di esempio con i risultati della convalida per un file XML che contiene un numero ridotto di errori. Il testo degli elementi \<error> è stato mandato a capo per migliorare la leggibilità.  
  
```xml  
  
<root xmlns:ns="https://schemas.microsoft.com/xmltools/2002/xmlvalidation">  
    <metadata>  
        <result>false</result>  
        <errors>2</errors>  
        <warnings>0</warnings>  
        <startTime>2015-05-28T10:45:09.538</startTime>  
        <endTime>2015-05-28T10:45:09.558</endTime>  
        <xmlFile>C:\Temp\TestData.xml</xmlFile>  
        <xsdFile>C:\Temp\TestSchema.xsd</xsdFile>  
    </metadata>  
    <messages>  
        <error line="5" position="26">The 'ApplicantRole' element is invalid - The value 'wer3' is invalid  
    according to its datatype 'ApplicantRoleType' - The Enumeration constraint failed.</error>  
        <error line="16" position="28">The 'Phone' element is invalid - The value 'we3056666666' is invalid  
     according to its datatype 'phone' - The Pattern constraint failed.</error>  
    </messages>  
</root>  
```  
  
## <a name="analyze-xml-validation-output-with-a-transact-sql-query"></a>Analizzare l'output della convalida XML con una query Transact-SQL  
 Se l'output della convalida XML contiene un numero elevato di errori, è possibile usare una query di [!INCLUDE[tsql](../../includes/tsql-md.md)] per caricare l'output in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. È quindi possibile analizzare l'elenco di errori con tutte le funzionalità del linguaggio T-SQL, inclusi WHERE, GROUP BY, ORDER BY, JOIN e così via.  
  
```sql  
DECLARE @xml XML;  
  
SELECT @xml = XmlDoc     
FROM OPENROWSET (BULK N'C:\Temp\XMLValidation_2016-02-212T10-41-00.xml', SINGLE_BLOB) AS Tab(XmlDoc);  
  
-- Query # 1, flat list of errors  
-- convert to relational/rectangular  
;WITH XMLNAMESPACES ('https://schemas.microsoft.com/xmltools/2002/xmlvalidation' AS ns), rs AS  
(  
SELECT col.value('@line','INT') AS line  
     , col.value('@position','INT') AS position  
     , col.value('.','VARCHAR(1024)') AS error  
FROM @XML.nodes('/root/messages/error') AS tab(col)  
)  
SELECT * FROM rs;  
-- WHERE error LIKE '%whatever_string%'  
  
-- Query # 2, count of errors grouped by the error message  
-- convert to relational/rectangular  
;WITH XMLNAMESPACES ('https://schemas.microsoft.com/xmltools/2002/xmlvalidation' AS ns), rs AS  
(  
SELECT col.value('@line','INT') AS line  
     , col.value('@position','INT') AS position  
     , col.value('.','VARCHAR(1024)') AS error  
FROM @XML.nodes('/root/messages/error') AS tab(col)  
)  
SELECT COALESCE(error,'Total # of errors:') AS [error], COUNT(*) AS [counter]  
FROM rs  
GROUP BY GROUPING SETS ((error), ())  
ORDER BY 2 DESC, COALESCE(error, 'Z');  
  
```  
  
 Ecco il risultato in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] della seconda query di esempio illustrata nel testo precedente.  
  
 ![Errori XML di query su gruppo in Management Studio](../../integration-services/control-flow/media/query-for-xml-errors.png "Errori XML di query su gruppo in Management Studio")  
  
## <a name="see-also"></a>Vedere anche  
 [Attività XML](../../integration-services/control-flow/xml-task.md)   
 [XML Task Editor &#40;General Page&#41;](./xml-task.md)  
  
