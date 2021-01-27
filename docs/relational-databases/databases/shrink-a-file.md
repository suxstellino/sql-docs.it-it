---
title: Compattare un file | Microsoft Docs
description: Informazioni su come compattare un file di dati o di log in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.shrinkfile.f1
helpviewer_keywords:
- shrinking files
- decreasing file size
- databases [SQL Server], shrinking
- reducing file size
- size [SQL Server], files
- file size [SQL Server]
ms.assetid: ce5c8798-c039-4ab2-81e7-90a8d688b893
author: stevestein
ms.author: sstein
ms.openlocfilehash: 077f7d98470c385b8333fc9db2f55d04bd13bb44
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812963"
---
# <a name="shrink-a-file"></a>Compattare un file
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento si illustra come compattare un file di dati o di log in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Compattando i file di dati si recupera spazio spostando le pagine di dati dalla fine del file allo spazio non occupato più vicino all'inizio del file. Quando alla fine del file viene creato sufficiente spazio libero, le pagine di dati possono essere deallocate e restituite al file system.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per compattare un file di dati o di log usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Le dimensioni del file di dati primario non possono essere inferiori a quelle del file primario nel database model.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   I dati spostati per ridurre un file possono essere dispersi in qualsiasi percorso disponibile nel file, provocando la frammentazione dell'indice e rallentando le prestazioni di query che eseguono ricerche in un intervallo dell'indice Per eliminare la frammentazione, valutare la possibilità di ricompilare gli indici sul file dopo la compattazione.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o al ruolo predefinito del database **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-shrink-a-data-or-log-file"></a>Per compattare un file di dati o di log  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] , quindi espandere questa istanza.  
  
2.  Espandere **Database** , quindi fare clic con il pulsante destro del mouse sul database che si desidera compattare.  
  
3.  Scegliere **Compatta** dal menu **Attività**, quindi fare clic su **File**.  
  
     **Database**  
     Consente di visualizzare il nome del database selezionato.  
  
     **Tipo di file**  
     Consente di selezionare il tipo di file. È possibile scegliere tra file di **Dati** e file di **Log** . La selezione predefinita è **Dati**. La selezione di un tipo di filegroup diverso determina la conseguente modifica delle selezioni negli altri campi.  
  
     **Filegroup**  
     Consente di selezionare un filegroup nell'elenco dei filegroup associato al **Tipo file** selezionato in precedenza. La selezione di un filegroup diverso determina la conseguente modifica delle selezioni negli altri campi.  
  
     **Nome file**  
     Consente di selezionare un file nell'elenco dei file disponibili relativo al filegroup e al tipo di file selezionati.  
  
     **Posizione**  
     Visualizza il percorso completo del file attualmente selezionato. Il percorso non è modificabile, ma può essere copiato negli Appunti.  
  
     **Spazio allocato**  
     In caso di file di dati, visualizza lo spazio attualmente allocato. In caso di file di log, visualizza lo spazio attualmente allocato calcolato in base all'output di DBCC SQLPERF(LOGSPACE).  
  
     **Spazio disponibile**  
     In caso di file di dati, visualizza lo spazio attualmente disponibile calcolato in base all'output di DBCC SHOWFILESTATS(fileid). In caso di file di log, visualizza lo spazio attualmente disponibile calcolato in base all'output di DBCC SQLPERF(LOGSPACE).  
  
     **Rilascia spazio inutilizzato**  
     Causa il rilascio al sistema operativo dello spazio non usato nei file e compatta il file fino all'ultimo extent allocato, riducendo le dimensioni del file senza spostare i dati. Non viene eseguito alcun tentativo di rilocazione delle righe in pagine non allocate.  
  
     **Riorganizza le pagine prima di rilasciare lo spazio inutilizzato**  
     Equivale a eseguire DBCC SHRINKFILE specificando le dimensioni del file di destinazione. Quando questa opzione è selezionata, è necessario specificare le dimensioni del file di destinazione nella casella **Dimensioni file compattato** .  
  
     **Dimensioni file compattato**  
     Consente di specificare le dimensioni del file di destinazione per l'operazione di compattazione. Le dimensioni non possono essere minori dello spazio allocato o maggiori del numero di extent totali allocati al file. L'immissione di un valore non compreso nel limite minimo o massimo determinerà il ripristino dei valori minimo e massimo in seguito alla modifica dello stato attivo o alla pressione di un pulsante sulla barra degli strumenti.  
  
     **Svuota il file eseguendo la migrazione dei dati in altri file nello stesso filegroup**  
     Consente di eseguire la migrazione di tutti i dati dal file specificato. Questa opzione consente l'eliminazione del file tramite l'istruzione ALTER DATABASE. Questa opzione equivale a eseguire DBCC SHRINKFILE con l'opzione EMPTYFILE.  
  
4.  Selezionare il tipo e il nome del file.  
  
5.  Facoltativamente, selezionare la casella di controllo **Rilascia spazio inutilizzato** .  
  
     Se selezionata, questa opzione consente di rilasciare al sistema operativo lo spazio inutilizzato del file e di compattare il file fino all'ultimo extent allocato, riducendo quindi le dimensioni del file senza spostare i dati.  
  
6.  Facoltativamente, selezionare la casella di controllo **Riorganizza i file prima di rilasciare lo spazio inutilizzato** . Se si seleziona questa opzione, è necessario specificare il valore di **Dimensioni file compattato** . Per impostazione predefinita, questa opzione è deselezionata.  
  
     Se selezionata, questa opzione consente di rilasciare al sistema operativo lo spazio inutilizzato del file e di spostare, se possibile, le righe in pagine non allocate.  
  
7.  Facoltativamente, immettere la percentuale massima di spazio libero da rendere disponibile nel database dopo la compattazione. I valori consentiti sono compresi tra 0 e 99. Questa opzione è disponibile solo se l'opzione **Riorganizza i file prima di rilasciare lo spazio inutilizzato** è abilitata.  
  
8.  Facoltativamente, selezionare la casella di controllo **Svuota il file eseguendo la migrazione dei dati in altri file nello stesso filegroup** .  
  
     Se selezionata, questa opzione consente di spostare tutti i dati dal file selezionato ad altri file nel filegroup. È quindi possibile eliminare il file vuoto. L'opzione è equivalente all'esecuzione dell'istruzione DBCC SHRINKFILE con l'opzione EMPTYFILE.  
  
9. Fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-shrink-a-data-or-log-file"></a>Per compattare un file di dati o di log  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio si usano [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) per compattare le dimensioni di un file di dati denominato `DataFile1` nel database `UserDB` a 7 MB.  
  
 [!code-sql[DBCC#DBCC_SHRINKFILE1](../../relational-databases/databases/codesnippet/tsql/shrink-a-file_1.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [DBCC SHRINKDATABASE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md)   
 [Compattare un database](../../relational-databases/databases/shrink-a-database.md)   
 [Eliminare file di dati o file di log da un database](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
  
  
