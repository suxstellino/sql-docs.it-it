---
title: Cmdlet di PowerShell per i gruppi di disponibilità
description: 'Riferimento per i vari cmdlet di PowerShell disponibili per gestire i gruppi di disponibilità Always On. '
ms.custom: seo-lt-2019
ms.date: 08/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], PowerShell cmdlets
- Availability Groups [SQL Server], about
- PowerShell [SQL Server], cmdlets
ms.assetid: b3fef0d5-b6d7-4386-a0f0-d06c165ad4de
author: cawrites
ms.author: chadam
ms.openlocfilehash: ae6462d46760f1a5f58f01a2170470e2d4ea7fac
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643495"
---
# <a name="overview-of-powershell-cmdlets-for-always-on-availability-groups"></a>Panoramica dei cmdlet di PowerShell per Gruppi di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] PowerShell è una shell della riga di comando basata su attività e un linguaggio di scripting progettato appositamente per l'amministrazione del sistema. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] fornisce un set di cmdlet di PowerShell in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] per distribuire, gestire e monitorare gruppi di disponibilità, repliche di disponibilità e database di disponibilità.  
  
> [!NOTE]  
>  Con l'inizio corretto di un'azione è possibile che venga completato un cmdlet di PowerShell. Ciò non implica che l'azione desiderata, ad esempio il failover di un gruppo di disponibilità, sia stata completata. Quando si genera lo script di una sequenza di azioni, può essere necessario controllare lo stato delle azioni e attenderne il completamento.  
  
