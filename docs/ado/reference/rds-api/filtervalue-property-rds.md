---
description: Proprietà FilterValue (Servizi Desktop remoto)
title: Proprietà FilterValue (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterValue property [ADO]
ms.assetid: 28f17186-b842-4cf9-b320-a9bb941c481b
author: rothja
ms.author: jroth
ms.openlocfilehash: af9276f641a3a9c419cc0dc49595bcddc049336e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163766"
---
# <a name="filtervalue-property-rds"></a>Proprietà FilterValue (Servizi Desktop remoto)
Indica il valore con cui filtrare i record.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DataControl.FilterValue = String  
```  
  
#### <a name="parameters"></a>Parametri  
 *DataControl*  
 Variabile oggetto che rappresenta un Servizi Desktop remoto [. Oggetto DataControl](./datacontrol-object-rds.md) .  
  
 *Stringa*  
 Valore **stringa** che rappresenta un valore di dati con il quale filtrare i record, ad esempio `'Programmer'` o `125` .  
  
## <a name="remarks"></a>Commenti  
 Le proprietà [SortColumn](./sortcolumn-property-rds.md), [SortDirection](./sortdirection-property-rds.md), **FilterValue**, [FilterCriterion](./filtercriterion-property-rds.md)e [FilterColumn offrono](./filtercolumn-property-rds.md) forniscono funzionalità di ordinamento e filtro nella cache sul lato client. La funzionalità di ordinamento ordina i record in base ai valori di una colonna. La funzionalità di filtro Visualizza un subset di record in base ai criteri di ricerca, mentre il [Recordset](../ado-api/recordset-object-ado.md) completo viene mantenuto nella cache. Il metodo [Reset](./reset-method-rds.md) eseguirà i criteri e sostituirà il **Recordset** corrente con un **Recordset** aggiornabile.  
  
 I valori null generano un errore di mancata corrispondenza del tipo.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà FilterColumn offrono, FilterCriterion, FilterValue, SortColumn e SortDirection e metodo Reset (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [Proprietà FilterColumn offrono (RDS)](./filtercolumn-property-rds.md)   
 [Proprietà FilterCriterion (RDS)](./filtercriterion-property-rds.md)   
 [Proprietà SortColumn (RDS)](./sortcolumn-property-rds.md)   
 [Proprietà SortDirection (Servizi Desktop remoto)](./sortdirection-property-rds.md)