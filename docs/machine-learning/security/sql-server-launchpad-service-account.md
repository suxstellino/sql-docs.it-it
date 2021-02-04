---
title: Configurazione dell'account di Launchpad
description: Come modificare l'account del servizio Launchpad di SQL Server usato per l'esecuzione di script esterni in SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/17/2018
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 0aff998d79d79c2ac2ef06c37ffba4b01eb73973
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076859"
---
# <a name="sql-server-launchpad-service-configuration"></a>Configurazione del servizio Launchpad di SQL Server
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] è un servizio che gestisce ed esegue script esterni, in modo analogo a come il servizio di indicizzazione e query full-text avvia un host separato per l'elaborazione delle query full-text.

Per altre informazioni, vedere le sezioni dedicate a Launchpad in [Architettura di estendibilità in Machine Learning Services per SQL Server](../../machine-learning/concepts/extensibility-framework.md#launchpad) e [Panoramica della sicurezza per il framework di estendibilità in Machine Learning Services per SQL Server](../../machine-learning/concepts/security.md#launchpad).

## <a name="account-permissions"></a>Autorizzazioni dell'account

Per impostazione predefinita, Launchpad di SQL Server è configurato per l'esecuzione con l'account **NT Service\MSSQLLaunchpad**, di cui viene effettuato il provisioning con tutte le autorizzazioni necessarie per l'esecuzione di script esterni. La rimozione delle autorizzazioni da questo account può impedire l'avvio di Launchpad o l'accesso all'istanza di SQL Server in cui devono essere eseguiti gli script esterni.

Se si modifica l'account del servizio, assicurarsi di usare la [console Criteri di sicurezza locali](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings).

Le autorizzazioni necessarie per questo account sono elencate nella tabella seguente.

| Impostazione di Criteri di gruppo | Nome costante |
|----------------------|---------------|
| [Regolazione limite risorse memoria per un processo](/windows/security/threat-protection/security-policy-settings/adjust-memory-quotas-for-a-process) | SeIncreaseQuotaPrivilege | 
| [Ignorare controllo incrociato](/windows/security/threat-protection/security-policy-settings/bypass-traverse-checking) | SeChangeNotifyPrivilege | 
| [Accedi come servizio](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service) | SeServiceLogonRight | 
| [Sostituzione di token a livello di processo](/windows/security/threat-protection/security-policy-settings/replace-a-process-level-token) | SeAssignPrimaryTokenPrivilege | 

Per altre informazioni sulle autorizzazioni necessarie per l'esecuzione di servizi di SQL Server, vedere [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

<a name="bkmk_ChangingConfig"></a> 

## <a name="configuration-properties"></a>Proprietà di configurazione

In genere, non esiste alcun motivo per modificare la configurazione del servizio. Le proprietà che possono essere modificate includono l'account del servizio, il numero di processi esterni (20 per impostazione predefinita) o i criteri di reimpostazione della password per gli account di lavoro.

1. Aprire [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md).

2. In Servizi di SQL Server fare clic con il pulsante destro del mouse su Launchpad di SQL Server e scegliere **Proprietà**.
  + Per modificare l'account del servizio, fare clic sulla scheda **Accesso**.
  + Per aumentare il numero di utenti, fare clic sulla scheda **Avanzate** e modificare **Conteggio contesti di sicurezza**.

> [!Note]
> Nelle prime versioni di R Services per SQL Server 2016, è possibile modificare alcune proprietà del servizio modificando il file di configurazione [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]. Questo file non viene più usato per la modifica delle configurazioni. Gestione configurazione SQL Server è l'approccio corretto per le modifiche alla configurazione del servizio, ad esempio l'account del servizio e il numero di utenti.

## <a name="debug-settings"></a>Impostazioni di debug

Alcune proprietà possono essere modificate solo tramite il file di configurazione di Launchpad e ciò può essere utile in casi limitati, ad esempio per il debug. Il file di configurazione viene creato durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e per impostazione predefinita viene salvato come file di testo normale in `<instance path>\binn\rlauncher.config`.

Per poter apportare modifiche a questo file è necessario essere amministratore del computer su cui è eseguito [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se si decide di modificare il file, si consiglia di crearne una copia di backup prima di salvare le modifiche.

Nella tabella seguente sono elencate le impostazioni avanzate per [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] con i valori consentiti.

|**Nome impostazione**|**Tipo**|**Descrizione**|
|----|----|----|
|JOB\_CLEANUP\_ON\_EXIT|Integer |Questa è un'impostazione esclusivamente interna, perciò questo valore non deve essere modificato. </br></br>Specifica se la cartella di lavoro temporanea creata per ciascuna sessione di runtime esterna deve essere ripulita dopo il completamento della sessione. L'impostazione è utile per il debug. </br></br>I valori supportati sono **0** (disabilitata) o **1** (abilitata). </br></br>Il valore predefinito è 1, che indica che i file di log vengono rimossi all'uscita.|
|TRACE\_LEVEL|Integer |Configura il livello di dettaglio della traccia di MSSQLLAUNCHPAD per scopi di debug. Questa impostazione influisce sui file di traccia nel percorso specificato dall'impostazione LOG_DIRECTORY. </br></br>I valori supportati sono: **1** (errore), **2** (prestazioni), **3** (avviso), **4** (informazioni). </br></br>Il valore predefinito è 1, che indica solo gli errori di output.|

Tutte le impostazioni assumono la forma di coppie chiave-valore e ogni impostazione si trova su una riga diversa. Ad esempio, per modificare il livello di traccia, è necessario aggiungere la riga `Default: TRACE_LEVEL=4`.

<a name="bkmk_EnforcePolicy"></a>

## <a name="enforcing-password-policy"></a>Applicazione dei criteri delle password

Se l'organizzazione ha criteri che richiedono la modifica delle password a intervalli regolari, potrebbe essere necessario forzare il servizio Launchpad in modo da rigenerare le password crittografate gestite dal servizio per gli account di lavoro.

Per abilitare questa impostazione e forzare l'aggiornamento delle password, aprire il riquadro **Proprietà** per il servizio Launchpad in Gestione configurazione SQL Server, fare clic su **Avanzate** e modificare l'impostazione di **Reimposta password utenti esterni** scegliendo **Sì**. Quando si applica questa modifica, le password vengono rigenerate immediatamente per tutti gli account utente. Per usare uno script esterno dopo questa modifica, è necessario riavviare il servizio Launchpad, che leggerà quindi le nuove password generate.

Per reimpostare le password a intervalli regolari, è possibile impostare questo flag manualmente o usare uno script.

## <a name="next-steps"></a>Passaggi successivi

+ [Framework di estendibilità](../concepts/extensibility-framework.md)
+ [Panoramica della sicurezza](../concepts/security.md)