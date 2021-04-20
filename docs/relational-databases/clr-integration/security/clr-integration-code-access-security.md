---
title: Sicurezza dall'accesso di codice per l'integrazione con CLR | Microsoft Docs
description: Per SQL Server clr, CLR supporta la sicurezza dall'accesso di codice per il codice gestito, in cui le autorizzazioni vengono concesse agli assembly in base all'identità del codice.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- UNSAFE assemblies
- permissions [CLR integration]
- common language runtime [SQL Server], security
- SAFE assemblies
- code access security [CLR integration]
- EXTERNAL_ACCESS assemblies
ms.assetid: 2111cfe0-d5e0-43b1-93c3-e994ac0e9729
author: rothja
ms.author: jroth
ms.openlocfilehash: a241bc50502099cfd2b2a27ffe86a16b035b440e
ms.sourcegitcommit: 708b3131db2e542b1d461d17dec30d673fd5f0fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/19/2021
ms.locfileid: "107729129"
---
# <a name="clr-integration-code-access-security"></a>Sicurezza da accesso di codice dell'integrazione con CLR
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Common Language Runtime (CLR) supporta un modello di sicurezza definito sicurezza dall'accesso di codice per il codice gestito che prevede che le autorizzazioni vengano concesse agli assembly in base all'identità del codice. Per ulteriori informazioni, vedere la sezione relativa alla sicurezza da accesso di codice in .NET Framework SDK (Software Development Kit).  
  
 I criteri di sicurezza che determinano le autorizzazioni concesse agli assembly vengono definiti in tre punti diversi:  
  
-   Criteri del computer: criteri attivi per tutto il codice gestito in esecuzione sul computer nel quale viene installato [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Criteri utente: criteri attivi per il codice gestito ospitato da un processo. Per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], i criteri utente sono specifici dell'account di Windows su cui è in esecuzione il servizio di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Criteri host: criteri impostati dall'host di CLR (in questo caso, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]), attivi per il codice gestito in esecuzione su quell'host.  
  
 Il meccanismo di sicurezza da accesso di codice supportato da CLR si basa sul presupposto che il runtime possa ospitare codice completamente o parzialmente attendibile. La protezione delle risorse mediante la sicurezza dell'accesso di codice di CRL in genere viene eseguita tramite wrapping di API gestite, che richiedono l'autorizzazione  corrispondente prima di consentire l'accesso alla risorsa. La richiesta di autorizzazione viene soddisfatta solo se tutti i chiamanti a livello di assembly nello stack di chiamate dispongono dell'autorizzazione corrispondente per la risorsa.  
  
 Il set di autorizzazioni della sicurezza dell'accesso di codice concesse al codice gestito durante l'esecuzione all'interno di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] rappresenta l'intersezione del set di autorizzazioni concesse dai tre livelli di criteri sopra descritti. Anche se [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] concede un set di autorizzazioni a un assembly caricato in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il set di autorizzazioni effettivo concesso al codice utente potrebbe essere limitato ulteriormente dai criteri utente e a livello di computer.  
  
## <a name="sql-server-host-policy-level-permission-sets"></a>Set di autorizzazioni a livello di criteri host di SQL Server  
 Il set di autorizzazioni della sicurezza da accesso di codice concesso agli assembly al livello di criteri host di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è determinato dal set di autorizzazioni specificato durante la creazione dell'assembly. Sono disponibili tre set di autorizzazioni: **SAFE,** **EXTERNAL_ACCESS** e **UNSAFE** (specificato tramite l'opzione **PERMISSION_SET** di CREATE ASSEMBLY [&#40;Transact-SQL&#41;](../../../t-sql/statements/create-assembly-transact-sql.md)).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] offre criteri di sicurezza a livello host per CLR, quando ospita tale ambiente. Questo livello di criteri aggiuntivo è sottostante ai due livelli sempre attivi e viene impostato per ogni dominio applicazione creato da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Non è indicato per il dominio applicazione predefinito che sarebbe attivo quando [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] crea un'istanza di CLR.  
  
 I criteri a livello host di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sono una combinazione dei criteri fissi di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per gli assembly di sistema e dei criteri specificati dall'utente per gli assembly utente.  
  
 I criteri fissi per gli assembly CLR e gli assembly di sistema di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] concedono loro l'attendibilità totale.  
  
 La parte dei criteri host di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] specificata dall'utente è basata su uno dei tre bucket di autorizzazione specificati per ogni assembly dal relativo proprietario. Per ulteriori informazioni sulle autorizzazioni di sicurezza elencate in basso, vedere .NET Framework SDK.  
  
