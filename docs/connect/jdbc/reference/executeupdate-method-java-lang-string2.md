---
description: Metodo executeUpdate (java.lang.String)
title: Metodo executeUpdate (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 02/07/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.executeUpdate (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 91ecb1cd-001d-4ac9-9ae8-5db05c3c2959
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8ce4dff5d7b17b1d88670189fa119d6a1c81915b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168432"
---
# <a name="executeupdate-method-javalangstring"></a>Metodo executeUpdate (java.lang.String)

Esegue l'istruzione SQL specificata, che può essere un'istruzione INSERT, UPDATE, MERGE o DELETE, oppure un'istruzione SQL che non restituisce nulla, quale un'istruzione SQL DDL.

## <a name="syntax"></a>Sintassi

```
public final int executeUpdate(java.lang.String sql)
```

#### <a name="parameters"></a>Parametri
*sql*

Valore **String** contenente l'istruzione SQL.

## <a name="return-value"></a>Valore restituito
Valore **int** che indica il numero di righe interessate oppure 0 se si usa un'istruzione DDL.

## <a name="exceptions"></a>Eccezioni
[SQLServerException](./sqlserverexception-class.md)

## <a name="remarks"></a>Osservazioni
Questo metodo executeUpdate viene specificato dal metodo executeUpdate nell'interfaccia java.sql.PreparedStatement.

La chiamata di questo metodo genera un'eccezione perché l'istruzione SQL per l'oggetto SQLServerPreparedStatement viene specificata quando viene creato l'oggetto.

## <a name="see-also"></a>Vedere anche

[Metodo executeUpdate &#40;SQLServerPreparedStatement&#41;](./executeupdate-method-sqlserverpreparedstatement.md)

[Membri di SQLServerPreparedStatement](./sqlserverpreparedstatement-members.md)

[Classe SQLServerPreparedStatement](./sqlserverpreparedstatement-class.md)
