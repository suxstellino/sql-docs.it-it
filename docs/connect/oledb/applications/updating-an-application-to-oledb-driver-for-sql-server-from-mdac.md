---
title: Aggiornamento di un'applicazione al driver OLE DB per SQL Server da MDAC
description: Informazioni sulle modifiche apportate al vecchio provider di OLE DB per SQL Server e sul nuovo OLE DB Driver per SQL Server e su cosa è necessario sapere per eseguire l'aggiornamento al nuovo driver.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- MSOLEDBSQL, vs. MDAC
- OLE DB Driver for SQL Server, vs. MDAC
- data access [OLE DB Driver for SQL Server], vs. MDAC
- OLE DB Driver for SQL Server, updating applications
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a6acf12dd61b4d772139811b9ead823ded0b635c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751211"
---
# <a name="updating-an-application-to-ole-db-driver-for-sql-server-from-mdac"></a>Aggiornamento di un'applicazione al driver OLE DB per SQL Server da MDAC
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Esistono alcune differenze tra il driver OLE DB per SQL Server e Microsoft Data Access Components (MDAC). A partire da Windows Vista, i componenti di accesso ai dati vengono chiamati Windows Data Access Components (o Windows DAC). Nonostante entrambe le tecnologie consentano l'accesso nativo ai dati dei database di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il driver OLE DB per SQL Server è stato appositamente progettato per esporre le nuove funzionalità di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pur mantenendo la compatibilità con le versioni precedenti.   

 Inoltre, sebbene MDAC contenga componenti per l'utilizzo di OLE DB, ODBC e ActiveX Data Objects (ADO), OLE DB Driver per SQL Server implementa solo OLE DB, anche se ADO può accedere alla funzionalità di OLE DB Driver per SQL Server.  

 OLE DB Driver per SQL Server e MDAC differiscono anche nelle aree seguenti:  

-   Gli utenti che usano ADO per accedere a OLE DB Driver per SQL Server potrebbero riscontrare una funzionalità di filtro meno avanzata quando accedono a un provider OLE DB SQL.  

-   Se un'applicazione ADO usa OLE DB Driver per SQL Server e tenta di aggiornare una colonna calcolata, viene generato un errore. Con MDAC l'aggiornamento è stato accettato, ma ignorato.  

-   OLE DB Driver per SQL Server è un singolo file DLL (libreria a collegamento dinamico) autonomo. Il numero di interfacce esposte pubblicamente è stato ridotto al minimo, per facilitare la distribuzione e limitare al tempo stesso i rischi legati alla sicurezza.  

-   Sono supportate solo le interfacce OLE DB.  

-   I nomi di OLE DB Driver per SQL Server sono diversi dai nomi usati con MDAC.  

-   Le funzionalità accessibili all'utente offerte dai componenti MDAC sono disponibili quando si usa OLE DB Driver per SQL Server. Le più importanti sono le seguenti: pool di connessioni, supporto ADO e supporto del cursore del client. Quando viene usata una di queste funzionalità, OLE DB Driver per SQL Server offre solo la connettività del database. MDAC fornisce funzionalità come la traccia, i controlli di gestione e i contatori delle prestazioni.  

-   Le applicazioni possono usare i servizi di base di OLE DB con il driver OLE DB per SQL Server, ma in presenza del motore del cursore di OLE DB, è necessario selezionare l'opzione relativa alla compatibilità dei tipi di dati per evitare qualsiasi problema potenziale che potrebbe insorgere quando il motore del cursore non conosce i nuovi tipi di dati di [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  

-   OLE DB Driver per SQL Server supporta l'accesso ai database di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] precedenti.  

-   OLE DB Driver per SQL Server non contiene l'integrazione XML. OLE DB Driver per SQL Server supporta SELECT ... FOR, ma non supporta altre funzionalità XML. Tuttavia, OLE DB Driver per SQL Server supporta il tipo di dati **xml** introdotto in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  

-   Il driver OLE DB per SQL Server supporta la configurazione delle librerie di rete sul lato client usando solo gli attributi della stringa di connessione. Per una configurazione delle libreria di rete più completa, è necessario utilizzare Gestione configurazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

-   Le stringhe di connessione MDAC consentono l'utilizzo di un valore booleano (**true**) per la parola chiave **Trusted_Connection**. Una stringa di connessione di OLE DB Driver per SQL Server deve usare **yes** o **no**.  

-   Gli avvisi e gli errori non hanno subito modifiche particolarmente rilevanti. Quelli restituiti dal server mantengono ora lo stesso livello di gravità quando vengono passati al driver OLE DB per SQL Server. Se l'intercettazione di determinati avvisi ed errori riveste un'importanza particolare, è necessario assicurarsi che l'applicazione sia stata sottoposta a test approfonditi.  

-   Il driver OLE DB per SQL Server prevede un controllo degli errori più rigido rispetto a MDAC. Ciò significa che le applicazioni che non sono completamente conformi alle specifiche OLE DB potrebbero avere un comportamento diverso. A differenza del driver OLE DB per SQL Server, il provider SQLOLEDB, ad esempio, non ha applicato la regola in base alla quale i nomi dei parametri devono iniziare con "\@" per i parametri del risultato.  