### <a name="safe"></a>SAFE  
 È consentito solo il calcolo interno e l'accesso locale ai dati. **SAFE** è il set di autorizzazioni più restrittivo. Il codice eseguito da un assembly con **autorizzazioni SAFE** non può accedere a risorse di sistema esterne, ad esempio file, rete, variabili di ambiente o registro.  
  
 **Gli** assembly SAFE hanno le autorizzazioni e i valori seguenti:  
  
|Autorizzazione|Valori/Descrizione|  
|----------------|-----------------------------|  
|**Securitypermission**|**Esecuzione:** Autorizzazione per l'esecuzione di codice gestito.|  
|**Sqlclientpermission**|**Context connection = true,** **context connection = yes:** è possibile usare solo context-connection e la stringa di connessione può specificare solo il valore "context connection=true" o "context connection=yes".<br /><br /> **AllowBlankPassword = false:**  Le password vuote non sono consentite.|  
  
### <a name="external_access"></a>EXTERNAL_ACCESS  
 EXTERNAL_ACCESS assembly hanno le stesse autorizzazioni degli assembly **SAFE,** con la possibilità aggiuntiva di accedere alle risorse di sistema esterne, ad esempio file, reti, variabili di ambiente e registro.  
  
 **EXTERNAL_ACCESS** assembly hanno anche le autorizzazioni e i valori seguenti:  
  
|Autorizzazione|Valori/Descrizione|  
|----------------|-----------------------------|  
|**DistributedTransactionPermission**|**Senza restrizioni:** Le transazioni distribuite sono consentite.|  
|**Dnspermission**|**Senza restrizioni:** Autorizzazione per richiedere informazioni ai server dei nomi di dominio.|  
|**Environmentpermission**|**Senza restrizioni:** È consentito l'accesso completo alle variabili di ambiente di sistema e utente.|  
|**Eventlogpermission**|**Amministrare:** Sono consentite le azioni seguenti: creazione di un'origine eventi, lettura di log esistenti, eliminazione di origini eventi o log, risposta alle voci, cancellazione di un log eventi, ascolto di eventi e accesso a una raccolta di tutti i log eventi.|  
|**Fileiopermission**|**Senza restrizioni:** È consentito l'accesso completo a file e cartelle.|  
|**KeyContainerPermission**|**Senza restrizioni:** È consentito l'accesso completo ai contenitori di chiavi.|  
|**Networkinformationpermission**|**Accesso:** Il ping è consentito.|  
|**Registrypermission**|Consente i diritti di **lettura HKEY_CLASSES_ROOT**, **HKEY_LOCAL_MACHINE**, **HKEY_CURRENT_USER**, **HKEY_CURRENT_CONFIG** e **HKEY_USERS.**|  
|**Securitypermission**|**Asserzione:** Possibilità di asserire che tutti i chiamanti di questo codice hanno l'autorizzazione necessaria per l'operazione.<br /><br /> **ControlPrincipal:** Possibilità di modificare l'oggetto Principal.<br /><br /> **Esecuzione:** Autorizzazione per l'esecuzione di codice gestito.<br /><br /> **SerializationFormatter:** Possibilità di fornire servizi di serializzazione.|  
|**Smtppermission**|**Accesso:** Sono consentite connessioni in uscita alla porta host SMTP 25.|  
|**Socketpermission**|**Connetti:** Sono consentite connessioni in uscita (tutte le porte, tutti i protocolli) in un indirizzo di trasporto.|  
|**Sqlclientpermission**|**Senza restrizioni:** È consentito l'accesso completo all'origine dati.|  
|**Storepermission**|**Senza restrizioni:** È consentito l'accesso completo agli archivi certificati X.509.|  
|**Webpermission**|**Connetti:** Sono consentite connessioni in uscita alle risorse Web.|  
  
### <a name="unsafe"></a>UNSAFE  
 UNSAFE concede agli assembly libero accesso a risorse interne ed esterne a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il codice in esecuzione dall'interno di un assembly **UNSAFE** può anche chiamare codice non gestito.  
  
 **Agli assembly UNSAFE** viene assegnato **FullTrust**.  
  
## <a name="recommended-permission-settings"></a>Impostazioni di autorizzazione consigliate

**SAFE** è l'impostazione di autorizzazione consigliata per gli assembly che eseguono attività di calcolo e gestione dei dati senza accedere alle risorse all'esterno di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . 

