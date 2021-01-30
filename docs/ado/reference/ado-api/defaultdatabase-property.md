---
description: Proprietà DefaultDatabase
title: Proprietà DefaultDatabase | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::DefaultDatabase
helpviewer_keywords:
- DefaultDatabase property
ms.assetid: 41e8a8dd-e69c-4a09-8736-93502e01961c
author: rothja
ms.author: jroth
ms.openlocfilehash: 8cff37ae3bc0940a62e2acc9af3ac6c25d1379a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167583"
---
# <a name="defaultdatabase-property"></a>Proprietà DefaultDatabase
Indica il database predefinito per un oggetto [Connection](../../../ado/reference/ado-api/connection-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **stringa** che restituisce il nome di un database disponibile dal provider.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **DefaultDatabase** per impostare o restituire il nome del database predefinito in un oggetto **connessione** specifico.  
  
 Se è presente un database predefinito, le stringhe SQL possono utilizzare una sintassi non qualificata per accedere agli oggetti in tale database. Per accedere agli oggetti in un database diverso da quello specificato nella proprietà **DefaultDatabase** , è necessario qualificare i nomi degli oggetti con il nome del database desiderato. Al momento della connessione, il provider scriverà le informazioni predefinite sul database nella proprietà **DefaultDatabase** . Alcuni provider consentono un solo database per connessione, nel qual caso non è possibile modificare la proprietà **DefaultDatabase** .  
  
 Alcune origini dati e i provider potrebbero non supportare questa funzionalità e possono restituire un errore o una stringa vuota.  
  
> [!NOTE]
>  **Utilizzo servizio dati remoto** Questa proprietà non è disponibile in un oggetto **connessione** sul lato client.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Connection (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà provider e DefaultDatabase (VB)](../../../ado/reference/ado-api/provider-and-defaultdatabase-properties-example-vb.md)   
 [Esempio delle proprietà Provider e DefaultDatabase (VC++)](../../../ado/reference/ado-api/provider-and-defaultdatabase-properties-example-vc.md)   