> [!NOTE]  
>  Per un elenco degli argomenti nella documentazione online di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] che illustrano come usare i cmdlet per eseguire attività [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , vedere la sezione "Attività correlate" di [Panoramica di Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
##  <a name="configuring-a-server-instance-for-always-on-availability-groups"></a><a name="ConfiguringServerInstance"></a> Configurazione di un'istanza del Server per i gruppi di disponibilità Always  
  
|Cmdlet|Descrizione|Supportati in|  
|-------------|-----------------|------------------|
|[**Disable-SqlAlwaysOn**](/powershell/module/sqlserver/disable-sqlalwayson)|Disabilita la funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] su un'istanza del server.|L'istanza del server è specificata dal parametro **Path**, **InputObject** o **Name** . (L'edizione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deve supportare [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]).|  
|[**Enable-SqlAlwaysOn**](/powershell/module/sqlserver/enable-sqlalwayson)|Abilita [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] su un'istanza di [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] che supporta la funzionalità [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Per informazioni sul supporto per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).|Qualsiasi edizione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che supporta [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].|  
|[**New-SqlHadrEndPoint**](/powershell/module/sqlserver/new-sqlhadrendpoint)|Crea un nuovo endpoint del mirroring del database in un'istanza del server. Questo endpoint è richiesto per lo spostamento di dati tra il database primario e quelli secondari.|Qualsiasi istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|  
|[**Set-SqlHadrEndpoint**](/powershell/module/sqlserver/set-sqlhadrendpoint)|Modifica le proprietà di un endpoint del mirroring del database esistente, ad esempio il nome, lo stato o le proprietà di autenticazione.|Istanza del server che supporta [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] e in cui non è presente un endpoint del mirroring del database|  

  
##  <a name="backing-up-and-restoring-databases-and-transaction-logs"></a><a name="BnRcmdlets"></a> Backup e ripristino dei database e log delle transazioni  
  
|Cmdlet|Descrizione|Supportati in|  
|-------------|-----------------|------------------|  
|[**Backup-SqlDatabase**](/powershell/module/sqlserver/backup-sqldatabase)|Crea un backup dei dati o del log.|Qualsiasi database online (per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], un database nell'istanza del server che ospita la replica primaria)|  
|[**Restore-SqlDatabase**](/powershell/module/sqlserver/restore-sqldatabase)|Ripristina un backup.|Qualsiasi istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], un'istanza del server che ospita una replica secondaria)<br /><br />

  >[!Important]
  >Quando si prepara un database secondario, è necessario usare il parametro **-NoRecovery** in ogni comando **Restore-SqlDatabase**. 
  
 Per informazioni sull'uso di questi cmdlet per preparare un database secondario, vedere [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
##  <a name="creating-and-managing-an-availability-group"></a><a name="DeployManageAGs"></a> Creazione e gestione di un gruppo di disponibilità  
  
|Cmdlet|Descrizione|Supportati in|  
|-------------|-----------------|------------------|  
|[**New-SqlAvailabilityGroup**](/powershell/module/sqlserver/new-sqlavailabilitygroup)|Crea un nuovo gruppo di disponibilità.|Istanza del server per ospitare la replica primaria|  
|[**Remove-SqlAvailabilityGroup**](/powershell/module/sqlserver/remove-sqlavailabilitygroup)|Elimina un gruppo di disponibilità.|Istanza del server abilitata per HADR|  
|[**Set-SqlAvailabilityGroup**](/powershell/module/sqlserver/set-sqlavailabilitygroup)|Imposta le proprietà di un gruppo di disponibilità; porta un gruppo di disponibilità online/offline|Istanza del server che ospita la replica primaria|  
|[**Switch-SqlAvailabilityGroup**](/powershell/module/sqlserver/switch-sqlavailabilitygroup)|Inizia una delle seguenti modalità di failover:<br /><br /> Failover forzato di un gruppo di disponibilità (con possibile perdita di dati).<br /><br /> Failover manuale di un gruppo di disponibilità.|Istanza del server che ospita la replica secondaria di destinazione|  
  
##  <a name="creating-and-managing-an-availability-group-listener"></a><a name="AGlisteners"></a> Creazione e gestione di un listener del gruppo di disponibilità  
  
|Cmdlet|Descrizione|Supportati in|  
|------------|-----------------|------------------|  
|[**New-SqlAvailabilityGroupListener**](/powershell/module/sqlserver/new-sqlavailabilitygrouplistener)|Crea un nuovo listener del gruppo di disponibilità e lo collega a un gruppo di disponibilità esistente.|Istanza del server che ospita la replica primaria|  
|[**Set-SqlAvailabilityGroupListener**](/powershell/module/sqlserver/set-sqlavailabilitygrouplistener)|Modifica l'impostazione della porta su un listener del gruppo di disponibilità esistente.|Istanza del server che ospita la replica primaria|  
|[**Add-SqlAvailabilityGroupListenerStaticIp**](/powershell/module/sqlserver/add-sqlavailabilitygrouplistenerstaticip)|Aggiunge un indirizzo IP statico a una configurazione del listener del gruppo di disponibilità esistente. L'indirizzo IP può essere un indirizzo IPv4 con subnet o un indirizzo IPv6.|Istanza del server che ospita la replica primaria|  
  
##  <a name="creating-and-managing-an-availability-replica"></a><a name="DeployManageARs"></a> Creazione e gestione di una Replica di disponibilità  
  
|Cmdlet|Descrizione|Supportati in|  
|-------------|-----------------|------------------|  
|[**New-SqlAvailabilityReplica**](/powershell/module/sqlserver/new-sqlavailabilityreplica)|Crea una nuova replica di disponibilità È possibile usare il parametro **-AsTemplate** per creare un oggetto della replica di disponibilità in memoria per ogni nuova replica di disponibilità.|Istanza del server che ospita la replica primaria|  
|[**Join-SqlAvailabilityGroup**](/powershell/module/sqlserver/join-sqlavailabilitygroup)|Viene creato un join della replica secondaria al gruppo di disponibilità.|Istanza del server che ospita la replica secondaria|  
|[**Remove-SqlAvailabilityReplica**](/powershell/module/sqlserver/remove-sqlavailabilityreplica)|Elimina una replica di disponibilità.|Istanza del server che ospita la replica primaria|  
|[**Set-SqlAvailabilityReplica**](/powershell/module/sqlserver/set-sqlavailabilityreplica)|Imposta le proprietà di una replica di disponibilità.|Istanza del server che ospita la replica primaria|  
  
##  <a name="adding-and-managing-an-availability-database"></a><a name="DeployManageDbs"></a> Aggiunta e gestione di un Database di disponibilità  
  
|Cmdlet|Descrizione|Supportati in|  
|-------------|-----------------|------------------|  
|[**Add-SqlAvailabilityDatabase**](/powershell/module/sqlserver/add-sqlavailabilitydatabase)|Nella replica primaria viene aggiunto un database a un gruppo di disponibilità.<br /><br /> In una replica secondaria viene creato un join di un database secondario a un gruppo di disponibilità.|Qualsiasi istanza del server che ospita una replica di disponibilità (il comportamento è diverso per le repliche primarie e per quelle secondarie)|  
|[**Remove-SqlAvailabilityDatabase**](/powershell/module/sqlserver/remove-sqlavailabilitydatabase)|Nella replica primaria il database viene rimosso dal gruppo di disponibilità.<br /><br /> In una replica secondaria il database secondario locale viene rimosso dalla replica secondaria locale.|Qualsiasi istanza del server che ospita una replica di disponibilità (il comportamento è diverso per le repliche primarie e per quelle secondarie)|  
|[**Resume-SqlAvailabilityDatabase**](/powershell/module/sqlserver/resume-sqlavailabilitydatabase)|Riprende lo spostamento dati per un database di disponibilità sospeso.|L'istanza del server in cui si trova il database è stata sospesa.|  
|[**Suspend-SqlAvailabilityDatabase**](/powershell/module/sqlserver/suspend-sqlavailabilitydatabase)|Sospende lo spostamento dati per un database di disponibilità.|Qualsiasi istanza del server che ospita una replica di disponibilità.|  
  
##  <a name="monitoring-availability-group-health"></a><a name="MonitorTblshtAGs"></a> Monitoraggio integrità gruppo di disponibilità  
 Con i cmdlet [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] seguenti è possibile monitorare lo stato di un gruppo di disponibilità, nonché delle repliche e dei database relativi.  
  
> [!IMPORTANT]  
>  È necessario disporre delle autorizzazioni CONNECT, VIEW SERVER STATE e VIEW ANY DEFINITION per eseguire questi cmdlet.  
  
|Cmdlet|Descrizione|Supportati in|  
|------------|-----------------|------------------|  
|[**Test-SqlAvailabilityGroup**](/powershell/module/sqlserver/test-sqlavailabilitygroup)|Valuta l'integrità di un gruppo di disponibilità valutando i criteri della gestione basata su criteri di SQL Server.|Qualsiasi istanza del server che ospita una replica di disponibilità*.|  
|[**Test-SqlAvailabilityReplica**](/powershell/module/sqlserver/test-sqlavailabilityreplica)|Valuta l'integrità delle repliche di disponibilità valutando i criteri della gestione basata su criteri di SQL Server.|Qualsiasi istanza del server che ospita una replica di disponibilità*.|  
|[**Test-SqlDatabaseReplicaState**](/powershell/module/sqlserver/test-sqldatabasereplicastate)|Valuta l'integrità di un database di disponibilità su tutte le repliche di disponibilità aggiunte valutando i criteri della gestione basata su criteri di SQL Server.|Qualsiasi istanza del server che ospita una replica di disponibilità*.|  
  
 \* Per visualizzare informazioni su tutte le repliche di disponibilità in un gruppo di disponibilità, usare l'istanza del server che ospita la replica primaria.  
  
 Per altre informazioni, vedere [Usare i criteri Always On per visualizzare l'integrità di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Visualizzare la Guida di SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
