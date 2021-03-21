---
description: sys.syslanguages (Transact-SQL)
title: Linguaggi sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.syslanguages
- sys.syslanguages_TSQL
- syslanguages
- syslanguages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syslanguages system table
- sys.syslanguages compatibility view
ms.assetid: f216d1cd-997c-42f0-a737-abbdfcd88383
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3fb34e09ceb553c4b1fc592d37e9871195aff77e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753251"
---
# <a name="syssyslanguages-transact-sql"></a>sys.syslanguages (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Include una riga per ogni lingua presente nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|langid|**smallint**|ID di lingua univoco.|  
|dateformat|**nchar(3)**|Formato di data, ad esempio DMY.|  
|datefirst|**tinyint**|Primo giorno della settimana: 1 per lunedì, 2 per martedì e così via fino a 7 per domenica.|  
|aggiornamento|**int**|Riservato per l'utilizzo nel sistema.|  
|name|**sysname**|Nome della lingua ufficiale, ad esempio, Français.|  
|alias|**sysname**|Nome di lingua alternativo, ad esempio Francese.|  
|months|**nvarchar(372)**|Elenco delimitato da virgole dei nomi completi dei mesi ordinati da gennaio a dicembre. Ogni nome contiene un massimo di 20 caratteri.|  
|shortmonths|**nvarchar(132)**|Elenco delimitato da virgole dei nomi brevi dei mesi ordinati da gennaio a dicembre. Ogni nome contiene un massimo di 9 caratteri.|  
|days|**nvarchar(217)**|Elenco delimitato da virgole dei nomi dei giorni da lunedì a domenica. Ogni nome contiene un massimo di 30 caratteri.|  
|lcid|**int**|ID delle impostazioni locali di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows relative alla lingua.|  
|msglangid|**smallint**|ID del gruppo di messaggi di [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] include le lingue installate seguenti.  
  
|Nome in italiano|Identificatore delle impostazioni locali (LCID) di Windows|ID del gruppo di messaggi di [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|---------------------|------------------|-----------------------------------------|  
|Inglese|1033|1033|  
|Tedesco|1031|1031|  
|Francese|1036|1036|  
|Giapponese|1041|1041|  
|Danese|1030|1030|  
|Spagnolo|3082|3082|  
|Italiano|1040|1040|  
|Olandese|1043|1043|  
|Norvegese|2068|2068|  
|Portoghese|2070|2070|  
|Finlandese|1035|1035|  
|Svedese|1053|1053|  
|Ceco|1029|1029|  
|Ungherese|1038|1038|  
|Polacco|1045|1045|  
|Romeno|1048|1048|  
|Croato|1050|1050|  
|Slovacco|1051|1051|  
|Sloveno|1060|1060|  
|Greco|1032|1032|  
|Bulgaro|1026|1026|  
|Russo|1049|1049|  
|Turco|1055|1055|  
|Inglese (Regno Unito)|2057|1033|  
|Estone|1061|1061|  
|Lettone|1062|1062|  
|Lituano|1063|1063|  
|Portoghese (Brasile)|1046|1046|  
|Cinese tradizionale|1028|1028|  
|Coreano|1042|1042|  
|Cinese semplificato|2052|2052|  
|Arabo|1025|1025|  
|Thai|1054|1054|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste di compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
