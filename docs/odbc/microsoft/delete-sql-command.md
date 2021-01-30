---
description: DELETE (comando SQL)
title: Comando DELETE-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE [ODBC]
ms.assetid: 0d5bd477-626f-4f22-a05a-f531d9f8c5e7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42c5ff48b0ac330c2a305650141ada2255629c1f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200503"
---
# <a name="delete---sql-command"></a>DELETE (comando SQL)
Contrassegna i record per l'eliminazione.  
  
 Il driver ODBC Visual FoxPro supporta la sintassi nativa del linguaggio Visual FoxPro per questo comando. Per informazioni specifiche del driver, vedere la sezione Osservazioni.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DELETE FROM [DatabaseName!]TableName  
   [WHERE FilterCondition1 [AND | OR FilterCondition2 ...]]  
```  
  
## <a name="arguments"></a>Argomenti  
 DA [ *DatabaseName*]] *TableName*  
 Specifica la tabella in cui i record vengono contrassegnati per l'eliminazione.  
  
 *DatabaseName!* Specifica il nome di un database che contiene la tabella se il database contenitore non è il database specificato con l'origine dati. È necessario includere il nome di un database che contiene la tabella se il database non è il database specificato con l'origine dati. Includere il delimitatore punto esclamativo (!) dopo il nome del database e prima del nome della tabella.  
  
 DOVE *FilterCondition1*[e &#124; o *FilterCondition2*...]  
 Specifica che Visual FoxPro contrassegna solo determinati record per l'eliminazione.  
  
 *FilterCondition* specifica i criteri che i record devono soddisfare per essere contrassegnati per l'eliminazione. È possibile includere tutte le condizioni di filtro desiderate, connetterle con l'operatore AND o OR. È inoltre possibile utilizzare l'operatore NOT per invertire il valore di un'espressione logica oppure è possibile utilizzare **empty**() per verificare la presenza di un campo vuoto.  
  
## <a name="remarks"></a>Commenti  
 Se l'impostazione DELETED è impostata su ON, i record contrassegnati per l'eliminazione vengono ignorati da tutti i comandi che includono un ambito.  
  
 DELETE-SQL usa il blocco dei record quando si contrassegnano più record per l'eliminazione nelle tabelle aperte per l'accesso condiviso. In questo modo si riduce la contesa di record in situazioni multiutente, ma è possibile ridurre le prestazioni. Per ottenere le prestazioni massime, aprire la tabella per l'uso esclusivo.  
  
## <a name="driver-remarks"></a>Osservazioni del driver  
 Quando l'applicazione invia l'istruzione SQL ODBC DELETE all'origine dati, il driver ODBC Visual FoxPro converte il comando nel comando Visual FoxPro DELETE senza conversione.  
  
## <a name="see-also"></a>Vedere anche  
 [SET DELETED (comando)](../../odbc/microsoft/set-deleted-command.md)
