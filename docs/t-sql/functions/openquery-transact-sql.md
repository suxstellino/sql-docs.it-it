---
description: OPENQUERY (Transact-SQL)
title: OPENQUERY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENQUERY_TSQL
- OPENQUERY
dev_langs:
- TSQL
helpviewer_keywords:
- DELETE statement [SQL Server], OPENQUERY function
- OPENQUERY function
- FROM clause, OPENQUERY function
- UPDATE statement [SQL Server], OPENQUERY function
- pass-through queries [SQL Server]
- INSERT statement [SQL Server], OPENQUERY function
ms.assetid: b805e976-f025-4be1-bcb0-3a57b0c57717
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: d7af8ffa6f8fe26bf31c699dece7f24e7f6c32fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180207"
---
# <a name="openquery-transact-sql"></a>OPENQUERY (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Esegue la query pass-through specificata nel server collegato specificato. Il server è un'origine dei dati OLE DB. È possibile fare riferimento alla funzione OPENQUERY nella clausola FROM di una query come se fosse un nome di tabella. È inoltre possibile fare riferimento alla funzione OPENQUERY come tabella di destinazione di un'istruzione INSERT, UPDATE o DELETE, a seconda delle capacità del provider OLE DB. Anche quando la query restituisce più set di risultati, la funzione OPENQUERY restituisce solo il primo set.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
OPENQUERY ( linked_server ,'query' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *linked_server*  
 Identificatore che rappresenta il nome del server collegato.  
  
 **'** *query* **'**  
 Stringa della query eseguita nel server collegato. La lunghezza massima della stringa è pari a 8 KB.  
  
## <a name="remarks"></a>Osservazioni  
 La funzione OPENQUERY non accetta variabili come argomenti.  
  
 OPENQUERY non può essere utilizzata per eseguire stored procedure estese in un server collegato. È tuttavia possibile eseguire una stored procedure estesa in un server collegato utilizzando un nome in quattro parti, Ad esempio:  
  
```sql  
EXEC SeattleSales.master.dbo.xp_msver  
```  
  
 Qualsiasi chiamata a OPENDATASOURCE, OPENQUERY o OPENROWSET nella clausola FROM viene valutata separatamente e indipendentemente da qualsiasi altra chiamata a queste funzioni utilizzate come destinazione dell'aggiornamento, anche se alle due chiamate vengono forniti argomenti identici. In particolare, le condizioni di filtro o join applicate al risultato di una di tali chiamate non hanno effetto sui risultati dell'altra.  
  
## <a name="permissions"></a>Autorizzazioni  
 Qualsiasi utente può eseguire OPENQUERY. Le autorizzazioni utilizzate per la connessione al server remoto sono ottenute dalle impostazioni definite per il server collegato.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-executing-an-update-pass-through-query"></a>R. Esecuzione di una query pass-through UPDATE  
 Nell'esempio seguente viene eseguita una query pass-through `UPDATE` sul server collegato creato nell'esempio A.  
  
```sql  
UPDATE OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE id = 101')   
SET name = 'ADifferentName';  
```  
  
### <a name="b-executing-an-insert-pass-through-query"></a>B. Esecuzione di una query pass-through INSERT  
 Nell'esempio seguente viene eseguita una query pass-through `INSERT` sul server collegato creato nell'esempio A.  
  
```sql  
INSERT OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles')  
VALUES ('NewTitle');  
```  
  
### <a name="c-executing-a-delete-pass-through-query"></a>C. Esecuzione di una query pass-through DELETE  
 Nell'esempio seguente viene eseguita una query pass-through `DELETE` per eliminare la riga inserita nell'esempio C.  
  
```sql  
DELETE OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE name = ''NewTitle''');  
```  
  
### <a name="d-executing-a-select-pass-through-query"></a>D. Esecuzione di una query pass-through SELECT  
 Nell'esempio seguente viene eseguita una query pass-through `SELECT` per selezionare la riga inserita nell'esempio C.  
  
```sql  
SELECT * FROM OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE name = ''NewTitle''');  
```  
    
## <a name="see-also"></a>Vedere anche  
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
