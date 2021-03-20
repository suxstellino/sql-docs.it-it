---
title: Proprietà server (pagina Avanzate) | Microsoft Docs
description: Acquisire familiarità con le impostazioni avanzate del server di SQL Server. Informazioni sulle opzioni FILESTREAM, varie soglie, alternative di aggiornamento full-text e altre impostazioni.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.advanced.f1
ms.assetid: cc5e65c2-448e-4f37-9ad4-2dfb1cc84ebe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5df2313ce8dc5067828b8d21dc416694b2c17853
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673523"
---
# <a name="server-properties---advanced-page"></a>Proprietà server (pagina Avanzate)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilizzare questa pagina per visualizzare o modificare le impostazioni del server avanzate.  
  
 **Per visualizzare le pagine Proprietà server**  
  
-   [Visualizzare o modificare le proprietà del server &#40;SQL Server&#41;](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)  
  
## <a name="containment"></a>Containment  
 Abilitazione di database indipendenti  
 Indica se questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente database indipendenti. Se **True**, il database indipendente può essere creato, ripristinato o collegato. Se **False**, il database indipendente non può essere creato, ripristinato o collegato all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La modifica della proprietà di indipendenza può influire sulla sicurezza del database. Se si abilitano i database indipendenti, ai relativi proprietari viene concesso l'accesso all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Disabilitando i database indipendenti è possibile impedire agli utenti di effettuare la connessione. Per informazioni sull'impatto della proprietà di indipendenza, vedere [Database indipendenti](../../relational-databases/databases/contained-databases.md) e [Procedure consigliate per la sicurezza in database indipendenti](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
## <a name="filestream"></a>FILESTREAM  
 **Livello di accesso FILESTREAM**  
 Indica il livello corrente del supporto FILESTREAM nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per modificare il livello di accesso, selezionare uno dei valori seguenti:  
  
 **Disabilitato**  
 I dati di oggetti binari di grandi dimensioni (BLOB) non possono essere archiviati nel file system. Si tratta del valore predefinito.  
  
 **Accesso Transact-SQL abilitato**  
 I dati FILESTREAM sono accessibili tramite [!INCLUDE[tsql](../../includes/tsql-md.md)], ma non tramite il file system.  
  
 **Accesso completo abilitato**  
 I dati FILESTREAM sono accessibili tramite [!INCLUDE[tsql](../../includes/tsql-md.md)] e il file system.  
  
 La prima volta che si abilita FILESTREAM, può essere necessario riavviare il computer per configurare i driver.  
  
 **Nome condivisione FILESTREAM**  
 Consente di visualizzare il nome di sola lettura della condivisione di FILESTREAM selezionato durante l'installazione. Per altre informazioni, vedere [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
## <a name="miscellaneous"></a>Varie  
 **Consenti attivazione trigger da altri trigger**  
 Consente l'attivazione di trigger da altri trigger. I trigger possono essere nidificati fino a un massimo di 32 livelli. Per altre informazioni, vedere la sezione relativa ai trigger annidati in [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 **Blocked Process Threshold**  
 Soglia, in secondi, in corrispondenza della quale vengono generati i report sui processi bloccati. La soglia può essere compresa tra 0 e 86.400. Per impostazione predefinita, non vengono generati report relativi ai processi bloccati. Per altre informazioni, vedere [Opzione di configurazione del server blocked process threshold](../../database-engine/configure-windows/blocked-process-threshold-server-configuration-option.md).  
  
 **Cursor Threshold**  
 Consente di specificare il numero di righe del set del cursore oltre il quale i keyset del cursore vengono generati in modo asincrono. Quando i cursori generano un keyset per un set di risultati, Query Optimizer produce una stima del numero di righe che verranno restituite per tale set di risultati. Se il numero stimato di righe restituite supera la soglia, il cursore viene generato in modo asincrono ed è pertanto possibile recuperare le righe dal cursore durante il popolamento del cursore stesso. In caso contrario, il cursore viene generato in modo sincrono e la query attende che vengano restituite tutte le righe.  
  
 Se si imposta su -1, tutti i keyset vengono generati in modo sincrono, a vantaggio dei set di cursori di dimensioni ridotte. Se si imposta su 0, tutti i keyset del cursore vengono generati in modo asincrono. Se si impostano altri valori, Query Optimizer analizza il numero di righe previste nel set del cursore e se questo numero supera il valore impostato, compila il keyset in modo asincrono. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server cursor threshold](../../database-engine/configure-windows/configure-the-cursor-threshold-server-configuration-option.md).  
  
 **Default Full-Text Language**  
 Specifica una lingua predefinita per le colonne con indicizzazione full-text. L'analisi linguistica dei dati con indicizzazione full-text dipende dalla lingua dei dati. Il valore predefinito per questa opzione corrisponde alla lingua impostata per il server. Per la lingua corrispondente all'impostazione visualizzata, vedere [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md).  
  
 **Lingua predefinita**  
 Lingua predefinita per tutti i nuovi account di accesso, salvo diversa impostazione.  
  
 **Opzione di aggiornamento full-text**  
 Consente di controllare il modo in cui viene eseguita la migrazione degli indici full-text durante l'aggiornamento di un database da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Questa proprietà si applica ai casi in cui viene eseguito l'aggiornamento tramite il collegamento di un database, il ripristino di un backup di database o di un backup di file oppure la copia del database tramite la Copia guidata database.  
  
 Sono disponibili le alternative seguenti:  
  
 **Importa**  
 I cataloghi full-text vengono importati. Questa operazione è molto più rapida di **Ricompila**. Un catalogo full-text importato, tuttavia, non utilizza i nuovi word breaker ottimizzati introdotti in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]. Di conseguenza, potrebbe essere necessario ricompilare i cataloghi full-text.  
  
 Se un catalogo full-text non è disponibile, gli indici full-text associati vengono ricreati. Questa opzione è disponibile solo per i database di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
 **Ricostruzione**  
 I cataloghi full-text vengono ricompilati utilizzando i nuovi word breaker ottimizzati. La ricompilazione degli indici può richiedere tempo e dopo l'aggiornamento potrebbe essere necessaria una quantità significativa di CPU e di memoria.  
  
 **Reimpostazione**  
 I cataloghi full-text vengono ripristinati. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] I file del catalogo full-text vengono rimossi, ma i metadati per i cataloghi e per gli indici full-text vengono mantenuti. Dopo l'aggiornamento, in tutti gli indici full-text il rilevamento delle modifiche viene disabilitato e le ricerche per indicizzazione non vengono avviate automaticamente. Il catalogo resterà vuoto fino a quando non si eseguirà manualmente un popolamento completo al termine dell'aggiornamento.  
  
 Per informazioni sulla scelta dell'opzione di aggiornamento full-text, vedere [Aggiornamento della ricerca full-text](../../relational-databases/search/upgrade-full-text-search.md).  
  
