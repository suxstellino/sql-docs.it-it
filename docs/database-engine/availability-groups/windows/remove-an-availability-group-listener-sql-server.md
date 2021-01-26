---
title: Rimuovere un listener del gruppo di disponibilità
description: Descrive come rimuovere un listener del gruppo di disponibilità Always On con SQL Server Management Studio (SSMS), Transact-SQL (T-SQL) o SQL PowerShell.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removeaglistener.default.f1
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
ms.assetid: fd9bba9a-d29f-4c23-8ecd-aaa049ed5f1b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3e2c826818f7b1fd35b3ef86ad0b7245a56b6af8
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766163"
---
# <a name="remove-an-availability-group-listener-sql-server"></a>Rimuovere un listener del gruppo di disponibilità (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo argomento illustra come rimuovere un listener da un gruppo di disponibilità AlwaysOn usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   È necessario essere connessi all'istanza del server che ospita la replica primaria.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Raccomandazioni  
 Prima di eliminare un listener del gruppo di disponibilità, si consiglia di verificare che non sia utilizzato da alcuna applicazione.  
 
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 **Per rimuovere un listener del gruppo di disponibilità**  
  
1.  In Esplora oggetti connettersi all'istanza del server in cui viene ospitata la replica primaria e fare clic sul nome del server per espandere il relativo albero.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Espandere il nodo del gruppo di disponibilità ed espandere il nodo **Listener gruppo di disponibilità** .  
  
4.  Fare clic con il pulsante destro del mouse sul listener da rimuovere, quindi scegliere il comando **Elimina** .  
  
5.  Verrà aperta la finestra di dialogo **Rimuovi listener dal gruppo disponibilità** . Per ulteriori informazioni, vedere [Rimuovi listener dal gruppo di disponibilità](#AgListenerPropertiesDialog), più avanti in questo argomento.  
  
###  <a name="remove-listener-from-availability-group-dialog-box"></a><a name="AgListenerPropertiesDialog"></a> Rimuovi listener dal gruppo disponibilità (finestra di dialogo)  
 **Nome**  
 Nome del listener da rimuovere.  
  
 **Risultato**  
 Viene visualizzato un collegamento, **Esito positivo** o **Errore**, su cui è possibile fare clic per altre informazioni.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per rimuovere un listener del gruppo di disponibilità**  
  
1.  Connettersi all'istanza del server che ospita la replica primaria.  
  
2.  Utilizzare l'istruzione [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , come indicato di seguito:  
  
     ALTER AVAILABILITY GROUP *group_name* REMOVE LISTENER **'** _dns_name_ **'**  
  
     dove *group_name* è il nome del gruppo di disponibilità e *dns_name* è il nome DNS del listener del gruppo di disponibilità.  
  
     Nell'esempio seguente viene eliminato il listener del gruppo di disponibilità `AccountsAG` . Il nome DNS è AccountsAG_Listener.  
  
    ```  
    ALTER AVAILABILITY GROUP AccountsAG REMOVE LISTENER 'AccountsAG_Listener';  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Con PowerShell  
 **Per rimuovere un listener del gruppo di disponibilità**  
  
1.  Impostare il valore predefinito (**cd**) sull'istanza del server che ospita la replica primaria.  
  
2.  Usare il cmdlet **Remove-Item** predefinito per rimuovere un listener. Ad esempio, il seguente comando rimuove il listener `MyListener` dal gruppo di disponibilità `MyAg`.  
  
    ```  
    Remove-Item `   
    SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AGListeners\MyListener  
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Visualizzare le proprietà del listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
