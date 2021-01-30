---
description: MSSQLSERVER_10539
title: MSSQLSERVER_10539 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10539 (Database Engine error)
ms.assetid: 49c26ff7-18b8-4f07-a087-f45f63463b3b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 659e04c3cafce5290877099d653c595e126c3eb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197350"
---
# <a name="mssqlserver_10539"></a>MSSQLSERVER_10539
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|10539|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|PG_NO_PLAN_FOR_STMT|  
|Testo del messaggio|Impossibile creare la guida di piano '%.*ls' dalla cache perché non è disponibile un piano di query per l'istruzione con offset iniziale %d. Questo problema può verificarsi se l'istruzione dipende da oggetti di database non ancora creati. Verificare che esistano tutti gli oggetti di database necessari ed eseguire l'istruzione prima di creare la guida di piano.|  
  
## <a name="explanation"></a>Spiegazione  
Un piano di query non è disponibile nella cache dei piani per l'istruzione con l'offset iniziale specificato.  
  
## <a name="user-action"></a>Azione dell'utente  
Verificare che esistano tutti gli oggetti di database necessari ed eseguire l'istruzione prima di creare la guida di piano.  
  
## <a name="see-also"></a>Vedere anche  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