> [!NOTE]  
>  L'opzione di aggiornamento full-text può anche essere impostata con l'azione [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)upgrade_option.  
  
 Una volta collegato, ripristinato o copiato un [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] database, il database diventa immediatamente disponibile e viene aggiornato automaticamente. Se il database include indici full-text, questi vengono importati, reimpostati o ricompilati dal processo di aggiornamento, a seconda dell'impostazione della proprietà del server **Opzione di aggiornamento full-text** . Se l'opzione di aggiornamento è impostata su **Importa** o **Ricompila**, gli indici full-text non saranno disponibili durante l'aggiornamento. A seconda della quantità di dati indicizzati, l'importazione può richiedere diverse ore, mentre la ricompilazione può risultare dieci volte più lunga. Si noti inoltre che, quando l'opzione di aggiornamento è impostata su **Importa** e un catalogo full-text non è disponibile, gli indici full-text associati vengono ricompilati. Per informazioni sulla visualizzazione o sulla modifica dell'impostazione della proprietà **Opzione di aggiornamento full-text** , vedere [Gestione e monitoraggio della ricerca full-text per un'istanza del server](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md).  
  
 **Max Text Replication Size**  
 Specifica le dimensioni massime, in byte, di dati di tipo **text**, **ntext**, **varchar(max)** , **nvarchar(max)** , **xml** e **image** che è possibile aggiungere a una colonna replicata o acquisita tramite un'unica istruzione INSERT, UPDATE, WRITETEXT o UPDATETEXT. La modifica dell'impostazione diventa effettiva immediatamente. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md).  
  
 **Scan For Startup Procs**  
 Consente di specificare che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve eseguire un'analisi per l'esecuzione automatica di stored procedure all'avvio. Se questa opzione è impostata su **True**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procede all'analisi ed esegue automaticamente tutte le stored procedure definite nel server. Se è impostata su **False** , ovvero il valore predefinito, non viene eseguita alcuna analisi. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server scan for startup procs](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md).  
  
 **Cambio data per anno a due cifre**  
 Indica il numero più alto che può essere immesso come anno a due cifre. L'anno indicato e i 99 anni precedenti possono essere immessi con due cifre. Tutti gli altri anni devono essere immessi con quattro cifre.  
  
 Ad esempio, l'impostazione predefinita 2049 indica che la data '14/03/49' verrà interpretata come 14 marzo 2049, mentre la data '14/03/50' verrà interpretata come 14 marzo 1950. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server two-digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).  
  
