---
description: Trasformazioni XSLT
title: Trasformazioni XSLT | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XSLT transformations in ADO
ms.assetid: 1a46196e-839f-4734-a59e-2c64609ffb9e
author: rothja
ms.author: jroth
ms.openlocfilehash: 75a29504d0d19300f8137959be150670f71e8b5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032204"
---
# <a name="xslt-transformations"></a>Trasformazioni XSLT
XSLT può essere applicato al codice XML generato per trasformarlo in un altro formato. La comprensione del formato XML in ADO facilita lo sviluppo di modelli XSLT in grado di trasformarli in un formato più semplice e intuitivo.  
  
 Si sa, ad esempio, che ogni riga del recordset viene salvata come elemento z:Row all'interno dell'elemento RS: data. Analogamente, ogni campo del recordset viene salvato come una coppia attributo-valore per questo elemento.  
  
## <a name="remarks"></a>Commenti  
 È possibile applicare lo script XSLT seguente al codice XML illustrato nella sezione precedente per trasformarlo in una tabella HTML da visualizzare nel browser:  
  
```  
<?xml version="1.0" encoding="ISO-8859-1"?>  
<html xmlns:xsl="http://www.w3.org/TR/WD-xsl">  
<body STYLE="font-family:Arial, helvetica, sans-serif; font-size:12pt; background-color:white">  
<table border="1" style="table-layout:fixed" width="600">  
  <col width="200"></col>  
  <tr bgcolor="teal">  
    <th><font color="white">CustomerId</font></th>  
    <th><font color="white">CompanyName</font></th>  
    <th><font color="white">ContactName</font></th>  
  </tr>  
<xsl:for-each select="xml/rs:data/z:row">  
  <tr bgcolor="navy">  
    <td><font color="white"><xsl:value-of select="@CustomerID"/></font></td>  
    <td><font color="white"><xsl:value-of select="@CompanyName"/></font></td>  
    <td><font color="white"><xsl:value-of select="@ContactName"/></font></td>   
  </tr>  
</xsl:for-each>  
</table>  
</body>  
</html>  
```  
  
 XSLT converte il flusso XML generato dal metodo ADO Save in una tabella HTML che visualizza ogni campo del recordset insieme a un'intestazione di tabella. Alle intestazioni di tabella e alle righe vengono assegnati anche tipi di carattere e colori diversi.  
  
## <a name="see-also"></a>Vedere anche  
 [Persistenza di record in formato XML](./persisting-records-in-xml-format.md)