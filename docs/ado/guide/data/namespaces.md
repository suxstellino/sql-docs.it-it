---
description: Spazi dei nomi
title: Spazi dei nomi | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- namespaces in ADO
ms.assetid: efff5569-db52-451d-a039-2e74870534da
author: rothja
ms.author: jroth
ms.openlocfilehash: 6a7657e9758ebaa3507a35aef65d6a077e4dcf35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032673"
---
# <a name="namespaces"></a>Spazi dei nomi
Il formato di persistenza XML in ADO utilizza i quattro spazi dei nomi seguenti.  
  
## <a name="remarks"></a>Commenti  
 Il formato di persistenza XML in ADO utilizza i quattro spazi dei nomi seguenti.  
  
|Prefisso|Descrizione|  
|------------|-----------------|  
|s|Si riferisce allo spazio dei nomi "XML-Data" contenente gli elementi e gli attributi che definiscono lo schema del recordset corrente.|  
|dt|Fa riferimento alla specifica delle definizioni dei tipi di dati.|  
|rs|Si riferisce allo spazio dei nomi contenente gli elementi e gli attributi specifici delle proprietà e degli attributi del recordset ADO.|  
|z|Si riferisce allo schema del set di righe corrente.|  
  
 Un client non deve aggiungere i propri tag a questi spazi dei nomi, come definito dalla specifica. Un client, ad esempio, non deve definire uno spazio dei nomi come "urn: schemas-microsoft-com: rowset", quindi scrivere qualcosa come "RS: MyOwnTag". Per ulteriori informazioni sugli spazi dei nomi, vedere la [raccomandazione W3C Namespaces in XML](http://www.w3.org/TR/REC-xml-names/).  
  
> [!IMPORTANT]
>  L'ID del tag dello schema deve essere "RowsetSchema" e lo spazio dei nomi utilizzato per fare riferimento allo schema del set di righe corrente deve puntare a "#RowsetSchema".  
  
 Si noti che il prefisso dello spazio dei nomi, la parte tra i due punti e il segno di uguale, è arbitrario.  
  
```  
xmlns:rs="urn:schemas-microsoft-com:rowset"  
```  
  
 L'utente può definire questa operazione come qualsiasi nome, purché questo nome venga usato in modo coerente in tutto il documento XML. ADO scrive sempre "s," "RS", "DT" e "z", ma questi nomi di prefisso non sono hardcoded nel componente di caricamento.  
  
## <a name="see-also"></a>Vedere anche  
 [Persistenza di record in formato XML](./persisting-records-in-xml-format.md)