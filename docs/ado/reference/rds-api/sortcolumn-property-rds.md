---
description: Proprietà SortColumn (Servizi Desktop remoto)
title: Proprietà SortColumn (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- SortColumn property [RDS]
ms.assetid: f6f80f67-f0fb-4e63-a5f5-8fdf312aac63
author: rothja
ms.author: jroth
ms.openlocfilehash: edbfa8f6c4dbb1296e9bf86aed43148bb7b12ec2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168775"
---
# <a name="sortcolumn-property-rds"></a>Proprietà SortColumn (Servizi Desktop remoto)
Indica in quale colonna ordinare i record.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DataControl.SortColumn = String  
```  
  
#### <a name="parameters"></a>Parametri  
 *DataControl*  
 Variabile oggetto che rappresenta un Servizi Desktop remoto [. Oggetto DataControl](./datacontrol-object-rds.md) .  
  
 *Stringa*  
 Valore **stringa** che rappresenta il nome o l'alias della colonna in base alla quale ordinare i record.  
  
## <a name="remarks"></a>Commenti  
 Le proprietà **SortColumn**, [SortDirection](./sortdirection-property-rds.md), [FilterValue](./filtervalue-property-rds.md), [FilterCriterion](./filtercriterion-property-rds.md)e [FilterColumn offrono](./filtercolumn-property-rds.md) forniscono funzionalità di ordinamento e filtro nella cache sul lato client. La funzionalità di ordinamento ordina i record in base ai valori di una colonna. La funzionalità di filtro Visualizza un subset di record in base ai criteri di ricerca, mentre il [Recordset](../ado-api/recordset-object-ado.md) completo viene mantenuto nella cache. Il metodo [Reset](./reset-method-rds.md) eseguirà i criteri e sostituirà il **Recordset** corrente con un **Recordset** aggiornabile.  
  
 Per eseguire l'ordinamento in un **Recordset**, è innanzitutto necessario salvare le modifiche in sospeso. Se si utilizza **RDS. DataControl**, è possibile usare il metodo [SubmitChanges](./submitchanges-method-rds.md) . Ad esempio, se il Servizi Desktop remoto **. DataControl** è denominato ADC1, il codice è `ADC1.SubmitChanges` . Se si utilizza un **Recordset** ADO, è possibile utilizzare il relativo metodo [UpdateBatch](../ado-api/updatebatch-method.md) . L'uso di **UpdateBatch** è il metodo consigliato per gli oggetti **Recordset** creati con il metodo [CreateRecordset](./createrecordset-method-rds.md) . Ad esempio, il codice potrebbe essere `myRS.UpdateBatch` o `ADC1.Recordset.UpdateBatch` .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà FilterColumn offrono, FilterCriterion, FilterValue, SortColumn e SortDirection e metodo Reset (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [Sort (proprietà)](../ado-api/sort-property.md)   
 [Proprietà SortDirection (Servizi Desktop remoto)](./sortdirection-property-rds.md)