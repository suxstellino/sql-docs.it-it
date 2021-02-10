---
description: Metodo CancelUpdate (Servizi Desktop remoto)
title: Metodo CancelUpdate (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- CancelUpdate method [RDS]
ms.assetid: 76d8a6e9-bc6c-4ea0-8e7a-2bae5ed06650
author: rothja
ms.author: jroth
ms.openlocfilehash: a321c84beb8277f05b0779a540a72b6a82b87933
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049541"
---
# <a name="cancelupdate-method-rds"></a>Metodo CancelUpdate (Servizi Desktop remoto)
Annulla tutte le modifiche apportate alla riga corrente o nuova di un oggetto [Recordset](../ado-api/recordset-object-ado.md) .  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DataControl.CancelUpdate  
```  
  
#### <a name="parameters"></a>Parametri  
 *DataControl*  
 Variabile oggetto che rappresenta un Servizi Desktop remoto [. Oggetto DataControl](./datacontrol-object-rds.md) .  
  
## <a name="remarks"></a>Commenti  
 Il servizio cursore per OLE DB mantiene una copia dei valori originali e una cache di modifiche. Quando si chiama **CancelUpdate**, la cache delle modifiche viene reimpostata su Empty e tutti i controlli associati vengono aggiornati con i dati originali.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo CancelUpdate (VBScript)](./cancelupdate-method-example-vbscript.md)   
 [Pulsanti di comando Rubrica](../../guide/remote-data-service/address-book-command-buttons.md)   
 [Metodo Cancel (ADO)](../ado-api/cancel-method-ado.md)   
 [Metodo Cancel (RDS)](./cancel-method-rds.md)   
 [Metodo CancelBatch (ADO)](../ado-api/cancelbatch-method-ado.md)   
 [Metodo CancelUpdate (ADO)](../ado-api/cancelupdate-method-ado.md)   
 [Metodo Refresh (RDS)](./refresh-method-rds.md)   
 [Metodo SubmitChanges (Servizi Desktop remoto)](./submitchanges-method-rds.md)