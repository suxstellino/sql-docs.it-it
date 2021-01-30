---
description: GOTO (Transact-SQL)
title: GOTO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GOTO
- GOTO_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- skipping statements
- Transact-SQL statements, skipping
- labels [SQL Server]
- statements [SQL Server], skipping
- GOTO statement
ms.assetid: 589b6f8e-dc80-416f-9e74-48bed5337f58
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4653639f9e83ebc8d9e3fa727b8f966555aeadfa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99115010"
---
# <a name="goto-transact-sql"></a>GOTO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Reindirizza il flusso di esecuzione a un'etichetta. L'istruzione o le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] successive a GOTO vengono ignorate e l'elaborazione viene ripresa in corrispondenza dell'etichetta. È possibile utilizzare istruzioni ed etichette GOTO in qualsiasi punto di una procedura, un batch o un blocco di istruzioni. Le istruzioni GOTO possono essere nidificate.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Define the label:   
label:   
Alter the execution:  
GOTO label   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *label*  
 Posizione da cui inizia l'elaborazione se la destinazione dell'istruzione GOTO è impostata su tale etichetta. Le etichette devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). È possibile adottare le etichette come metodo per l'inserimento di commenti indipendentemente dall'utilizzo dell'istruzione GOTO.  
  
## <a name="remarks"></a>Osservazioni  
 L'istruzione GOTO può essere utilizzata in istruzioni condizionali per il controllo di flusso, in blocchi di istruzioni o in procedure ma non è possibile associarla a un'etichetta all'esterno del batch. Le diramazioni implementate tramite l'istruzione GOTO possono fare riferimento a un'etichetta definita prima o dopo GOTO.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni per l'istruzione GOTO vengono assegnate per impostazione predefinita a qualsiasi utente valido.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrato come utilizzare `GOTO` come meccanismo di diramazione.  
  
```  
DECLARE @Counter int;  
SET @Counter = 1;  
WHILE @Counter < 10  
BEGIN   
    SELECT @Counter  
    SET @Counter = @Counter + 1  
    IF @Counter = 4 GOTO Branch_One --Jumps to the first branch.  
    IF @Counter = 5 GOTO Branch_Two  --This will never execute.  
END  
Branch_One:  
    SELECT 'Jumping To Branch One.'  
    GOTO Branch_Three; --This will prevent Branch_Two from executing.  
Branch_Two:  
    SELECT 'Jumping To Branch Two.'  
Branch_Three:  
    SELECT 'Jumping To Branch Three.';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Elementi del linguaggio per il controllo di flusso &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)   
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [BREAK &#40;Transact-SQL&#41;](../../t-sql/language-elements/break-transact-sql.md)   
 [CONTINUE &#40;Transact-SQL&#41;](../../t-sql/language-elements/continue-transact-sql.md)   
 [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)   
 [WAITFOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/waitfor-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  
  
  
