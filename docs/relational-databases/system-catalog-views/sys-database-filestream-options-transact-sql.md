---
description: sys.database_filestream_options (Transact-SQL)
title: sys.database_filestream_options (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- database_filestream_options
- sys.database_filestream_options_TSQL
- database_filestream_options_TSQL
- sys.database_filestream_options
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_filestream_options catalog view
ms.assetid: 3383c607-0bbc-456a-ab37-7230f4cbf0e9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1b944db082e93fc75041cdd5ef7034462c06545a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093192"
---
# <a name="sysdatabase_filestream_options-transact-sql"></a>sys.database_filestream_options (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di visualizzare informazioni sul livello di accesso non transazionale a dati FILESTREAM in tabelle FileTable abilitato. Contiene una riga per ogni database nell'istanza di SQL Server.  
  
 Per altre informazioni sugli oggetti FileTable, vedere [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).  
  
  
|Colonna|Type|Descrizione|  
|------------|----------|-----------------|  
|**database_id**|**int**|ID del database. Questo valore è univoco all'interno dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**directory_name**|**nvarchar(255)**|Directory a livello di database per tutti gli spazi dei nomi della tabella FileTable.|  
|**non_transacted_access**|**tinyint**|Livello di accesso non transazionale a dati FILESTREAM abilitato. Il livello di accesso viene impostato dall'opzione NON_TRANSACTED_ACCESS dell'istruzione **create database** o **ALTER database** .<br /><br /> I possibili valori di questa impostazione sono i seguenti:<br /><br /> 0: non abilitato. Si tratta del valore predefinito. Questo livello viene impostato specificando il valore **per l'** opzione **NON_TRANSACTED_ACCESS** .<br /><br /> 1: accesso in sola lettura. Questo livello viene impostato specificando il valore **READ_ONLY** per l'opzione **NON_TRANSACTED_ACCESS** .<br /><br /> 3: accesso completo. Questo livello viene impostato specificando il valore **full** per l'opzione **NON_TRANSACTED_ACCESS** .<br /><br /> 5: in transizione verso READONLY.<br /><br /> 6-in transizione a OFF|  
|**non_transacted_access_desc**|**nvarchar(60)**|Descrizione del livello di accesso non transazionale identificato in non_transacted_access.<br /><br /> I possibili valori di questa impostazione sono i seguenti:<br /><br /> NONE: valore predefinito.<br /><br /> READ_ONLY<br /><br /> FULL<br /><br /> IN_TRANSITION_TO_READ_ONLY<br /><br /> IN_TRANSITION_TO_OFF|  
  
## <a name="see-also"></a>Vedere anche  
 [Abilitare i prerequisiti per la tabella FileTable](../../relational-databases/blob/enable-the-prerequisites-for-filetable.md)  
  
  
