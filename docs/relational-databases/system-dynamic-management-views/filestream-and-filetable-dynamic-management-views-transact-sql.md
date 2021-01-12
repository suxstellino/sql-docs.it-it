---
description: DMV per FILESTREAM e tabelle FileTable (Transact-SQL)
title: Viste a gestione dinamica FILESTREAM e FileTable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- FileTables [SQL Server], dynamic management views
ms.assetid: e50a135d-6644-42a4-a0df-1c7a2b722051
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3af7a4d72a93d2c44f470f83f6a0010737d3adcc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099031"
---
# <a name="filestream-and-filetable-dynamic-management-views-transact-sql"></a>DMV per FILESTREAM e tabelle FileTable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questa sezione vengono descritti le DMV correlate alle caratteristiche FILESTREAM e FileTable.  
  
## <a name="filestream-dynamic-management-views-and-functions"></a>DMV e funzioni per FILESTREAM  
 [sys.dm_filestream_file_io_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-file-io-handles-transact-sql.md)  
 Consente di visualizzare gli handle di file transazionali attualmente aperti.  
  
 [sys.dm_filestream_file_io_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-file-io-requests-transact-sql.md)  
 Vengono visualizzate le richieste di input e output di file correnti.  
  
## <a name="filetable-dynamic-management-views-and-functions"></a>DMV e funzioni per tabelle FileTable  
 [sys.dm_filestream_non_transacted_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md)  
 Consente di visualizzare gli handle di file non transazionali aperti per dati di tabelle FileTable.  

## <a name="see-also"></a>Vedere anche
[Filestream](../../relational-databases/blob/filestream-sql-server.md)
<br>[Tabelle FileTable](../../relational-databases/blob/filetables-sql-server.md)
<br>[Viste del catalogo Filestream e FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[Stored procedure di sistema per Filestream e tabelle FileTable (Transact-SQL)](../system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)
  
