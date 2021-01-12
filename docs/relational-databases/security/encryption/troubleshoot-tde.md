---
title: Errori comuni con le chiavi gestite dal cliente in Azure Key Vault
description: Informazioni su come identificare e risolvere i problemi di accesso e gli errori comuni relativi a Transparent Data Encryption (TDE) e alle chiavi gestite dal cliente in Azure Key Vault.
ms.custom: seo-lt-2019
helpviewer_keywords:
- troublshooting, tde akv
- tde akv configuration, troubleshooting
- tde troubleshooting
author: jaszymas
ms.prod: sql
ms.technology: security
ms.reviewer: vanto
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: jaszymas
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: b1725b11a5cc491c4624a7196240546a649f9afa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102230"
---
# <a name="common-errors-for-transparent-data-encryption-with-customer-managed-keys-in-azure-key-vault"></a>Errori comuni relativi a Transparent Data Encryption (TDE) con chiavi gestite dal cliente in Azure Key Vault

[!INCLUDE[asdb-asdbmi-asa](../../../includes/applies-to-version/asdb-asdbmi-asa.md)]

Questo articolo descrive come identificare e risolvere problemi di accesso alle chiavi di Azure Key Vault che hanno reso inaccessibile un database configurato per l'uso di [Transparent Data Encryption (TDE) con chiavi gestite dai clienti in Azure Key Vault](/azure/sql-database/transparent-data-encryption-byok-azure-sql).

## <a name="introduction"></a>Introduzione
Quando la tecnologia TDE è configurata per l'uso di una chiave gestita dal cliente in Azure Key Vault, per mantenere online il database è necessario disporre dell'accesso continuo alla protezione TDE.  Se il server SQL logico non ha più accesso alla protezione TDE gestita dal cliente in Azure Key Vault, un database inizierà a negare tutte le connessioni con il messaggio di errore appropriato e cambierà lo stato in *Inaccessibile* nel portale di Azure.

Per le prime 8 ore, se il problema sottostante di accesso alla chiave di Azure Key Vault viene risolto, il database verrà riparato e riportato online automaticamente. Ciò significa che per tutti i casi di interruzione intermittente e temporanea della connessione di rete non è richiesta alcuna azione da parte dell'utente e il database viene riportato online automaticamente. Nella maggior parte dei casi, per risolvere il problema sottostante di accesso alla chiave di Key Vault è necessario l'intervento dell'utente. 

Se un database inaccessibile non è più necessario, è possibile eliminarlo immediatamente per evitare l'addebito di costi. Tutte le altre azioni sul database non sono consentite fino a quando non viene ripristinato l'accesso alla chiave di Azure Key Vault e il database non è di nuovo online. Quando un database crittografato con chiavi gestite dal cliente non è accessibile, non è nemmeno possibile modificare l'opzione TDE da chiavi gestite dal cliente a chiavi gestite dal servizio nel server. Questo vincolo è necessario per proteggere i dati da accessi non autorizzati mentre le autorizzazioni alla tecnologia di protezione TDE sono state revocate. 

Quando un database rimane inaccessibile per più di 8 ore, non verrà più eseguita la riparazione automatica. Se l'accesso alla chiave di Azure Key Vault richiesta viene ripristinato dopo tale periodo, è necessario riconvalidare manualmente l'accesso alla chiave per riportare online il database. Il ripristino dello stato online del database in questo caso può richiedere una notevole quantità di tempo a seconda delle dimensioni del database. Quando il database tornerà di nuovo online, le impostazioni configurate in precedenza, come il [gruppo di failover](/azure/sql-database/sql-database-auto-failover-group), la cronologia del recupero temporizzato e i tag, **andranno perse**. È pertanto consigliabile implementare un sistema di notifica basato su [Gruppi di azioni](/azure/azure-monitor/platform/action-groups) che consenta di venire a conoscenza della situazione e di risolvere quanto prima i problemi di accesso sottostanti relativi all'insieme di credenziali delle chiavi. 

## <a name="common-errors-causing-databases-to-become-inaccessible"></a>Errori comuni che causano l'inaccessibilità dei database

La maggior parte dei problemi che si verificano quando si usa TDE con Azure Key Vault è causata da uno degli errori di configurazione seguenti:

### <a name="the-key-vault-is-unavailable-or-doesnt-exist"></a>L'insieme di credenziali delle chiavi non è disponibile o non esiste

- L'insieme di credenziali delle chiavi è stato eliminato per errore.
- Il firewall è stato configurato per Azure Key Vault, ma non consente l'accesso ai servizi Microsoft.
- Un errore di rete intermittente rende non disponibile l'insieme di credenziali delle chiavi.

### <a name="no-permissions-to-access-the-key-vault-or-the-key-doesnt-exist"></a>Non sono disponibili le autorizzazioni per accedere all'insieme di credenziali delle chiavi o la chiave non esiste

