---
description: DELETE TAG (comando)
title: Comando DELETE TAG | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE TAG command [ODBC]
ms.assetid: 4f4e1362-a5f3-4b15-8a3c-d4e96605f221
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: aa431c50fcd151d57ce7613586587a1c2342a5c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181564"
---
# <a name="delete-tag-command"></a>DELETE TAG (comando)
Rimuove un tag o tag da un file di indice composto (con estensione CDX).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DELETE TAG TagName1 [OF CDXFileName1]  
   [, TagName2 [OF CDXFileName2]] ...  
  Or   
DELETE TAG ALL [OF CDXFileName]  
```  
  
## <a name="arguments"></a>Argomenti  
 *TagName1* DI *CDXFileName1*[, *TagName2*[of *CDXFileName2*]]...  
 Specifica un tag da rimuovere da un file di indice composto. È possibile eliminare più tag con un TAG DELETE includendo un elenco di nomi di tag separati da virgole. Se nei file di indice aperti sono presenti due o più tag con lo stesso nome, è possibile rimuovere un tag da un file di indice specifico includendo *CDXFileName*.  
  
 ALL [di *CDXFileName*]  
 Rimuove ogni tag da un file di indice composto. Se la tabella corrente include un file di indice composto strutturale, tutti i tag vengono rimossi dal file di indice, il file di indice viene eliminato dal disco e il flag nell'intestazione della tabella che indica la presenza di un file di indice composto strutturale associato viene rimosso. Utilizzare ALL con di *CDXFileName* per rimuovere tutti i tag da un file di indice composto aperto diverso dal file di indice composto strutturale.  
  
## <a name="remarks"></a>Commenti  
 I file di indice composti creati con INDEX contengono tag corrispondenti alle voci di indice. Il TAG DELETE viene usato per rimuovere tag o tag da file di indice composti aperti. È possibile eliminare solo i tag dai file di indice composti aperti nell'area di lavoro corrente. Se si rimuovono tutti i tag da un file di indice composto, il file viene eliminato dal disco.  
  
 Visual FoxPro cerca innanzitutto un tag nel file di indice composto strutturale (se ne è aperto uno). Se il tag non è presente nel file di indice composto strutturale, Visual FoxPro Cerca il tag negli altri file di indice composti aperti.  
  
## <a name="see-also"></a>Vedere anche  
 [INDEX (comando)](../../odbc/microsoft/index-command.md)
