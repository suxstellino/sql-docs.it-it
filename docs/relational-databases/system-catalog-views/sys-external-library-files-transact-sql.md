---
description: sys.external_library_files (Transact-SQL)
title: sys.external_library_files (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- external_library_files
- external_library_files_TSQL
- sys.external_library_files
- sys.external_library_files_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_library_files catalog view
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 139843ab9a4c4c6a398c96cc3bcc3f7b50b297f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191551"
---
# <a name="sysexternal_library_files-transact-sql"></a>sys.external_library_files (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Elenca una riga per ogni file che costituisce una libreria esterna.

|Nome colonna |Tipo di dati |Descrizione|
|------|------|-----|
|external_library_id | INT |ID dell'oggetto libreria esterna. |
|contenuto |varbinary(max) |Contenuto dell'elemento del file di libreria esterno. |
|Piattaforma |TINYINT |ID della piattaforma host in cui è installato SQL Server. |
|platform_desc | nvarchar(60) |Nome della piattaforma host. I valori validi sono ' WINDOWS ',' LINUX '. |

### <a name="see-also"></a>Vedi anche  

[sys.external_libraries](sys-external-libraries-transact-sql.md)  
[CREA LIBRERIA ESTERNA](../../t-sql/statements/create-external-library-transact-sql.md)  
