---
description: UPDATE (comando SQL)
title: Comando UPDATE-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- update [ODBC]
ms.assetid: ff1e0331-c060-4304-b280-039725b45f63
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a3e38ad98f69faf2ed6288c767d43167314465b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209824"
---
# <a name="update---sql-command"></a>UPDATE (comando SQL)
Aggiorna i record in una tabella con nuovi valori.  
  
 Il driver ODBC Visual FoxPro supporta la sintassi nativa del linguaggio Visual FoxPro per questo comando. Per informazioni specifiche del driver, vedere **Note sul driver**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
UPDATE [DatabaseName1!]TableName1  
SET Column_Name1 = eExpression1  
   [, Column_Name2 = eExpression2 ...]  
   WHERE FilterCondition1 [AND | OR FilterCondition2 ...]  
```  
  
## <a name="arguments"></a>Argomenti  
 AGGIORNAMENTO [ *DatabaseName1!*] *TableName1*  
 Specifica la tabella in cui i record vengono aggiornati con i nuovi valori.  
  
 *DatabaseName1!* Specifica il nome di un database diverso dal database specificato con l'origine dati che contiene la tabella. Se il database non è quello corrente, è necessario includere il nome del database che contiene la tabella. Includere il delimitatore punto esclamativo (!) dopo il nome del database e prima del nome della tabella.  
  
 SET *Column_Name1* =  *eExpression1*[, *Column_Name2* =  *eExpression2*  
 Specifica le colonne aggiornate e i relativi nuovi valori. Se si omette la clausola WHERE, ogni riga della colonna viene aggiornata con lo stesso valore.  
  
 DOVE *FilterCondition1*[e &#124; o *FilterCondition2*...]  
 Specifica i record aggiornati con i nuovi valori.  
  
 *FilterCondition* specifica i criteri che i record devono soddisfare per essere aggiornati con nuovi valori. È possibile includere tutte le condizioni di filtro desiderate, connetterle con l'operatore AND o OR. È inoltre possibile utilizzare l'operatore NOT per invertire il valore di un'espressione logica oppure è possibile utilizzare **empty**() per verificare la presenza di un campo vuoto.  
  
## <a name="remarks"></a>Commenti  
 UPDATE-SQL può aggiornare solo i record in una singola tabella.  
  
 Diversamente da REPLACE, UPDATE-SQL usa il blocco dei record quando si aggiornano più record nelle tabelle aperte per l'accesso condiviso. In questo modo si riduce la contesa di record in situazioni multiutente, ma è possibile ridurre le prestazioni. Per ottenere le prestazioni massime, aprire la tabella per l'utilizzo esclusivo oppure utilizzare **Flock**() per bloccare la tabella.  
  
## <a name="driver-remarks"></a>Osservazioni del driver  
 Quando l'applicazione invia l'aggiornamento dell'istruzione SQL ODBC all'origine dati, il driver ODBC Visual FoxPro converte il comando nel comando Visual FoxProUPDATE senza conversione.  
  
## <a name="see-also"></a>Vedere anche  
 [Comando DELETE-SQL](../../odbc/microsoft/delete-sql-command.md)   
 [INSERT (comando SQL)](../../odbc/microsoft/insert-sql-command.md)
