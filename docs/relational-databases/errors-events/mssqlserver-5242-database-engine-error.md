---
description: MSSQLSERVER_5242
title: MSSQLSERVER_5242 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5242 (Database Engine error)
ms.assetid: 712b1a10-2f87-41df-a111-1ed9f14102d4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 58ac27fac4af3c8d343102403549758ea428d494
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198129"
---
# <a name="mssqlserver_5242"></a>MSSQLSERVER_5242
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|5242|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico||  
|Testo del messaggio|Rilevata un'inconsistenza durante un'operazione interna nel database '%.*ls'(ID:%d) sulla pagina %S_PGID. Contattare il supporto tecnico. Numero di riferimento %ld.|  
  
## <a name="explanation"></a>Spiegazione  
Nella pagina del database a cui si fa riferimento nel messaggio di errore si è verificata un'inconsistenza strutturale.  
  
## <a name="see-also"></a>Vedere anche  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Filegroup e file di database](~/relational-databases/databases/database-files-and-filegroups.md)  
  
