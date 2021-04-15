---
title: Generare uno schema XDR inline | Microsoft Docs
description: Visualizzare informazioni sulla generazione di uno schema XDR inline e sulla deprecazione della direttiva XMLDATA nella clausola FOR XML.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- XDR schemas [SQL Server]
- inline XDR schema generation [SQL Server]
- XMLDATA option
- FOR XML clause, inline XDR schema generation
ms.assetid: 2a40d617-9724-4f7d-80a4-a85c702f14d0
author: rothja
ms.author: jroth
ms.openlocfilehash: c858e68f46f7408c1fcbdf14bacd086760e31401
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490856"
---
# <a name="generate-an-inline-xdr-schema"></a>Generazione di uno schema XDR inline
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  La direttiva **XMLDATA** in FOR XML restituisce uno schema XDR inline insieme al risultato della query. Lo schema XDR tuttavia non supporta tutti i nuovi tipi di dati e i miglioramenti introdotti in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive. In alternativa, è possibile richiedere uno schema XSD inline usando [la direttiva XMLSCHEMA](../../relational-databases/xml/generate-an-inline-xsd-schema.md).  
  
> [!IMPORTANT]  
>  La direttiva XMLDATA all'opzione FOR XML è deprecata. Utilizzare la generazione XSD in caso di modalità RAW e AUTO. Non sono disponibili sostituzioni per direttiva XMLDATA in modalità EXPLICIT. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Per il supporto dello schema XDR inline, notare inoltre quanto segue:  
  
-   Se il risultato della query FOR XML include colonne di tipo **xml** e viene richiesto uno schema XDR inline, sarà generato un errore. Gli schemi XDR inline non supportano infatti questi tipi.  
  
-   Verrà eseguito il mapping dei tipi **(n)varchar(max)** e **(n)varbinary(max)** rispettivamente a **(n)varchar(n)** e **varbinary(n)** .  
  
-   Se la modalità di compatibilità è impostata su 90 o superiore, i valori **timestamp** vengono considerati come dati **varbinary(8)** , trattati come dati binari e restituiti nel risultato nel modo seguente:  
  
    -   Se si specifica **binary base64** , viene usata la codifica Base64.  
  
    -   Se non si specifica **binary base64** , viene usata la codifica URL in modalità AUTO.  
  
  
