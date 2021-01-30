---
description: Oggetto DataFactory (RDSServer)
title: Oggetto DataFactory (RDSServer) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- DataFactory object [ADO]
ms.assetid: e75240c2-b749-471e-b6ea-98cae232efbe
author: rothja
ms.author: jroth
ms.openlocfilehash: 75c727e8c857dcb5c5922f52e56a37e99f223293
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163824"
---
# <a name="datafactory-object-rdsserver"></a>Oggetto DataFactory (RDSServer)
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Questo oggetto business predefinito sul lato server implementa i metodi che forniscono l'accesso ai dati in lettura/scrittura alle origini dati specificate per le applicazioni lato client.  
  
 L'oggetto **RDSServer. DataFactory** è progettato come un oggetto di automazione lato server che riceve le richieste client. In un'implementazione Internet, si trova in un server Web e ne viene creata un'istanza dal componente ADISAPI. L'oggetto **RDSServer. DataFactory** fornisce l'accesso in lettura e scrittura alle origini dati specificate, ma non contiene alcuna logica di convalida o di regole business.  
  
 Se si usa un metodo disponibile sia in **RDSServer. DataFactory** che in Servizi Desktop remoto [. Oggetti DataControl](./datacontrol-object-rds.md) , Remote Data Service USA **RDS. Versione di DataControl** per impostazione predefinita. Per impostazione predefinita si presuppone uno scenario di programmazione di base, in cui **RDSServer. DataFactory** funge da oggetto business generico sul lato server.  
  
 Se si vuole che l'applicazione Web gestisca l'elaborazione sul lato server specifica dell'attività, è possibile sostituire **RDSServer. DataFactory** con un oggetto business personalizzato.  
  
 È possibile creare oggetti business sul lato server che chiamano i metodi **RDSServer. DataFactory** , ad esempio [query](./query-method-rds.md) e [CreateRecordset](./createrecordset-method-rds.md). Questa operazione è utile se si desidera aggiungere funzionalità agli oggetti business, ma si sfruttano le tecnologie Remote Data Service esistenti.  
  
 L'oggetto **DataFactory** non è sicuro per gli script eseguiti sul lato client.  
  
 L'ID di classe per l'oggetto **RDSServer. DataFactory** è 9381D8F5-0288-11D0-9501-00AA00B911A5.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto DataFactory (RDSServer)](./datafactory-object-rdsserver-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio dell'oggetto DataFactory e dei metodi Query e CreateObject (VBScript)](./datafactory-object-query-method-and-createobject-method-example-vbscript.md)