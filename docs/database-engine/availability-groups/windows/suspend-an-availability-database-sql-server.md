---
title: Sospendere un database di disponibilità
description: Informazioni su come sospendere lo spostamento dei dati per un database all'interno di un gruppo di disponibilità Always On usando SQL Server Management Studio, Transact-SQL o PowerShell.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.suspenddatamove.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], suspending a database
- Availability Groups [SQL Server], databases
ms.assetid: 86858982-6af1-4e80-9a93-87451f0d7ee9
author: cawrites
ms.author: chadam
ms.openlocfilehash: f918daa031b25ba8f1110dc9f2aaeaebef0a9361
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642987"
---
# <a name="suspend-an-availability-database-sql-server"></a>Sospendere un database di disponibilità (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  È possibile sospendere un database di disponibilità in [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Si noti che è necessario eseguire un comando di sospensione nell'istanza del server in cui viene ospitato il database da sospendere o riprendere.  
  
 L'effetto del comando di sospensione dipende dalla scelta di sospendere un database primario o secondario, come segue:  
  
|Database sospeso|Effetto del comando di sospensione|  
|------------------------|-------------------------------|  
|Database secondario|Viene sospeso solo il database secondario locale e il relativo stato di sincronizzazione diventa NOT SYNCHRONIZED. Gli altri database secondari non sono influenzati. Nel database sospeso non vengono più eseguite la ricezione e l'applicazione di dati (record di log) e viene persa la sincronizzazione con il database primario. Le connessioni esistenti nel database secondario leggibile rimangono utilizzabili. Non sono consentite nuove connessioni al database sospeso nel database secondario leggibile finché non viene ripreso lo spostamento di dati. Questo comportamento si applica solo quando le connessioni vengono aperte usando il listener e il routing di sola lettura.<br /><br /> Il database primario rimane disponibile. Se si sospende ogni database secondario corrispondente, il database primario viene eseguito senza mirroring.<br /><br /> **\*\* Importante \*\*** Durante la fase di sospensione di un database secondario, nella coda di invio del database primario corrispondente verranno accumulati record del log delle transazioni non inviati. Tramite le connessioni alla replica secondaria vengono restituiti i dati disponibili quando lo spostamento di dati è stato sospeso.|  
|Database primario|Nel database primario viene arrestato lo spostamento di dati a ogni database secondario connesso. Il database primario rimane in esecuzione, in modalità senza mirroring. Il database primario rimane disponibile ai client e le connessioni esistenti in un database secondario leggibile rimangono utilizzabili ed è possibile effettuare nuove connessioni.|  
  
> [!NOTE]  
>  La sospensione di un database secondario Always On non incide direttamente sulla disponibilità del database primario. Tuttavia, la sospensione di un database secondario può avere un impatto sulle funzionalità di ridondanza e failover del database primario. Questo comportamento è diverso rispetto al mirroring del database, in cui lo stato del mirroring risulta sospeso sia sul database mirror che sul database principale. La sospensione di un database primario AlwaysOn comporta la sospensione dello spostamento di dati su tutti i corrispondenti database secondari e le funzionalità di ridondanza e failover cessano per tale database finché non viene ripreso il database primario.  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Prerequisiti](#Prerequisites)  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per sospendere un database tramite:**  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [PowerShell](#PowerShellProcedure)  
  
-   **Completamento:** [Come evitare il riempimento del log delle transazioni](#FollowUp)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 Un comando SUSPEND viene restituito non appena è stato accettato dalla replica che ospita il database di destinazione, ma la sospensione effettiva del database avviene in modo asincrono.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
 È necessario essere connessi all'istanza del server che ospita il database che si desidera sospendere. Per sospendere un database primario e i database secondari corrispondenti, connettersi all'istanza del server che ospita la replica primaria. Per sospendere un database secondario lasciando disponibile il database primario, connettersi alla replica secondaria.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
 Durante i colli di bottiglia, potrebbe essere utile sospendere brevemente uno o più database secondari per migliorare temporaneamente le prestazioni sulla replica primaria. Finché un database secondario rimane sospeso, il log delle transazioni del database primario corrispondente non può essere troncato. Per questo motivo, i record del log si accumulano sul database primario. È pertanto consigliabile riprendere o rimuovere rapidamente un database secondario sospeso. Per altre informazioni, vedere [Completamento: Come evitare il riempimento del log delle transazioni](#FollowUp), più avanti in questo argomento.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per il database.  
  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 **Per sospendere un database**  
  
1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica di disponibilità in cui si desidera sospendere un database ed espandere l'albero del server. Per altre informazioni, vedere la sessione [Prerequisiti](#Prerequisites)più indietro in questo argomento.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Espandere il gruppo di disponibilità.  
  
4.  Espandere il nodo **Database di disponibilità** , fare clic con il pulsante destro del mouse sul database e scegliere **Sospendi spostamento dati**.  
  
5.  Nella finestra di dialogo **Sospendi spostamento dati** fare clic su **OK**.  
  
     In Esplora oggetti il database sospeso viene contrassegnato con l'icona di un indicatore di pausa.  
  
> [!NOTE]  
>  Per sospendere database aggiuntivi in questo percorso di replica, ripetere i passaggi 4 e 5 per ogni database.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 **Per sospendere un database**  
  
1.  Connettersi all'istanza del server che ospita la replica di cui si desidera sospendere il database. Per altre informazioni, vedere la sessione [Prerequisiti](#Prerequisites)più indietro in questo argomento.  
  
2.  Sospendere il database usando l'istruzione [ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) seguente:  
  
     ALTER DATABASE *nome_database* SET HADR SUSPEND;
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilizzo di PowerShell  
 **Per sospendere un database**  
  
1.  Passare alla directory (**cd**) dell'istanza del server che ospita la replica di cui si vuole sospendere il database. Per altre informazioni, vedere la sessione [Prerequisiti](#Prerequisites)più indietro in questo argomento.  
  
2.  Usare il cmdlet **Suspend-SqlAvailabilityDatabase** per sospendere il gruppo di disponibilità.  
  
     Ad esempio, il seguente comando sospende la sincronizzazione dati per il database di disponibilità `MyDb3` nel gruppo di disponibilità `MyAg` nell'istanza del server denominata `Computer\Instance`.  
  
    ```  
    Suspend-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\Databases\MyDb3  
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-avoiding-a-full-transaction-log"></a><a name="FollowUp"></a> Completamento: Come evitare il riempimento del log delle transazioni  
 In genere, quando su un database viene eseguito un checkpoint automatico, il relativo log delle transazioni viene troncato in corrispondenza di tale checkpoint dopo il successivo backup del log. Tuttavia, quando un database secondario viene sospeso, tutti i record del log correnti rimangono attivi sul database primario. Se il log delle transazioni si riempie, perché raggiunge le dimensioni massime o l'istanza del server esaurisce lo spazio, il database non può eseguire ulteriori aggiornamenti.  
  
 Per evitare il problema, effettuare una delle azioni seguenti:  
  
-   Aggiungere ulteriore spazio di log per il database primario.  
  
-   Riprendere il database secondario prima che il log si riempia. Per ulteriori informazioni, vedere [Riprendere un database di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md).  
  
-   Rimuovere il database secondario. Per altre informazioni, vedere [Rimuovere un database secondario da un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md).  
  
 **Per risolvere i problemi di un log delle transazioni pieno**  
  
-   [Risolvere i problemi relativi a un log delle transazioni completo &#40;Errore di SQL Server 9002&#41;](../../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Riprendere un database di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Riprendere un database di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
