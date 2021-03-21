---
title: Preparare i dati per l'importazione o l'esportazione bulk
description: In questo articolo viene descritto come pianificare operazioni di importazione ed esportazione bulk, inclusi i requisiti del formato del file di dati e quando usare l'utilità bcp.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], planning
- bulk importing [SQL Server], from a CSV file
- data formats [SQL Server], planning operations
- CSV files [SQL Server]
- quoted fields in CSV files [SQL Server]
ms.assetid: 783fd581-2e5f-496b-b79c-d4de1e09ea30
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: b33c6d9587ddb06d5315d7198663e4a9da84b656
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737711"
---
# <a name="prepare-data-for-bulk-export-or-import"></a>Preparare i dati per l'importazione o l'esportazione bulk
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questa sezione vengono illustrati i fattori da considerare nella pianificazione di operazioni di esportazione bulk, nonché i requisiti per le operazioni di importazione bulk.  
  
> [!NOTE]  
>  In caso di dubbio riguardo alla formattazione da applicare a un file di dati per l'importazione in blocco, usare l'utilità **bcp** per esportare i dati dalla tabella in un file di dati. La formattazione applicata a ogni campo dati di questo file indica la formattazione necessaria per l'importazione bulk dei dati nella colonna della tabella corrispondente. Usare la stessa formattazione per i campi del file di dati da importare.  
  
## <a name="data-file-format-considerations-for-bulk-export"></a>Considerazioni sul formato dei file di dati per l'esportazione bulk  
 Prima di eseguire un'operazione di esportazione in blocco usando il comando **bcp** , tenere presente quanto segue:  
  
-   Quando si esportano dati in un file, il comando **bcp** crea automaticamente il file di dati utilizzando il nome di file specificato. Se tale nome è già usato, il contenuto esistente del file di dati verrà sovrascritto con i dati di cui si intende eseguire la copia bulk.  
  
-   Per eseguire l'esportazione bulk da una tabella o da una vista in un file di dati, è necessario disporre dell'autorizzazione SELECT per la tabella o la vista da copiare.  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può usare analisi parallele per recuperare i dati. Pertanto in generale non c'è nessuna garanzia che le righe della tabella di cui si esegue l'esportazione bulk da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siano riportate in un determinato ordine nel file di dati. Per far sì che le righe della tabella siano disposte in un ordine specifico nel file di dati, usare l'opzione **queryout** per eseguire l'esportazione in blocco da una query e specificare una clausola ORDER BY.  
  
## <a name="data-file-format-requirements-for-bulk-import"></a>Requisiti relativi al formato dei file di dati per l'importazione bulk  
 Per importare dati da un file di dati, il file deve soddisfare i requisiti di base seguenti:  
  
-   È necessario che i dati siano disposti in righe e colonne.  
  
> [!NOTE]  
>  Non è necessario che la struttura del file di dati corrisponda esattamente alla struttura della tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , in quanto è possibile ignorare o riordinare le colonne durante il processo di importazione bulk.  
  
-   È necessario che il file di dati sia in un formato supportato, ad esempio carattere o nativo.  
  
-   Per i dati è possibile usare il formato carattere o binario nativo, incluso Unicode.  
  
-   Per importare dati usando un comando **bcp**, un'istruzione BULK INSERT o un'istruzione INSERT... SELECT * FROM OPENROWSET(BULK...), è necessario che la tabella di destinazione sia già presente.  
  
-   È necessario che ogni campo del file di dati sia compatibile con la colonna corrispondente della tabella di destinazione. Non è ad esempio possibile caricare un campo **int** in una colonna **datetime** . Per altre informazioni, vedere [Formati di dati per l'importazione o l'esportazione in blocco &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md) e [Specificare i formati di dati per la compatibilità con bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md).  
  
    > [!NOTE]  
    >  Per specificare un subset di righe da importare da un file di dati invece dell'intero file, è possibile usare un comando **bcp** con l'opzione **-F** *first_row* e/o l'opzione **-L** *last_row*. Per altre informazioni, vedere [bcp Utility](../../tools/bcp-utility.md).  
  
