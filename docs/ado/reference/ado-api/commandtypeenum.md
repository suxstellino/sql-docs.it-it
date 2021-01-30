---
description: CommandTypeEnum
title: CommandTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CommandTypeEnum
helpviewer_keywords:
- CommandTypeEnum enumeration [ADO]
ms.assetid: 4b1feb9c-a855-40fe-a906-efe688687e9f
author: rothja
ms.author: jroth
ms.openlocfilehash: 9cea565eed8ae7fa9800284c19b03cdbc704b5e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164655"
---
# <a name="commandtypeenum"></a>CommandTypeEnum
Specifica la modalità di interpretazione di un argomento di comando.  
  
 È importante convalidare i valori *commandString* forniti dall'utente per evitare che gli utenti dell'applicazione possano inserire comandi potenzialmente pericolosi per l'esecuzione di ADO.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adCmdUnspecified**|-1|Non specifica l'argomento tipo di comando.|  
|**adCmdText**|1|Restituisce [CommandText](./commandtext-property-ado.md) come definizione testuale di un comando o di una chiamata stored procedure.|  
|**adCmdTable**|2|Restituisce **CommandText** come nome di tabella le cui colonne vengono tutte restituite da una query SQL generata internamente.|  
|**adCmdStoredProc**|4|Restituisce **CommandText** come nome stored procedure.|  
|**adCmdUnknown**|8|Valore predefinito. Indica che il tipo di comando nella proprietà **CommandText** non è noto.<br /><br /> Quando il tipo di comando non è noto, ADO effettuerà diversi tentativi di interpretare **CommandText**.<br /><br /> -   **CommandText** viene interpretato come definizione testuale di un comando o di una chiamata stored procedure. Questo comportamento è identico a quello di **adCmdText**.<br />-   **CommandText** è il nome di un stored procedure. Questo comportamento è identico a quello di **adCmdStoredProc**.<br />-   **CommandText** viene interpretato come il nome di una tabella. Tutte le colonne vengono restituite da una query SQL generata internamente. Questo comportamento è identico a quello di **adCmdTable**.|  
|**adCmdFile**|256|Restituisce **CommandText** come nome file di un [Recordset](./recordset-object-ado.md)archiviato in modo permanente. Utilizzato con **Recordset.** [Apre](./open-method-ado-recordset.md) o [esegue](./requery-method.md) nuovamente la query.|  
|**adCmdTableDirect**|512|Restituisce **CommandText** come nome di tabella le cui colonne vengono restituite. Utilizzato con **Recordset. Open** o solo la **riesecuzione della query** . Per utilizzare il metodo [Seek](./seek-method.md) , il **Recordset** deve essere aperto con **adCmdTableDirect**.<br /><br /> Questo valore non può essere combinato con il valore di [ExecuteOptionEnum](./executeoptionenum.md) **adAsyncExecute**.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. CommandType. Unspecified|  
|AdoEnums. CommandType. TEXT|  
|AdoEnums. CommandType. TABLE|  
|AdoEnums. CommandType. STOREDPROC|  
|AdoEnums. CommandType. UNKNOWN|  
|AdoEnums. CommandType. FILE|  
|AdoEnums. CommandType. TABLEDIRECT|  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Proprietà CommandType (ADO)](./commandtype-property-ado.md)  
        [Metodo Execute (Command - ADO)](./execute-method-ado-command.md)  
    :::column-end:::
    :::column:::
        [Metodo Execute (Connection - ADO)](./execute-method-ado-connection.md)  
        [Metodo Open (Recordset - ADO)](./open-method-ado-recordset.md)  
    :::column-end:::
    :::column:::
        [Metodo Requery](./requery-method.md)  
    :::column-end:::
:::row-end:::