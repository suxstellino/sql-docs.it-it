---
description: MSSQLSERVER_2530
title: MSSQLSERVER_2530 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
ms.assetid: 5d4be07a-38a5-4b25-819c-4dcb4636cc15
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 49e0a196a0ce17a5c95c495ca5feaab9d9889d6b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188972"
---
# <a name="mssqlserver_2530"></a>MSSQLSERVER_2530
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|2530|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC_INDEX_IS_OFFLINE|  
|Testo del messaggio|L'indice "%.*ls" sulla tabella "%.\*ls" è disabilitato.|  
  
## <a name="explanation"></a>Spiegazione  
L'istruzione DBCC non può proseguire perché l'indice specificato è disabilitato. Dopo che un indice viene disabilitato, resta in questo stato fino a quando non viene ricompilato o eliminato e quindi ricreato.  
  
## <a name="user-action"></a>Azione dell'utente  
  
1.  Abilitare l'indice disabilitato utilizzando uno dei metodi seguenti:  
  
    -   Istruzione ALTER INDEX con la clausola REBUILD  
  
    -   Istruzione CREATE INDEX con la clausola DROP_EXISTING  
  
    -   DBCC DBREINDEX  
  
2.  Rieseguire l'istruzione DBCC.  
  
## <a name="see-also"></a>Vedere anche  
[Abilitare indici e vincoli](~/relational-databases/indexes/enable-indexes-and-constraints.md)  
[ALTER INDEX &#40;Transact-SQL&#41;](~/t-sql/statements/alter-index-transact-sql.md)  
[CREATE INDEX &#40;Transact-SQL&#41;](~/t-sql/statements/create-index-transact-sql.md)  
[DBCC DBREINDEX &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)  
  
