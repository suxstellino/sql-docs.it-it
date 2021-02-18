---
title: Configurare le impostazioni della proprietà FailureConditionLevel
description: Usare la proprietà FailureConditionLevel (FCI) per impostare le condizioni per il failover o il riavvio dell'istanza del cluster di failover AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 513dd179-9a46-46da-9fdd-7632cf6d0816
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4cbfa09010bbe5e0523049d109e499fb47e416b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346648"
---
# <a name="configure-failureconditionlevel-property-settings"></a>Configurare le impostazioni della proprietà FailureConditionLevel
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Usare la proprietà FailureConditionLevel (FCI) per impostare le condizioni per il failover o il riavvio dell'istanza del cluster di failover AlwaysOn. Le modifiche apportate a questa proprietà vengono applicate immediatamente senza richiedere il riavvio del servizio cluster di failover di Windows Server (WSFC) o della risorsa istanza cluster di failover.  
  
-   **Prima di iniziare:**  [Impostazioni della proprietà FailureConditionLevel](#Restrictions), [Sicurezza](#Security)  
  
-   **Per configurare le impostazioni della proprietà FailureConditionLevel usando** [PowerShell](#PowerShellProcedure), [Gestione cluster di failover](#WSFC), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="failureconditionlevel-property-settings"></a><a name="Restrictions"></a> Impostazioni della proprietà FailureConditionLevel  
 Le condizioni di errore vengono impostate in base a un ordine crescente. In ognuno dei livelli 1-5 sono incluse tutte le condizioni dei livelli precedenti oltre alle proprie condizioni specifiche. Pertanto, in ogni livello la probabilità di un failover o di un riavvio è maggiore.  Per ulteriori informazioni, vedere la sezione "Determinazione di errori" nell'argomento [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md) .  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre delle autorizzazioni ALTER SETTINGS e VIEW SERVER STATE.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilizzo di PowerShell  
  
##### <a name="to-configure-failureconditionlevel-settings"></a>Per configurare le impostazioni FailureConditionLevel  
  
1.  Avviare Windows PowerShell con privilegi elevati tramite **Esegui come amministratore**.  
  
2.  Importare il modulo **FailoverClusters** per abilitare i cmdlet del cluster.  
  
3.  Usare il cmdlet **Get-ClusterResource** per trovare la risorsa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , quindi usare il cmdlet **Set-ClusterParameter** per impostare la proprietà **FailureConditionLevel** per un'istanza del cluster di failover.  
  
> [!TIP]  
>  Ogni volta che viene aperta una nuova finestra di PowerShell, è necessario importare il modulo **FailoverClusters** .  
  
### <a name="example-powershell"></a>Esempio (PowerShell)  
 Nell'esempio seguente, l'impostazione FailureConditionLevel nella risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] "`SQL Server (INST1)`" viene impostata per eseguire il failover o il riavvio in caso di errori critici del server.  
  
```powershell  
Import-Module FailoverClusters  
  
$fci = "SQL Server (INST1)"  
Get-ClusterResource $fci | Set-ClusterParameter FailureConditionLevel 3  
  
```  
  
### <a name="related-content-powershell"></a>Contenuto correlato (PowerShell)  
  
-   [Clustering and High-Availability](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) (Failover Clustering and Network Load Balancing Team Blog) (Clustering e disponibilità elevata - Blog del team di clustering di failover e bilanciamento del carico di rete)  
  
-   [Introduzione a Windows PowerShell in un cluster di failover](https://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [Comandi di risorse cluster e cmdlet di Windows PowerShell equivalenti](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619744(v=ws.10)#BKMK_resource)  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WSFC"></a> Utilizzo dello snap-in Gestione cluster di failover  
 **Per configurare le impostazioni della proprietà FailureConditionLevel:**  
  
1.  Aprire lo snap-in Gestione cluster di failover.  
  
2.  Espandere **Servizi e applicazioni** e selezionare l'istanza del cluster di failover.  
  
3.  Fare clic con il pulsante destro del mouse su **Risorsa di SQL Server** in **Altre risorse**, quindi, nel menu, selezionare **Proprietà** . Verrà aperta la finestra di dialogo **Proprietà** della risorsa di SQL Server.  
  
4.  Selezionare la scheda **Proprietà** , immettere il valore desiderato per la proprietà **FailureConditionLevel** , quindi fare clic su **OK** per applicare la modifica.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 **Per configurare le impostazioni della proprietà FailureConditionLevel:**  
  
 Con l'istruzione [ALTER SERVER CONFIGURATION](../../../t-sql/statements/alter-server-configuration-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] è possibile specificare il valore della proprietà FailureConditionLevel.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 Quando nel seguente esempio la proprietà FailureConditionLevel viene impostata su 0, viene indicato che con qualsiasi condizione di errore non verrà attivato automaticamente alcun failover o riavvio.  
  
```  
ALTER SERVER CONFIGURATION SET FAILOVER CLUSTER PROPERTY FailureConditionLevel = 0;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)   
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
