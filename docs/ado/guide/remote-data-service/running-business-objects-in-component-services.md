---
description: Esecuzione di oggetti business nei Servizi componenti
title: Esecuzione di oggetti business in Servizi componenti | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- component services in RDS [ADO]
ms.assetid: 3077d0b6-42d6-4f10-8e5d-42e6204f1109
author: rothja
ms.author: jroth
ms.openlocfilehash: 7129254d609e1bd80853ccf5008527bfbf938c30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036381"
---
# <a name="running-business-objects-in-component-services"></a>Esecuzione di oggetti business nei Servizi componenti
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Gli oggetti business possono essere file eseguibili (con estensione exe) o librerie a collegamento dinamico (con estensione dll). La configurazione utilizzata per eseguire l'oggetto business varia a seconda che l'oggetto sia un file con estensione dll o exe:  
  
-   Gli oggetti business creati come file con estensione exe possono essere chiamati tramite DCOM. Se questi oggetti business vengono utilizzati tramite Internet Information Services (IIS), sono soggetti a un ulteriore marshalling dei dati, rallentando le prestazioni del client.  
  
-   Gli oggetti business creati come file con estensione dll possono essere utilizzati tramite IIS e quindi anche tramite HTTP. Possono inoltre essere utilizzati su DCOM solo tramite Servizi componenti o tramite Microsoft Transaction Server, se si utilizza Windows NT. Le dll degli oggetti business dovranno essere registrate nel computer server IIS per accedervi tramite IIS. Per informazioni su come configurare una DLL per l'esecuzione su DCOM, vedere la sezione relativa all' [Abilitazione di una dll per l'esecuzione su DCOM](./enabling-a-dll-to-run-on-dcom.md).  
  
> [!NOTE]
>  Quando gli oggetti business nel livello intermedio vengono implementati come componenti di Servizi componenti tramite **GetObjectContext**, **secomplete** e **SetAbort**, gli oggetti business possono utilizzare i servizi componenti (o MTS, se si utilizzano gli oggetti di contesto di Windows NT) per mantenere lo stato tra più chiamate del client. Questo scenario è possibile con DCOM, che in genere viene implementato tra client attendibili e server in una rete Intranet. In questo caso, il Servizi Desktop remoto [. L'oggetto DataSpace](../../reference/rds-api/dataspace-object-rds.md) e il metodo [CreateObject](../../reference/rds-api/createobject-method-rds.md) sul lato client vengono sostituiti dall'oggetto del contesto di transazione e dal metodo **CreateInstance** , forniti dall'interfaccia **ITransactionContext** e implementati da Servizi componenti.  
  
## <a name="see-also"></a>Vedere anche  
 [Nozioni fondamentali su RDS](./rds-fundamentals.md)