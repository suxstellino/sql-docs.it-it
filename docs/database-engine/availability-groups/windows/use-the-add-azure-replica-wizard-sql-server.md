---
title: Configurare una macchina virtuale di Azure come replica secondaria in un gruppo di disponibilità
description: La procedura guidata Aggiungi replica Azure consente di creare una nuova macchina virtuale di Azure in un ambiente IT ibrido e di configurarla come replica secondaria per un gruppo di disponibilità Always On nuovo o esistente.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.azurereplica.f1
ms.assetid: b89cc41b-07b4-49f3-82cc-bc42b2e793ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7fb9c44c90dbedbdc0a123f4b8c09088f7392d50
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641820"
---
# <a name="configure-azure-vm-as-a-secondary-replica-in-an-availability-group"></a>Configurare una macchina virtuale di Azure come replica secondaria in un gruppo di disponibilità
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  La procedura guidata Aggiungi replica Azure consente di creare una nuova macchina virtuale di Azure in un ambiente IT ibrido e di configurarla come replica secondaria per un gruppo di disponibilità Always On nuovo o esistente.  

>  [!IMPORTANT]  
>  Azure offre due diversi modelli di distribuzione per creare e usare le risorse: Gestione risorse e la distribuzione classica. Questo articolo illustra l'uso del modello di distribuzione classica. Microsoft consiglia di usare il modello di Gestione risorse per le distribuzioni più recenti. I passaggi descritti in questo articolo non sono applicabili se si distribuisce la macchina virtuale di Azure usando il modello di Resource Manager.   

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
 Se non è mai stata aggiunta una replica di disponibilità a un gruppo di disponibilità, vedere le sezioni "Istanze del server" e "Repliche e gruppi di disponibilità" in [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   È necessario essere connessi all'istanza del server che ospita la replica primaria corrente.  
  
-   È necessario usare un ambiente IT ibrido in cui nella subnet locale sia presente una VPN da sito a sito con Azure. Per altre informazioni, vedere [Creare una rete virtuale con una connessione VPN da sito a sito usando il portale di Azure classico](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal).  
  
-   Il gruppo di disponibilità deve contenere le repliche di disponibilità locali.  
  
-   I client nel listener del gruppo di disponibilità devono avere la connettività a Internet se vogliono mantenere la connettività con il listener quando viene eseguito il failover del gruppo di disponibilità in una replica di Azure.  
  
-   **Prerequisiti per l'utilizzo della sincronizzazione dei dati iniziale completa** È necessario specificare una condivisione di rete affinché la procedura guidata possa creare e accedere ai backup. Per la replica primaria, all'account usato per avviare il [!INCLUDE[ssDE](../../../includes/ssde-md.md)] devono essere associate le autorizzazioni del file system in lettura e scrittura per una condivisione di rete. Per le repliche secondarie, all'account deve essere associata l'autorizzazione di lettura per la condivisione di rete.  
  
     Se non è possibile usare la procedura guidata per eseguire la sincronizzazione dei dati iniziale completa, sarà necessario preparare i database secondari manualmente. Tale operazione può essere eseguita prima o dopo l'esecuzione della procedura guidata. Per altre informazioni, vedere [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  
  
 È inoltre necessaria l'autorizzazione CONTROL ON ENDPOINT se si desidera gestire l'endpoint del mirroring del database tramite la Procedura guidata Aggiungi replica a gruppo di disponibilità.  
  
##  <a name="using-the-add-azure-replica-wizard-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo della procedura guidata Aggiungi replica Azure (SQL Server Management Studio)  

>  [!IMPORTANT]  
>  La procedura guidata per l'aggiunta della replica di Azure supporta solo le macchine virtuali create con il modello di distribuzione classica. Le nuove distribuzioni di macchine virtuali devono usare il modello di Resource Manager più recente. Se si usano macchine virtuali con Resource Manager, è necessario aggiungere manualmente la replica secondaria di Azure usando comandi Transact-SQL, che non sono descritti in questo articolo. La procedura guidata non funziona in uno scenario di Resource Manager. 
>
>  La procedura guidata Aggiungi replica Azure non è disponibile nelle versioni più recenti (versioni 18.x e 17.x) di SQL Server Management Studio.
        
 La procedura guidata Aggiungi replica Azure può essere avviata dalla [Pagina Specifica repliche](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md). Questa pagina può essere visualizzata in due modi:  
  
-   [Usare la Creazione guidata Gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Usare la procedura guidata Aggiungi replica a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
 Dopo aver avviato la procedura guidata Aggiungi replica Azure effettuare i passaggi seguenti:  
  
1.  Scaricare prima un certificato di gestione per la sottoscrizione di Azure. Fare clic su **Download** per aprire la pagina di accesso.  
  
2.  Accedere a Microsoft Azure con il proprio account Microsoft o aziendale. L'account Microsoft o aziendale è nel formato di un indirizzo di posta elettronica, ad esempio HYPERLINK "mailto:patc@contoso.com" patc@contoso.com. Per altre informazioni sulle credenziali di Azure, vedere [Domande frequenti sull'account Microsoft per le organizzazioni](/previous-versions/jj592903(v=msdn.10)) e [Risoluzione dei problemi di accesso con l'account dell'organizzazione](https://support.microsoft.com/kb/2756852).  
  
3.  Connettersi quindi alla sottoscrizione facendo clic su **Connetti**. Dopo aver eseguito la connessione, gli elenchi a discesa vengono popolati con i parametri di Azure, ad esempio **Rete virtuale** e **Subnet rete virtuale**.  
  
4.  Specificare le impostazioni per la macchina virtuale di Azure che ospiterà la nuova replica secondaria:  
  
     Immagine  
     Nome dell'immagine di SQL Server da usare per la macchina virtuale di Azure  
  
     Dimensioni macchina virtuale  
     Dimensioni della macchina virtuale di Azure  
  
     Nome macchina virtuale  
     Nome DNS della macchina virtuale di Azure  
  
     Nome utente VM  
     Nome utente dell'amministratore predefinito della macchina virtuale di Azure  
  
     Password amministratore VM (e Conferma password)  
     Password dell'amministratore predefinito della macchina virtuale di Azure  
  
     Rete virtuale  
     Rete virtuale in cui posizionare la macchina virtuale di Azure  
  
     Subnet rete virtuale  
     Subnet della rete virtuale in cui posizionare la macchina virtuale di Azure  
  
     Dominio  
     Dominio Active Directory (AD) a cui aggiungere la macchina virtuale di Azure  
  
     Nome utente di dominio  
     Nome utente di Active Directory usato per aggiungere la macchina virtuale di Azure al dominio  
  
     Password  
     Password usata per aggiungere la macchina virtuale di Azure al dominio  
  
5.  Fare clic su **OK** per eseguire il commit delle impostazioni e chiudere la procedura guidata Aggiungi replica Azure.  
  
6.  Continuare con il resto dei passaggi di configurazione della [Pagina Specifica repliche](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md) come per qualsiasi nuova replica.  
  
     Dopo avere completato la creazione guidata gruppo di disponibilità o la procedura guidata Aggiungi replica a gruppo di disponibilità, il processo di configurazione esegue tutte le operazioni in Azure per creare la nuova macchina virtuale, aggiungerla al dominio di Active Directory, aggiungerla al cluster di Windows, abilitare la Disponibilità elevata Always On e aggiungere la nuova replica al gruppo di disponibilità.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Aggiungere una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Aggiungere una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
