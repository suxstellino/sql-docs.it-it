---
description: Proprietà dinamiche dei recordset in XML
title: Proprietà dinamiche del recordset in XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset dynamic properties in XML [ADO]
ms.assetid: 52f8e379-812a-4db8-9210-94458926301c
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e966c3570ce8f9589ede5f60fca1fc0fa70bbac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037091"
---
# <a name="recordset-dynamic-properties-in-xml"></a>Proprietà dinamiche dei recordset in XML
Le seguenti proprietà specifiche del provider recordset (dal motore di cursore client) sono attualmente rese permanente nel formato XML:  
  
-   Aggiornamento della risincronizzazione  
  
-   tabella univoca  
  
-   Schema univoco  
  
-   Catalogo univoco  
  
-   Comando di risincronizzazione  
  
-   IRowsetChange  
  
-   IRowsetUpdate  
  
-   CommandTimeout  
  
-   BatchSize  
  
-   UpdateCriteria  
  
-   Nome riforma  
  
-   AutoRecalc  
  
 Queste proprietà vengono salvate nella sezione dello schema come attributi della definizione dell'elemento per il recordset da salvare in modo permanente. Questi attributi sono definiti nello spazio dei nomi dello schema del set di righe e devono avere il prefisso "RS:".  
  
## <a name="see-also"></a>Vedere anche  
 [Persistenza di record in formato XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
