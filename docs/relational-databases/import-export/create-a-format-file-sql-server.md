---
title: Creare un file di formato (SQL Server) | Microsoft Docs
description: Quando si esegue l'importazione o l'esportazione bulk di una tabella di SQL Server, un file di formato consente la scrittura di file di dati con poche modifiche o la lettura di file di dati da altri programmi.
ms.custom: ''
ms.date: 02/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], creating
ms.assetid: f680b4a0-630f-4052-9c79-d348c1076f7b
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a097686535a2e8fae1e5a26d8bcea69b91d31d0c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749691"
---
# <a name="create-a-format-file-sql-server"></a>Creazione di un file di formato (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Quando si esegue l'importazione bulk in una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dei dati da una tabella, è possibile utilizzare un file di formato per un sistema flessibile per la scrittura di file di dati che non richiede alcuna modifica o richiede modifiche minime per la conformità con altri formati di dati o la lettura di file di dati da un altro programma software.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta due tipi di file di formato, ovvero non XML e XML. Il formato non XML è il formato originale supportato dalle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 In generale, i file di formato XML e non XML sono intercambiabili. È tuttavia consigliabile utilizzare la sintassi XML per i nuovi file di formato, in quanto questo tipo di file offre numerosi vantaggi rispetto ai file di formato non XML.  
  
> [!NOTE]  
>  La versione dell'utilità **bcp** (Bcp.exe) usata per leggere un file di formato deve essere uguale o successiva alla versione usata per creare il file di formato. Ad esempio, l'utilità **bcp** di [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] può leggere un file di formato versione 10.0 generato dall'utilità **bcp** di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], ma l'utilità **bcp** di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] non può leggere un file di formato versione 11.0 generato dall'utilità **bcp** di [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 Questo argomento descrive come usare l' [utilità bcp](../../tools/bcp-utility.md) per creare un file di formato per una tabella specifica. Il file di formato è basato sull'opzione relativa al tipo di dati specificata ( **-n**, **-c**, **-w** o **-N**) e sui delimitatori della tabella o della vista.  
  
## <a name="creating-a-non-xml-format-file"></a>Creazione di un file di formato non XML  
 Per usare un comando **bcp** per creare un file di formato, specificare l'argomento **format** e usare **nul** anziché un percorso del file di dati. L'opzione **format** richiede anche l'opzione **-f** , come nell'esempio seguente:  
  
 **bcp** _tabella_o_vista_ **format** nul **-f**_nome_file_formato_  
  
