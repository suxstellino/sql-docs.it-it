---
description: Creazione di recordset gerarchici
title: Fabbricazione di recordset gerarchici | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset fabrication [ADO]
- hierarchical Recordsets [ADO]
- fabricating hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: a584e642-a4a3-418e-bc20-3aff81a5625a
author: rothja
ms.author: jroth
ms.openlocfilehash: 836c1e0091166485f68c04226c632e9999038469
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033114"
---
# <a name="fabricating-hierarchical-recordsets"></a>Creazione di recordset gerarchici
Nell'esempio seguente viene illustrato come costruire un recordset gerarchico senza un'origine dati sottostante utilizzando la grammatica di data shaping per definire le colonne per i **Recordset** padre, figlio e nipote.  
  
 Per costruire un **Recordset** gerarchico, è necessario specificare il [servizio di Data Shaping Microsoft per OLE DB (provider di servizi ADO)](../appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) (MSDataShape) ed è possibile specificare un valore provider di dati None nel parametro della stringa di connessione del metodo [Open](../../reference/ado-api/open-method-ado-connection.md) dell'oggetto [Connection](../../reference/ado-api/connection-object-ado.md) . Per ulteriori informazioni, vedere [provider richiesti per la definizione dei dati](./required-providers-for-data-shaping.md).  
  
```  
Dim cn As New ADODB.Connection  
Dim rsCustomers As New ADODB.Recordset  
  
cn.Open "Provider=MSDataShape;Data Provider=NONE;"  
  
strShape = _  
"SHAPE APPEND NEW adInteger AS CustID," & _  
            " NEW adChar(25) AS FirstName," & _  
            " NEW adChar(25) AS LastName," & _  
            " NEW adChar(12) AS SSN," & _  
            " NEW adChar(50) AS Address," & _  
         " ((SHAPE APPEND NEW adChar(80) AS VIN_NO," & _  
                        " NEW adInteger AS CustID," & _  
                        " NEW adChar(20) AS BodyColor, " & _  
                     " ((SHAPE APPEND NEW adChar(80) AS VIN_NO," & _  
                                    " NEW adChar(20) AS Make, " & _  
                                    " NEW adChar(20) AS Model," & _  
                                    " NEW adChar(4) AS Year) " & _  
                        " AS VINS RELATE VIN_NO TO VIN_NO))" & _  
            " AS Vehicles RELATE CustID TO CustID) "  
  
rsCustomers.Open strShape, cn, adOpenStatic, adLockOptimistic, -1  
```  
  
 Non appena il **Recordset** è stato fabbricato, può essere popolato, modificato o reso permanente in un file.  
  
## <a name="see-also"></a>Vedere anche  
 [Accesso alle righe in un recordset gerarchico](./accessing-rows-in-a-hierarchical-recordset.md)   
 [Grammatica forma formale](./formal-shape-grammar.md)   
 [Provider richiesti per la definizione dei dati](./required-providers-for-data-shaping.md)   
 [Clausola APPEND della forma](./shape-append-clause.md)   
 [Comandi Shape in generale](./shape-commands-in-general.md)