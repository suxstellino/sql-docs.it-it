---
description: Stored procedure di sistema per Filestream e tabelle FileTable (Transact-SQL)
title: Stored procedure di sistema FILESTREAM e FileTable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- FileTables [SQL Server], catalog views
ms.assetid: 2c83a4a7-720b-4435-a3b5-788c29f56949
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7ee0eced0370c3ea16091827555e90f4951d6cf7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346699"
---
# <a name="filestream-and-filetable-system-stored-procedures-transact-sql"></a>Stored procedure di sistema FILESTREAM e FileTable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questa sezione vengono descritte le stored procedure di sistema per la funzionalità FileTable e FileStream.  

## <a name="filestream-and-filetable-system-stored-procedures"></a>Stored procedure di sistema FILESTREAM e FileTable
  [sp_filestream_force_garbage_collection (Transact-SQL)](filestream-and-filetable-sp-filestream-force-garbage-collection.md)

   Forza il Garbage Collector di FILESTREAM all'esecuzione, eliminando qualsiasi file FILESTREAM non necessario.

  [sp_kill_filestream_non_transacted_handles (Transact-SQL)](filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md)

  Consente di chiudere handle di file non transazionali per dati di tabelle FileTable.


## <a name="see-also"></a>Vedi anche
[Filestream](../../relational-databases/blob/filestream-sql-server.md)
<br>[Tabelle FileTable](../../relational-databases/blob/filetables-sql-server.md)
<br>[DMV per FILESTREAM e tabelle FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Viste del catalogo Filestream e FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
  
  
