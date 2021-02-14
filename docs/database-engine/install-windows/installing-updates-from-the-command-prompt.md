---
title: Installazione degli aggiornamenti dal prompt dei comandi | Microsoft Docs
description: Questo articolo descrive la sintassi dei comandi per l'installazione degli aggiornamenti di SQL Server. È possibile provare gli script di installazione e adattarli alle esigenze dell'organizzazione.
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: bc98ba2b-aae9-4d01-aa85-d4c36428cb0b
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ff4d078c165b91f147e39cecb3976939929cac7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353060"
---
# <a name="installing-updates-from-the-command-prompt"></a>Installazione degli aggiornamenti dal prompt dei comandi

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Provare gli script di installazione e adattarli alle esigenze della propria azienda. 
 
## <a name="sample-syntax-for-installation"></a>Sintassi di esempio per l'installazione 
Il nome del pacchetto di aggiornamento può variare e includere una lingua, un'edizione e un componente processore. Applicare un aggiornamento dal prompt dei comandi sostituendo <package_name> con il nome del pacchetto di aggiornamento. 
 
- Aggiornare una singola istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e tutti i componenti condivisi, come [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e gli strumenti di gestione: L'istanza può essere specificata utilizzando il parametro InstanceName o InstanceID. Per aggiornare un'istanza predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è necessario specificare il parametro InstanceID.

    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /InstanceName=MyInstance
    ```
    o 
    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /InstanceID=\<Instance ID>. 
    ```

- La configurazione consente di integrare gli aggiornamenti più recenti del pacchetto con l'installazione del prodotto principale, in modo che il prodotto principale e i relativi aggiornamenti applicabili vengano installati contemporaneamente. È possibile preparare un'installazione dell'istanza del motore di database affinché includa l'aggiornamento del prodotto: 

    ```
    setup.exe /q /IAcceptSQLServerLicenseTerms /ACTION=PrepareImage /UpdateEnabled=True /UpdateSource=\<path where the update is downloaded> /INSTANCEID=\<Instance ID> /FEATURES=SQLEngine. 
    ```

- Aggiornare solo i componenti condivisi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], come ad esempio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e gli strumenti di gestione: 

    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch 
    ```

- Aggiornare tutte le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer e tutti i componenti condivisi, come ad esempio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e gli strumenti di gestione: 

    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /AllInstances. 
    ```

- Rimuovere un aggiornamento da una singola istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e tutti i componenti condivisi, come ad esempio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e gli strumenti di gestione: 

    ```
    <package_name>.exe /qs /Action=RemovePatch /InstanceName=MyInstance. 
    ```

- Eliminare un aggiornamento dai componenti condivisi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], come ad esempio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e gli strumenti di gestione: 

    ```
    <package_name>.exe /qs /Action=RemovePatch 
    ```

  > [!NOTE] 
  > Il programma di installazione dell'aggiornamento verifica che la versione dei componenti condivisi sia sempre corrispondente o superiore alla versione dell'istanza di livello più alto. 
 
## <a name="supported-parameters"></a>Parametri supportati 
 
> [!IMPORTANT] 
> Se possibile, specificare le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file di script, proteggere tale file per impedire l'accesso non autorizzato. 
 
|Opzione|Descrizione| 
|------------|-----------------| 
|**/?**|Visualizza la Guida del prompt dei comandi per l'installazione automatica| 
|**/action=Patch or /action=RemovePatch**|Specifica l'azione di installazione: Patch o RemovePatch.| 
|**/allinstances**|Applica l'aggiornamento di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a tutte le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e a tutti i componenti condivisi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che non supportano le istanze.| 
|**/instancename=InstanceName***|Applica l'aggiornamento di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominata InstanceName e a tutti i componenti condivisi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che non supportano le istanze.| 
|**/InstanceID=Inst1**|Applica l'aggiornamento di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] all'istanza Inst1 di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e a tutti i componenti condivisi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che non supportano le istanze.| 
|**/quiet**|Esegue l'installazione dell'aggiornamento di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modalità automatica.| 
|**/qs**|Visualizza solo la finestra di dialogo dell'interfaccia utente relativa allo stato.| 
|**/UpdateEnabled**|Specifica se durante la configurazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debbano essere individuati e inclusi aggiornamenti del prodotto. I valori validi sono True e False oppure 1 e 0. Per impostazione predefinita, gli aggiornamenti individuati verranno inclusi nel programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .| 
|**/IAcceptSQLServerLicenseTerms**|Obbligatorio solo quando il parametro /Q o /QS è specificato per le installazioni automatiche.| 
 
 *Non è possibile specificare questo parametro per applicare un aggiornamento a un'istanza predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Specificare il parametro /instanceID. 
 
## <a name="see-also"></a>Vedere anche 
 [Panoramica sull'installazione dei servizi SQL Server](./install-sql-server-servicing-updates.md) 
 
