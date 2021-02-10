---
description: Acquisisci codici di errore ADO
title: Codici di errore ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], error codes
ms.assetid: 3aee61c7-a9b7-4596-b78e-5828a00d0281
author: rothja
ms.author: jroth
ms.openlocfilehash: 2d3b0fd0c3a673cc841d7a5318872151bc20b311
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029505"
---
# <a name="capture-ado-error-codes"></a>Acquisisci codici di errore ADO
Oltre agli errori del provider restituiti negli oggetti [Error](../../reference/ado-api/error-object.md) della raccolta [Errors](../../reference/ado-api/errors-collection-ado.md) , ADO può restituire errori al meccanismo di gestione delle eccezioni dell'ambiente di Runtime. Usare il meccanismo di intercettazione degli errori del linguaggio di programmazione, ad esempio l'istruzione **On Error** in Microsoft® Visual Basic o il blocco **try-catch** in Microsoft Visual C++®, per acquisire gli errori ADO.

 Per l'elenco dei codici di errore ADO, vedere [ErrorValueEnum](../../reference/ado-api/errorvalueenum.md).

## <a name="see-also"></a>Vedere anche
 Raccolta errori [oggetto errore](../../reference/ado-api/error-object.md) [(ADO)](../../reference/ado-api/errors-collection-ado.md)