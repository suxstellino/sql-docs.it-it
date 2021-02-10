---
description: Proprietà DataSource (ADO)
title: Proprietà DataSource (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::DataSource
helpviewer_keywords:
- DataSource property [ADO]
ms.assetid: 300a702a-3544-48c5-b759-83b511fe97e0
author: rothja
ms.author: jroth
ms.openlocfilehash: 73058aa92e594528ef214a9ffa93fa0f58d29b76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025668"
---
# <a name="datasource-property-ado"></a>Proprietà DataSource (ADO)
Indica un oggetto che contiene i dati che devono essere rappresentati come oggetto [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) .  
  
## <a name="remarks"></a>Commenti  
 Questa proprietà viene utilizzata per creare controlli con associazione a dati con l'ambiente di dati. L'ambiente dati mantiene raccolte di dati (origini dati) che contengono oggetti denominati (membri dati) che verranno rappresentati come un oggetto **Recordset** .  
  
 Le proprietà [DataMember](../../../ado/reference/ado-api/datamember-property.md) e **DataSource** devono essere utilizzate insieme.  
  
 L'oggetto a cui si fa riferimento deve implementare l'interfaccia **IDataSource** e deve contenere un'interfaccia **IRowset** .  
  
## <a name="usage"></a>Utilizzo  
  
```  
Dim rs as New ADODB.Recordset  
rs.DataMember = "Command"     'Name of the rowset to bind to.  
Set rs.DataSource = myDE      'Name of the object containing an IRowset.  
```  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà DataMember](../../../ado/reference/ado-api/datamember-property.md)
