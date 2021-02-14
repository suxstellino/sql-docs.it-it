---
title: Specificare i formati di dati per la compatibilità con bcp
description: Per le esportazioni bulk in SQL Server con bcp, i formati di dati potrebbero non essere compatibili con il layout previsto. Un file di formato non XML specifica i formati di dati per la compatibilità.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], compatibility
- bulk importing [SQL Server], compatibility
- compatibility [SQL Server], data formats
- data formats [SQL Server], compatibility
- bcp utility [SQL Server], compatibility
ms.assetid: cd5fc8c8-eab1-4165-9468-384f31e53f0a
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: afdaa7a7243c87876b55cb79155e652ae029c495
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351308"
---
# <a name="specify-compatibility-data-formats-when-using-bcp-sql-server"></a>Specificare i formati di dati per la compatibilità con bcp (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Questo argomento descrive gli attributi del formato dati, prompt specifici di campo e l'archiviazione dei dati dei singoli campi in un file non in formato XML con il comando **bcp** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La comprensione di questi concetti può risultare utile per l'esportazione bulk di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a fini di importazione in un altro programma, ad esempio un altra applicazione di database. I formati di dati predefiniti (native, character o Unicode) nella tabella di origine potrebbero non essere compatibili con il layout di dati previsto dall'altra applicazione. In questo caso, quando si esportano i dati è necessario descriverne il layout.  
  
> [!NOTE]  
>  Se non si ha familiarità con i formati di dati per l'importazione o esportazione di dati, vedere [Formati di dati per l'importazione o l'esportazione bulk &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md).  
  
  
##  <a name="bcp-data-format-attributes"></a><a name="bcpDataFormatAttr"></a> Attributi di formato e dati di bcp  
 Il comando **bcp** consente di specificare la struttura di tutti i campi di un file di dati con i seguenti attributi di formato dati:  
  
-   tipo di archiviazione di file  
  
     Il *tipo di archiviazione di file* indica la modalità con la quale vengono archiviati i dati in un file. I dati possono essere esportati in un file usando il tipo di dati della tabella del database in cui si trovano (formato nativo), come caratteri (formato carattere) oppure usando qualsiasi tipo di dati nel caso in cui sia supportata la conversione implicita. È possibile ad esempio copiare il tipo **smallint** come **int**. I tipi di dati definiti dall'utente vengono esportati utilizzando il tipo di dati di base corrispondente. Per altre informazioni, vedere [Specifica del tipo di archiviazione di file tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md).  
  
-   Lunghezza del prefisso  
  
     Per fornire il tipo di archiviazione file con la massima compressione durante l'esportazione in blocco dei dati in formato nativo in un file di dati, il comando **bcp** inserisce davanti a ogni campo uno o più caratteri che ne indicano la lunghezza. Tali caratteri sono denominati *caratteri per il prefisso di lunghezza*. Per altre informazioni, vedere [Specificare la lunghezza del prefisso nei file di dati tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md).  
  
-   Lunghezza del campo  
  
     La lunghezza del campo indica il numero massimo di caratteri necessari per rappresentare i dati in formato carattere. Se i dati sono archiviati in formato nativo, la lunghezza del campo è già nota. Per altre informazioni, vedere [Definizione della lunghezza di campo tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md).  
  
