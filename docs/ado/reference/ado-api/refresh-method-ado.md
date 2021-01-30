---
description: Metodo Refresh (ADO)
title: Metodo Refresh (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Collection::Refresh
- Parameters::Refresh
- Properties::Refresh
helpviewer_keywords:
- Refresh method [ADO]
ms.assetid: 089b7ca7-684f-4259-8032-5bd1ecc54426
author: rothja
ms.author: jroth
ms.openlocfilehash: f72f41f58ed43665acd3838a5f8e2228c736fd78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170401"
---
# <a name="refresh-method-ado"></a>Metodo Refresh (ADO)
Aggiorna gli oggetti di una raccolta in modo da riflettere gli oggetti disponibili e specifici del provider.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
collection.Refresh  
```  
  
## <a name="remarks"></a>Osservazioni  
 Il metodo **Refresh** esegue diverse attività a seconda della raccolta da cui viene chiamata.  
  
### <a name="parameters"></a>Parametri  
 L'utilizzo del metodo **Refresh** sulla raccolta [Parameters](./parameters-collection-ado.md) di un oggetto [Command](./command-object-ado.md) consente di recuperare informazioni sui parametri lato provider per la query stored procedure o con parametri specificata nell'oggetto **Command** . La raccolta sarà vuota per i provider che non supportano chiamate di stored procedure o query con parametri.  
  
 È necessario impostare la proprietà [ActiveConnection](./activeconnection-property-ado.md) dell'oggetto **Command** su un oggetto [Connection](./connection-object-ado.md) valido, la proprietà [CommandText](./commandtext-property-ado.md) su un comando valido e la proprietà [CommandType](./commandtype-property-ado.md) su **adCmdStoredProc** prima di chiamare il metodo **Refresh** .  
  
 Se si accede alla raccolta **Parameters** prima di chiamare il metodo **Refresh** , ADO chiamerà automaticamente il metodo e popola la raccolta automaticamente.  
  
> [!NOTE]
>  Se si utilizza il metodo **Refresh** per ottenere informazioni sui parametri dal provider e vengono restituiti uno o più oggetti [parametro](./parameter-object.md) di tipo di dati a lunghezza variabile, ADO può allocare memoria per i parametri in base alle dimensioni massime potenziali, che causeranno un errore durante l'esecuzione. È necessario impostare in modo esplicito la proprietà [size](./size-property-ado-parameter.md) per questi parametri prima di chiamare il metodo [Execute](./execute-method-ado-command.md) per evitare errori.  
  
### <a name="fields"></a>Campi  
 L'utilizzo del metodo **Refresh** sulla raccolta [Fields](./fields-collection-ado.md) non ha alcun effetto visibile. Per recuperare le modifiche dalla struttura del database sottostante, è necessario utilizzare il metodo [Requery](./requery-method.md) oppure, se l'oggetto [Recordset](./recordset-object-ado.md) non supporta i segnalibri, il metodo [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) .  
  
### <a name="properties"></a>Proprietà  
 Se si usa il metodo **Refresh** su una raccolta **Properties** di alcuni oggetti, la raccolta viene popolata con le proprietà dinamiche esposte dal provider. Queste proprietà forniscono informazioni sulle funzionalità specifiche del provider, oltre alle proprietà predefinite supportate da ADO.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Raccolta assi](../ado-md-api/axes-collection-ado-md.md)  
        [Raccolta Columns](../adox-api/columns-collection-adox.md)  
        [Raccolta CubeDefs](../ado-md-api/cubedefs-collection-ado-md.md)  
        [Raccolta Dimensions](../ado-md-api/dimensions-collection-ado-md.md)  
        [Raccolta di errori](./errors-collection-ado.md)  
        [Raccolta Fields](./fields-collection-ado.md)  
        [Raccolta di gruppi](../adox-api/groups-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Raccolta Hierarchies](../ado-md-api/hierarchies-collection-ado-md.md)  
        [Raccolta Indexes](../adox-api/indexes-collection-adox.md)  
        [Raccolta di chiavi](../adox-api/keys-collection-adox.md)  
        [Raccolta levels](../ado-md-api/levels-collection-ado-md.md)  
        [Raccolta members](../ado-md-api/members-collection-ado-md.md)  
        [Raccolta Parameters](./parameters-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Raccolta Positions](../ado-md-api/positions-collection-ado-md.md)  
        [Raccolta Procedures](../adox-api/procedures-collection-adox.md)  
        [Raccolta delle proprietà](./properties-collection-ado.md)  
        [Raccolta Tables](../adox-api/tables-collection-adox.md)  
        [Raccolta utenti](../adox-api/users-collection-adox.md)  
        [Raccolta views](../adox-api/views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo Refresh (VB)](./refresh-method-example-vb.md)   
 [Esempio di metodo Refresh (VC + +)](./refresh-method-example-vc.md)   
 [Proprietà Count (ADO)](./count-property-ado.md)   
 [Metodo Refresh (Servizi Desktop remoto)](../rds-api/refresh-method-rds.md)