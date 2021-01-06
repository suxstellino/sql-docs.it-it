---
title: Configurare HealthCheckTimeout per il gruppo di disponibilità
description: Configurare HealthCheckTimeout per un gruppo di disponibilità AlwaysOn, che specifica il tempo di attesa della DLL della risorsa di SQL Server prima di segnalare la mancata risposta.
ms.custom: seo-lt-2019
ms.date: 03/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 3bbeb979-e6fc-4184-ad6e-cca62108de74
author: cawrites
ms.author: chadam
ms.openlocfilehash: f1b7fb37f0f72475c86bbaa256eeb3634f9ccb4b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642725"
---
# <a name="configure-healthchecktimeout-property-settings"></a>Configurazione delle impostazioni HealthCheckTimeout
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  L'impostazione HealthCheckTimeout viene usata per specificare la durata, in millisecondi, dell'attesa della DLL della risorsa di SQL Server relativamente alle informazioni restituite dalla stored procedure [sp_server_diagnostics](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) prima di stabilire la mancata risposta da parte dell'istanza del cluster di failover AlwaysOn. Le modifiche apportate alle impostazioni del timeout vengono applicate immediatamente e non richiedono il riavvio della risorsa di SQL Server.  
  
-   **Prima di iniziare:**  [Limitazioni e restrizioni](#Limits), [Sicurezza](#Security)  
  
-   **Per configurare l'impostazione HeathCheckTimeout tramite:**  [PowerShell](#PowerShellProcedure), [Gestione cluster di failover](#WSFC), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Limits"></a> Limitazioni e restrizioni  
 Il valore predefinito di questa proprietà è 30.000 millisecondi (30 secondi). Il valore minimo è 15.000 millisecondi (15 secondi).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre delle autorizzazioni ALTER SETTINGS e VIEW SERVER STATE.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilizzo di PowerShell  
  
##### <a name="to-configure-healthchecktimeout-settings"></a>Per configurare le impostazioni HealthCheckTimeout  
  
1.  Avviare Windows PowerShell con privilegi elevati tramite **Esegui come amministratore**.  
  
2.  Importare il modulo **FailoverClusters** per abilitare i cmdlet del cluster.  
  
3.  Usare il cmdlet **Get-ClusterResource** per cercare la risorsa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , quindi usare il cmdlet **Set-ClusterParameter** per impostare la proprietà **HealthCheckTimeout** per l'istanza del cluster di failover.  
  
> [!TIP]  
>  Ogni volta che viene aperta una nuova finestra di PowerShell, è necessario importare il modulo **FailoverClusters** .  
  
### <a name="example-powershell"></a>Esempio (PowerShell)  
 Nell'esempio seguente, l'impostazione HealthCheckTimeout nella risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] "`SQL Server (INST1)`" viene impostata su 60.000 millisecondi.  
  
```powershell  
Import-Module FailoverClusters  
  
$fci = "SQL Server (INST1)"  
Get-ClusterResource $fci | Set-ClusterParameter HealthCheckTimeout 60000  
  
```  
  
### <a name="related-content-powershell"></a>Contenuto correlato (PowerShell)  
  
-   [Clustering and High-Availability](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) (Failover Clustering and Network Load Balancing Team Blog) (Clustering e disponibilità elevata - Blog del team di clustering di failover e bilanciamento del carico di rete)  
  
-   [Introduzione a Windows PowerShell in un cluster di failover](https://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [Comandi di risorse cluster e cmdlet di Windows PowerShell equivalenti](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619744(v=ws.10)#BKMK_resource)  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WSFC"></a> Utilizzo dello snap-in Gestione cluster di failover  
 **Per configurare le impostazioni HealthCheckTimeout**  
  
1.  Aprire lo snap-in Gestione cluster di failover.  
  
2.  Espandere **Servizi e applicazioni** e selezionare l'istanza del cluster di failover.  
  
3.  Fare clic con il pulsante destro del mouse sulla risorsa **SQL Server** in **Altre risorse** e selezionare **Proprietà** dal menu di scelta rapida. Verrà aperta la finestra di dialogo **Proprietà** della risorsa di SQL Server.  
  
4.  Selezionare la scheda **Proprietà** , immettere il valore desiderato per la proprietà **HealthCheckTimeout** , quindi fare clic su **OK** per applicare la modifica.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 Mediante l'istruzione [ALTER SERVER CONFIGURATION](../../../t-sql/statements/alter-server-configuration-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] , è possibile specificare il valore della proprietà HealthCheckTimeOut.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 Nell'esempio seguente l'opzione HealthCheckTimeout viene impostata su 15.000 millisecondi (15 secondi).  
  
```  
ALTER SERVER CONFIGURATION   
SET FAILOVER CLUSTER PROPERTY HealthCheckTimeout = 15000;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
