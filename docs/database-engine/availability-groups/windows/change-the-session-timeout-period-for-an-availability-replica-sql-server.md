---
title: Modificare il timeout della sessione per una replica del gruppo di disponibilità
description: Illustra come configurare il periodo di timeout della sessione di una replica all'interno di un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], session timeout
- session timeout [SQL Server]
ms.assetid: e23c6e06-1cd1-4d4a-9bc2-e3e06ab2933d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5475a3959bb77dcd7aba00ea409267453f69cc63
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643077"
---
# <a name="modify-the-session-timeout-period-for-an-availability-group-replica"></a>Modificare il periodo di timeout della sessione per una replica del gruppo di disponibilità
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo argomento illustra come configurare il periodo di timeout della sessione di una replica di disponibilità AlwaysOn usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Il periodo di timeout della sessione è una proprietà della replica che determina i secondi di attesa di una replica di disponibilità per una risposta del ping da una replica connessa prima di considerare la connessione non riuscita. Per impostazione predefinita, l'attesa di una replica è di 10 secondi per una risposta del ping. Questa proprietà della replica si applica solo alla connessione tra una determinata replica secondaria e la replica primaria del gruppo di disponibilità. Per altre informazioni sul periodo di timeout della sessione, vedere [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
   
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   È necessario essere connessi all'istanza del server che ospita la replica primaria.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Raccomandazioni  
 È consigliabile usare un periodo di timeout di almeno 10 secondi. Con un valore inferiore a 10 secondi, può verificarsi un sovraccarico del sistema, con perdita di PING e generazione di falsi errori.  
  
  
## <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 **Per modificare il periodo di timeout della sessione per una replica di disponibilità**  
  
1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica primaria ed espandere l'albero del server.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Fare clic sul gruppo di disponibilità di cui si desidera configurare la replica di disponibilità.  
  
4.  Fare clic con il pulsante destro del mouse sulla replica da configurare e scegliere **Proprietà**.  
  
5.  Nella finestra di dialogo **Proprietà replica di disponibilità** usare il campo **Timeout sessione (secondi)** per modificare il numero di secondi per il periodo di timeout della sessione su questa replica.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per modificare il periodo di timeout della sessione per una replica di disponibilità**  
  
1.  Connettersi all'istanza del server che ospita la replica primaria.  
  
2.  Utilizzare l'istruzione [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , come indicato di seguito:  
  
     ALTER AVAILABILITY GROUP *nome_gruppo*  
  
     MODIFY REPLICA ON '*nome_istanza*' WITH ( SESSION_TIMEOUT =*secondi* )  
  
     dove *nome_gruppo* è il nome del gruppo di disponibilità, *nome_istanza* è il nome dell'istanza del server che ospita la replica di disponibilità da modificare e *secondi* specifica il numero minimo di secondi di attesa della replica prima di applicare log ai database quando funge da replica secondaria. Il valore predefinito è 0 secondi, che indica la non applicazione di ritardo.  
  
     Nell'esempio seguente, relativo alla replica primaria del gruppo di disponibilità `AccountsAG` , il valore del timeout della sessione viene impostato su `15` secondi per la replica presente nell'istanza del server `INSTANCE09` .  
  
    ```  
    ALTER AVAILABILITY GROUP AccountsAG   
       MODIFY REPLICA ON 'INSTANCE09' WITH (SESSION_TIMEOUT = 15);  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Con PowerShell  
 **Per modificare il periodo di timeout della sessione per una replica di disponibilità**  
  
1.  Cambiare la directory (**cd**) impostandola sull'istanza del server che ospita la replica primaria.  
  
2.  Usare il cmdlet **Set-SqlAvailabilityReplica** con il parametro **SessionTimeout** per modificare il numero di secondi per il periodo di timeout della sessione su una replica di disponibilità specificata.  
  
     Ad esempio, nel comando seguente viene impostato un periodo della sessione di timeout su 15 secondi.  
  
    ```  
    Set-SqlAvailabilityReplica -SessionTimeout 15 `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
