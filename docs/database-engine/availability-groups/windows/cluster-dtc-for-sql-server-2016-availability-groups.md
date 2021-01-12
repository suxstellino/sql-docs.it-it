---
title: Inserire in cluster il servizio DTC per un gruppo di disponibilità
description: 'Descrive i requisiti e i passaggi per il clustering del servizio Microsoft Distributed Transaction Coordinator (DTC) per un gruppo di disponibilità Always On. '
ms.custom: seo-lt-2019
ms.date: 08/30/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: a47c5005-20e3-4880-945c-9f78d311af7a
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4fd420fd7d07af5a6efa81cdb7e716020c03a89b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091801"
---
# <a name="how-to-cluster-the-dtc-service-for-an-always-on-availability-group"></a>Come inserire in cluster il servizio DTC per un gruppo di disponibilità Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

Questo argomento descrive i requisiti e i passaggi per il clustering del servizio Microsoft Distributed Transaction Coordinator (DTC) per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Per altre informazioni sulle transazioni distribuite e [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vedere [Transazioni tra database non supportate per il mirroring del database o i gruppi di disponibilità Always On (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).

 ## <a name="checklist-preliminary-requirements"></a>Elenco di controllo: requisiti preliminari

|Attività|Riferimento|  
|-----------------|----------|  
|Verificare che tutti i nodi, i servizi e il gruppo di disponibilità siano stati configurati correttamente.|[Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)|
|Verificare che i requisiti DTC del gruppo di disponibilità siano soddisfatti.|[Transazioni tra database non supportate per il mirroring del database o i gruppi di disponibilità Always On (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)

## <a name="checklist-clustered-dtc-resource-dependencies"></a>Elenco di controllo: dipendenze delle risorse DTC cluster

|Attività|Informazioni di riferimento|  
|-----------------|----------|  
|Unità di archiviazione condivisa.|[Configurazione dell'unità di archiviazione condivisa](https://msdn.microsoft.com/library/cc982358(v=bts.10).aspx). Si consiglia di usare la lettera di unità **M**.|
|Risorsa nome di rete DTC univoca.  Il nome viene registrato come oggetto computer del cluster in Active Directory.<br /><br />Verificare che una delle due affermazioni seguenti sia vera:<br /><br />• L'utente che crea la risorsa nome di rete DTC ha l'autorizzazione per la creazione di oggetti computer per l'unità organizzativa o il contenitore in cui si trova la risorsa nome di rete DTC.<br /><br />• Se l'utente non ha l'autorizzazione per la creazione di oggetti computer, chiedere all'amministratore di dominio di pre-installare un oggetto computer del cluster per la risorsa nome di rete DTC.|[Pre-installare gli oggetti computer del cluster in Active Directory Domain Services](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn466519(v=ws.11))|
|Un indirizzo IP statico valido disponibile e la subnet mask appropriata per l'indirizzo IP.||

## <a name="cluster-the-dtc-resource"></a>Inserire la risorsa DTC nel cluster
Dopo aver creato la risorsa gruppo di disponibilità, creare una risorsa DTC cluster e aggiungerla al gruppo di disponibilità.  È possibile visualizzare uno script di esempio in [Creare DTC cluster per un gruppo di disponibilità AlwaysOn](../../../database-engine/availability-groups/windows/create-clustered-dtc-for-an-always-on-availability-group.md).


## <a name="checklist-post-clustered-dtc-resource-configurations"></a>Elenco di controllo: configurazioni successive delle risorse DTC cluster

|Attività|Informazioni di riferimento|  
|-----------------|----------|  
|Abilitare l'accesso alla rete sicuro per la risorsa DTC cluster.|[Abilitare l'accesso alla rete sicuro per MS DTC](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753620(v=ws.10))|
|Arrestare e disabilitare il servizio DTC locale.|[Configurare la modalità di avvio di un servizio](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755249(v=ws.11))|
|Eseguire ciclicamente il servizio SQL Server per ogni istanza del gruppo di disponibilità.  Eseguire il failover del gruppo di disponibilità in base alle esigenze.|[Eseguire un failover manuale pianificato di un gruppo di disponibilità (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)<br /><br />[Avviare, arrestare, sospendere, riprendere, riavviare il motore di database, SQL Server Agent o SQL Server Browser](../../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)|

- Se il server è Windows Server 2012 R2 è necessario che nel sistema operativo sia applicato [KB 3030373](https://support.microsoft.com/kb/3090973) .

- Preparare i server per i gruppi di disponibilità in base agli elenchi di controllo riportati in [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).

- Configurare le istanze del server per i [**gruppi di disponibilità Always On**](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md).

### <a name="resources"></a>RISORSE


[Altre informazioni sul test del servizio DTC nei gruppi di disponibilità:](/archive/blogs/dataplatform/sql-server-2016-dtc-support-in-availability-groups)

[Monitorare Gruppi di disponibilità (Transact-SQL)](monitor-availability-groups-transact-sql.md)

[Creare un gruppo di disponibilità (Transact-SQL)](create-an-availability-group-transact-sql.md)


[Supporto DTC di SQL Server 2016 nei gruppi di disponibilità](/archive/blogs/dataplatform/sql-server-2016-dtc-support-in-availability-groups) 

[Collegamento esterno: Configurare DTC per un'istanza cluster di SQL Server con Windows Server 2008 R2](https://sqlha.com/2013/03/12/how-to-properly-configure-dtc-for-clustered-instances-of-sql-server-with-windows-server-2008-r2/)