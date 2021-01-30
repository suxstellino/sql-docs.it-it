---
description: MSSQLSERVER_10534
title: MSSQLSERVER_10534 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10534 (Database Engine error)
ms.assetid: e65bb118-99d5-4fdb-b1d5-0ec70f0a677b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f53c5a8ce3a2965cda3f9fa1028189e2fbc8c266
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197393"
---
# <a name="mssqlserver_10534"></a>MSSQLSERVER_10534
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|10534|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|PG_INVALID_PARAMS|  
|Testo del messaggio|Impossibile creare la guida di piano '%.\*ls' perché il valore specificato per **\@params** non è valido. Specificare il valore nel formato *nome_parametro tipo_parametro* o specificare NULL.|  
  
## <a name="explanation"></a>Spiegazione  
Il valore specificato per **\@params** non è valido.  
  
## <a name="user-action"></a>Azione dell'utente  
Specificare il valore nel formato *nome_parametro tipo_parametro* o specificare NULL.  
  
## <a name="see-also"></a>Vedere anche  
[Guide di piano](~/relational-databases/performance/plan-guides.md)  
[sp_create_plan_guide &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
