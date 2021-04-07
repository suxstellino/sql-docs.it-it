---
title: SQL Server con abilitazione di Azure Arc - Note sulla versione
description: Note sulla versione più recenti
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 04/06/2021
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 96239b337ed917b74effc58ac4e55b67b568a9e4
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557534"
---
# <a name="release-notes---azure-arc-enabled-sql-server-preview"></a>Note sulla versione - SQL Server con abilitazione di Azure Arc (anteprima)

> [!NOTE]
> In quanto funzionalità di anteprima, la tecnologia presentata in questo articolo è soggetta alle [condizioni per l'utilizzo supplementari per le anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="april-2021"></a>Aprile 2021

### <a name="breaking-change"></a>Modifica

Nessuna modifica di rilievo

### <a name="other-changes"></a>Altre modifiche

È stata aggiunta una nuova proprietà *LicenseType* al tipo di risorsa **SQL Server-Azure Arc** . Indica se l'istanza di SQL Server richiede una licenza. La proprietà può avere uno dei valori seguenti:

| **Valore** | **Descrizione** |
|:--|:--|
|Paid|Indica che l'istanza di SQL utilizza Enterprise, standard o Web Edition di SQL Server|
|Gratuito|Indica che l'istanza di SQL utilizza Express o Developer Edition di SQL Server|
|HADR|Indica che l'istanza di SQL è una replica in un gruppo di disponibilità. Se è coperto da Software Assurance, potrebbe non richiedere una licenza. Per ulteriori informazioni, vedere [SQL Server condizioni di licenza commerciale](https://www.microsoft.com/licensing/terms/productoffering/SQLServer/EAEAS).

> [!NOTE]
> Per le risorse **SQL Server-Azure Arc** esistenti, questa proprietà mostrerà un valore *null* . Verrà automaticamente aggiornata con il valore corretto dopo l'abilitazione di Azure Arc SQL Server diventa disponibile a livello generale.

## <a name="december-2020"></a>Dicembre 2020

### <a name="breaking-change"></a>Modifica

Questa versione introduce un [provider di risorse](/azure/azure-resource-manager/management/azure-services-resource-providers) aggiornato chiamato `Microsoft.AzureArcData`. Prima di poter continuare a usare SQL Server con abilitazione di Azure Arc, è necessario registrare questo provider di risorse. Vedere le istruzioni per la registrazione del provider di risorse nella sezione [Prerequisiti](connect.md#prerequisites).

Se sono presenti risorse SQL Server - Azure Arc esistenti, seguire questa procedura per eseguirne la migrazione allo spazio dei nomi Microsoft.AzureArcData.

1. Avviare [Cloud Shell](https://shell.azure.com/). Per indicazioni dettagliate, [leggere altre informazioni su PowerShell in Cloud Shell](/azure/cloud-shell/quickstart-powershell).

2. Caricare lo script nella shell usando il comando seguente:

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/manage/azure-arc-enabled-sql-server/migrate-to-azure-arc-data.ps1 -o migrate-to-azure-arc-data.ps1
    ```
3. Eseguire lo script.  

    ```console
   ./migrate-to-azure-arc-data.ps1
    ```

> [!NOTE]
> - Per incollare i comandi nella shell, usare `Ctrl-Shift-V` in Windows o `Cmd-v` in MacOS.
> - Il `curl` comando copia lo script direttamente nella cartella Home associata alla sessione di cloud Shell.
> - Lo script richiederà il nome del gruppo di risorse e stamperà un messaggio al termine della migrazione.

### <a name="other-changes"></a>Altre modifiche

* La proprietà *TCPPorts* nel tipo di risorsa **SQL Server - Azure Arc** è stata rinominata *TCPStaticPorts*
* Le autorizzazioni necessarie non sono estese come in precedenza. Per informazioni dettagliate, vedere la sezione [Autorizzazione richiesta](overview.md#required-permissions).

### <a name="known-issues"></a>Problemi noti

* La proprietà *CreateTime* non verrà aggiunta alle risorse appena create nello spazio dei nomi AzureArcData, incluse le risorse **SQL Server - Azure Arc**.

## <a name="october-2020"></a>Ottobre 2020

L'aggiornamento di ottobre include i miglioramenti seguenti:

* Il pannello Register Azure Arc Enabled SQL Server (Registra SQL Server con abilitazione di Azure Arc) include ora la scheda **Tag**. I tag sono inclusi nello script di registrazione e si riflettono nelle risorse **SQL Server - Azure Arc**. Per informazioni dettagliate, vedere [Connettere l'istanza di SQL Server ad Azure Arc](connect.md).

* La voce **Environment Health** (Integrità ambiente) supporta ora l'attivazione di **Valutazione SQL** dal portale tramite la distribuzione di una *CustomScriptExtension*. Per informazioni dettagliate, vedere [Configurare Valutazione SQL](assess.md#run-on-demand-sql-assessment).

### <a name="known-issues"></a>Problemi noti

Nella versione di ottobre sono presenti i problemi seguenti:

* Per la connessione di istanze di SQL Server ad Azure Arc è necessario un account con un ampio set di autorizzazioni. Per informazioni dettagliate, vedere [Autorizzazioni necessarie](overview.md#required-permissions).

## <a name="september-2020"></a>Settembre 2020

SQL Server con abilitazione di Azure Arc è stato rilasciato per l'anteprima pubblica. SQL Server con abilitazione di Azure Arc estende i servizi di Azure alle istanze di SQL Server ospitate all'esterno di Azure nel data center del cliente, nella rete perimetrale o in un ambiente multi-cloud.

Per informazioni dettagliate, vedere [Panoramica di SQL Server con abilitazione di Azure Arc](overview.md)

### <a name="known-issues"></a>Problemi noti

Nella versione di settembre sono presenti i problemi seguenti:

* Il pannello **Register Azure Arc Enabled SQL Server** (Registra SQL Server con abilitazione di Azure Arc) non supporta la configurazione di tag personalizzati. Per aggiungere tag personalizzati, aprire la risorsa **SQL Server - Azure Arc** dopo la registrazione e modificare i tag nella pagina **Panoramica**.

* Per la connessione di istanze di SQL Server ad Azure Arc è necessario un account con un ampio set di autorizzazioni. Per informazioni dettagliate, vedere [Autorizzazioni necessarie](overview.md#required-permissions).

## <a name="next-steps"></a>Passaggi successivi

**Si desidera fare semplicemente una prova?**  Iniziare rapidamente con la [guida di avvio rapido per SQL Server con abilitazione di Azure Arc](https://aka.ms/AzureArcSqlServerJumpstart).