---
description: Provider necessari per il data shaping
title: Provider richiesti per la data shaping | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], data shaping
- data shaping [ADO], providers required
ms.assetid: d49d48d2-ac2d-4c11-895c-5a149b444620
author: rothja
ms.author: jroth
ms.openlocfilehash: eb3d91f23666c900f59a6ffc8e6b94af9bcdb4d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032533"
---
# <a name="required-providers-for-data-shaping"></a>Provider necessari per il data shaping
Il data shaping richiede in genere due provider. Il provider di servizi, [Data Shaping Service per OLE DB](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md), fornisce la funzionalità di data shaping e un provider di dati, ad esempio il provider di OLE DB per SQL Server, fornisce righe di dati per popolare il [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)a forma di.  
  
 Il nome del provider di servizi (MSDataShape) può essere specificato come valore della proprietà del [provider](../../../ado/reference/ado-api/provider-property-ado.md) di oggetti [connessione](../../../ado/reference/ado-api/connection-object-ado.md) o della parola chiave della stringa di connessione "provider = MSDataShape;".  
  
 Il nome del provider di dati può essere specificato come valore della proprietà dinamica **provider di dati** , che viene aggiunta alla raccolta delle [proprietà](../../../ado/reference/ado-api/properties-collection-ado.md) dell'oggetto **connessione** dal servizio Data Shaping per OLE DB o dalla parola chiave "**provider di dati =** _provider_" della stringa di connessione.  
  
 Non è necessario alcun provider di dati se il **Recordset** non è popolato, ad esempio in un **Recordset** fabbricato in cui le colonne vengono create con la parola chiave New. In tal caso, specificare "**provider di dati =** None;".  
  
## <a name="example"></a>Esempio  
  
```  
Dim cnn As New ADODB.Connection  
cnn.Provider = "MSDataShape"  
cnn.Open "Data Provider=SQLOLEDB;Integrated Security=SSPI;Database=Northwind"  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di data shaping](../../../ado/guide/data/data-shaping-example.md)   
 [Grammatica forma formale](../../../ado/guide/data/formal-shape-grammar.md)   
 [Comandi Shape in generale](../../../ado/guide/data/shape-commands-in-general.md)
