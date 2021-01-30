---
description: Proprietà FetchOptions (Servizi Desktop remoto)
title: Proprietà FetchOptions (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FetchOptions property [ADO]
ms.assetid: 7b2e254a-9354-4541-bc98-bb185276388f
author: rothja
ms.author: jroth
ms.openlocfilehash: 1809c23267057a34c0f2f594a41bdfe62304545c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163785"
---
# <a name="fetchoptions-property-rds"></a>Proprietà FetchOptions (Servizi Desktop remoto)
Indica il tipo di recupero asincrono.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="setting-and-return-values"></a>Impostazione e valori restituiti  
 Imposta o restituisce uno dei valori seguenti.  
  
|Costante|Descrizione|  
|--------------|-----------------|  
|**adcFetchUpFront**|Tutti i record del [Recordset](../ado-api/recordset-object-ado.md) vengono recuperati prima che il controllo venga restituito all'applicazione. Il **Recordset** completo viene recuperato prima che l'applicazione possa eseguire qualsiasi operazione.|  
|**adcFetchBackground**|Il controllo può tornare all'applicazione non appena il primo batch di record è stato recuperato. Una successiva lettura del **Recordset** che tenta di accedere a un record non recuperato nel primo batch verrà posticipata fino a quando il record cercato non viene effettivamente recuperato, a cui il controllo del tempo torna all'applicazione.|  
|**adcFetchAsync**|Valore predefinito. Il controllo viene restituito immediatamente all'applicazione durante il recupero dei record in background. Se l'applicazione tenta di leggere un record che non è ancora stato recuperato, il record più vicino al record cercato verrà letto e il controllo verrà restituito immediatamente, a indicare che è stata raggiunta la fine corrente del **Recordset** . Una chiamata a [MoveLast](./movefirst-movelast-movenext-and-moveprevious-methods-rds.md) , ad esempio, sposterà la posizione del record corrente nell'ultimo record effettivamente recuperato, anche se più record continueranno a popolare il **Recordset**.|  
  
> [!NOTE]
>  Ogni file eseguibile lato client che utilizza queste costanti deve fornire le relative dichiarazioni. È possibile tagliare e incollare le dichiarazioni di costanti desiderate dal file Adcvbs. Inc, che si trova nella cartella di installazione predefinita per la libreria RDS.  
  
## <a name="remarks"></a>Commenti  
 In un'applicazione Web, in genere si vuole usare **adcFetchAsync** (valore predefinito), perché fornisce prestazioni migliori. In un'applicazione client compilata, in genere si vuole usare **adcFetchBackground**.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ExecuteOptions e FetchOptions (VBScript)](./executeoptions-and-fetchoptions-properties-example-vbscript.md)   
 [Metodo Cancel (Servizi Desktop remoto)](./cancel-method-rds.md)