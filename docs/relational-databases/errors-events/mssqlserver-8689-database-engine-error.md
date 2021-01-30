---
description: MSSQLSERVER_8689
title: MSSQLSERVER_8689 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8689 (Database Engine error)
ms.assetid: 99467a32-6576-4272-a076-b16c06933f2a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d07f9a50046d33e5c5f4ee122359f41c5b77d400
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194074"
---
# <a name="mssqlserver_8689"></a>MSSQLSERVER_8689
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|8689|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|USEPLAN_ERR_NO_DB|  
|Testo del messaggio|Il database '%.*ls' specificato nell'hint USE PLAN non esiste. Specificare un database esistente.|  
  
## <a name="explanation"></a>Spiegazione  
Un database specificato nell'hint USE PLAN non esiste.  
  
## <a name="user-action"></a>Azione dell'utente  
Verificare che tutti i database specificati nell'hint USE PLAN esistano.  
  
## <a name="see-also"></a>Vedere anche  
[Hint di query &#40;Transact-SQL&#41;](~/t-sql/queries/hints-transact-sql-query.md)  
[Guide di piano](~/relational-databases/performance/plan-guides.md)  
  
