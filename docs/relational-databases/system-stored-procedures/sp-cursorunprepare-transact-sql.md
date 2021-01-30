---
description: sp_cursorunprepare (Transact-SQL)
title: sp_cursorunprepare (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorunprepare_TSQL
- sp_cursorunprepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorunprepare
ms.assetid: b46d4813-c4a9-4f9d-9979-2b5082ecf06a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2b15e39aa20c9ced1fa289eff2223a11505665b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209737"
---
# <a name="sp_cursorunprepare-transact-sql"></a>sp_cursorunprepare (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina il piano di esecuzione sviluppato nella sp_cursorprepare stored procedure. sp_cursorunprepare viene richiamato specificando ID = 6 in un pacchetto del flusso TDS (Tabular Data Stream).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_cursorunprepare handle  
```  
  
## <a name="arguments"></a>Argomenti  
 *gestire*  
 Valore dell' *handle* restituito da sp_cursorprepare quando l'istruzione viene preparata.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_cursorprepare &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorprepare-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