- La chiave è stata accidentalmente eliminata o disabilitata oppure è scaduta.
- L'AppId dell'istanza di SQL Server logica è stato eliminato per errore.
- L'istanza di SQL Server logica è stata spostata in un'altra sottoscrizione. Se il server logico viene spostato in una sottoscrizione diversa, è necessario creare un nuovo AppId.
- Le autorizzazioni concesse all'AppId per le chiavi non sono sufficienti (non includono le autorizzazioni Ottieni chiave, Esegui il wrapping della chiave e Annulla il wrapping della chiave).
- Le autorizzazioni per l'AppId dell'istanza di SQL Server logica sono state revocate.

## <a name="identify-and-resolve-common-errors"></a>Identificare e risolvere gli errori comuni

In questa sezione sono elencati i passaggi di risoluzione dei problemi per gli errori più comuni.

### <a name="missing-server-identity"></a>Identità del server mancante

**Messaggio di errore**

_401 AzureKeyVaultNoServerIdentity - L'identità del server non è configurata correttamente nel server. Contattare il supporto tecnico._

**Rilevamento**

Usare il cmdlet o il comando seguente per assicurarsi che all'istanza di SQL Server logica sia stata assegnata un'identità:

- Azure PowerShell: [Get-AzureRMSqlServer](/powershell/module/AzureRM.Sql/Get-AzureRmSqlServer) 

