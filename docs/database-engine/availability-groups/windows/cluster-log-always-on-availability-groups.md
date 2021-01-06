---
title: Generare e analizzare CLUSTER.LOG per i gruppi di disponibilità
description: Informazioni dettagliate su come generare e analizzare il log del cluster per un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 06/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 01a9e3c1-2a5f-4b98-a424-0ffc15d312cf
author: rothja
ms.author: jroth
ms.openlocfilehash: a03d616a289228695a46f4553e4123ceb9a4810e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643081"
---
# <a name="generate-and-analyze-the-clusterlog-for-an-always-on-availability-group"></a>Generare e analizzare CLUSTER. LOG per un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Come risorsa cluster di failover, esistono interazioni esterne tra SQL Server, il servizio WSFC (Windows Server Failover Cluster) e la DLL risorse SQL Server (hadrres.dll) che non possono essere monitorati all'interno di SQL Server. CLUSTER.LOG è un log WSFC che consente di diagnosticare problemi all'interno del cluster WSFC o nella DLL risorse SQL Server. 
  
## <a name="generate-cluster-log"></a>Generare il log del cluster  
 È possibile generare i log dei cluster in due modi:  
  
1.  Usare il comando `cluster /log /g` al prompt dei comandi. Questo comando genera i log del cluster per la directory \windows\cluster\reports in ogni nodo WSFC. Grazie a questo metodo è possibile specificare il livello di dettaglio nei log generati usando l'opzione `/level`. Invece non è purtroppo possibile specificare la directory di destinazione per i log del cluster generati. Per altre informazioni, vedere [How to create the cluster.log in Windows Server 2008 Failover Clustering](https://techcommunity.microsoft.com/t5/failover-clustering/how-to-create-the-cluster-log-in-windows-server-2008-failover/ba-p/371283) (Come creare i log del cluster nel clustering di failover di Windows Server 2008).  
  
2.  Usare il cmdlet [Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10)) di PowerShell. Questo metodo consente di generare il file di log del cluster da tutti i nodi in una directory di destinazione nel nodo che esegue il cmdlet. Non è tuttavia possibile specificare il livello di dettaglio nei log generati.  
  
 I comandi seguenti di PowerShell consentono di generare i log del cluster da tutti i nodi del cluster partendo dagli ultimi 15 minuti e salvarli nella directory corrente. Eseguire i comandi in una finestra di PowerShell con privilegi amministrativi.  
  
```powershell  
Import-Module FailoverClusters   
Get-ClusterLog -TimeSpan 15 -Destination .  
```  
  
## <a name="always-on-log-verbosity"></a>Livello di dettaglio del log Always On  
 È possibile aumentare il livello di dettaglio dei log in CLUSTER. LOG per un gruppo di disponibilità. Per modificare il livello di dettaglio, attenersi alla procedura seguente:  
  
1.  Aprire **Gestione cluster di failover** dal menu **Start**.  
  
2.  Espandere il cluster e il nodo **Services and applications** (Servizi e applicazioni), quindi selezionare il nome del gruppo di disponibilità.  
  
3.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sulla risorsa del gruppo di disponibilità e selezionare **Proprietà**.  
  
4.  Fare clic sulla scheda **Proprietà**.  
  
5.  Modificare la proprietà **VerboseLogging**. Per impostazione predefinita, la proprietà **VerboseLogging** è impostata su `0` che consente di segnalare informazioni, avvisi ed errori. È possibile impostare la proprietà **VerboseLogging** su un valore compreso tra `0` e `2`.  
  
6.  Fare clic su **OK**.  
  
7.  Fare di nuovo clic con il pulsante destro del mouse sulla risorsa del gruppo di disponibilità e selezionare **Take this resource offline** (Disconnetti risorsa).  
  
8.  Fare di nuovo clic con il pulsante destro del mouse sulla risorsa del gruppo di disponibilità e selezionare **Bring this resource online** (Connetti risorsa).  
  
