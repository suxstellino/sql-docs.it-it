---
title: Table Properties - SSMS
description: Table Properties - SSMS
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql12.SWB.SELECTCOLUMNS.F1
- sql13.SWB.SELECTCOLUMNS.F1
- sql13.swb.tableproperties.storage.f1
- sql13.swb.tableproperties.changetracking.f1
- sql13.swb.tableproperties.general.f1
- sql13.swb.tableproperties.filetable.f1
ms.assetid: ad8a2fd4-f092-4c0f-be85-54ce8b9d725a
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b1deece76c019978bfcb76fc575985105bdefc4e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748941"
---
# <a name="table-properties---ssms"></a>Table Properties - SSMS

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  In questo argomento vengono descritte le proprietà della tabella visualizzate nella finestra di dialogo Proprietà tabella in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per altre informazioni su come visualizzare queste proprietà, vedere [Visualizzare la definizione di una tabella](../../relational-databases/tables/view-the-table-definition.md).  
  
 **Contenuto dell'articolo**  
  
1.  [Pagina Generale](#GeneralPage)  
  
2.  [Pagina Rilevamento modifiche](#ChangeTracking)  
  
3.  [Pagina di Tabella di File](#FileTable)  
  
4.  [Pagina Archivio](#Storage)  

##  <a name="general-page"></a><a name="GeneralPage"></a> Pagina Generale  
 **Database**  
 Nome del database che contiene la tabella.  
  
 **Server**  
 Nome dell'istanza del server corrente.  
  
 **Utente**  
 Nome dell'utente della connessione.  
  
 **Data creazione**  
 Data e ora di creazione della tabella.  
  
 **Nome**  
 Nome della tabella.  
  
 **Schema**  
 Schema proprietario della tabella.  
  
 **Oggetto di sistema**  
 Indica che la tabella è una tabella di sistema usata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per contenere informazioni interne. È consigliabile non modificare direttamente le tabelle di sistema o fare riferimento a tali tabelle.  
  
 **ANSI NULLs**  
 Indica se l'oggetto è stato creato con l'opzione ANSI NULLs impostata su ON. Per altre informazioni, vedere [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md).  
  
 **Identificatore delimitato**  
 Indica se l'oggetto è stato creato con l'opzione quoted identifier impostata su ON. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **Escalation blocchi**  
 Indica la granularità dell'escalation dei blocchi della tabella. Per altre informazioni sui blocchi nel motore di database, vedere [Guida per il controllo delle versioni delle righe e il blocco della transazione di SQL Server](../sql-server-transaction-locking-and-row-versioning-guide.md). I valori possibili sono:  
  
 AUTO  
 Questa opzione consente al [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] di selezionare la granularità dell'escalation dei blocchi appropriata per lo schema della tabella.  
  
- Se la tabella è partizionata, sarà consentita l'escalation dei blocchi nella granularità a livello di heap o albero B (HoBT). In altre parole, l'escalation sarà consentita al livello di partizione. Una volta eseguita l'escalation del blocco nel livello HoBT, non verrà eseguita alcuna successiva escalation del blocco nella granularità TABLE.
- Se la tabella non è partizionata, l'escalation blocchi viene eseguita nella granularità TABLE. 
  
 TABLE  
 L'escalation dei blocchi viene eseguita con una granularità a livello di tabella, indipendentemente dal fatto che la tabella sia o meno partizionata. TABLE rappresenta il valore predefinito.  
  
 DISABLE  
 Evita che venga eseguita l'escalation blocchi nella maggior parte dei casi. I blocchi a livello di tabella non vengono completamente disattivati. Quando si esegue l'analisi di una tabella che non dispone di alcun indice cluster nel livello di isolamento serializzabile, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] deve acquisire un blocco di tabella per proteggere l'integrità dei dati.  
  
 **Tabella replicata**  
 Indica se la tabella è stata replicata in un altro database usando la replica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . I valori possibili sono **True** e **False**.  
  
##  <a name="change-tracking-page"></a><a name="ChangeTracking"></a> Pagina Rilevamento modifiche  
 **Rilevamento delle modifiche**  
 Indica se il rilevamento delle modifiche è abilitato per la tabella. Il valore predefinito è **False**.  
  
 Questa opzione è disponibile solo quando il rilevamento delle modifiche è abilitato per il database.  
  
 Per abilitare il rilevamento delle modifiche, è necessario che la tabella disponga di una chiave primaria e che si disponga dell'autorizzazione per modificare la tabella. È anche possibile configurare il rilevamento delle modifiche usando [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 **Colonne di rilevamento aggiornate**  
 Indica se [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] rileva le colonne che sono state aggiornate.  
  
 Per altre informazioni sul rilevamento delle modifiche, vedere [Informazioni sul rilevamento delle modifiche &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-tracking-sql-server.md).  
  
##  <a name="filetable-page"></a><a name="FileTable"></a> Pagina FileTable  
 Consente di visualizzare le proprietà della tabella correlate alle tabelle FileTable. Per altre informazioni, vedere [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).  
  
 **Regole di confronto colonna Name tabella FileTable**  
 Regole di confronto applicate alla colonna **Name** in una tabella FileTable. La colonna **Name** contiene i nomi di file e directory.  
  
 **Nome di directory FileTable**  
 Cartella radice per la tabella FileTable.  
  
 **Spazio dei nomi FileTable abilitato**  
 Se impostato su **True**, questo valore indica che la tabella è una tabella FileTable. Se si modifica questo valore in **False**, la tabella FileTable viene modificata in una comune tabella utente. Se in seguito si desidera modificare di nuovo la tabella in tabella FileTable, questa dovrà superare un controllo di coerenza per tabelle FileTable affinché la conversione riesca.  
  
##  <a name="storage-page"></a><a name="Storage"></a> Pagina Archivio  
 Consente di visualizzare le proprietà relative all'archiviazione della tabella selezionata.  
  
### <a name="compression"></a>Compressione  
 **Tipo di compressione**  
 Tipo di compressione della tabella. Questa proprietà è disponibile solo per le tabelle non partizionate. Per altre informazioni, vedere [Data Compression](../../relational-databases/data-compression/data-compression.md).  
  
 **Partizioni che usano la compressione di pagina**  
 Numeri di partizioni che usano la compressione di pagina. Questa proprietà è disponibile solo per le tabelle partizionate.  
  
 **Partizioni non compresse**  
 Numeri di partizioni non compresse. Questa proprietà è disponibile solo per le tabelle partizionate.  
  
 **Partizioni che usano la compressione di riga**  
 Numeri di partizioni che usano la compressione di riga. Questa proprietà è disponibile solo per le tabelle partizionate.  
  
### <a name="filegroup"></a>Filegroup  
 **Filegroup dati di testo**  
 Nome del filegroup che contiene i dati di testo della tabella.  
  
 **Filegroup**  
 Nome del filegroup che contiene la tabella.  
  
 **Tabella partizionata**  
 I valori possibili sono **True** e **False**.  
  
 **Filegroup FILESTREAM**  
 Specificare il nome del filegroup di dati FILESTREAM se la tabella contiene una colonna **varbinary(max)** con l'attributo FILESTREAM. Il filegroup di dati FILESTREAM predefinito è indicato per impostazione predefinita.  
  
 Se la tabella non contiene dati FILESTREAM, il campo è vuoto.  
  
### <a name="general"></a>Generale  
 **Formato di archiviazione vardecimal abilitato**  
 Se impostato su **True**, questo valore di sola lettura indica che i tipi di dati **decimal** e **numeric** vengono archiviati con il formato di archiviazione vardecimal. Per modificare questa opzione, usare l'opzione **vardecimal storage format** di [sp_tableoption](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Il formato di archiviazione vardecimal è deprecato. ed è necessario usare il tipo di compressione ROW.  
  
 **Spazio degli indici**  
 Quantità di spazio occupata dagli indici nella tabella, espresso in megabyte. Questo valore non include lo spazio usato dagli indici XML per la tabella. Se gli indici XML appartengono alla tabella, usare in alternativa [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) .  
  
 **Numero di righe**  
 Numero di righe nella tabella.  
  
 **Spazio dati**  
 Quantità di spazio occupata dai dati nella tabella, espressa in megabyte.  
  
### <a name="partitioning"></a>Partizionamento  
 Questa sezione è disponibile solo se la tabella è partizionata. Per ulteriori informazioni, vedere [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 **Colonna di partizione**  
 Nome della colonna in relazione alla quale è partizionata la tabella.  
  
 **Schema di partizione**  
 Nome dello schema di partizione, se la tabella è partizionata. Se la tabella non è partizionata, il campo è vuoto.  
  
 **Numero di partizioni**  
 Numero di partizioni della tabella.  
  
 **Schema di partizione FILESTREAM**  
 Nome dello schema di partizione FILESTREAM, se la tabella è partizionata. Se la tabella non è partizionata, il campo è vuoto.  
  
 Lo schema di partizione FILESTREAM deve essere simmetrico rispetto allo schema specificato nell'opzione **Schema partizione** .  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare la definizione di una tabella](../../relational-databases/tables/view-the-table-definition.md)   
 [Modificare colonne &#40;Motore di database&#41;](../../relational-databases/tables/modify-columns-database-engine.md)  
  
