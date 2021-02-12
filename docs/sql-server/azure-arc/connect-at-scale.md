---
title: Connettere istanze di SQL Server ad Azure Arc su larga scala
titleSuffix: ''
description: Questo articolo illustra come connettere istanze di SQL Server come SQL Server abilitato per Azure Arc (anteprima) usando un'entità servizio.
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: b78c76504cb9ffb76b62c1867630760fe52cf22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063186"
---
# <a name="connect-sql-server-instances-to-azure-arc-at-scale"></a>Connettere istanze di SQL Server ad Azure Arc su larga scala

È possibile connettere ad Azure Arc più istanze di SQL Server installate in più computer Windows o Linux usando lo stesso [script generato per un singolo computer](connect.md). Lo script connette e registra ogni computer e le istanze di SQL Server installate sul computer ad Azure Arc. Per un'esperienza ottimale è consigliabile usare un'[entità servizio](/azure/active-directory/develop/app-objects-and-service-principals) Azure Active Directory. Un'entità servizio è un'identità di gestione limitata speciale, alla quale viene concessa solo l'autorizzazione minima necessaria per connettere i computer ad Azure e per creare risorse di Azure per il server abilitato per Azure Arc e per SQL Server abilitato per Azure Arc. Questa soluzione è più sicura rispetto all'uso di un account con autorizzazioni elevate, come un Amministratore tenant, e segue le procedure consigliate per la sicurezza del controllo di accesso.  

I metodi di installazione per installare e configurare l'agente di Connected Machine richiedono che il metodo automatizzato usato disponga delle autorizzazioni di amministratore per i computer. In Linux, tramite l'account radice, e in Windows, come membri del gruppo Administrators locale.