> [!NOTE]  
> Per distinguere un file di formato non XML, è consigliabile utilizzare l'estensione fmt, ad esempio MyTable.fmt.  
  
 Per informazioni sulla struttura e sui campi dei file di formato non XML, vedere [File in formato non XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
### <a name="examples"></a>Esempi  
 Questa sezione contiene gli esempi seguenti che illustrano come usare i comandi **bcp** per creare un file di formato non XML:  
  
-   R. Creazione di un file di formato non XML per dati nativi  
  
-   B. Creazione di un file di formato non XML per dati di tipo carattere  
  
-   C. Creazione di un file di formato non XML per dati nativi Unicode  
  
-   D. Creazione di un file di formato non XML per dati di tipo carattere Unicode  
  
-   F. Uso di un file di formato con l'opzione della tabella codici  
  
 Negli esempi viene utilizzata la tabella `HumanResources.Department` del database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . La tabella `HumanResources.Department` contiene quattro colonne, ovvero `DepartmentID`, `Name`, `GroupName`e `ModifiedDate`.  
  
#### <a name="a-creating-a-non-xml-format-file-for-native-data"></a>R. Creazione di un file di formato non XML per dati nativi  
 Nell'esempio seguente viene creato un file di formato XML, `Department-n.xml`, per la tabella [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` . Nel file di formato vengono utilizzati tipi di dati nativi. Il contenuto del file di formato generato viene visualizzato dopo il comando.  
  
 Per il comando **bcp** sono disponibili i qualificatori seguenti.  
  
|Qualificatori|Descrizione|  
|----------------|-----------------|  
|**formatnul-f** _file_formato_|Specifica il file di formato non XML.|  
|**-n**|Specifica i tipi di dati nativi.|  
|**-T**|Specifica che l'utilità **bcp** si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con una connessione trusted che usa la sicurezza integrata. Se non si specifica **-T** , è necessario specificare **-U** e **-P** per eseguire correttamente l'accesso.|  
  
 Al prompt dei comandi di Windows digitare il comando `bcp` seguente:  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -n -f Department-n.fmt  
```  
  
 Il file di formato generato, `Department-n.fmt`, contiene le informazioni seguenti:  
  
```  
12.0  
4  
1  SQLSMALLINT   0       2       ""   1     DepartmentID         ""  
2  SQLNCHAR      2       100     ""   2     Name                 SQL_Latin1_General_CP1_CI_AS  
3  SQLNCHAR      2       100     ""   3     GroupName            SQL_Latin1_General_CP1_CI_AS  
4  SQLDATETIME   0       8       ""   4     ModifiedDate         ""  
```  
  
 Per altre informazioni, vedere [File in formato non XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
#### <a name="b-creating-a-non-xml-format-file-for-character-data"></a>B. Creazione di un file di formato non XML per dati di tipo carattere  
 Nell'esempio seguente viene creato un file di formato XML, `Department.fmt`, per la tabella [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` . Nel file di formato vengono utilizzati formati di dati di tipo carattere e un carattere di terminazione del campo non predefinito (`,`). Il contenuto del file di formato generato viene visualizzato dopo il comando.  
  
 Per il comando **bcp** sono disponibili i qualificatori seguenti.  
  
|Qualificatori|Descrizione|  
|----------------|-----------------|  
|**formatnul-f** _file_formato_|Specifica un file di formato non XML.|  
|**-c**|Specifica i dati di tipo carattere.|  
|**-T**|Specifica che l'utilità **bcp** si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con una connessione trusted che usa la sicurezza integrata. Se non si specifica **-T** , è necessario specificare **-U** e **-P** per eseguire correttamente l'accesso.|  
  
 Al prompt dei comandi di Windows digitare il comando `bcp` seguente:  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -c -f Department-c.fmt -T  
```  
  
 Il file di formato generato, `Department-c.fmt`, contiene le informazioni seguenti:  
  
```  
12.0  
4  
1  SQLCHAR       0       7       "\t"     1     DepartmentID            ""  
2  SQLCHAR       0       100     "\t"     2     Name                    SQL_Latin1_General_CP1_CI_AS  
3  SQLCHAR       0       100     "\t"     3     GroupName               SQL_Latin1_General_CP1_CI_AS  
4  SQLCHAR       0       24      "\r\n"   4     ModifiedDate            ""  
```  
  
 Per altre informazioni, vedere [File in formato non XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
#### <a name="c-creating-a-non-xml-format-file-for-unicode-native-data"></a>C. Creazione di un file di formato non XML per dati nativi Unicode  
 Per creare un file di formato non XML per i dati nativi Unicode della tabella `HumanResources.Department`, utilizzare il comando seguente:  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -N -f Department-n.fmt  
```  
  
 Per altre informazioni sull'uso dei dati di tipo carattere Unicode, vedere [Utilizzare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).  
  
#### <a name="d-creating-a-non-xml-format-file-for-unicode-character-data"></a>D. Creazione di un file di formato non XML per dati di tipo carattere Unicode  
 Per creare un file di formato non XML per i dati di tipo carattere Unicode della tabella `HumanResources.Department` che utilizza i caratteri di terminazione predefiniti, utilizzare il comando seguente:  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -w -f Department-w.fmt  
```  
  
 Per altre informazioni sull'uso dei dati di tipo carattere Unicode, vedere [Utilizzo del formato carattere per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).  
  
#### <a name="f-using-a-format-file-with-the-code-page-option"></a>F. Uso di un file di formato con l'opzione della tabella codici  
Se si crea un file di formato con il comando bcp (ovvero usando `bcp format`), le informazioni sulle regole di confronto o sulla tabella codici verranno scritte nel file di formato.   
Il seguente file di formato di esempio per una tabella con 5 colonne include le regole di confronto.  
  
```  
13.0  
5  
1  SQLCHAR         0       0       "**\t**"         1     c_0          Cyrillic_General_CS_AS  
2  SQLCHAR         0       0       "**\t**"         2     c_1          Cyrillic_General_CS_AS  
3  SQLCHAR         0       3000    "**\t**"         3     c_2          Cyrillic_General_CS_AS  
4  SQLCHAR         0       5       "**\t**"         4     c_3          ""  
5  SQLCHAR         0       41      "!!!\r\r\n"      5     c_4          ""  
  
```  
  
 Se si prova a importare i dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando `bcp in -c -C65001 -f format_file` ..." o "`BULK INSERT`/`OPENROWSET` ... `FORMATFILE='format_file' CODEPAGE=65001` ...", le informazioni su regole di confronto/tabella codici avranno la priorità rispetto all'opzione 65001.  
Di conseguenza, se si genera un file di formato, è necessario eliminare manualmente le informazioni sulle regole di confronto dal file di formato generato prima di iniziare a reimportare i dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
Ecco un esempio di file di formato senza le informazioni sulle regole di confronto.  
  
```  
13.0  
5  
1  SQLCHAR         0       0       "**\t**"         1     c_0              ""  
2  SQLCHAR         0       0       "**\t**"         2     c_1              ""  
3  SQLCHAR         0       3000    "**\t**"         3     c_2              ""  
4  SQLCHAR         0       5       "**\t**"         4     c_3              ""  
5  SQLCHAR         0       41      "!!!\r\r\n"      5     c_4              ""  
```  
  
## <a name="creating-an-xml-format-file"></a>Creazione di un file di formato XML  
 Per usare un comando **bcp** per creare un file di formato, specificare l'argomento **format** e usare **nul** anziché un percorso del file di dati. L'opzione **format** richiede sempre l'opzione **-f** e per creare un file di formato XML è necessario anche specificare l'opzione **-x** , come nell'esempio seguente:  
  
 **bcp** _tabella_o_vista_ **format nul-f** _nome_file_formato_ **-x**  
  
> [!NOTE]  
> Per distinguere un file di formato XML, è consigliabile utilizzare l'estensione xml, ad esempio MyTable.xml.  
  
 Per informazioni sulla struttura e sui campi dei file di formato XML, vedere [File in formato XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md).  
  
### <a name="examples"></a>Esempi  
 Questa sezione contiene gli esempi seguenti che illustrano come usare i comandi **bcp** per creare un file di formato XML:  
  
-   R. Creazione di un file di formato XML per dati di tipo carattere  
-   B. Creazione di un file di formato XML per dati nativi  
  
 Negli esempi viene utilizzata la tabella `HumanResources.Department` del database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . La tabella `HumanResources.Department` contiene quattro colonne, ovvero `DepartmentID`, `Name`, `GroupName`e `ModifiedDate`.  
  
> [!NOTE]  
>  [!INCLUDE[ssSampleDBdesc](../../includes/sssampledbdesc-md.md)]  
  
#### <a name="a-creating-an-xml-format-file-for-character-data"></a>R. Creazione di un file di formato XML per dati di tipo carattere  
 Nell'esempio seguente viene creato un file di formato XML, `Department.xml`, per la tabella [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` . Nel file di formato vengono utilizzati formati di dati di tipo carattere e un carattere di terminazione del campo non predefinito (`,`). Il contenuto del file di formato generato viene visualizzato dopo il comando.  
  
 Per il comando **bcp** sono disponibili i qualificatori seguenti.  
  
|Qualificatori|Descrizione|  
|----------------|-----------------|  
|**formatnul-f** _file_formato_ **-x**|Specifica il file di formato XML.|  
|**-c**|Specifica i dati di tipo carattere.|  
|**-t** `,`|Specifica la virgola ( **,** ) come carattere di terminazione del campo.<br /><br /> Nota: se il file di dati usa il carattere di terminazione del campo predefinito (`\t`), l'opzione **-t** non è necessaria.|  
|**-T**|Specifica che l'utilità **bcp** si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con una connessione trusted che usa la sicurezza integrata. Se non si specifica **-T** , è necessario specificare **-U** e **-P** per eseguire correttamente l'accesso.|  
  
 Al prompt dei comandi di Windows digitare il comando `bcp` seguente:  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -c -x -f Department-c.xml -t, -T  
```  
  
 Il file di formato generato, `Department-c.xml`, contiene gli elementi XML seguenti:  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>  
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="24"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="DepartmentID" xsi:type="SQLSMALLINT"/>  
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="3" NAME="GroupName" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="4" NAME="ModifiedDate" xsi:type="SQLDATETIME"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
 Per informazioni sulla sintassi di questo file di formato, vedere [File in formato XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md). Per informazioni sui dati di tipo carattere, vedere [Utilizzo del formato carattere per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).  
  
#### <a name="b-creating-an-xml-format-file-for-native-data"></a>B. Creazione di un file di formato XML per dati nativi  
 Nell'esempio seguente viene creato un file di formato XML, `Department-n.xml`, per la tabella `HumanResources.Department` . Nel file di formato vengono utilizzati tipi di dati nativi. Il contenuto del file di formato generato viene visualizzato dopo il comando.  
  
 Per il comando **bcp** sono disponibili i qualificatori seguenti.  
  
|Qualificatori|Descrizione|  
|----------------|-----------------|  
|**formatnul-f** _file_formato_ **-x**|Specifica il file di formato XML.|  
|**-n**|Specifica i tipi di dati nativi.|  
|**-T**|Specifica che l'utilità **bcp** si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con una connessione trusted che usa la sicurezza integrata. Se non si specifica **-T** , è necessario specificare **-U** e **-P** per eseguire correttamente l'accesso.|  
  
 Al prompt dei comandi di Windows digitare il comando `bcp` seguente:  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -x -f Department-n.xml -n -T  
```  
  
 Il file di formato generato, `Department-n.xml`, contiene gli elementi XML seguenti:  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="NativeFixed" LENGTH="2"/>  
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="3" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="4" xsi:type="NativeFixed" LENGTH="8"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="DepartmentID" xsi:type="SQLSMALLINT"/>  
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="3" NAME="GroupName" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="4" NAME="ModifiedDate" xsi:type="SQLDATETIME"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
 Per informazioni sulla sintassi di questo file di formato, vedere [File in formato XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md). Per informazioni su come usare i dati nativi, vedere [Utilizzo del formato nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).  
  
## <a name="mapping-data-fields-to-table-columns"></a>Mapping tra campi dati e colonne della tabella  
 Un file di formato creato da **bcp** descrive tutte le colonne della tabella in ordine. È possibile modificare un file di formato per spostare o omettere righe della tabella. In questo modo, è possibile personalizzare un file di formato in base a un file di dati i cui campi non eseguono il mapping direttamente alle colonne della tabella. Per altre informazioni, vedere gli argomenti seguenti:  
  
-   [Utilizzo di un file di formato per ignorare una colonna di una tabella &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Utilizzo di un file di formato per escludere un campo di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Utilizzo di un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [Usare un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)   
 [Utilizzo di un file di formato per ignorare una colonna di una tabella &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [Utilizzo di un file di formato per escludere un campo di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)   
 [File in formato non XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [File in formato XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md)  
  
  
