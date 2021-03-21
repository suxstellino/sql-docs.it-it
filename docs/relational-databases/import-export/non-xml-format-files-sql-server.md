---
title: File in formato non XML (SQL Server) | Microsoft Docs
description: SQL Server 2019 supporta file di formato non XML e file di formato XML per l'esportazione e l'importazione bulk. Informazioni sui file di formato non XML e sui relativi vantaggi.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- non-XML format files
- format files [SQL Server], non-XML format files
- bulk importing [SQL Server], format files
ms.assetid: f566db3e-0a3b-4a61-9c84-49f8d42f5760
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c455f1b4328392d03bdb68a4e1b499f6b7b3abb0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755331"
---
# <a name="non-xml-format-files-sql-server"></a>File in formato non XML (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]sono supportati due tipi di file di formato per l'esportazione e l'importazione bulk: *file di formato non XML* e *file di formato XML*.  
  
 **Contenuto dell'argomento**  
  
-   [Vantaggi](#Benefits)  
  
-   [Struttura dei file di formato non XML](#Structure)  
  
-   [Esempio di file di formato non XML](#Examples)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="benefits-of-non-xml-format-files"></a><a name="Benefits"></a> Vantaggi dei file di formato non XML  
  
-   È possibile creare automaticamente un file di formato non XML specificando l'opzione **format** in un comando **bcp** .  
  
-   Quando si specifica un file di formato esistente in un comando **bcp** , quest'ultimo utilizza i valori registrati nel file di formato e non richiede il tipo di archiviazione di file, la lunghezza del prefisso, la lunghezza del campo o il carattere di terminazione del campo.  
  
-   È possibile creare un file di formato per un particolare tipo di dati quali dati di tipo carattere o dati nativi.  
  
-   È possibile creare un file di formato non XML contenente attributi specificati in modo interattivo per ogni campo dati. Per altre informazioni, vedere [Impostazione dei formati di dati per la compatibilità mediante bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md).  
  
> [!NOTE]  
>  I file di formato XML offrono diversi vantaggi rispetto ai file di formato non XML. Per altre informazioni, vedere [File in formato XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md).  
  
##  <a name="structure-of-non-xml-format-files"></a><a name="Structure"></a> Struttura dei file di formato non XML  
 Un file di formato non XML è un file di testo con una struttura specifica. Il file di formato non XML contiene informazioni sul tipo di archiviazione di file, sulla lunghezza del prefisso, sulla lunghezza del campo e sul carattere di terminazione del campo di ogni colonna della tabella.  
  
 Nella figura seguente vengono illustrati i campi del file di formato per un file di formato non XML di esempio.  
  
 ![Identifica i campi di un file di formato non XML](../../relational-databases/import-export/media/mydepart-fmt-ident-c.gif)
  
 I campi **Versione** e **Numero di colonne** sono presenti una sola volta. I significati di questi campi sono descritti nella tabella seguente.  
  
|Campo del file di formato|Descrizione|  
|------------------------|-----------------|  
|Versione|Numero di versione dell'utilità **bcp** :<br /><br /> 9.0 = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> 10.0 = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]<br /><br /> 11.0 = [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]<br /><br /> 12.0 = [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]<br /><br /> Il numero di versione viene riconosciuto solo dall'utilità **bcp**, non da [!INCLUDE[tsql](../../includes/tsql-md.md)].<br /><br /> <br /><br /> Nota: la versione dell'utilità **bcp** (Bcp.exe) usata per leggere un file di formato deve essere uguale o successiva alla versione usata per creare il file di formato. Ad esempio, l'utilità [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]**bcp** può leggere un file di formato versione 10.0 generato dall'utilità [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]**bcp**; tuttavia l'utilità [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]**bcp** non riesce a leggere un file di formato versione 12.0 generato dall'utilità [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]**bcp**.|  
|Numero di colonne|Numero di campi del file di dati. Tutte le righe devono contenere lo stesso numero di campi.|  
  
 Gli altri campi del file di formato descrivono i campi dati di cui viene eseguita l'importazione o l'esportazione bulk. Per ogni campo dati è necessaria una riga separata nel file di formato. Ogni riga del file di formato contiene i valori dei campi del file di formato descritti nella tabella seguente.  
  
|Campo del file di formato|Descrizione|  
|------------------------|-----------------|  
|**Ordine dei campi nel file host**|Numero che indica la posizione di ogni campo nel file di dati. Il primo campo della riga è 1 e così via.|  
|**Tipo di dati del file host**|Indica il tipo di dati archiviati in un determinato campo del file di dati. Per i file di dati ASCII, utilizzare SQLCHAR. Per i file di dati in formato nativo, utilizzare i tipi di dati predefiniti. Per altre informazioni, vedere [Specifica del tipo di archiviazione di file tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md).|  
|**Lunghezza del prefisso**|Numero di caratteri della lunghezza del prefisso per il campo. Le lunghezze del prefisso valide sono 0, 1, 2, 4 e 8. Per evitare di specificare la lunghezza del prefisso, impostarlo su 0. È necessario specificare una lunghezza per il prefisso se il campo contiene valori di dati NULL. Per altre informazioni, vedere [Specificare la lunghezza del prefisso nei file di dati tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md).|  
|**Lunghezza dei dati del file host**|Lunghezza massima, in byte, del tipo di dati archiviati nel campo specifico del file di dati.<br /><br /> Se si sta creando un file di formato non XML per un file di testo delimitato, è possibile specificare il valore 0 per la lunghezza dei dati del file host di ogni campo dati. Quando viene importato un file di testo delimitato con una lunghezza del prefisso uguale a 0 e un carattere di terminazione, il valore relativo alla lunghezza del campo viene ignorato, in quanto lo spazio di archiviazione utilizzato dal campo equivale alla lunghezza dei dati più il carattere di terminazione.<br /><br /> Per altre informazioni, vedere [Definizione della lunghezza di campo tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md).|  
|**Carattere di terminazione**|Delimitatore di separazione dei campi di un file di dati. I caratteri di terminazione più comuni sono la virgola (,), il carattere di tabulazione (\t) e i caratteri di fine riga (\r\n). Per altre informazioni, vedere [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).|  
|**Ordine delle colonne nel server**|Ordine in cui sono disposte le colonne nella tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se, ad esempio, il quarto campo del file di dati esegue il mapping alla sesta colonna di una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'ordine delle colonne nel server per il quarto campo è 6.<br /><br /> Per impedire che una colonna della tabella riceva dati dal file di dati, impostare il valore dell'ordine delle colonne del server su 0.|  
|**Nome della colonna del server**|Nome della colonna copiata dalla tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il nome effettivo del campo non è obbligatorio, ma il campo nel file di formato non può essere vuoto.|  
|**Regole di confronto a livello di colonna**|Regole di confronto utilizzate per archiviare i dati di tipo carattere e Unicode nel file di dati.|  
  
> [!NOTE]  
>  È possibile modificare un file di formato per consentire l'importazione bulk da un file di dati in cui il numero o l'ordine dei campi è diverso dal numero o dall'ordine delle colonne della tabella. Per ulteriori informazioni, vedere l'elenco [Attività correlate](#RelatedTasks) più avanti in questo argomento.  
  
##  <a name="example-of-a-non-xml-format-file"></a><a name="Examples"></a> Esempio di file di formato non XML  
 Nell'esempio seguente viene illustrato un file di formato non XML creato in precedenza (`myDepartmentIdentical-f-c.fmt`). Questo file descrive un campo dati di tipo carattere per ogni colonna della tabella `HumanResources.Department` nel database di esempio `AdventureWorks2012` .  
  
 Il file di formato generato, `myDepartmentIdentical-f-c.fmt`, contiene le informazioni seguenti:  
  
```  
12.0  
4  
1       SQLCHAR       0       7       "\t"     1     DepartmentID     ""  
2       SQLCHAR       0       100     "\t"     2     Name             SQL_Latin1_General_CP1_CI_AS  
3       SQLCHAR       0       100     "\t"     3     GroupName        SQL_Latin1_General_CP1_CI_AS  
4       SQLCHAR       0       24      "\r\n"   4     ModifiedDate     ""  
```  
  
> [!NOTE]  
>  Per una figura in cui vengono illustrati i campi del file di formato in relazione a questo file di formato non XML di esempio, vedere [Struttura dei file di formato non XML](#Structure)più indietro in questo argomento.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Creazione di un file di formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
-   [Usare un file di formato per l'importazione in blocco dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [Utilizzo di un file di formato per ignorare una colonna di una tabella &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Utilizzo di un file di formato per escludere un campo di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Utilizzo di un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [Creazione di un file di formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)   
 [File in formato XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md)   
 [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)  
  
  