Prima di iniziare, esaminare i [prerequisiti](overview.md#prerequisites) e verificare di aver creato un ruolo personalizzato che soddisfa le autorizzazioni necessarie.

## <a name="connecting-multiple-sql-server-instances-on-windows-using-azure-powershell"></a>Connessione di più istanze di SQL Server in Windows con Azure PowerShell

[Azure PowerShell](/powershell/azure/install-az-ps) deve essere installato in ogni computer.

1. Creare l'entità servizio usando PowerShell tramite il cmdlet [`New-AzADServicePrincipal`](/powershell/module/az.resources/new-azadserviceprincipal). Verificare di archiviare l'output in una variabile. In caso contrario non sarà possibile recuperare la password che verrà richiesta in un secondo momento.

    ```azurepowershell-interactive
    $sp = New-AzADServicePrincipal -DisplayName "Arc-for-servers" -Role <your custom role>
    $sp
    ```

    ```output
    Secret                : System.Security.SecureString
    ServicePrincipalNames : {ad9bcd79-be9c-45ab-abd8-80ca1654a7d1, https://Arc-for-servers}
    ApplicationId         : ad9bcd79-be9c-45ab-abd8-80ca1654a7d1
    ObjectType            : ServicePrincipal
    DisplayName           : Hybrid-RP
    Id                    : 5be92c87-01c4-42f5-bade-c1c10af87758
    Type                  :
    ```

   > [!NOTE]
   > Quando si crea un'entità servizio, è necessario essere un proprietario o un amministratore Accesso utente nella sottoscrizione che si vuole usare per l'onboarding. Se non si hanno autorizzazioni sufficienti per creare assegnazioni di ruolo, l'entità servizio potrebbe essere creata, ma non sarà in grado di eseguire l'onboarding dei computer. Le istruzioni per creare un ruolo personalizzato sono disponibili in [Autorizzazioni necessarie](overview.md#required-permissions).

2. Recuperare la password archiviata nella variabile `$sp`:

   ```azurepowershell-interactive
   $credential = New-Object pscredential -ArgumentList "temp", $sp.Secret
   $credential.GetNetworkCredential().password
   ```
3. Recuperare il valore dell'ID tenant dell'entità servizio:
 
   ```azurepowershell-interactive
   $tenantId= (Get-AzContext).Tenant.Id
   ```
4. Copiare e salvare i valori di password, ID applicazione e ID tenant usando le procedure di sicurezza appropriate. Se si dimentica o si perde la password dell'entità servizio, è possibile reimpostarla usando il cmdlet [`New-AzADSpCredential`](/powershell/module/azurerm.resources/new-azurermadspcredential).

   > [!NOTE]
   > Si noti che Azure Arc per server non supporta attualmente l'accesso con un certificato, pertanto l'entità servizio deve avere un segreto con il quale va eseguita l'autenticazione.

5. Scaricare lo script di PowerShell dal portale seguendo le istruzioni riportate in [Connettere l'istanza SQL Server ad Azure Arc](connect.md).

6. Aprire lo script in un'istanza di amministrazione di PowerShell ISE e sostituire le variabili di ambiente seguenti usando i valori generati durante il provisioning dell'entità servizio descritto in precedenza. Queste variabili sono inizialmente vuote.

   ```azurepowershell-interactive
   $servicePrincipalAppId="{serviceprincipalAppID}"
   $servicePrincipalSecret="{serviceprincipalPassword}"
   $servicePrincipalTenantId="{serviceprincipalTenantId}"
   ```

7. Eseguire lo script in ogni computer di destinazione

## <a name="connecting-multiple-sql-server-instances-on-linux-using-azure-cli"></a>Connessione di più istanze SQL Server in Linux tramite l'interfaccia della riga di comando di Azure

In ogni computer di destinazione deve essere installata l'[interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli). Lo script di registrazione esegue automaticamente l'accesso ad Azure con le credenziali dell'entità servizio, se sono disponibili e se nessun altro utente è già connesso. Usare la procedura seguente per connettere le istanze di SQL Server in più computer Linux.

1. Creare un'entità servizio usando il comando ["az ad sp create-for-rbac"](/cli/azure/ad/sp#az_ad_sp_create_for_rbac).

   ```azurecli-interactive
   az ad sp create-for-rbac --name <your service principal name> --role <your custom role name>
   ```

   ```output
   { "appId": "d2ff754a-e10a-4eb6-9cdc-ce6e7a4687db",
     "displayName": "Arc-for-servers",
     "name": "http://Arc-for-servers",
     "password": {SomeRandomlyGeneratedPassword}",
     "tenant": "2530e75f-673b-4841-8270-47ca6a34ef4f"
   }
   ```

   > [!NOTE]
   > Quando si crea un'entità servizio, è necessario essere un proprietario o un amministratore Accesso utente nella sottoscrizione che si vuole usare per l'onboarding. Se non si hanno autorizzazioni sufficienti per creare assegnazioni di ruolo, l'entità servizio potrebbe essere creata, ma non sarà in grado di eseguire l'onboarding dei computer. Le istruzioni per creare un ruolo personalizzato sono disponibili in [Autorizzazioni necessarie](overview.md#required-permissions).

2. Scaricare lo script della shell di Linux dal portale seguendo le istruzioni riportate in [Connettere l'istanza SQL Server ad Azure Arc](connect.md).

3. Sostituire le variabili seguenti nello script usando i valori restituiti dal comando "az ad sp create-for-rbac". Queste variabili sono inizialmente vuote.

   ```bash
   servicePrincipalAppId="{serviceprincipalAppID}"
   servicePrincipalSecret="{serviceprincipalPassword}"
   servicePrincipalTenant="{serviceprincipalTenant}"
   ```

3. Eseguire lo script in ogni computer di destinazione
 
   ```bash
   sudo chmod +x ./RegisterSqlServerArc.sh
   ./RegisterSqlServerArc.sh
   ```

## <a name="validate-successful-onboarding"></a>Verificare che l'onboarding sia riuscito

Dopo aver registrato le istanze di SQL Server con SQL Server abilitato per Azure Arc (anteprima), passare al [portale di Azure](https://aka.ms/azureportal) e visualizzare le risorse di Azure Arc appena create. Viene visualizzata una nuova risorsa __Computer - Azure Arc__ per ogni computer connesso e una nuova risorsa __SQL Server - Azure Arc__ per ogni istanza di SQL Server registrata. 

![Onboarding riuscito](./media/join-at-scale/successful-onboard.png)

## <a name="next-steps"></a>Passaggi successivi

- Informazioni su come gestire il computer usando i [criteri di Azure](/azure/governance/policy/overview), ad esempio la configurazione di VM [Guest](/azure/governance/policy/concepts/guest-configuration), verificare che il computer stia segnalando l'area di lavoro Log Analytics prevista, abilitare il monitoraggio con [Monitoraggio di Azure con macchine virtuali](/azure/azure-monitor/insights/vminsights-enable-policy) e molto altro ancora.

- Altre informazioni sull'[agente Log Analytics](/azure/azure-monitor/platform/log-analytics-agent). L'agente di Log Analytics per Windows e Linux è necessario quando si vuole monitorare in modo proattivo il sistema operativo e i carichi di lavoro in esecuzione nella macchina virtuale, gestirla con runbook di automazione o soluzioni come Gestione aggiornamenti o usare altri servizi di Azure, come il [Centro sicurezza di Azure](/azure/security-center/security-center-intro).

- Informazioni su come [configurare l'istanza di SQL Server per il controllo di integrità periodico dell'ambiente con Valutazione SQL su richiesta](assess.md)

- Informazioni su come [configurare la sicurezza dei dati avanzata per l'istanza di SQL Server](configure-advanced-data-security.md)