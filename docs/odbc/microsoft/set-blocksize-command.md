---
description: SET BLOCKSIZE (comando)
title: Comando SET BLOCKSIZE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set blocksize command [ODBC]
ms.assetid: 0c11580f-37f5-4a8e-99be-9fb9c44bb433
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f1777b62d279db78154383a3f328591776c0a5c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208658"
---
# <a name="set-blocksize-command"></a>SET BLOCKSIZE (comando)
Specifica la modalità di allocazione dello spazio su disco per l'archiviazione dei campi di memo.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
SET BLOCKSIZE TO nBytes  
```  
  
## <a name="arguments"></a>Argomenti  
 *nBytes*  
 Specifica la dimensione del blocco in cui è allocato lo spazio su disco per i campi Memo. Se *nBytes* è 0, lo spazio su disco viene allocato in byte singoli (blocchi di 1 byte). Se *nBytes* è un numero intero compreso tra 1 e 32, lo spazio su disco viene allocato in blocchi di byte *nBytes* moltiplicato per 512. Se *nBytes* è maggiore di 32, lo spazio su disco viene allocato in blocchi di *nBytes* byte. Se si specifica un valore di dimensione blocco maggiore di 32, è possibile risparmiare spazio su disco sostanziale.  
  
## <a name="remarks"></a>Commenti  
 Il valore predefinito per SET BLOCKSIZE è 64. Per reimpostare la dimensione del blocco su un valore diverso dopo che il file è stato creato, impostarlo su un nuovo valore e quindi usare COPY per creare una nuova tabella. La nuova tabella presenta le dimensioni del blocco specificate.
