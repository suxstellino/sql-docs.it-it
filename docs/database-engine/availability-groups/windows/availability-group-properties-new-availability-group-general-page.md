---
title: Pagina Generale (finestre di dialogo Nuovo gruppo di disponibilità e Proprietà)
titleSuffix: SQL Server
description: Descrizione delle varie proprietà disponibili nella pagina "Generale" delle finestre di dialogo "Nuovo gruppo di disponibilità" e "Proprietà gruppo di disponibilità" in SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.availabilitygroupproperties.general.f1
ms.assetid: 9af5379f-91b8-4729-9f75-4a80242a30e9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 035b32b57862564039dfd12a38496d7ff323e64f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641074"
---
# <a name="availability-group-properties-new-availability-group-general-page"></a>Proprietà del gruppo di disponibilità: Nuovo gruppo di disponibilità (pagina Generale)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo argomento si applica alla scheda **Generale** della finestra di dialogo **Nuovo gruppo di disponibilità** e della finestra di dialogo **Proprietà gruppo di disponibilità** .  Nella finestra di dialogo **Nuovo gruppo di disponibilità** è possibile creare un nuovo gruppo di disponibilità senza usare la [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]. Nella finestra di dialogo **Proprietà gruppo di disponibilità** è possibile visualizzare e modificare la configurazione di un gruppo di disponibilità esistente.  
  
 **Per visualizzare le proprietà del gruppo di disponibilità**  
  
-   [Visualizzazione delle Proprietà dei gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
-   [Usare il dashboard Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="ui-element-list"></a>Elenco di elementi dell'interfaccia utente  
 **Nome del gruppo di disponibilità**  
 Nome del gruppo di disponibilità. Si tratta di un nome specificato dall'utente che deve essere univoco all'interno del cluster di failover di Windows Server (WSFC).  
  
## <a name="availability-databases"></a>Database di disponibilità  
 **Nome database**  
 Nome di un database aggiunto al gruppo di disponibilità.  
  
 **Aggiungere**  
 Fare clic per aggiungere un database al gruppo di disponibilità.  
  
 **Rimuovi**  
 Fare clic per rimuovere un database selezionato dal gruppo di disponibilità.  
  
## <a name="availability-replicas"></a>Repliche di disponibilità  
 **Istanza del server**  
 Nome del server dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospita la replica corrente e, per un'istanza non predefinita, il nome dell'istanza.  
  
 **Ruolo**  
 **Server/istanza primaria**  
 Attualmente la replica primaria.  
  
 **Server/istanza secondaria**  
 Attualmente una replica secondaria.  
  
 **Risoluzione**  
 È in corso la risoluzione del ruolo della replica nel ruolo primario o secondario.  
  
 **Modalità di disponibilità**  
 Modalità di disponibilità della replica. I valori possibili sono:  
  
 **Commit asincrono**  
 La replica primaria può eseguire il commit delle transazioni senza attendere che la replica secondaria salvi il log su disco.  
  
 **Commit sincrono**  
 La replica primaria attende che la replica secondaria salvi la transazione su disco prima di eseguirne il commit.  
  
 Per altre informazioni, vedere [Modalità di disponibilità &#40;gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 **Modalità di failover**  
 Modalità di failover della replica. I valori possibili sono:  
  
 **Automatico**  
 Failover automatico. La replica è una destinazione per i failover automatici. Questa opzione è supportata solo se la modalità di disponibilità è impostata sul commit sincrono.  
  
 **Manuale**  
 Failover manuale. Il failover della replica può essere eseguito solo manualmente dall'amministratore di database.  
  
 **Connessioni nel ruolo primario**  
 Tipo di connessioni client supportate quando la replica detiene il ruolo primario.  
  
 **Consenti tutte le connessioni**  
 Sono consentite tutte le connessioni ai database nella replica primaria. Si tratta dell'impostazione predefinita.  
  
 **Consenti connessioni in lettura/scrittura**  
 Non sono consentite le connessioni in cui la proprietà di connessione Finalità dell'applicazione è impostata su **ReadOnly** . Se la proprietà Finalità dell'applicazione è impostata su **Lettura/Scrittura** o se tale proprietà non è impostata, la connessione è consentita. Per altre informazioni sulla proprietà di connessione Finalità dell'applicazione, vedere [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 **Secondario leggibile**  
 Specifica se una replica di disponibilità che esegue il ruolo secondario, ovvero una replica secondaria, può accettare connessioni dai client. I valori possibili sono:  
  
 **No**  
 Non sono consentite connessioni dirette ai database secondari di questa replica. I database non sono disponibili per l'accesso in lettura. Si tratta dell'impostazione predefinita.  
  
 **Solo con finalità di lettura**  
 Sono consentite solo connessioni dirette di sola lettura ai database secondari di questa replica. Il database o i database secondari sono tutti disponibili per l'accesso in lettura.  
  
 **Sì**  
 Sono consentite tutte le connessioni ai database secondari di questa replica, ma solo per l'accesso in lettura. Il database o i database secondari sono tutti disponibili per l'accesso in lettura.  
  
 **Timeout sessione (secondi)**  
 Numero di secondi per il periodo di timeout della sessione su questa replica.  
  
 **URL endpoint**  
 URL dell'endpoint. Per informazioni sul formato di questi URL, vedere [Specificare l'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
 **Aggiungere**  
 Fare clic per aggiungere una replica secondaria al gruppo di disponibilità.  
  
 **Rimuovi**  
 Fare clic per rimuovere una replica secondaria specificata dal gruppo di disponibilità.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