-   OLE DB Driver per SQL Server si comporta in modo diverso rispetto a MDAC per le connessioni non riuscite. MDAC, ad esempio, restituisce valori delle proprietà memorizzati nella cache per una connessione non riuscita, mentre il driver OLE DB per SQL Server segnala un errore all'applicazione chiamante.  

-   Il driver OLE DB per SQL Server non genera eventi di Visual Studio Analyzer, bensì eventi di traccia di Windows.  

-   Non è possibile usare OLE DB Driver per SQL Server con perfmon. Perfmon è un strumento di Windows disponibile solo con i DSN che utilizzano il driver MDAC SQLODBC incluso in Windows.  

-   Quando OLE DB Driver per SQL Server è connesso a [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] e versioni successive, l'errore del server 16947 viene restituito come SQL_ERROR. Tale errore si verifica quando tramite un'eliminazione o un aggiornamento posizionato non è possibile aggiornare o eliminare una riga. Con MDAC, in caso di connessione a una versione qualsiasi di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'errore del server 16947 viene restituito come avviso (SQL_SUCCESS_WITH_INFO).  

-   Il driver OLE DB per SQL Server implementa l'interfaccia **IDBDataSourceAdmin**, ovvero un'interfaccia OLE DB facoltativa non implementata in precedenza, di cui tuttavia viene implementato solo il metodo **CreateDataSource**. [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  

-   Il driver OLE DB per SQL Server restituisce sinonimi nei set di righe degli schemi TABLES e TABLE_INFO, con TABLE_TYPE impostato su SYNONYM.  

-   I valori restituiti del tipo di dati **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml**, **udt** o altri tipi LOB non possono essere restituiti a versioni client precedenti a [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Se si vuole usare questi tipi come valori restituiti, è necessario usare OLE DB Driver per SQL Server.  

-   A differenza del driver OLE DB per SQL Server, MDAC consente l'esecuzione delle istruzioni seguenti all'inizio delle transazioni manuali e implicite. L'esecuzione deve avvenire in modalità autocommit.  

    -   Tutte le operazioni full-text (DLL indice e catalogo)  

    -   Tutte le operazioni del database (creazione, modifica ed eliminazione del database)  

    -   Riconfigurare  

    -   Shutdown  

    -   Kill  

    -   Backup  

-   Quando le applicazioni MDAC si connettono a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], i tipi di dati introdotti in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] vengono visualizzati come tipi di dati compatibili con [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)], come mostrato nella tabella seguente.  

    |Tipo di SQL Server 2005|Tipo di SQL Server 2000|  
    |--------------------------|--------------------------|  
    |**ntext**|**text**|  
    |**nvarchar(max)**|**ntext**|  
    |**varbinary(max)**|**image**|  
    |**udt**|**varbinary**|  
    |**xml**|**ntext**|  

     Questo mapping dei tipi influisce sui valori restituiti per i metadati delle colonne. Ad esempio, una colonna **text** ha una dimensione massima di 2.147.483.647, mentre OLE DB Driver per SQL Server indica le dimensioni massime delle colonne **varchar(max)** come 2.147.483.647 o -1, a seconda della piattaforma.  

-   Il driver OLE DB per SQL Server supporta l'ambiguità nelle stringhe di connessione. In pratica, alcune parole chiave possono essere specificate più volte e, qualora siano in conflitto, viene adottata una risoluzione basata sulla posizione o sulla precedenza per garantire la compatibilità con le versioni precedenti. Nelle versioni future di OLE DB Driver per SQL Server è possibile che non sia più consentita l'ambiguità nelle stringhe di connessione. Quando si modificano le applicazioni, è consigliabile usare il driver OLE DB per SQL Server per eliminare l'eventuale dipendenza dall'ambiguità delle stringhe di connessione.  

-   Se si usa una chiamata OLE DB per avviare le transazioni, OLE DB Driver per SQL Server e MDAC presentano un comportamento diverso. Con OLE DB Driver per SQL Server le transazioni hanno inizio immediatamente, mentre con MDAC si attende il primo accesso al database. Questo può influire sul comportamento di stored procedure e batch perché per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è necessario che @@TRANCOUNT non cambi al termine dell'esecuzione di un batch o di una stored procedure.  

-   Con OLE DB Driver per SQL Server, ITransactionLocal::BeginTransaction provocherà l'avvio immediato di una transazione. Con MDAC l'avvio della transazione viene ritardato fino all'esecuzione da parte dell'applicazione di un'istruzione che ha richiesto una transazione in modalità implicita. Per altre informazioni, vedere [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../../t-sql/statements/set-implicit-transactions-transact-sql.md).  


 Sia OLE DB Driver per SQL Server che MDAC supportano l'isolamento delle transazioni Read Committed tramite il controllo delle versioni delle righe, ma solo OLE DB Driver per SQL Server supporta l'isolamento delle transazioni snapshot. In termini di programmazione, l'isolamento della transazione Read Committed mediante il controllo delle versioni delle righe equivale a una transazione Read Committed.  

## <a name="see-also"></a>Vedere anche  
 [Compilazione di applicazioni con il driver OLE DB per SQL Server](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