## <a name="network"></a>Rete  
 **Dimensioni pacchetto di rete**  
 Consente di impostare le dimensioni in byte del pacchetto utilizzate nell'intera rete. Le dimensioni predefinite del pacchetto sono pari a 4.096 byte. Se un'applicazione esegue operazioni di copia bulk o invia e riceve quantità elevate di dati **text** o **image** , l'utilizzo di pacchetti di dimensioni maggiori rispetto a quelle predefinite può determinare un miglioramento delle prestazioni, poiché riduce il numero di operazioni di lettura e scrittura di rete. Se un'applicazione invia e riceve quantità limitate di dati, è possibile impostare le dimensioni del pacchetto su 512 byte, un valore sufficiente per la maggior parte dei trasferimenti. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server network packet size](../../database-engine/configure-windows/configure-the-network-packet-size-server-configuration-option.md).  
  
> [!NOTE]  
>  È consigliabile modificare le dimensioni dei pacchetti solo se si è certi che l'operazione determinerà un miglioramento delle prestazioni. Per la maggior parte delle applicazioni, le dimensioni predefinite risultano ottimali.  
  
 **Remote Login Timeout**  
 Consente di specificare il numero di secondi di attesa dell'esito di un tentativo di accesso remoto non riuscito da parte di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questa impostazione ha effetto sulle connessioni a provider OLE DB create per l'esecuzione di query eterogenee. Il valore predefinito è 20 secondi. Il valore 0 determina un tempo di attesa infinito. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server remote login timeout](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md).  
  
 La modifica dell'impostazione diventa effettiva immediatamente.  
  
## <a name="parallelism"></a>Parallelism (Parallelismo):  
 **Cost Threshold for Parallelism**  
 Consente di specificare la soglia oltre la quale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea ed esegue piani paralleli per le query. Il costo equivale al tempo (in secondi) stimato per l'esecuzione del piano seriale in una configurazione hardware specifica. Impostare questa opzione solo su multiprocessori simmetrici. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server cost threshold for parallelism](../../database-engine/configure-windows/configure-the-cost-threshold-for-parallelism-server-configuration-option.md).  
  
 **Locks**  
 Consente di impostare il numero massimo di blocchi disponibili e pertanto di limitare la quantità di memoria utilizzata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per i blocchi. L'impostazione predefinita è 0. Questa impostazione consente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di allocare e deallocare i blocchi dinamicamente in base alle variazioni dei requisiti di sistema.  
  
 La configurazione consigliata prevede che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzi i blocchi dinamicamente. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server locks](../../database-engine/configure-windows/configure-the-locks-server-configuration-option.md).  
  
 **Max Degree of Parallelism**  
 Consente di limitare il numero dei processori da utilizzare per l'esecuzione di piani paralleli (fino a un massimo di 64). Con il valore predefinito, ovvero 0, vengono utilizzati tutti i processori disponibili. Se il valore è 1, viene soppressa la generazione di piani paralleli. Se il valore è maggiore di 1, viene limitato il numero massimo di processori utilizzati dall'esecuzione di una singola query. Se il valore è maggiore di quello dei processori disponibili, viene utilizzato il numero effettivo di processori disponibili. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
  
 **Query Wait**  
 Consente di specificare il tempo in secondi (da 0 a 2.147.483.647) prima che si verifichi il timeout di una query in attesa di risorse. Se viene utilizzato il valore predefinito -1, il timeout viene impostato automaticamente su un valore pari a 25 volte il costo previsto della query. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server query wait](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  
  
  