**EXTERNAL_ACCESS** consigliato per gli assembly che accedono alle risorse all'esterno di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . **EXTERNAL_ACCESS** assembly vengono eseguiti per impostazione predefinita come [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] account del servizio. È possibile che il **codice EXTERNAL_ACCESS** rappresentare in modo esplicito il contesto di sicurezza dell'autenticazione di Windows del chiamante. Poiché l'impostazione predefinita è l'esecuzione come account del servizio, l'autorizzazione per eseguire EXTERNAL_ACCESS deve essere concessa solo agli account di accesso attendibili per l'esecuzione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] come account del servizio.  

Dal punto di vista della sicurezza, **EXTERNAL_ACCESS** e gli assembly **UNSAFE** sono identici. Tuttavia, **EXTERNAL_ACCESS** assembly forniscono diverse protezioni di affidabilità e affidabilità non presenti negli assembly **UNSAFE.** 

La specifica **di UNSAFE** consente al codice nell'assembly di eseguire operazioni non corrette sullo spazio del processo e può quindi compromettere la affidabilità e [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] la scalabilità di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni sulla creazione di assembly CLR in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vedere Gestione di assembly di integrazione [CLR](../../../relational-databases/clr-integration/assemblies/managing-clr-integration-assemblies.md).  

> [!IMPORTANT]
> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] contiene assembly CLR utilizzati dal motore di database per fornire determinate funzionalità. `Microsoft.SQLServer.Types`L'assembly incluso nell SQL Server installazione viene visualizzato nei metadati come assembly **UNSAFE.** Questo si verifica per motivi strutturali. Questi assembly sono considerati attendibili & sicuro per impostazione predefinita.
  
## <a name="accessing-external-resources"></a>Accesso a risorse esterne  
 Se un tipo definito dall'utente (UDT), un stored procedure o un altro tipo di assembly del costrutto è registrato con il set di autorizzazioni **SAFE,** il codice gestito in esecuzione nel costrutto non è in grado di accedere alle risorse esterne. Tuttavia, se vengono specificati **i EXTERNAL_ACCESS** o **UNSAFE** e il codice gestito tenta di accedere a risorse esterne, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] applica le regole seguenti:  
  
|Se|Risultato|  
|--------|----------|  
|Il contesto di esecuzione corrisponde a un account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|I tentativi di accesso a risorse esterne vengono negati e viene generata un'eccezione di sicurezza.|  
|Il contesto di esecuzione corrisponde a un account di accesso di Windows e rappresenta il chiamante originale.|L'accesso alla risorsa esterna viene effettuato nel contesto di sicurezza dell'account di servizio di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|Il chiamante non è il chiamante originale.|L'accesso viene negato e viene generata un'eccezione di sicurezza.|  
|Il contesto di esecuzione corrisponde a un account di accesso di Windows e rappresenta il chiamante originale, mentre il chiamante è stato rappresentato.|L'accesso verrà effettuato nel contesto di sicurezza del chiamante e non in quello dell'account del servizio.|  
  
## <a name="permission-set-summary"></a>Riepilogo dei set di autorizzazioni  
 Nel grafico seguente vengono riepilogate le restrizioni e le autorizzazioni concesse ai set di autorizzazioni **SAFE,** **EXTERNAL_ACCESS** e **UNSAFE.**  
  
|Funzionalità|**Sicuro**|**EXTERNAL_ACCESS**|**Pericoloso**|   
|-|-|-|-|  
|Autorizzazioni di sicurezza dall'accesso di codice|Sola esecuzione|Esecuzione più accesso a risorse esterne|Senza restrizioni (incluso P/Invoke)|  
|Restrizioni del modello di programmazione|Sì|Sì|Nessuna restrizione|  
|Requisito di verificabilità|Sì|Sì|No|  
|Accesso ai dati locali|Sì|Sì|Sì|  
|Possibilità di chiamare il codice nativo|No|No|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza dell'integrazione con CLR](../../../relational-databases/clr-integration/security/clr-integration-security.md)   
 [Attributi di protezione host e programmazione dell'integrazione CON CLR](../../../relational-databases/clr-integration-security-host-protection-attributes/host-protection-attributes-and-clr-integration-programming.md)   
 [Restrizioni del modello di programmazione dell'integrazione CON CLR](../../../relational-databases/clr-integration/database-objects/clr-integration-programming-model-restrictions.md)   
 [Ambiente CLR](../../../relational-databases/clr-integration/clr-integration-architecture-clr-hosted-environment.md)  
  
  
