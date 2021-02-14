---
title: Monitoraggio mirroring del database (pagina Stato) | Microsoft Docs
description: Usare questa pagina Monitoraggio mirroring del database di sola lettura per visualizzare lo stato del mirroring per le istanze del server principale e del server mirror di un database selezionato.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.status.f1
ms.assetid: 4f64b4e1-89e9-4827-98fa-b92c3dc73b48
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2d912623497cc6dc0baac1c4fbb4c8e794363ce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353115"
---
# <a name="database-mirroring-monitor-status-page"></a>Monitoraggio mirroring del database (pagina Stato)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Questa pagina di sola lettura mostra lo stato di mirroring più recente per le istanze del server principale e mirror del database attualmente selezionato nell'albero di navigazione. Se le informazioni relative a un'istanza non sono attualmente disponibili, alcune delle celle nella griglia **Stato** corrispondenti all'istanza sono disattivate e visualizzano la dicitura **Sconosciuto**.  
  
 **Per utilizzare SQL Server Management Studio per il monitoraggio del mirroring del database**  
  
-   [Avviare il monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>Opzioni  
 **Status**  
 Contiene una griglia che indica lo stato del mirroring di livello elevato più recente di ognuna delle istanze del server principale e mirror. L'ordine delle righe della griglia **Stato** è il seguente:  
  
-   Istanza server principale  
  
-   Istanza server mirror  
  
 Le colonne sono le seguenti:  
  
|Nome colonna|Descrizione|  
|-----------------|-----------------|  
|**Istanza del server**|Nome dell'istanza del server il cui stato è visualizzato nella riga **Stato** .|  
|**Ruolo corrente**|Ruolo corrente dell'istanza del server, **Principale** o **Mirror**.|  
|**Stato mirroring**|Lo stato del mirroring segnalato dall'istanza del server e un'icona che ne indica la gravità. Gli stati possibili e le relative icone sono indicati di seguito:<br /><br /> Icona: -, stato **Sconosciuto**. Il monitoraggio non è connesso ad alcun partner. Le uniche informazioni disponibili sono relative agli elementi memorizzati nella cache dal monitoraggio.<br /><br /> Icona: icona di avviso, stato **Sincronizzazione**. Il contenuto del database mirror è in ritardo rispetto a quello del database principale. L'istanza del server principale invia record di log all'istanza del server mirror, che applica le modifiche al database mirror per eseguirne il rollforward. All'avvio della sessione di mirroring del database, i database mirror e principale sono in questo stato.<br /><br /> Icona: cilindro del database standard, stato **Sincronizzato**. Quando il server mirror è sufficientemente aggiornato rispetto al server principale, lo stato del database diventa **Sincronizzato**. Il database resta in questo stato fino a quando il server principale invia modifiche al server mirror e quest'ultimo le applica al database mirror.  Per la modalità a protezione elevata, il failover automatico e quello manuale sono entrambi possibili, senza perdita di dati.  In modalità a prestazioni elevate è possibile che si verifichi la perdita di dati anche nello stato **Sincronizzato** .<br /><br /> Icona: icona di avviso, stato **Sospeso**. <br />                            Il database principale è disponibile, ma non viene inviato alcun log al server mirror.<br /><br /> Icona: icona di errore, stato **Disconnesso**. L'istanza del server non può connettersi al proprio partner.|  
|**Connessione server di controllo del mirroring del database**|Stato della connessione del server di controllo del mirroring, preceduto da un'icona di stato: **Sconosciuto**, **Connesso** o **Disconnesso**.|  
|**History**|Fare clic per visualizzare la cronologia del mirroring sull'istanza del server. Viene aperta la finestra di dialogo **Cronologia mirroring del database** , in cui sono visualizzate la cronologia dello stato del mirroring e le statistiche per un database con mirroring per un'istanza del server determinata.<br /><br /> Il pulsante **Cronologia** è visualizzato in grigio se il monitoraggio non è connesso all'istanza del server.|  
  
 **Log principale (** *\<time>* **)**  
 Stato del log sull'istanza del server principale all'ora locale nell'istanza del server, indicata da *\<time>* . Sono visualizzati i parametri seguenti:  
  
 **Log non inviato**  
 Quantità del log in attesa nella coda di invio, espressa in KB.  
  
 **Transazione non inviata meno recente**  
 Tempo di memorizzazione della transazione non inviata meno recente nella coda di invio. Il tempo di memorizzazione di questa transazione indica la quantità di transazioni, espressa in minuti, non ancora inviata all'istanza del server mirror. Questo valore consente di misurare la potenziale perdita di dati in termini di tempo.  
  
 **Tempo stimato per l'invio del log**  
 Periodo di tempo approssimativo necessario all'istanza del server principale per inviare il log attualmente nella coda di invio all'istanza del server mirror ( *velocità di invio*). Dato che la frequenza delle transazioni in ingresso può variare notevolmente, il tempo di invio del log rappresenta una stima. Tuttavia, la velocità di invio può essere utile per stimare approssimativamente il tempo necessario per un failover manuale.  
  
 **Velocità di invio corrente**  
 Velocità con cui avviene l'invio delle transazioni all'istanza del server mirror, espressa in kilobyte (KB) al secondo.  
  
 **Frequenza corrente nuove transazioni**  
 Frequenza alla quale le transazioni in entrata vengono immesse nel log del server principale, espressa in KB al secondo. Per stabilire se il mirroring è in ritardo, procede secondo le previsioni o sta recuperando, confrontare questo valore con il valore **Tempo stimato per l'invio del log** .  
  
 **Log mirror (** *\<time>* **)**  
 Stato del log nell'istanza del server mirror all'ora locale nell'istanza del server, indicata da *\<time>* . Sono visualizzati i parametri seguenti:  
  
 **Log non ripristinato**  
 Quantità del log in attesa nella coda di rollforward, espressa in KB.  
  
 **Tempo stimato per il ripristino del log**  
 Numero approssimativo di minuti necessari per l'applicazione del log presente nella coda di rollforward al database mirror.  
  
 **Velocità di ripristino corrente**  
 Frequenza alla quale le transazioni vengono ripristinate nel database mirror, espressa in KB al secondo.  
  
 **Overhead commit mirror**  
 Ritardo medio per transazione, espresso in millisecondi, consentito prima che venga generato un avviso nell'istanza del server principale. Questo ritardo rappresenta la quantità di overhead generato mentre l'istanza del server principale è in attesa che l'istanza del server mirror scriva il record di log della transazione nella coda di rollforward. Questo valore è rilevante solo nella modalità a sicurezza elevata.  
  
 **Tempo stimato per l'invio e il ripristino dell'intero log corrente**  
 Tempo necessario per inviare e ripristinare completamente il log di cui è stato eseguito il commit nell'entità all'ora corrente. Tale periodo può essere inferiore alla somma dei valori nei campi **Tempo stimato per l'invio del log** e **Tempo stimato per il ripristino del log** , dato che l'invio e il ripristino possono avvenire in parallelo. Tale stima non indica il tempo necessario a inviare e ripristinare le nuove transazioni di cui viene eseguito il commit nell'entità mentre vengono elaborati i backlog nella coda di invio.  
  
 **Indirizzo server di controllo del mirroring**  
 Indirizzo di rete dell'istanza del server di controllo del mirroring. Per informazioni sul formato di questo indirizzo, vedere [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
 **Modalità operativa**  
 La modalità operativa della sessione di mirroring del database:  
  
-   **Prestazioni elevate (asincrona)**  
  
-   **Protezione elevata senza failover automatico (sincrona)**  
  
-   **Protezione elevata con failover automatico (sincrona)**  
  
## <a name="remarks"></a>Osservazioni  
 I membri del ruolo predefinito del database **dbm_monitor** possono visualizzare lo stato di mirroring esistente usando Monitoraggio mirroring del database o la stored procedure **sp_dbmmonitorresults** . Questi utenti non possono tuttavia aggiornare la tabella dello stato. Dipendono dal **processo di Monitoraggio mirroring del database** per aggiornare la tabella dello stato a intervalli regolari. Per ottenere la durata dello stato visualizzato l'utente può consultare i tempi nelle etichette **Log principale (** _\<time>_ **)** e **Log mirror (** _\<time>_ **)** .  
  
 Se questo processo non esiste o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent è stato arrestato, lo stato diventa sempre più obsoleto ed è possibile che non rifletta più la configurazione della sessione di mirroring. Dopo un failover, ad esempio, può sembrare che i partner condividano lo stesso ruolo, principale o mirror, oppure il server principale corrente può essere indicato come mirror e, viceversa, il server mirror corrente come principale.  
  
## <a name="see-also"></a>Vedere anche  
 [Avviare il monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Avvio della Configurazione guidata sicurezza mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
