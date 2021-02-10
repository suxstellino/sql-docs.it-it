---
description: Proprietà ReadyState (Servizi Desktop remoto)
title: Proprietà ReadyState (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ReadyState property [ADO]
ms.assetid: 5be75bc7-1171-4440-a37e-c8cc6b5cd865
author: rothja
ms.author: jroth
ms.openlocfilehash: a70a2f7fc26e83b21cdb85edb3ddf4a664785d30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049211"
---
# <a name="readystate-property-rds"></a>Proprietà ReadyState (Servizi Desktop remoto)
Indica lo stato di un oggetto [DataControl](./datacontrol-object-rds.md) quando recupera i dati nell'oggetto [Recordset](../ado-api/recordset-object-ado.md) .  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce uno dei valori seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**adcReadyStateLoaded**|La query corrente è ancora in esecuzione e non è stata recuperata alcuna riga. Il **Recordset** dell'oggetto **DataControl** non è disponibile per l'utilizzo.|  
|**adcReadyStateInteractive**|Un set iniziale di righe recuperato dalla query corrente è stato archiviato nel **Recordset** dell'oggetto **DataControl** e disponibile per l'utilizzo. È ancora in corso il recupero delle righe rimanenti.|  
|**adcReadyStateComplete**|Tutte le righe recuperate dalla query corrente sono state archiviate nel **Recordset** dell'oggetto **DataControl** e sono disponibili per l'utilizzo.<br /><br /> Questo stato sarà presente anche se un'operazione è stata interrotta a causa di un errore o se l'oggetto **Recordset** non è inizializzato.|  
  
> [!NOTE]
>  Ogni file eseguibile lato client che utilizza queste costanti deve fornire le relative dichiarazioni. È possibile tagliare e incollare le dichiarazioni di costanti desiderate dal file Adcvbs. Inc, che si trova nella cartella di installazione predefinita per la libreria RDS.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare l'evento [onReadyStateChange](./onreadystatechange-event-rds.md) per monitorare le modifiche apportate alla proprietà **ReadyState** durante un'operazione di query asincrona. Questa operazione è più efficiente rispetto al controllo periodico del valore della proprietà.  
  
 Se si verifica un errore durante un'operazione asincrona, la proprietà **ReadyState** viene modificata in **adcReadyStateComplete**, la proprietà [state](../ado-api/state-property-ado.md) passa da **AdStateExecuting** a **adStateClosed** e la proprietà del [valore](../ado-api/value-property-ado.md) dell'oggetto **Recordset** rimane *null*.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ReadyState (VBScript)](./readystate-property-example-vbscript.md)   
 [Metodo Cancel (RDS)](./cancel-method-rds.md)   
 [Proprietà ExecuteOptions (Servizi Desktop remoto)](./executeoptions-property-rds.md)