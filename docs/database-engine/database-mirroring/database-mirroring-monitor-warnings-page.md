---
title: Monitoraggio mirroring del database (pagina Avvisi) | Microsoft Docs
description: Usare questa pagina Monitoraggio mirroring del database di sola lettura per visualizzare gli avvisi supportati negli eventi di mirroring del database e i valori di soglia degli avvisi, se disponibili.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.warningsandalerts.f1
ms.assetid: 01936122-961d-436b-ba3c-5f79fefe5469
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 02acad8ee93c311c18c80e325450105ac47ee876
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642321"
---
# <a name="database-mirroring-monitor-warnings-page"></a>Monitoraggio mirroring del database (pagina Avvisi)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Consente di visualizzare un elenco di sola lettura degli avvisi supportati negli eventi di mirroring del database e i valori soglia degli avvisi specificati, se disponibili.  
  
 **Per utilizzare SQL Server Management Studio per il monitoraggio del mirroring del database**  
  
-   [Avviare il monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="columns"></a>Colonne  
 **Warning**  
 Gli avvisi per i quali è possibile definire una soglia includono:  
  
|Avviso|Soglia|  
|-------------|---------------|  
|**Avvisa se il log non inviato supera la soglia**|Specifica la quantità di log non inviati, espressa in kilobyte (KB), che può accumularsi prima che venga generato un avviso nell'istanza del server principale. Questo avviso consente di quantificare il rischio potenziale di perdita dei dati in termini di KB ed è particolarmente rilevante per la modalità a prestazioni elevate. L'avviso risulta tuttavia utile anche per la modalità a sicurezza elevata quando il mirroring viene sospeso in seguito alla disconnessione dei partner.|  
|**Avvisa se il log non ripristinato supera la soglia**|Specifica la quantità di log non ripristinati, espressa in kilobyte (KB), che può accumularsi prima che venga generato un avviso nell'istanza del server mirror. Questo avviso è utile per una misurazione del tempo di failover in termini di kilobyte. Il *tempo di failover* corrisponde essenzialmente al tempo necessario al server mirror precedente per eseguire il rollforward di tutti i log rimanenti nella propria coda di rollforward, più un breve tempo aggiuntivo.<br /><br /> Nota: Per un failover automatico, il tempo necessario al sistema per rilevare l'errore è indipendente dal tempo di failover.<br /><br /> Per altre informazioni, vedere [Stimare l'interruzione del servizio durante il cambio di ruolo &#40;mirroring del database&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md).|  
|**Avvisa se il tempo di memorizzazione della transazione non inviata meno recente è superiore alla soglia**|Specifica la quantità di transazioni, espressa in minuti, che può accumularsi nella coda di invio prima che venga generato un avviso nell'istanza del server principale. Questo avviso consente di quantificare il rischio potenziale di perdita dei dati in termini di tempo ed è particolarmente rilevante per la modalità a prestazioni elevate. L'avviso risulta tuttavia utile anche per la modalità a sicurezza elevata quando il mirroring viene sospeso in seguito alla disconnessione dei partner.|  
|**Avvisa se l'overhead di commit del mirror supera la soglia**|Specifica il ritardo medio per transazione, espresso in millisecondi, che è consentito prima che venga generato un avviso nell'istanza del server principale. Questo ritardo rappresenta la quantità di overhead generato mentre l'istanza del server principale è in attesa che l'istanza del server mirror scriva il record di log della transazione nella coda di rollforward. Questo valore è rilevante solo nella modalità a sicurezza elevata.|  
  
 **Valore soglia su '** _<server_instance>_ **'**  
 Per ogni avviso, visualizza l'eventuale soglia corrente specificata dall'utente, per una delle istanze del server. Il nome completo dell'istanza del server è indicato nell'intestazione di colonna corrispondente.  
  
 Per ulteriori informazioni, vedere la sezione "Note" più avanti in questo argomento.  
  
 **Imposta valori soglia**  
 Fare clic sul pulsante per impostare una soglia per un avviso su ogni partner di failover.  
  
 Per ulteriori informazioni, vedere la sezione "Note" più avanti in questo argomento.  
  
## <a name="remarks"></a>Osservazioni  
 Se attualmente le informazioni per un'istanza del server non sono disponibili, le celle della colonna **Valore soglia su** sono visualizzate con uno sfondo grigio e un testo filigrana. Se il monitoraggio non è connesso all'istanza del server, in ogni cella della griglia viene visualizzato **Non connesso a** _<NOME_SISTEMA>_ o **Non connesso a** _<NOME_SISTEMA>_ **\\** _<nome_istanza>_ , a seconda che l'istanza sia quella predefinita o una denominata. Se il monitoraggio è in attesa della restituzione di una query, in ogni cella della griglia viene visualizzato **In attesa di dati**.  
  
 Quando le informazioni sono disponibili, nella cella per ogni avviso viene visualizzato un valore soglia specificato (insieme all'unità di misura) o **Non abilitato**.  
  
 Se una soglia viene superata al momento dell'aggiornamento della tabella dello stato, nel registro eventi di Windows viene registrato un evento quando la riga di stato viene registrata. Per impostazione predefinita, la riga di stato viene registrato una volta al minuto quando il monitoraggio non è in esecuzione. È possibile configurare un avviso su ogni tipo di evento registrato utilizzando SQL Server Agent o un altro programma, ad esempio Microsoft Management Operations Manager (MOM).  
  
 Su un determinato partner, gli eventi registrati dipendono dal ruolo corrente del partner, principale o mirror. Tuttavia, è consigliabile impostare una soglia di avviso per un determinato evento su entrambi i partner per assicurare che l'avviso venga mantenuto in caso di failover del database. La soglia appropriata per ogni partner dipende dalle capacità in termini di prestazioni del sistema di tale partner.  
  
> [!NOTE]  
>  È anche possibile usare la stored procedure di sistema **sp_dbmmonitorchangealert** per configurare le soglie per gli eventi equivalenti: log non inviato, log non recuperato, transazione non inviata meno recente e overhead di commit del mirror. Per altre informazioni, vedere [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md).  
  
 Nella tabella seguente è illustrato l'ID evento associato a ogni avviso.  
  
|Avviso di Monitoraggio mirroring del database|Nome evento|ID evento|  
|----------------------------------------|----------------|--------------|  
|**Avvisa se il log non inviato supera la soglia**|Log non inviato|32042|  
|**Avvisa se il log non ripristinato supera la soglia**|Log non ripristinato|32043|  
|**Avvisa se il tempo di memorizzazione della transazione non inviata meno recente è superiore alla soglia**|Transazione non inviata meno recente|32044|  
|**Avvisa se l'overhead di commit del mirror supera la soglia**|Overhead commit mirror|32045|  
  
## <a name="permissions"></a>Autorizzazioni  
 Per l'accesso completo è richiesta l'appartenenza al ruolo predefinito del server **sysadmin** . Solo i membri di **sysadmin** possono configurare e visualizzare i valori soglia degli avvisi per le metriche delle prestazioni chiave.  
  
 L'appartenenza al ruolo **dbm_monitor** consente di visualizzare nella pagina **Avvisi** solo la riga di stato più recente.  
  
## <a name="see-also"></a>Vedere anche  
 [Avviare il monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Avvio della Configurazione guidata sicurezza mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
