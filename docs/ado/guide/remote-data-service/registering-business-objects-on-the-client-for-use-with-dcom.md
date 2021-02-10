---
description: Registrazione degli oggetti business sul client per l'uso con DCOM
title: Registrazione di oggetti business sul client per l'utilizzo con DCOM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- business objects in RDS [ADO]
ms.assetid: 75a21910-607f-463a-ae18-a17130dafb7e
author: rothja
ms.author: jroth
ms.openlocfilehash: 708a59b8c931b723c8f4142f036849da4965fc13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031823"
---
# <a name="registering-business-objects-on-the-client-for-use-with-dcom"></a>Registrazione degli oggetti business sul client per l'uso con DCOM
Per gli oggetti business personalizzati è necessario assicurarsi che il lato client sia in grado di eseguire il mapping del nome del programma (ProgId) a un identificatore (CLSID) che può essere utilizzato su DCOM. Per questo motivo, il ProgID dell'oggetto DCOM deve trovarsi nel registro di sistema sul lato client ed eseguire il mapping all'ID classe dell'oggetto business sul lato server. Per gli altri protocolli supportati (HTTP, HTTPS e in-process), questa operazione non è necessaria.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Se, ad esempio, si espone un oggetto business sul lato server denominato MyBObj con un ID di classe specifico, ad esempio, "{00112233-4455-6677-8899-00AABBCCDDEE}", assicurarsi che le voci seguenti vengano aggiunte al registro di sistema sul lato client:  
  
```console
[HKEY_CLASSES_ROOT]  
\MyBObj\Clsid\(Default) "{00112233-4455-6677-8899-00AABBCCDDEE}"  
```