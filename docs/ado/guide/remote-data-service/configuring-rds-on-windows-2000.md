---
description: Configurazione di RDS in Windows 2000
title: Configurazione di RDS in Windows 2000 | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS configuration [ADO], Windows 2000
ms.assetid: ef37e858-c05f-4f52-a65f-3ce6037e0d03
author: rothja
ms.author: jroth
ms.openlocfilehash: caa8fe846651f65e4316c81f29f2f18078628658
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036571"
---
# <a name="configuring-rds-on-windows-2000"></a>Configurazione di RDS in Windows 2000
Se si verificano problemi durante il funzionamento corretto di RDS dopo l'aggiornamento a Windows 2000, attenersi alla procedura seguente per risolvere il problema:  
  
1.  Verificare che il servizio di pubblicazione World Wide Web venga eseguito per primo passando al server https://utilizzando Internet Explorer. Se non è possibile accedere al server Web in questo modo, aprire un prompt dei comandi e immettere il comando seguente "NET START W3SVC".  
  
2.  Nel menu Start selezionare Esegui. Digitare msdfmap.ini e quindi fare clic su OK per aprire il file di msdfmap.ini nel blocco note. Selezionare la sezione [CONNECT DEFAULT] e se il parametro ACCESS è impostato su NoAccess, impostarlo su READONLY.  
  
3.  Utilizzando l'utilità RegEdit, passare a "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DataFactory\HandlerInfo" e verificare che **HandlerRequired** sia impostato su 0 e **DefaultHandler** sia "" (stringa null).  
  
     **Nota** Se si apportano modifiche a questa sezione del registro di sistema, è necessario arrestare e riavviare il servizio di pubblicazione World Wide Web immettendo i comandi seguenti al prompt dei comandi: "NET STOP W3SVC" e "NET START W3SVC".  
  
4.  Usando l'utilità RegEdit, spostarsi nel registro di sistema in "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC\Parameters\ADCLaunch" e verificare che sia presente una chiave denominata **RDSServer. DataFactory**. In caso contrario, crearlo.  
  
5.  Utilizzando Gestione servizi Internet, individuare il sito Web predefinito e visualizzare le proprietà della radice virtuale MSADC. Esaminare le restrizioni relative alla sicurezza, all'indirizzo IP e al nome di dominio della directory. Se l'opzione "accesso negato" è selezionata, selezionare "concesso".  
  
 Assicurarsi di provare a riavviare il server se le modifiche apportate a non sembrano risolvere prima il problema.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/). A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. Eseguire la migrazione di applicazioni che utilizzano Servizi Desktop remoto a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Vedere anche  
 [Nozioni fondamentali su RDS](./rds-fundamentals.md)