---
description: Metodo Refresh (Servizi Desktop remoto)
title: Metodo Refresh (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
f1_keywords:
- Refresh
- RDS.DataControl::Refresh
- DataControl::Refresh
helpviewer_keywords:
- Refresh method [RDS]
ms.assetid: c90a8050-0ff4-4c83-9925-261f2f2ccfe9
author: rothja
ms.author: jroth
ms.openlocfilehash: f35574b0e9623560ddfafe123340e302a9fc5220
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168794"
---
# <a name="refresh-method-rds"></a>Metodo Refresh (Servizi Desktop remoto)
Esegue una query sull'origine dati specificata nella proprietà [Connect](./connect-property-rds.md) e aggiorna i risultati della query.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DataControl.Refresh  
```  
  
#### <a name="parameters"></a>Parametri  
 *DataControl*  
 Variabile oggetto che rappresenta un Servizi Desktop remoto [. Oggetto DataControl](./datacontrol-object-rds.md) .  
  
## <a name="remarks"></a>Commenti  
 È necessario impostare le proprietà [Connect](./connect-property-rds.md), [Server](./server-property-rds.md)e [SQL](./sql-property.md) prima di utilizzare il metodo **Refresh** . Tutti i controlli associati a dati nel form associato a un Servizi Desktop remoto **. L'oggetto DataControl** rifletterà il nuovo set di record. Qualsiasi oggetto [Recordset](../ado-api/recordset-object-ado.md) preesistente viene rilasciato e tutte le modifiche non salvate vengono ignorate. Il metodo **Refresh** rende automaticamente il primo record il record corrente.  
  
 È consigliabile chiamare periodicamente il metodo **Refresh** quando si utilizzano i dati. Se si recuperano dati e quindi lo si lascia in un computer client per un periodo di tempo, è probabile che diventi obsoleto. È possibile che le modifiche apportate non vengano eseguite correttamente, perché qualcun altro potrebbe aver modificato il record e inviato le modifiche prima dell'utente.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo Refresh (VB)](../ado-api/refresh-method-example-vb.md)   
 [Esempio di metodo Refresh (VBScript)](./refresh-method-example-vbscript.md)   
 [Pulsanti di comando Rubrica](../../guide/remote-data-service/address-book-command-buttons.md)   
 [Metodo CancelUpdate (RDS)](./cancelupdate-method-rds.md)   
 [Metodo Refresh (ADO)](../ado-api/refresh-method-ado.md)   
 [Metodo SubmitChanges (Servizi Desktop remoto)](./submitchanges-method-rds.md)