- Interfaccia della riga di comando di Azure: [az-sql-server-show](/cli/azure/sql/server#az-sql-server-show)

**Mitigazione**

Usare il cmdlet o il comando seguente per configurare un'identità di Azure AD (AppId) per l'istanza di SQL Server logica:

- Azure PowerShell: [Set-AzureRmSqlServer](/powershell/module/azurerm.sql/set-azurermsqlserver) con l'opzione `-AssignIdentity`.

- Interfaccia della riga di comando di Azure: [az sql server update](/cli/azure/sql/server#az-sql-server-update) con l'opzione `--assign_identity`.

Nel portale di Azure passare all'insieme di credenziali delle chiavi e quindi passare a **Criteri di accesso**. Seguire questa procedura: 

 1. Usare il pulsante **Aggiungi nuovo** per aggiungere l'AppId per il server creato nel passaggio precedente. 
 1. Assegnare le autorizzazioni seguenti: Ottieni chiave, Esegui il wrapping della chiave e Annulla il wrapping della chiave 

Per altre informazioni, vedere [Assegnare un'identità Azure AD al server](/azure/sql-database/transparent-data-encryption-byok-azure-sql-configure#assign-an-azure-ad-identity-to-your-server).

> [!IMPORTANT]
> Se l'istanza di SQL Server logica è stata spostata in un nuovo tenant dopo la configurazione iniziale di TDE con Azure Key Vault, è necessario ripetere il passaggio di configurazione dell'identità di Azure AD per creare il nuovo AppId. Aggiungere quindi l'AppId all'insieme di credenziali delle chiavi e assegnare le autorizzazioni corrette alla chiave. 
>

### <a name="missing-key-vault"></a>Insieme di credenziali delle chiavi mancante

**Messaggio di errore**

_503 AzureKeyVaultConnectionFailed - Non è stato possibile completare l'operazione nel server perché i tentativi di connettersi all'insieme di credenziali delle chiavi di Azure non sono riusciti._

**Rilevamento**

Per identificare l'URI della chiave e l'insieme di credenziali delle chiavi:

1. Usare il cmdlet o il comando seguente per ottenere l'URI della chiave di un'istanza di SQL Server logica specifica:

    - Azure PowerShell: [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey)

    - Interfaccia della riga di comando di Azure: [az-sql-server-tde-key-show](/cli/azure/sql/server/tde-key#az-sql-server-tde-key-show) 

1. Usare l'URI della chiave per identificare l'insieme di credenziali delle chiavi:

    - Azure PowerShell: è possibile esaminare le proprietà della variabile $MyServerKeyVaultKey per ottenere dettagli sull'insieme di credenziali delle chiavi.

    - Interfaccia della riga di comando di Azure: esaminare la protezione della crittografia server restituita per ottenere dettagli sull'insieme di credenziali delle chiavi.

**Mitigazione**

Verificare che l'insieme di credenziali delle chiavi sia disponibile:

- Assicurarsi che l'insieme di credenziali delle chiavi sia disponibile e che l'istanza di SQL Server logica disponga dell'accesso.
- Se l'insieme di credenziali delle chiavi è protetto da un firewall, assicurarsi che sia selezionata la casella di controllo per consentire ai servizi Microsoft di accedere all'insieme di credenziali delle chiavi.
- Se l'insieme di credenziali delle chiavi è stato eliminato per errore, è necessario ripetere la configurazione dall'inizio.


### <a name="missing-key"></a>Chiave mancante

**Messaggi di errore**

_404 ServerKeyNotFound - La chiave server richiesta non è stata trovata nella sottoscrizione corrente._ 

_409 ServerKeyDoesNotExists - La chiave server non esiste._

**Rilevamento**

Per identificare l'URI della chiave e l'insieme di credenziali delle chiavi:

- Usare il cmdlet o i comandi in [Insieme di credenziali delle chiavi mancante](#missing-key-vault) per identificare l'URI della chiave aggiunto all'istanza di SQL Server logica. L'esecuzione dei comandi restituisce l'elenco delle chiavi.

**Mitigazione**

Verificare che la protezione TDE sia presente in Azure Key Vault:

1. Identificare l'insieme di credenziali delle chiavi, quindi passare all'insieme di credenziali delle chiavi nel portale di Azure.
1. Assicurarsi che la chiave identificata dall'URI della chiave sia presente.

### <a name="missing-permissions"></a>Autorizzazioni mancanti

**Messaggio di errore**

_401 AzureKeyVaultMissingPermissions - Nel server mancano le autorizzazioni obbligatorie per Azure Key Vault._

**Rilevamento**

Per identificare l'URI della chiave e l'insieme di credenziali delle chiavi: 

- Usare il cmdlet o i comandi in [Insieme di credenziali delle chiavi mancante](#missing-key-vault) per identificare l'insieme di credenziali delle chiavi usato dall'istanza di SQL Server logica.

**Mitigazione**

Verificare che l'istanza di SQL Server logica abbia le autorizzazioni per l'insieme di credenziali delle chiavi e le autorizzazioni corrette per accedere alla chiave:

- Nel portale di Azure passare all'insieme di credenziali delle chiavi e quindi passare a **Criteri di accesso**. Trovare l'AppId dell'istanza di SQL Server logica.  
- Se l'AppId è presente, verificare che abbia le autorizzazioni per le chiavi seguenti: Ottieni chiave, Esegui il wrapping della chiave e Annulla il wrapping della chiave.
- Se l'AppId non è presente, aggiungerlo facendo clic sul pulsante **Aggiungi nuovo**. 

## <a name="getting-tde-status-from-the-activity-log"></a>Recupero dello stato di TDE dal log attività

Per consentire il monitoraggio dello stato del database a causa di problemi di accesso alla chiave di Azure Key Vault, gli eventi seguenti verranno registrati nel [log attività](/azure/service-health/alerts-activity-log-service-notifications) per l'ID della risorsa in base all'URL di Azure Resource Manager e Subscription+Resourcegroup+ServerName+DatabaseName: 

**Evento registrato quando il servizio non ha più accesso alla chiave di Azure Key Vault**

Nome evento: MakeDatabaseInaccessible 

Stato: Started 

Descrizione: il database non ha più accesso alla chiave di Azure Key Vault ed è ora inaccessibile: <error message>   

 

**Evento registrato quando inizia il calcolo del tempo di attesa di 8 ore per la riparazione automatica** 

Nome evento: MakeDatabaseInaccessible 

Stato: InProgress 

Descrizione: il database rimane in attesa che l'accesso alla chiave di Azure Key Vault venga ristabilito dall'utente entro 8 ore.   

 

**Evento registrato quando il database è stato riportato online automaticamente**

Nome evento: MakeDatabaseAccessible 

Stato: Operazione completata 

Descrizione: l'accesso del database alla chiave di Azure Key Vault è stato ristabilito e il database è ora online. 

 

**Evento registrato quando il problema non è stato risolto entro 8 ore e l'accesso alla chiave di Azure Key Vault deve essere convalidato manualmente** 

Nome evento: MakeDatabaseInaccessible 

Stato: Operazione completata 

Descrizione: il database non è accessibile e richiede all'utente di risolvere gli errori di Azure Key Vault e di ristabilire l'accesso alla chiave di Azure Key Vault mediante la riconvalida della chiave. 

 

**Evento registrato quando il database viene riportato online dopo la riconvalida manuale della chiave**

Nome evento: MakeDatabaseAccessible 

Stato: Operazione completata 

Descrizione: l'accesso del database alla chiave di Azure Key Vault è stato ristabilito e il database è ora online. 

 

**Evento registrato quando la riconvalida dell'accesso alla chiave di Azure Key Vault ha esito positivo e il database viene riportato online**

Nome evento: MakeDatabaseAccessible 

Stato: Started 

Descrizione: il ripristino dell'accesso del database alla chiave di Azure Key Vault è stato avviato. 

 

**Evento registrato quando la riconvalida dell'accesso alla chiave di Azure Key Vault ha esito negativo**

Nome evento: MakeDatabaseAccessible 

Stato: Operazione non riuscita 

Descrizione: il ripristino dell'accesso del database alla chiave di Azure Key Vault non è riuscito. 


## <a name="next-steps"></a>Passaggi successivi

- Scoprire [Integrità risorse di Azure](/azure/service-health/resource-health-overview).
- Configurare [Gruppi di azioni](/azure/azure-monitor/platform/action-groups) per ricevere notifiche e avvisi in base alle preferenze, ad esempio Posta elettronica/SMS/Push/Messaggio vocale, App per la logica, Webhook, Gestione dei servizi IT o Runbook di Automazione.