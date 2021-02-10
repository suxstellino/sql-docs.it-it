---
description: Deadlock con livello di isolamento di lettura ripetibile
title: Deadlock con livello di isolamento Repeatable Read | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- deadlocks in RDS [ADO]
- read repeatable in RDS [ADO]
ms.assetid: 29f3683f-12f3-4304-8a54-fe133c25a423
author: rothja
ms.author: jroth
ms.openlocfilehash: 05b43a72ede823a3a755edfdaa48be1e767b3e74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036501"
---
# <a name="deadlocks-with-read-repeatable-isolation-level"></a>Deadlock con livello di isolamento di lettura ripetibile
Se un oggetto business personalizzato utilizza un livello di isolamento di Read ripetible per accedere a una SQL Server e l'oggetto business viene chiamato contemporaneamente da due client che inviano una query e aggiornano nella stessa transazione, è possibile che si verifichi un deadlock. Remote Data Service è progettato per consentire il timeout di uno dei processi per rilasciare il deadlock, ma l'aggiornamento avrà esito negativo per il client.  
  
 Utilizzare la proprietà dinamica **timeout comando** del [servizio cursore](../appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) per modificare la lunghezza del timeout.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Vedere anche  
 [Nozioni fondamentali su RDS](./rds-fundamentals.md)