## <a name="availability-group-resource-events"></a>Eventi della risorsa del gruppo di disponibilità  
 La tabella seguente illustra vari tipi di eventi che è possibile visualizzare in CLUSTER. LOG relativi alla risorsa del gruppo di disponibilità. For altre informazioni su Resource Hosting Subsystem (RHS) e Resource Control Monitor (RCM) in WSFC, vedere [Resource Hosting Subsystem (RHS) In Windows Server 2008 Failover Clusters](/archive/blogs/askcore/resource-hosting-subsystem-rhs-in-windows-server-2008-failover-clusters) (Resource Hosting Subsystem (RHS) nei cluster di failover di Windows Server 2008).  
  
|Identificatore|Source (Sorgente)|Esempio di CLUSTER. LOG|  
|----------------|------------|------------------------------|  
|Messaggi preceduti dal prefisso `[RES]` e `[hadrag]`|hadrres.dll (DLL risorse Always On)|00002cc4.00001264::2011/08/05-13:47:42.543 INFO  [RES] Gruppo di disponibilità SQL Server \<ag>: `[hadrag]` richiesta offline.<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.558 ERR   [RES] Gruppo di disponibilità SQL Server \<ag>: `[hadrag]` lease thread terminato<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.605 INFO  [RES] Gruppo di disponibilità SQL Server \<ag>: `[hadrag]` istruzione SQL libera<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.902 INFO  [RES] Gruppo di disponibilità SQL Server \<ag>: `[hadrag]` disconnessione da SQL Server|  
|Messaggi preceduti dal prefisso `[RHS]`|RHS. EXE (Resource Hosting Subsystem, processo host di hadrres.dll)|00000c40.00000a34::2011/08/10-18:42:29.498 INFO  [RHS] risorsa gruppo di disponibilità disattivata. RHS segnala lo stato della risorsa a RCM.|  
|Messaggi preceduti dal prefisso `[RCM]`|Resource Control Monitor (servizio cluster)|000011d0.00000f80::2011/08/05-13:47:42.480 INFO  [RCM] rcm::RcmGroup::Move: prima viene disattivato il gruppo "ag".<br /><br /> 000011d0.00000f80::2011/08/05-13:47:42.496 INFO  [RCM] TransitionToState(ag) Online-->OfflineCallIssued.|  
|RcmApi/ClusAPI|Chiamata API, ovvero SQL Server richiede l'azione|000011d0.00000f80::2011/08/05-13:47:42.465 INFO  [RCM] rcm::RcmApi::MoveGroup: (ag, 2)|  
  
## <a name="debug-always-on-resource-dll-in-isolation"></a>Eseguire il debug della DLL risorse Always On in modalità isolata  
 È la pratica di debug migliore per configurare il cluster ed eseguire la DLL risorse Always (hadrres.dll) in modalità isolata da altre DLL risorse. Per impostazione predefinita, il cluster WSFC viene eseguito in tutte le DLL risorse in una singola istanza di rhs.exe. In questo modo tutte le risorse all'interno del cluster condividono la stessa istanza rhs.exe. Quando si tenta di eseguire il debug hadrres.dll con un debugger, la sospensione in un punto di interruzione può causare la sospensione anche di altre risorse che condividono l'istanza di rhs.exe. Quando si eseguono più gruppi di disponibilità nello stesso cluster, la stessa configurazione può anche causare la sospensione di tutti i gruppi di disponibilità quando l'esecuzione viene sospesa in un punto di interruzione per eseguire il debug di un gruppo di disponibilità.  
  
 Per isolare un gruppo di disponibilità dalle altre DLL risorse del cluster, inclusi altri gruppi di disponibilità, attenersi alla procedura seguente per eseguire hadrres.dll all'interno di un processo rhs.exe distinto:  
  
1.  Aprire l'**editor del Registro di sistema** e selezionare la chiave seguente: HKEY_LOCAL_MACHINE\Cluster\Resources. Questa chiave contiene le chiavi per tutte le risorse, ognuna con un GUID diverso.  
  
2.  Individuare la chiave della risorsa che contiene un valore **Nome** che corrisponde al nome del gruppo di disponibilità.  
  
3.  Modificare il valore **SeparateMonitor** in **1**.  
  
4.  Riavviare il servizio cluster per il gruppo di disponibilità nel cluster WSFC.  
  