-   Per importare dati da file di dati con campi a lunghezza o a larghezza fissa, usare un file di formato. Per altre informazioni, vedere [File in formato XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md).  
  
-  A partire da SQL Server 2017, è possibile usare un file CSV come file di dati per un'importazione bulk di dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si noti che il carattere di terminazione del campo di un file CSV non può essere una virgola. Per poter essere usato come file di dati per l'importazione bulk, un file CSV deve essere conforme alle restrizioni seguenti:  
  
    -   I campi dati non possono mai contenere il carattere di terminazione del campo.  
  
    -   Nessuno o tutti i valori in un campo dati sono racchiusi tra virgolette ("").  
  
     Per eseguire un'importazione bulk di dati da un file di tabella di [!INCLUDE[msCoName](../../includes/msconame-md.md)] FoxPro o Visual FoxPro (con estensione dbf) o da un foglio di lavoro di [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] (con estensione xls), è necessario convertire i dati in un file CSV conforme alle restrizioni precedenti. L'estensione del file è in genere csv. È quindi possibile usare il file con estensione csv come file di dati in un'operazione di importazione bulk di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Nei sistemi a 32 bit (SQL Server 2014 e versioni precedenti) è possibile importare dati CSV in una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza operazioni di ottimizzazione dell'importazione in blocco usando [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) con il provider OLE DB per Jet. In Jet i file di testo vengono considerati tabelle il cui schema è definito dal file schema.ini, che si trova nella stessa directory dell'origine dati.  Per i dati CSV uno dei parametri nel file schema.ini è "FORMAT=CSVDelimited". Per usare questa soluzione è necessario acquisire familiarità con il funzionamento di Jet Text IISAM, ovvero con la sintassi della relativa stringa di connessione, l'uso del file schema.ini, le opzioni di impostazione del Registro di sistema e così via.  Le origini migliori per tali informazioni sono costituite dalla Guida di Microsoft Access e dagli articoli della Knowledge Base (KB). Per altre informazioni, vedere [Inizializzazione del driver per l'origine dati di testo](/office/client-developer/access/desktop-database-reference/initializing-the-text-data-source-driver), [Procedura: Usare una query distribuita di SQL Server 7.0 con un server collegato ai database di Access protetti](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/246255), [Procedura: Usare Jet OLE DB Provider 4.0 per la connessione a database ISAM](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/326548) e [Procedura: Aprire file con testo delimitato mediante Text IIsam di Jet Provider](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/262537).  
  
 Per l'importazione bulk di dati da un file a una tabella devono inoltre verificarsi le condizioni seguenti:  
  
-   Gli utenti devono disporre delle autorizzazioni INSERT e SELECT per la tabella, nonché dell'autorizzazione ALTER TABLE quando usano opzioni che prevedono operazioni DDL (Data Definition Language), quali la disabilitazione di vincoli.  
  
-   Quando si esegue l'importazione bulk di dati tramite BULK INSERT o INSERT ... SELECT * FROM OPENROWSET(BULK...), il file di dati deve risultare accessibile per le operazioni di lettura eseguite dal profilo di sicurezza del processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (se l'utente esegue l'accesso usando l'account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibile) oppure dall'account di accesso di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows usato con delega della sicurezza. Per leggere il file, l'utente deve inoltre disporre dell'autorizzazione ADMINISTER BULK OPERATIONS.  
  
> [!NOTE]  
>  L'importazione bulk in una vista partizionata non è supportata e avrà pertanto esito negativo.  
  
## <a name="external-resources"></a>Risorse esterne  
 [Come importare dati da Excel a SQL Server](https://support.microsoft.com/kb/321686)  
  
## <a name="change-history"></a>Cronologia modifiche  
  
|Contenuto aggiornato|  
|---------------------|  
|Aggiunta di informazioni sull'utilizzo del provider OLE DB per Jet per importare dati CSV.|  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Usare il formato carattere per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)   
 [Usare il formato nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
