---
description: InvokeService (Servizi Desktop remoto)
title: InvokeService (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- InvokeService [RDS]
ms.assetid: ad45c676-ec7e-4a3a-9a6b-a54f75eb3012
author: rothja
ms.author: jroth
ms.openlocfilehash: 4d162b0291a19a80ec0133307c785ad3af588aee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168890"
---
# <a name="invokeservice-rds"></a>InvokeService (Servizi Desktop remoto)
Restituisce un puntatore all'interfaccia richiesta su una versione più idonea dell'oggetto.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a  [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
object.InvokeService(REFID riid, IUknown* punkNotSoFunctionalInterface, IUknown** ppunkMoreFunctionalInterface) As HRESULT  
```  
  
#### <a name="parameters"></a>Parametri  
 *riid*  
  
 in Identificatore dell'interfaccia richiesta.  
  
 *punkNotSoFunctionalInterface*  
  
 in Oggetto di origine meno idoneo.  
  
 *ppunkMoreFunctionalInterface*  
  
 out Indirizzo della variabile puntatore che riceve il puntatore a interfaccia richiesto in *riid*. In caso di esito positivo, il parametro *ppunkMoreFunctionalInterface* contiene il puntatore a interfaccia richiesto per l'oggetto. Se l'oggetto non supporta l'interfaccia specificata in *riid*, *ppunkMoreFunctionalInterface* è impostato su null.  
  
## <a name="return-value"></a>Valore restituito  
 Valore HRESULT che indica se la chiamata al metodo **InvokeService** è stata eseguita correttamente.  
  
## <a name="remarks"></a>Commenti  
 L'implementazione del motore di cursori RDS di **InvokeService** accetta il set di righe di input (o più oggetti risultati), popola il motore di cursori dal set di righe di input e quindi restituisce un puntatore su se stesso.  
  
## <a name="applies-to"></a>Si applica a  
 [Interfaccia IRDSService (Servizi Desktop remoto)](./irdsservice-interface-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di Servizi Desktop remoto](./rds-methods.md)