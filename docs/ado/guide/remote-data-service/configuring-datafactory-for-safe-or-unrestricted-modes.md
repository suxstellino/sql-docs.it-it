---
description: Configurazione di DataFactory per la modalità sicura o senza restrizioni
title: Configurazione di DataFactory per modalità sicure o senza restrizioni | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory configuration in RDS [ADO]
ms.assetid: 8ff24805-dc7a-42ae-b600-5bad0e3f51b8
author: rothja
ms.author: jroth
ms.openlocfilehash: a1727608476b8522f0e9ec6f09ce471de4ffb419
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032049"
---
# <a name="configuring-datafactory-for-safe-or-unrestricted-modes"></a>Configurazione di DataFactory per la modalità sicura o senza restrizioni
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Per impostazione predefinita, ADO viene installato con una configurazione [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) "sicura". La modalità provvisoria per i componenti server RDS indica che sono soddisfatte le condizioni seguenti:  
  
1.  Il gestore è obbligatorio con RDSServer. DataFactory. questa operazione è obbligatoria da un'impostazione della chiave del registro di sistema.  
  
2.  Il gestore predefinito, msdfmap. Handler, viene registrato, presente nell'elenco dei gestori di sicurezza e contrassegnato come gestore predefinito.  
  
3.  Msdfmap.ini file viene installato nella directory Windows. È necessario configurare il file in base alle proprie esigenze, prima di utilizzare Servizi Desktop remoto in modalità a tre livelli.  
  
 Facoltativamente, è possibile configurare un'installazione di **DataFactory** senza restrizioni. **DataFactory** può essere usato direttamente senza il gestore personalizzato. Gli utenti possono comunque utilizzare un gestore personalizzato modificando le stringhe di connessione, ma non è obbligatorio. Per ulteriori informazioni sulle implicazioni dell'utilizzo dell'oggetto **RDSServer. DataFactory** , vedere [protezione delle applicazioni RDS](./securing-rds-applications.md).  
  
 Il file del registro di sistema Handsafe. reg è stato fornito per configurare le voci del registro di sistema del gestore per una configurazione sicura. Per l'esecuzione in modalità provvisoria, eseguire handsafe. reg.  
  
 Dopo l'esecuzione di handsafe. reg, è necessario arrestare e riavviare il servizio di pubblicazione World Wide Web sul server Web digitando i comandi seguenti in una finestra del prompt dei comandi: "NET STOP W3SVC" e "NET START W3SVC".  
  
## <a name="see-also"></a>Vedere anche  
 [Personalizzazione di datafactory](./datafactory-customization.md)   
 [Nozioni fondamentali su RDS](./rds-fundamentals.md)