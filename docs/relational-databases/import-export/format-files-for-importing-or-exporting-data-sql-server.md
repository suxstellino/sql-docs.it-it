---
title: Formattare i file per importare ed esportare i dati
description: Quando si esegue l'importazione bulk in una tabella di SQL Server o l'esportazione bulk da una tabella, un file di formato può archiviare le informazioni sul formato del campo per un file di dati relativo a una tabella.
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], format files
- bulk importing [SQL Server], format files
- format files [SQL Server]
ms.assetid: b7b97d68-4336-4091-aee4-1941fab568e3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 82314d905d6336ae35d564096db00aa4e426436c
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049076"
---
# <a name="format-files-to-import-or-export-data-sql-server"></a>Formattare i file per importare ed esportare i dati (SQL Server)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Quando si importando dati in blocco in una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o si esportano dati in blocco da una tabella, è possibile usare un *file di formato* per archiviare le informazioni sul formato necessarie all'esportazione o all'importazione in blocco dei dati. Sono incluse informazioni sul formato per ogni campo in un file di dati relativo a quella tabella.

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] supporta due tipi di file di formato: file di formato XML e file di formato non XML. In entrambi i tipi di file di formato XML e non XML sono contenute descrizioni di ogni campo in un file di dati; nei file di formato XML sono inoltre presenti descrizioni delle colonne della tabella corrispondente. In generale, i file di formato XML e non XML sono intercambiabili. È tuttavia consigliabile utilizzare la sintassi XML per i nuovi file di formato, in quanto questo tipo di file offre numerosi vantaggi rispetto ai file di formato non XML. Per altre informazioni, vedere [File in formato XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md).

## <a name="benefits-of-format-files"></a><a name="Benefits"></a> Vantaggi dei file di formato

- Fornisce un sistema flessibile per la scrittura di file di dati che non richiede alcuna modifica o richiede modifiche minime per la conformità con altri formati di dati o la lettura di file di dati da un altro prodotto software.
- Consente di eseguire l'importazione bulk dei dati senza necessità di aggiungere o eliminare dati superflui o di ripetere l'ordinamento dei dati esistenti nel file di dati. I file di formato sono particolarmente utili quando è presente una mancata corrispondenza tra i campi nel file di dati e le colonne nella tabella.

## <a name="examples-of-format-files"></a><a name="ExamplesOfFFs"></a> Esempi di file di formato

Negli esempi seguenti viene illustrato il layout di un file di formato non XML e di un file di formato XML. Questi file di formato corrispondono alla tabella `HumanResources.myTeam` nel database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Questa tabella contiene quattro colonne: `EmployeeID`, `Name`, `Title`e `ModifiedDate`.

> [!NOTE]
> Per informazioni su questa tabella e su come crearla, vedere [Tabella di esempio HumanResources.myTeam &#40;SQL Server&#41;](../../relational-databases/import-export/humanresources-myteam-sample-table-sql-server.md).

### <a name="a-using-a-non-xml-format-file"></a>R. Utilizzo di un file di formato non XML

Il file di formato non XML seguente utilizza il formato di dati nativo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la tabella `HumanResources.myTeam` . Questo file di formato è stato creato utilizzando il comando `bcp` seguente.

```cmd
bcp AdventureWorks.HumanResources.myTeam format nul -f myTeam.Fmt -n -T
The contents of this format file are as follows: 9.0
4
1       SQLSMALLINT   0       2       ""   1     EmployeeID               ""  
2       SQLNCHAR      2       100     ""   2     Name                     SQL_Latin1_General_CP1_CI_AS  
3       SQLNCHAR      2       100     ""   3     Title                    SQL_Latin1_General_CP1_CI_AS  
4       SQLNCHAR      2       100     ""   4     Background               SQL_Latin1_General_CP1_CI_AS  
```  

Per altre informazioni, vedere [File in formato non XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).

### <a name="b-using-an-xml-format-file"></a>B. Utilizzo di un file di formato XML

Il seguente file di formato XML utilizza il formato nativo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la tabella `HumanResources.myTeam` . Questo file di formato è stato creato utilizzando il comando `bcp` seguente.

```cmd
bcp AdventureWorks.HumanResources.myTeam format nul -f myTeam.Xml -x -n -T
```

Il file di formato contiene:

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
  <FIELD ID="1" xsi:type="NativePrefix" LENGTH="1"/>
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="EmployeeID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>
  <COLUMN SOURCE="3" NAME="Title" xsi:type="SQLNVARCHAR"/>
  <COLUMN SOURCE="4" NAME="Background" xsi:type="SQLNVARCHAR"/>
</ROW>
</BCPFORMAT>
```

Per altre informazioni, vedere [File in formato XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md). 

## <a name="when-is-a-format-file-required"></a><a name="WhenFFrequired"></a> Quando è necessario un file di formato?

- Un'istruzione INSERT ... SELECT * FROM OPENROWSET(BULK...) richiede sempre un file di formato.
- Per **bcp** o BULK INSERT, in situazioni non complesse, l'uso di un file di formato è facoltativo e solo raramente necessario. In situazioni di importazione bulk complesse, tuttavia, un file di formato è spesso necessario.

I file di formato sono necessari nei casi seguenti:

- Viene utilizzato lo stesso file di dati come origine per più tabelle con schemi diversi.
- I campi nel file di dati sono in numero diverso rispetto alle colonne della tabella di destinazione, ad esempio:

  - La tabella di destinazione include almeno una colonna per la quale un valore predefinito è definito o è consentito un valore NULL.
  - L'utente non dispone di autorizzazioni SELECT/INSERT su una o più colonne della tabella.
  - Un singolo file di dati è utilizzato con due o più tabelle con schemi diversi.

- L'ordine delle colonne è diverso nel file di dati rispetto alla tabella.
- I caratteri di terminazione o le lunghezze di prefisso sono diverse all'interno delle colonne del file di dati.

> [!NOTE]
> In assenza di un file di formato, se in un comando **bcp** viene specificata un'opzione di formato di dati ( **-n**, **-c**, **-w** o **-N**) o se in un'operazione BULK INSERT viene specificata l'opzione DATAFILETYPE, il formato di dati specificato viene usato come metodo predefinito per l'interpretazione dei campi del file di dati.

## <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate

- [Creazione di un file di formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)
- [Usare un file di formato per l'importazione in blocco dei dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Utilizzo di un file di formato per ignorare una colonna di una tabella &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Utilizzo di un file di formato per escludere un campo di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)
- [Utilizzo di un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Vedere anche

- [File in formato non XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
- [File in formato XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md)
- [Formati di dati per l'importazione o l'esportazione in blocco &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)
