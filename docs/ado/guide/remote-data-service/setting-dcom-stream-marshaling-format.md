---
description: Configurazione del formato di marshalling del flusso DCOM
title: Impostazione del formato di marshalling del flusso DCOM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- dcom stream marshaling format in rds [ADO]
ms.assetid: 46664ac5-d6e6-4457-8bae-3a98300f2a41
author: rothja
ms.author: jroth
ms.openlocfilehash: 8286ae626f4c505daa4b74e31dcc72a49e12d677
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036371"
---
# <a name="setting-dcom-stream-marshaling-format"></a>Configurazione del formato di marshalling del flusso DCOM
Un computer client che usa componenti di RDS 1,5 o versione precedente non è compatibile con un server che usa componenti di RDS 2,0 o versione successiva. Quando si utilizza DCOM come protocollo sottostante, il supporto per Servizi Desktop remoto 2,0 o versione successiva è più efficiente per il trasporto di oggetti [Recordset](../../reference/ado-api/recordset-object-ado.md) . Se il client esegue componenti da Servizi Desktop remoto 1,5 o versioni precedenti, è possibile impostare il server in modo che funzioni con il supporto Servizi Desktop remoto precedente (denominato RDS 1,0) o il supporto RDS più recente (denominato RDS 2,0 o versione successiva). Impostare una delle seguenti voci del registro di sistema:  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
```console
[HKEY_CLASSES_ROOT]  
\CLSID\[58ECEE30-E715-11CF-B0E3-00AA003F000F}\ADTGOptions]"MarshalFormat"="RDS10"  
```  
  
 -oppure-  
  
```console
[HKEY_CLASSES_ROOT]  
\CLSID\[58ECEE30-E715-11CF-B0E3-00AA003F000F}\ADTGOptions]"MarshalFormat"="RDS20"  
```