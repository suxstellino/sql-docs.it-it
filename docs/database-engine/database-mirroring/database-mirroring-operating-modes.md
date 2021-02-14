---
title: Modalità di funzionamento del mirroring del database | Microsoft Docs
description: Informazioni sulle modalità di funzionamento sincrono e asincrono per le sessioni di mirroring del database in SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], operating modes
ms.assetid: f8a579c2-55d7-4278-8088-f1da1de5b2e6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 88bc1762f45a27e94675713557903402cbea2cf7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341527"
---
# <a name="database-mirroring-operating-modes"></a>Modalità di funzionamento del mirroring del database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento vengono illustrate le modalità di funzionamento sincrona e asincrona per le sessioni di mirroring del database.  
  
> [!NOTE]  
>  Per un'introduzione al mirroring del database, vedere [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Termini e definizioni  
 In questa sezione vengono introdotti alcuni termini fondamentali ai fini della comprensione dell'argomento.  
  
 Modalità a prestazioni elevate  
 La sessione di mirroring del database viene eseguita in modo asincrono e utilizza unicamente il server principale e il server mirror. L'unica forma di cambio di ruolo è il servizio forzato (con possibile perdita di dati).  
  
 Modalità a sicurezza elevata  
 La sessione di mirroring del database viene eseguita in modo asincrono e facoltativamente utilizza un server di controllo, nonché il server principale e il server mirror.  
  
 Livello di sicurezza delle transazioni  
 Proprietà del database specifica del mirroring che determina l'esecuzione della sessione di mirroring del database in modalità sincrona o asincrona. Esistono due livelli di sicurezza: FULL e OFF.  
  
 Controllo  
 Per l'utilizzo nella sola modalità a sicurezza elevata. Istanza facoltativa di SQL Server che consente al server mirror di stabilire se avviare un failover automatico. A differenza dei due partner di failover, il server di controllo del mirroring non rende disponibile il database. Il supporto del failover automatico è l'unico ruolo del server di controllo del mirroring.  
  
## <a name="asynchronous-database-mirroring-high-performance-mode"></a>Mirroring asincrono del database (modalità a prestazioni elevate)  
 In questa sezione vengono descritti il funzionamento del mirroring del database asincrono, i casi in cui è consigliabile utilizzare la modalità a elevate prestazioni e le operazioni da effettuare in caso di errore del server principale.  
  
