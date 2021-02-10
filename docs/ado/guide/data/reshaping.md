---
description: Modifica della forma
title: Rimodellazione | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- reshaping previously shaped Recordset [ADO]
- data shaping [ADO], reshaping
ms.assetid: b1c965b7-3dad-4de6-9e0e-502ca8785be3
author: rothja
ms.author: jroth
ms.openlocfilehash: c4604bcb4713f02ffb3f804f86b429d1db3a6aab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037081"
---
# <a name="reshaping"></a>Modifica della forma
A un **Recordset** creato da una clausola di un comando Shape può essere assegnato un nome di *alias* (in genere con la parola chiave As). È possibile fare riferimento all'alias di un **Recordset** con forma in un comando completamente diverso. In altre condizioni, è possibile riutilizzare o *modificare la forma* di un **Recordset** precedentemente modellato in un nuovo comando Shape. Per supportare questa funzionalità, ADO fornisce una proprietà e un nome di modifica della [forma](../../../ado/reference/ado-api/reshape-name-property-dynamic-ado.md).  
  
 Il rishaping ha due funzioni principali. Il primo consiste nell'associare un **Recordset** esistente a un nuovo **Recordset** padre.  
  
## <a name="example"></a>Esempio  
  
```  
rs1.Open "SHAPE {select * from Customers} " & _  
         "APPEND ({select * from Orders} AS chapOrders " & _  
         "RELATE CustomerID to CustomerID)", cn  
  
rs2.Open "SHAPE {select * from Employees} " & _  
         "APPEND (chapOrders RELATE EmployeeID to EmployeeID)", cn  
```  
  
 La seconda funzione consiste nell'abilitare l'accesso non basato su un capitolo agli oggetti **Recordset** figlio esistenti, utilizzando la sintassi "Shape \<recordset reshape name> ".  
  
> [!NOTE]
>  Non è possibile aggiungere colonne a un **Recordset** esistente, modificare la forma di un **Recordset** con parametri o degli oggetti **Recordset** in qualsiasi clausola COMPUTE corrispondente oppure eseguire operazioni di aggregazione su qualsiasi discendente del **Recordset** dal **Recordset** da rimodellare. Il **Recordset** da modificare e il nuovo comando Shape devono entrambi usare la stessa [connessione](../../../ado/reference/ado-api/connection-object-ado.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di data shaping](../../../ado/guide/data/data-shaping-example.md)
