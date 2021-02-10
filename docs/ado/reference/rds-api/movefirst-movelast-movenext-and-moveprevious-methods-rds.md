---
description: Metodi MoveFirst, MoveLast, MoveNext e MovePrevious (Servizi Desktop remoto)
title: Metodi MoveFirst, MoveLast, MoveNext e MovePrevious (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- MoveLast method [RDS]
- MovePrevious method [RDS]
- MoveFirst method [RDS]
- MoveNext method [RDS]
ms.assetid: 45c80bb5-136f-4204-9df2-78740fa55574
author: rothja
ms.author: jroth
ms.openlocfilehash: 54395a8b0bdc88e873ad25d41ab92304e7f5efe6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049281"
---
# <a name="movefirst-movelast-movenext-and-moveprevious-methods-rds"></a>Metodi MoveFirst, MoveLast, MoveNext e MovePrevious (Servizi Desktop remoto)
Passa al record primo, ultimo, successivo o precedente in un oggetto [Recordset](../ado-api/recordset-object-ado.md) specificato.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DataControl.Recordset.{MoveFirst | MoveLast | MoveNext | MovePrevious}  
```  
  
#### <a name="parameters"></a>Parametri  
 *DataControl*  
 Variabile oggetto che rappresenta un Servizi Desktop remoto [. Oggetto DataControl](./datacontrol-object-rds.md) .  
  
## <a name="remarks"></a>Commenti  
 È possibile utilizzare i metodi **Move** con **RDS. Oggetto DataControl** per spostarsi tra i record di dati nei controlli associati a dati in una pagina Web. Si supponga, ad esempio, di visualizzare un **Recordset** in una griglia mediante l'associazione a un **RDS. Oggetto DataControl** . È quindi possibile includere i pulsanti primo, ultimo, successivo e precedente su cui gli utenti possono fare clic per passare al record primo, ultimo, successivo o precedente nel **Recordset** visualizzato. A tale scopo, chiamare i metodi **MoveFirst**, **MoveLast**, **MoveNext** e **MovePrevious** del Servizi Desktop remoto **. Oggetto DataControl** nelle procedure OnClick rispettivamente per i pulsanti First, Last, Next e Previous. Nell' [esempio](../../guide/remote-data-service/address-book-navigation-buttons.md) di Rubrica viene illustrato come eseguire questa operazione.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Move (ADO)](../ado-api/move-method-ado.md)   
 [Metodi MoveFirst, MoveLast, MoveNext e MovePrevious (ADO)](../ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)   
 [Metodo MoveRecord (ADO)](../ado-api/moverecord-method-ado.md)