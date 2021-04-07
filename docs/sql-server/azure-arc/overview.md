---
title: SQL Server con abilitazione di Azure Arc
titleSuffix: ''
description: Gestire le istanze di SQL Server con SQL Server con abilitazione di Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 04/06/2021
ms.topic: conceptual
ms.prod: sql
ms.custom: references_regions
ms.openlocfilehash: 664cf371e9ceb1dc59814b80099d5e3b93e2cd68
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557544"
---
# <a name="azure-arc-enabled-sql-server-preview"></a>SQL Server con abilitazione di Azure Arc (anteprima)

SQL Server con abilitazione di Azure Arc fa parte di Azure Arc per server. Estende i servizi di Azure alle istanze di SQL Server ospitate all'esterno di Azure nel data center del cliente, nella rete perimetrale o in un ambiente multi-cloud.

Per abilitare i servizi di Azure, un'istanza di SQL Server in esecuzione deve essere registrata con Azure ARC usando i portale di Azure e uno script di registrazione. Dopo la registrazione, l'istanza verrà rappresentata in Azure come risorsa __SQL Server - Azure Arc__. Le proprietà di questa risorsa riflettono un subset delle impostazioni di configurazione di SQL Server.

SQL Server può essere installato in un computer fisico o virtuale che esegue Windows o Linux, connesso ad Azure Arc attraverso l'agente Connected Machine. L'agente è installato e il computer viene registrato automaticamente come parte della registrazione dell'istanza di SQL Server. L'agente Connected Machine comunica in modo sicuro in uscita con Azure Arc sulla porta TCP 443. Se il computer si connette attraverso un firewall o un server proxy HTTP per comunicare in Internet, vedere i [requisiti della configurazione di rete per l'agente Connected Machine](/azure/azure-arc/servers/agent-overview#prerequisites).

L'anteprima pubblica di SQL Server con abilitazione di Azure Arc supporta un set di soluzioni che richiedono che l'estensione del server MMA (Microsoft Monitoring Agent) sia installata e connessa a un'area di lavoro di Azure Log Analytics per la raccolta dei dati e la creazione di report. Queste soluzioni includono la sicurezza dei dati avanzata usando il Centro sicurezza di Azure e Azure Sentinel e i controlli di integrità dell'ambiente SQL usando la funzionalità Valutazione SQL su richiesta.

Il diagramma seguente illustra l'architettura di SQL Server con abilitazione di Azure Arc.

![Architettura dell'anteprima pubblica](media/overview/pubic-preview-architecture.png)

## <a name="prerequisites"></a>Prerequisiti

### <a name="supported-sql-versions-and-operating-systems"></a>Versioni di SQL e sistemi operativi supportati

SQL Server con abilitazione di Azure Arc supporta SQL Server 2012 o versione successiva in esecuzione in una delle seguenti versioni del sistema operativo Windows o Linux:

- Windows Server 2012 R2 e versioni successive
- Ubuntu 16.04 e 18.04 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64) 
- SUSE Linux Enterprise Server (SLES) 15 (x64)

### <a name="required-permissions"></a>Autorizzazioni necessarie

Per connettere le istanze di SQL Server e il computer di hosting ad Azure Arc, è necessario avere un account con privilegi per eseguire le azioni seguenti:
   * Microsoft.AzureArcData/sqlServerInstances/read
   * Microsoft.AzureArcData/sqlServerInstances/write
   * Microsoft.HybridCompute/machines/read
   * Microsoft.HybridCompute/machines/write
   * Microsoft.GuestConfiguration/guestConfigurationAssignments/read

Per una sicurezza ottimale, è consigliabile creare in Azure un ruolo personalizzato che abbia le autorizzazioni minime elencate. Per informazioni su come creare un ruolo personalizzato in Azure con queste autorizzazioni, vedere [Panoramica dei ruoli personalizzati](/azure/active-directory/users-groups-roles/roles-custom-overview). Per aggiungere l'assegnazione di ruolo, vedere [Aggiungere o rimuovere assegnazioni di ruolo di Azure tramite il portale di Azure](/azure/role-based-access-control/role-assignments-portal) oppure [Aggiungere o rimuovere assegnazioni di ruolo in Azure tramite l'interfaccia della riga di comando di Azure](/azure/role-based-access-control/role-assignments-cli).

### <a name="azure-subscription-and-service-limits"></a>Limiti del servizio e della sottoscrizione di Azure

Prima di configurare le istanze di SQL Server e i computer con Azure Arc, esaminare i [limiti della sottoscrizione](/azure/azure-resource-manager/management/azure-subscription-service-limits#subscription-limits) di Azure Resource Manager e i [limiti del gruppo di risorse](/azure/azure-resource-manager/management/azure-subscription-service-limits#resource-group-limits) per pianificare il numero di macchine virtuali da connettere.

### <a name="networking-configuration-and-resource-providers"></a>Configurazione di rete e provider di risorse

Verificare [la configurazione di rete, il protocollo TLS (Transport Layer Security) e i provider di risorse](/azure/azure-arc/servers/agent-overview#prerequisites) richiesti per l'agente Connected Machine.

Il provider di risorse `Microsoft.AzureArcData` è necessario per connettere le istanze di SQL Server ad Azure Arc. Vedere le istruzioni per la registrazione del provider di risorse nella sezione [Prerequisiti](connect.md#prerequisites).

Se sono già disponibili istanze di SQL Server connesse ad Azure Arc, seguire questa procedura per eseguire la migrazione delle risorse esistenti **SQL Server - Azure Arc** al nuovo spazio dei nomi.

### <a name="supported-azure-regions"></a>Aree di Azure supportate

L'anteprima pubblica è disponibile nelle aree seguenti:
- Stati Uniti orientali
- Stati Uniti orientali 2
- Stati Uniti occidentali 2
- Australia orientale
- Asia sud-orientale
- Europa settentrionale
- Europa occidentale
- Regno Unito meridionale

## <a name="next-steps"></a>Passaggi successivi

- [Connettere l'istanza di SQL Server ad Azure Arc](connect.md)
- [Configurare l'istanza di SQL Server per il controllo di integrità periodico dell'ambiente con Valutazione SQL su richiesta](assess.md)
- [Configurare la sicurezza dei dati avanzata per l'istanza di SQL Server](configure-advanced-data-security.md)