> [!NOTE]  
>  La maggior parte delle edizioni di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] supporta solo il mirroring del database sincrono ("solo sicurezza FULL"). Per informazioni sulle edizioni che supportano completamente il mirroring del database, vedere "Disponibilità elevata (AlwaysOn)" in [Edizioni e funzionalità supportate per SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).
  
 Quando il livello di protezione delle transazioni viene impostato su OFF, la sessione di mirroring del database opera in modo asincrono. Le operazioni asincrone supportano solo una modalità operativa, ovvero la modalità a prestazioni elevate. Questa modalità migliora le prestazioni, ma comporta una riduzione della disponibilità. La modalità a prestazioni elevate utilizza solo il server principale e il server mirror. I problemi relativi al server mirror non hanno mai alcun impatto sul server principale. In caso di perdita del server principale, il database mirror viene contrassegnato come DISCONNECTED, ma rimane disponibile come standby a caldo (warm standby).  
  
 La modalità a prestazioni elevate supporta solo una forma di cambio di ruolo, ovvero il servizio forzato (con possibile perdita dei dati), che prevede l'utilizzo del server mirror come server warm standby. Il servizio forzato rappresenta una delle risposte possibili all'errore del server principale. Poiché può verificarsi una perdita di dati, è consigliabile valutare altre alternative prima di forzare il servizio nel server mirror. Per ulteriori informazioni, vedere [Risposta agli errori del server principale](#WhenPrincipalFails)più avanti in questo argomento.  
  
 Nella figura seguente viene illustrata la configurazione di una sessione con l'utilizzo della modalità a prestazioni elevate.  
  
 ![Configurazione di una sessione per soli partner](../../database-engine/database-mirroring/media/dbm-high-performance-mode.gif "Configurazione di una sessione per soli partner")  
  
 In modalità a prestazioni elevate, non appena il server principale invia il log per una transazione al server mirror, il server principale invia una conferma al client, senza attendere un acknowledgement dal server mirror. Il commit delle transazioni viene eseguito senza attendere la scrittura su disco del log da parte del server mirror. L'operazione asincrona consente l'esecuzione del server principale con una latenza minima per le transazioni.  
  
 Il server mirror tenta di restare sincronizzato con i record del log inviati dal server principale. Il database mirror, tuttavia, è soggetto sempre a un certo ritardo rispetto al database principale. Il divario tra i database è in genere di entità ridotta, ma può diventare significativo se il server principale è soggetto a un ingente carico di lavoro o se il sistema del server mirror è sottoposto a overload.  
  
 **Contenuto della sezione**  
  
-   [Casi in cui la modalità a prestazioni elevate rappresenta la soluzione più appropriata](#WhenUseHighPerf)  
  
-   [Impatto di un server di controllo del mirroring in modalità a prestazioni elevate](#WitnessImpactOnHighPerf)  
  
-   [Risposta agli errori del server principale](#WhenPrincipalFails)  
  
###  <a name="when-is-high-performance-mode-appropriate"></a><a name="WhenUseHighPerf"></a> Casi in cui la modalità a prestazioni elevate rappresenta la soluzione più appropriata  
 La modalità a prestazioni elevate può essere utile in uno scenario di ripristino di emergenza in cui i server principale e mirror sono separati da una distanza significativa e in cui non si desidera che piccoli errori abbiano un impatto sul server principale.  
  
> [!NOTE]  
>  Il log shipping può fungere da integrazione al mirroring del database e offre un'alternativa utile al mirroring asincrono. Per informazioni sui vantaggi del log shipping, vedere [Soluzioni a disponibilità elevata &#40;SQL Server&#41;](../sql-server-business-continuity-dr.md). Per informazioni sull'uso del log shipping con il mirroring del database, vedere [Mirroring del database e log shipping &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-log-shipping-sql-server.md).  
  
###  <a name="the-impact-of-a-witness-on-high-performance-mode"></a><a name="WitnessImpactOnHighPerf"></a> Impatto di un server di controllo del mirroring in modalità a prestazioni elevate  
 Se si utilizza Transact-SQL per configurare la modalità a prestazioni elevate, quando la proprietà SAFETY è impostata su OFF è consigliabile impostare su OFF anche la proprietà WITNESS. Un server di controllo del mirroring può coesistere con la modalità a prestazioni elevate, ma non offre vantaggi e introduce un rischio.  
  
 Se il server di controllo del mirroring viene disconnesso dalla sessione quando uno dei partner si arresta, il database non sarà più disponibile. Questo funzionamento dipende dal fatto che, sebbene per la modalità a prestazioni elevate non sia necessario un server di controllo del mirroring, per la sessione è necessario un quorum costituito da due o più istanze del server. Se la sessione perde il quorum, non può rendere disponibile il database.  
  
 Quando in una sessione in modalità a prestazioni elevate è impostato un server di controllo del mirroring, l'applicazione del quorum comporta quanto segue:  
  
-   Se il server mirror viene perduto, il server principale deve essere connesso al server di controllo del mirroring. In caso contrario, il server principale porta il proprio database offline finché il server di controllo del mirroring o il server mirror non si unisce nuovamente alla sessione.  
  
-   Se il server principale viene perduto, per forzare il servizio sul server mirror è necessario che il server mirror sia connesso al server di controllo del mirroring.  
  
> [!NOTE]  
>  Per informazioni sui tipi di quorum, vedere [Quorum: Impatto di un server di controllo del mirroring sulla disponibilità del database &#40;mirroring del database&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
###  <a name="responding-to-failure-of-the-principal"></a><a name="WhenPrincipalFails"></a> Risposta agli errori del server principale  
 Quando si verifica un errore nel server principale, il proprietario del database può scegliere tra le operazioni seguenti:  
  
-   Rendere il database non disponibile fino a quando il server principale non torna a essere nuovamente disponibile.  
  
     Se il database principale e il relativo log delle transazioni sono intatti, questa scelta consente di mantenere tutte le transazioni di cui è stato eseguito il commit, a fronte di una riduzione della disponibilità.  
  
-   Arrestare la sessione di mirroring del database, aggiornare manualmente il database e quindi avviare una nuova sessione di mirroring del database.  
  
     Se il database principale viene perduto ma il server principale resta in esecuzione, tentare immediatamente di eseguire il backup della parte finale del log nel database principale. Se il backup della parte finale del log ha esito positivo, la rimozione del mirroring può rappresentare l'alternativa più efficace. Dopo avere rimosso il mirroring, è possibile ripristinare il log nel database mirror precedente per mantenere tutti i dati.  
  
    > [!NOTE]  
    >  Se il backup della parte finale del log ha esito negativo e non è possibile attendere il recupero del server principale, valutare la possibilità di forzare il servizio, operazione che consente di mantenere lo stato della sessione.  
  
-   Forzare il servizio (con possibile perdita dei dati) nel server mirror.  
  
     Il servizio forzato rappresenta esclusivamente un metodo di ripristino di emergenza e deve essere utilizzato solo se necessario. È possibile forzare il servizio solo se il server principale non è attivo, la sessione è asincrona (la protezione delle transazioni è impostata su OFF) e non è attivo alcun server di controllo del mirroring (la proprietà WITNESS è impostata su OFF), oppure il server di controllo del mirroring è connesso al server mirror (ovvero è disponibile il quorum).  
  
     Forzando il servizio si consente al server mirror di assumere il ruolo di server principale e di rendere disponibile la propria copia del database ai client. Quando il servizio viene forzato, tutti i log delle transazioni non ancora inviati dal server principale al server mirror vengono perduti. È pertanto consigliabile limitare il servizio forzato alle situazioni in cui la perdita dei dati rappresenta un'opzione accettabile e la disponibilità immediata del database è un fattore critico. Per informazioni sul funzionamento del servizio forzato e sulle procedure di utilizzo consigliate, vedere [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).  
  
##  <a name="synchronous-database-mirroring-high-safety-mode"></a><a name="Sync"></a> Mirroring sincrono del database (modalità a sicurezza elevata)  
 In questa sezione vengono descritti il funzionamento del mirroring del database sincrono e le modalità a protezione elevata alternative (con o senza failover automatico), incluse alcune informazioni sul ruolo del server di controllo del mirroring nel failover automatico.  
  
 Quando il livello di sicurezza delle transazioni viene impostato su FULL, la sessione di mirroring del database viene eseguita in modalità a sicurezza elevata ed opera in modo sincrono dopo una fase di sincronizzazione iniziale. In questa sezione vengono fornite informazioni dettagliate sulle sessioni di mirroring del database configurate per il funzionamento sincrono.  
  
 Per ottenere il funzionamento sincrono per una sessione, è necessario che tramite il server mirror vengano sincronizzati il database mirror e il database principale. All'avvio della sessione, il server principale inizia a inviare il proprio log attivo al server mirror. Appena possibile, il server mirror scrive sul disco tutti i record di log in ingresso. Non appena tutti i record di log ricevuti sono stati scritti su disco, i database vengono sincronizzati. I database rimangono sincronizzati fino a quando i partner restano in comunicazione.  
  
> [!NOTE]  
>  Per monitorare le modifiche dello stato in una sessione di mirroring del database, utilizzare la classe di evento **Database Mirroring State Change** . Per altre informazioni, vedere [Database Mirroring State Change Event Class](../../relational-databases/event-classes/database-mirroring-state-change-event-class.md).  
  
 Al termine della sincronizzazione, per ogni transazione di cui viene eseguito il commit nel database principale viene eseguito il commit anche nel server mirror, a garanzia della protezione dei dati. A tale scopo, per l'esecuzione del commit di una transazione nel database principale si attende fino a quando il server principale riceve un messaggio dal server mirror indicante il consolidamento del log della transazione nel disco. Si noti che l'attesa di tale messaggio comporta un aumento della latenza della transazione.  
  
 Il tempo necessario per la sincronizzazione dipende essenzialmente dal ritardo del database mirror rispetto al database principale al momento dell'avvio della sessione, misurato in base al numero di record di log ricevuti inizialmente dal server principale, dal carico di lavoro nel database principale e dalla velocità del sistema mirror. Dopo la sincronizzazione di una sessione, il log consolidato del quale deve ancora essere eseguito il rollforward nel database mirror rimane nella coda di rollforward.  
  
 Non appena il database mirror è sincronizzato, lo stato di entrambe le copie del database viene impostato come SYNCHRONIZED.  
  
 Il funzionamento sincrono viene mantenuto nel modo seguente:  
  
1.  Quando riceve una transazione da un client, il server principale scrive il log per la transazione nel log delle transazioni.  
  
2.  Il server principale scrive la transazione nel database e, contemporaneamente, invia il record di log al server mirror. Il server principale attende un acknowledgement dal server mirror prima di confermare al client il commit o il rollback di una transazione.  
  
3.  Il server mirror consolida il log nel disco e restituisce un acknowledgement al server principale.  
  
4.  Quando riceve l'acknowledgement dal server mirror, il server principale invia un messaggio di conferma al client.  
  
 La modalità a sicurezza elevata consente di proteggere i dati tramite la richiesta di sincronizzazione tra due posizioni. Tutte le transazioni di cui è stato eseguito il commit vengono scritte nel disco del server mirror.  
  
 **Contenuto della sezione**  
  
-   [Modalità a sicurezza elevata senza failover automatico](#HighSafetyWithOutAutoFailover)  
  
-   [Modalità a sicurezza elevata con failover automatico](#HighSafetyWithAutoFailover)  
  
###  <a name="high-safety-mode-without-automatic-failover"></a><a name="HighSafetyWithOutAutoFailover"></a> Modalità a sicurezza elevata senza failover automatico  
 Nella figura seguente viene illustrata la configurazione della modalità a sicurezza elevata senza failover automatico. La configurazione è composta solo dai due partner.  
  
 ![Comunicazioni tra i partner senza un server di controllo del mirroring](../../database-engine/database-mirroring/media/dbm-high-protection-mode.gif "Comunicazioni tra i partner senza un server di controllo del mirroring")  
  
 Se i partner sono connessi e il database è già sincronizzato, è supportato il failover manuale. Se l'istanza del server mirror si arresta, l'istanza del server principale non ne risentirà e verrà eseguita esposta, ovvero senza mirroring dei dati. Se il server principale viene perso, il mirroring viene sospeso, ma è possibile forzare manualmente il servizio nel server mirror, con possibile perdita di dati. Per altre informazioni, vedere [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)(Mirroring del database e log shipping).  
  
###  <a name="high-safety-mode-with-automatic-failover"></a><a name="HighSafetyWithAutoFailover"></a> Modalità a sicurezza elevata con failover automatico  
 Il failover automatico garantisce disponibilità elevata assicurando il servizio al database anche nel caso di perdita di un server. Per il failover automatico è necessario che la sessione disponga di una terza istanza del server, il *server di controllo del mirroring*, che idealmente dovrebbe trovarsi in un terzo computer. Nella figura seguente viene illustrata la configurazione di una sessione in modalità a sicurezza elevata che supporta il failover automatico.  
  
 ![Server di controllo del mirroring e due partner di una sessione](../../database-engine/database-mirroring/media/dbm-high-availability-mode.gif "Server di controllo del mirroring e due partner di una sessione")  
  
 A differenza dei due partner, il server di controllo del mirroring non serve il database, ma supporta semplicemente il failover automatico mediante la verifica dell'efficienza del server principale. Il server mirror avvia il failover automatico solo se il server mirror e il server di controllo del mirroring rimangono connessi tra loro dopo la disconnessione di entrambi dal server principale.  
  
 Quando viene impostato un server di controllo del mirroring, la sessione richiede il *quorum*, una relazione tra un minimo di due istanze del server che consente di rendere disponibile il database. Per altre informazioni, vedere [Server di controllo del mirroring del database](../../database-engine/database-mirroring/database-mirroring-witness.md) e [Quorum: Impatto di un server di controllo del mirroring sulla disponibilità del database &#40;mirroring del database&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
 Per l'esecuzione del failover automatico è necessario che si verifichino le condizioni seguenti:  
  
-   Il database è già sincronizzato.  
  
-   L'errore si verifica mentre tutte e tre le istanze del server sono connesse e il server di controllo del mirroring rimane connesso al server mirror.  
  
 La perdita di un partner produce l'effetto seguente:  
  
-   Se il server principale non è più disponibile in base alle condizioni precedenti, si verifica il failover automatico. Il server mirror assume il ruolo di server principale e il relativo database diventa il database principale.  
  
-   Se il server principale diventa non disponibile quando queste condizioni non sono soddisfatte, si potrebbe riuscire a forzare il servizio, con una possibile perdita di dati. Per altre informazioni, vedere [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)(Mirroring del database e log shipping).  
  
-   Se solo il server mirror non è più disponibile, le attività nel server principale e nel server di controllo del mirroring continueranno.  
  
 Se la sessione perde il relativo server di controllo del mirroring, per il quorum sono necessari entrambi i partner. Se uno dei partner perde il quorum, entrambi lo perdono e il database diventa non disponibile finché il quorum non viene ristabilito. Questo requisito di quorum garantisce che in assenza un server di controllo del mirroring il database non venga mai eseguito *senza mirroring*.  
  
> [!NOTE]  
>  Se si prevede che il server di controllo del mirroring resterà disconnesso per un periodo di tempo prolungato, è consigliabile rimuoverlo dalla sessione finché non diventa disponibile.  
  
##  <a name="transact-sql-settings-and-database-mirroring-operating-modes"></a><a name="TsqlSettingsAndOpModes"></a> Impostazioni di Transact-SQL e modalità operative del mirroring del database  
 In questa sezione viene descritta una sessione di mirroring del database in termini di impostazioni ALTER DATABASE e degli stati del database con mirroring e del server di controllo del mirroring, se presente. Le informazioni di questa sezione sono rivolte agli utenti che gestiscono il mirroring del database utilizzando principalmente o esclusivamente [!INCLUDE[tsql](../../includes/tsql-md.md)]anziché [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
> [!TIP]  
>  In alternativa a [!INCLUDE[tsql](../../includes/tsql-md.md)], è possibile controllare la modalità operativa di una sessione in Esplora oggetti mediante la pagina **Mirroring** della finestra di dialogo **Proprietà database** . Per altre informazioni, vedere [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
 **Contenuto della sezione**  
  
-   [Effetti della sicurezza delle transazioni e dello stato del server di controllo del mirroring sulla modalità operativa](#TxnSafetyAndWitness)  
  
-   [Visualizzazione dell'impostazione di sicurezza e dello stato del server di controllo del mirroring](#ViewWitness)  
  
-   [Fattori che influiscono sul funzionamento della sessione in caso di perdita del server principale](#FactorsOnLossOfPrincipal)  
  
###  <a name="how-transaction-safety-and-witness-state-affect-the-operating-mode"></a><a name="TxnSafetyAndWitness"></a> Effetti della sicurezza delle transazioni e dello stato del server di controllo del mirroring sulla modalità operativa  
 La modalità operativa di una sessione è determinata dalla combinazione dell'impostazione del livello di protezione delle transazioni e dello stato del server di controllo del mirroring. In qualsiasi momento, il proprietario del database può modificare il livello di sicurezza delle transazioni e aggiungere o rimuovere il server di controllo del mirroring.  
  
 **Contenuto della sezione**  
  
-   [Transaction Safety](#TxnSafety)  
  
-   [Stato del server di controllo del mirroring](#WitnessState)  
  
####  <a name="transaction-safety"></a><a name="TxnSafety"></a> Transaction Safety  
 Il livello di sicurezza delle transazioni è una proprietà del database specifica del mirroring che determina l'esecuzione della sessione di mirroring del database in modalità sincrona o asincrona. Esistono due livelli di sicurezza: FULL e OFF.  
  
-   SAFETY FULL  
  
     Il livello di sicurezza delle transazioni completo determina l'esecuzione della sessione in modo sincrono in modalità a sicurezza elevata. Se è presente un server di controllo del mirroring, una sessione supporta il failover automatico.  
  
     Quando si stabilisce una sessione utilizzando le istruzioni ALTER DATABASE, tale sessione inizia con la proprietà SAFETY impostata su FULL, ovvero in modalità a sicurezza elevata. Dopo l'inizio della sessione, è possibile aggiungere un server di controllo del mirroring.  
  
     Per altre informazioni, vedere [Mirroring sincrono del database (modalità a sicurezza elevata)](#Sync), più indietro in questo argomento.  
  
-   SAFETY OFF  
  
     La disabilitazione della sicurezza delle transazioni determina l'esecuzione della sessione in modalità asincrona a prestazioni elevate. Se la proprietà SAFETY è impostata su OFF, è necessario impostare sul valore predefinito OFF anche la proprietà WITNESS. Per informazioni sull'effetto del server di controllo del mirroring in modalità a prestazioni elevate, vedere [Stato del server di controllo del mirroring](#WitnessState), più avanti in questo argomento. Per informazioni sull'esecuzione con la sicurezza delle transizioni disabilitata, vedere [Mirroring asincrono del database (modalità a prestazioni elevate)](#asynchronous-database-mirroring-high-performance-mode), più indietro in questo argomento.  
  
 L'impostazione della sicurezza delle transazioni per il database viene registrata per ogni partner nelle colonne **mirroring_safety_level** e **mirroring_safety_level_desc** della vista del catalogo **sys.database_mirroring**. Per altre informazioni, vedere [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
 Il proprietario del database può modificare il livello di sicurezza delle transazioni in qualsiasi momento.  
  
####  <a name="the-state-of-the-witness"></a><a name="WitnessState"></a> Stato del server di controllo del mirroring  
 Se è stato impostato un server di controllo del mirroring, il quorum è necessario e pertanto lo stato di tale server è sempre significativo.  
  
 Se è disponibile, il server di controllo del mirroring dispone di due stati:  
  
-   Quando è connesso a un partner, il server di controllo del mirroring si trova nello stato CONNECTED in relazione a tale partner e dispone del quorum con quest'ultimo. In questo caso, è possibile rendere disponibile il database, anche se uno dei partner non è disponibile.  
  
-   Quando è disponibile ma non è connesso a un partner, il server di controllo del mirroring si trova nello stato UNKNOWN o DISCONNECTED in relazione a tale partner. In questo caso, il server di controllo del mirroring non dispone del quorum con tale partner e, se i partner non sono connessi tra loro, il database non sarà più disponibile.  
  
 Per altre informazioni sul quorum, vedere [Quorum: Impatto di un server di controllo del mirroring sulla disponibilità del database &#40;mirroring del database&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
 Lo stato di ogni server di controllo del mirroring in un'istanza del server viene registrato nelle colonne **mirroring_witness_state** e **mirroring_witness_state_desc** della vista del catalogo **sys.database_mirroring**. Per altre informazioni, vedere [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
 Nella tabella seguente sono riepilogati gli effetti dell'impostazione della protezione delle transazioni e dello stato del server di controllo del mirroring sulla modalità operativa di una sessione.  
  
|Modalità operativa|Livello di sicurezza delle transazioni|Stato del server di controllo del mirroring|  
|--------------------|------------------------|-------------------|  
|Modalità a prestazioni elevate|OFF|NULL (nessun server di controllo del mirroring)**|  
|Modalità a sicurezza elevata senza failover automatico|FULL|NULL (nessun server di controllo del mirroring)|  
|Modalità a sicurezza elevata con failover automatico*|FULL|CONNECTED|  
  
 *Se il server di controllo del mirroring viene disconnesso, è consigliabile impostare WITNESS OFF finché l'istanza del server di controllo del mirroring non diventa disponibile.  
  
 **Se un server di controllo del mirroring è presente in modalità a prestazioni elevate, non partecipa alla sessione. Tuttavia, per rendere disponibile il database, è necessario che almeno due istanze del server rimangano connesse. È pertanto consigliabile mantenere la proprietà WITNESS impostata su OFF nelle sessioni in modalità a prestazioni elevate. Per altre informazioni, vedere [Quorum: Impatto di un server di controllo del mirroring sulla disponibilità del database &#40;mirroring del database&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
###  <a name="viewing-the-safety-setting-and-state-of-the-witness"></a><a name="ViewWitness"></a> Visualizzazione dell'impostazione di sicurezza e dello stato del server di controllo del mirroring  
 Per visualizzare l'impostazione di sicurezza e lo stato del server di controllo del mirroring per un database, usare la vista del catalogo **sys.database_mirroring** . Le colonne rilevanti sono le seguenti:  
  
|Fattore|Colonne|Descrizione|  
|------------|-------------|-----------------|  
|Livello di sicurezza delle transazioni|**mirroring_safety_level** o **mirroring_safety_level_desc**|Impostazione del livello di protezione delle transazioni per gli aggiornamenti nel database mirror, scelta tra le seguenti:<br /><br /> UNKNOWN<br /><br /> OFF<br /><br /> FULL<br /><br /> NULL = database non online|  
|Disponibilità di un server di controllo del mirroring|**mirroring_witness_name**|Nome del server di controllo del mirroring del database o valore NULL se tale server non esiste.|  
|Stato del server di controllo del mirroring|**mirroring_witness_state** o **mirroring_witness_state_desc**|Stato del server di controllo del mirroring nel database di un determinato partner:<br /><br /> UNKNOWN<br /><br /> CONNECTED<br /><br /> DISCONNECTED<br /><br /> NULL = server di controllo del mirroring non disponibile o database non online|  
  
 Nel server principale o nel server mirror, ad esempio, immettere:  
  
```  
SELECT mirroring_safety_level_desc, mirroring_witness_name, mirroring_witness_state_desc FROM sys.database_mirroring  
```  
  
 Per altre informazioni su questa vista del catalogo, vedere [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
###  <a name="factors-affecting-behavior-on-loss-of-the-principal-server"></a><a name="FactorsOnLossOfPrincipal"></a> Fattori che influiscono sul funzionamento della sessione in caso di perdita del server principale  
 Nella tabella seguente sono riepilogati gli effetti combinati dell'impostazione del livello di protezione delle transazioni, dello stato del database e dello stato del server di controllo del mirroring sul funzionamento di una sessione di mirroring in caso di perdita del server principale.  
  
|Livello di sicurezza delle transazioni|Stato di mirroring del database mirror|Stato del server di controllo del mirroring|Funzionamento in caso di perdita del server principale|  
|------------------------|----------------------------------------|-------------------|-------------------------------------|  
|FULL|SYNCHRONIZED|CONNECTED|Si verifica il failover automatico.|  
|FULL|SYNCHRONIZED|DISCONNECTED|Il server mirror si arresta. Non è possibile eseguire il failover né rendere disponibile il database.|  
|OFF|SUSPENDED o DISCONNECTED|NULL (nessun server di controllo del mirroring)|È possibile forzare il servizio nel server mirror ma potrebbe verificarsi perdita di dati.|  
|FULL|SYNCHRONIZING o SUSPENDED|NULL (nessun server di controllo del mirroring)|È possibile forzare il servizio nel server mirror ma potrebbe verificarsi perdita di dati.|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Aggiunta o sostituzione di un server di controllo del mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
-   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Aggiungere un server di controllo del mirroring del database tramite l'autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [Rimuovere il server di controllo del mirroring da una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)  
  
-   [Modificare la sicurezza delle transazioni in una sessione di mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Server di controllo del mirroring del database](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