-   Carattere di terminazione del campo  
  
     Nei campi dati di tipo carattere, i caratteri di terminazione facoltativi consentono di indicare la fine di ogni campo nel file di dati (tramite un *carattere di terminazione del campo*) e di ogni riga (tramite un *carattere di terminazione della riga*). I caratteri di terminazione indicano ai programmi che leggono il file il punto in cui termina il campo o la riga e inizia il campo o la riga successiva. Per altre informazioni, vedere [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
  
##  <a name="overview-of-the-field-specific-prompts"></a><a name="FieldSpecificPrompts"></a> Panoramica dei prompt specifici dei campi  
 Se un comando **bcp** interattivo contiene l'opzione **in** o **out** , ma non contiene l'opzione del file di formato ( **-f**) o un'opzione di formato dati ( **-n**, **-c**, **-w** o **-N**), per ciascuna colonna della tabella di origine o di destinazione il comando richiede uno alla volta tutti gli attributi precedenti. In ogni prompt, il comando **bcp** offre un valore predefinito in base al tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] della colonna di tabella. Accettare il valore predefinito per tutti i prompt equivale a specificare il formato nativo ( **-n**) nella riga di comando. Ogni prompt mostra un valore predefinito fra parentesi: [*predefinito*]. Per accettare i valori predefiniti, premere INVIO. Per specificare un valore diverso da quello predefinito, immettere il valore desiderato al prompt.  
  
### <a name="example"></a>Esempio  
 L'esempio seguente usa il comando **bcp** per l'esportazione in blocco interattiva dei dati dalla tabella `HumanResources.myTeam` al file `myTeam.txt` . Prima di eseguire l'esempio è necessario creare questa tabella. Per informazioni sulla modalità di creazione e sulla tabella, vedere [Tabella di esempio HumanResources.myTeam &#40;SQL Server&#41;](../../relational-databases/import-export/humanresources-myteam-sample-table-sql-server.md).  
  
 Il comando non specifica un file di formato né un tipo di dati. Ciò provoca la richiesta di informazioni di formato dati da parte di **bcp** . Al prompt dei comandi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows digitare:  
  
```cmd
bcp AdventureWorks.HumanResources.myTeam out myTeam.txt -T  
```  
  
 Per ogni colonna, bcp richiede valori specifici del campo. Nell'esempio seguente vengono illustrati i prompt specifici di campo per le colonne `EmployeeID` e `Name` della tabella e viene suggerito il tipo di archiviazione file predefinito (il formato nativo) per ogni colonna. Le lunghezze del prefisso delle colonne `EmployeeID` e `Name` sono rispettivamente 0 e 2. L'utente specifica una virgola (`,`) come carattere di terminazione di ogni campo.  
  
 `Enter the file storage type of field EmployeeID [smallint]:`  
  
 `Enter prefix-length of field EmployeeID [0]:`  
  
 `Enter field terminator [none]:,`  
  
 `Enter the file storage type of field Name [nvarchar]:`  
  
 `Enter prefix length of field Name [2]:`  
  
 `Enter field terminator [none]:,`  
  
 `.`  
  
 `.`  
  
 `.`  
  
 Per ogni colonna della tabella, in ordine e se necessario, vengono visualizzati prompt equivalenti.  
  
  
##  <a name="storing-field-by-field-data-in-a-non-xml-format-file"></a><a name="FieldByFieldNonXmlFF"></a> Archiviazione di dati campo per campo in un file di formato non XML  
 Una volta specificate tutte le colonne della tabella, il comando **bcp** chiede se si vuole generare un file di formato non XML per l'archiviazione delle informazioni dei singoli campi appena inserite (vedere l'esempio precedente). Se si sceglie di generare un file di formato, è possibile utilizzarlo ogni volta che si esportano dati da quella tabella o si importano dati di struttura simile in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Il file di formato può essere utilizzato per eseguire l'importazione bulk dal file di dati a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o per eseguire l'esportazione bulk di dati dalla tabella senza dover nuovamente specificare il formato. Per altre informazioni, vedere [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 Nell'esempio seguente viene creato un file di formato non XML denominato `myFormatFile.fmt`:  
  
 `Do you want to save this format information in a file? [Y/n] y`  
  
 `Host filename: [bcp.fmt]myFormatFile.fmt`  
  
 Il nome predefinito per il file di formato è bcp.fmt, ma è possibile specificare un nome diverso.  
  
> [!NOTE]  
>  Per un file di dati che usa un unico formato dati per il tipo di archiviazione, ad esempio il formato carattere o nativo, è possibile creare rapidamente un file di formato senza esportare o importare dati usando l'opzione **format** . Questo approccio ha il vantaggio della semplicità e consente di creare un file di formato XML o non XML. Per altre informazioni, vedere [Creazione di un file di formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
  
## <a name="related-tasks"></a>Attività correlate  
  
-   [Specifica del tipo di archiviazione di file tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
-   [Specificare la lunghezza del prefisso nei file di dati con bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
-   [Definizione della lunghezza di campo tramite bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)  
  
-   [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
## <a name="related-content"></a>Contenuto correlato  
 No.  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni sull'importazione ed esportazione bulk di dati &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [Formati di dati per l'importazione o l'esportazione bulk &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)   
 [Utilità bcp](../../tools/bcp-utility.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
  
  
