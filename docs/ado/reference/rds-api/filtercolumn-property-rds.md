---
description: Proprietà FilterColumn (Servizi Desktop remoto)
title: Proprietà FilterColumn offrono (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterColumn property [ADO]
ms.assetid: 0a5473e8-8ce6-4518-83fb-4920b827e285
author: rothja
ms.author: jroth
ms.openlocfilehash: 8235600daed0998a9ba5cb3876b18f7f7a954f1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049401"
---
# <a name="filtercolumn-property-rds"></a>Proprietà FilterColumn (Servizi Desktop remoto)
Indica la colonna in cui valutare i criteri di filtro.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DataControl.FilterColumn = String  
```  
  
#### <a name="parameters"></a>Parametri  
 *DataControl*  
 Variabile oggetto che rappresenta un Servizi Desktop remoto [. Oggetto DataControl](./datacontrol-object-rds.md) .  
  
 *Stringa*  
 Valore **stringa** che specifica la colonna in cui valutare i criteri di filtro. I criteri di filtro vengono specificati nella proprietà [FilterCriterion](./filtercriterion-property-rds.md) .  
  
## <a name="remarks"></a>Commenti  
 Le proprietà [SortColumn](./sortcolumn-property-rds.md), [SortDirection](./sortdirection-property-rds.md), [FilterValue](./filtervalue-property-rds.md), [FilterCriterion](./filtercriterion-property-rds.md)e **FilterColumn offrono** forniscono funzionalità di ordinamento e filtro nella cache sul lato client. La funzionalità di ordinamento ordina i record in base ai valori di una colonna. La funzionalità di filtro Visualizza un subset di record in base ai criteri di ricerca, mentre il [Recordset](../ado-api/recordset-object-ado.md) completo viene mantenuto nella cache. Il metodo [Reset](./reset-method-rds.md) eseguirà i criteri e sostituirà il **Recordset** corrente con un **Recordset** aggiornabile.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà FilterColumn offrono, FilterCriterion, FilterValue, SortColumn e SortDirection e metodo Reset (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [Proprietà FilterCriterion (RDS)](./filtercriterion-property-rds.md)   
 [Proprietà FilterValue (RDS)](./filtervalue-property-rds.md)   
 [Proprietà SortColumn (RDS)](./sortcolumn-property-rds.md)   
 [Proprietà SortDirection (Servizi Desktop remoto)](./sortdirection-property-rds.md)