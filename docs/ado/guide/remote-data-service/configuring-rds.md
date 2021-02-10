---
description: Configurazione di RDS
title: Configurazione di RDS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS configuration [ADO]
ms.assetid: 5dd48483-858a-48c2-98ce-f2359abe1f59
author: rothja
ms.author: jroth
ms.openlocfilehash: 1af0a8c1fd4c54f73ad0751e4ac4e93b45a04783
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032033"
---
# <a name="configuring-rds"></a>Configurazione di RDS
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Per implementare servizi desktop remoto in modo efficiente, assicurarsi di avere familiarità con le diverse configurazioni disponibili. Questa sezione include informazioni importanti su sicurezza e scalabilità nell'implementazione di Servizi Desktop remoto. Per informazioni sulla configurazione dei computer per l'utilizzo di Servizi Desktop remoto, vedere gli argomenti seguenti.  
  
-   [Concessione dei privilegi Guest a un computer server Web](./granting-guest-privileges-to-a-web-server-computer.md)  
  
-   [Registrazione di un oggetto business personalizzato](./registering-a-custom-business-object.md)  
  
-   [Contrassegnare gli oggetti business come sicuri per lo scripting](./marking-business-objects-as-safe-for-scripting.md)  
  
-   [Registrazione degli oggetti business sul client per l'uso con DCOM](./registering-business-objects-on-the-client-for-use-with-dcom.md)  
  
-   [Configurazione del formato di marshalling del flusso DCOM](./setting-dcom-stream-marshaling-format.md)  
  
-   [Abilitazione di un file DLL per l'esecuzione su DCOM](./enabling-a-dll-to-run-on-dcom.md)  
  
-   [Configurazione dei server virtuali su IIS](./configuring-virtual-servers-on-iis.md)  
  
-   [Sicurezza delle applicazioni RDS](./securing-rds-applications.md)  
  
-   [Configurazione di DataFactory per la modalità sicura o senza restrizioni](./configuring-datafactory-for-safe-or-unrestricted-modes.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo di tecnologie correlate con RDS](./using-related-technologies-with-rds.md)   
 [Personalizzazione di datafactory](./datafactory-customization.md)   
 [Risoluzione dei problemi di RDS](./troubleshooting-rds.md)