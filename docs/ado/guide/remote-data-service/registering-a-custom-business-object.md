---
description: Registrazione di un oggetto business personalizzato
title: Registrazione di un oggetto business personalizzato | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- custom business object in RDS [ADO]
- registering custom business objects in RDS [ADO]
- business objects in RDS [ADO]
ms.assetid: e9032ad8-d14c-42e3-ba13-cb5f00084a79
author: rothja
ms.author: jroth
ms.openlocfilehash: 76f3311a622abbd249aa942e1ebdbda33ad06966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036421"
---
# <a name="registering-a-custom-business-object"></a>Registrazione di un oggetto business personalizzato
Per avviare correttamente un oggetto business personalizzato (file con estensione dll o exe) tramite il server Web, è necessario immettere il ProgID dell'oggetto business nel registro di sistema, come illustrato in questa procedura. Questa funzionalità RDS protegge la sicurezza del server Web eseguendo solo file eseguibili approvati.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
> [!NOTE]
>  Per MDAC 2,0 e versioni successive e Windows DAC, l'oggetto business predefinito, [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md), non viene registrato per impostazione predefinita durante l'installazione dell'applicazione livello dati MDAC/Windows. Tuttavia, se **RDSServer. DataFactory** è stato registrato come Safe per l'esecuzione nel computer prima dell'installazione, la voce del registro di sistema viene mantenuta per la nuova installazione.  
  
### <a name="to-register-a-custom-business-object"></a>Per registrare un oggetto business personalizzato:  
  
1.  Fare clic sul pulsante **Start** , quindi scegliere **Esegui**.  
  
2.  Digitare **Regedit** e fare clic su **OK**.  
  
3.  Nell'editor del registro di sistema passare alla chiave del registro di sistema **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\W3SVC\Parameters\ADCLaunch** .  
  
4.  Selezionare la chiave **ADCLaunch** , scegliere **nuovo** dal menu **modifica** e quindi fare clic su **chiave**.  
  
5.  Digitare il ProgID dell'oggetto business personalizzato e premere **invio**. Lasciare vuota la voce **valore** .