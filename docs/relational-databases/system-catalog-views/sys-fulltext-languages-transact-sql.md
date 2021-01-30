---
description: sys.fulltext_languages (Transact-SQL)
title: sys.fulltext_languages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_languages
- sys.fulltext_languages_TSQL
- fulltext_languages
- fulltext_languages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- languages [full-text search]
- sys.fulltext_languages catalog view
ms.assetid: 2ed6b53d-1cf2-4763-9d58-36ea24a610ef
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 47032ad75fb96842f278533e184307b99017c798
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191501"
---
# <a name="sysfulltext_languages-transact-sql"></a>sys.fulltext_languages (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In questa vista del catalogo è contenuta una riga per ogni lingua i cui word breaker sono registrati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In ogni riga è visualizzato l'identificatore LCID e il nome della lingua. Quando i Word breaker sono registrati per una lingua, le altre risorse linguistiche, ovvero stemmer, parole non significative (parole non significative) e file del thesaurus, diventano disponibili per le operazioni di indicizzazione e query full-text. È possibile specificare il valore di **Name** o **LCID** nelle istruzioni per le query full-text e per gli indici full-text [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
   
|Colonna|Tipo di dati|Descrizione|  
|------------|---------------|-----------------|  
|**lcid**|**int**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Identificatore delle impostazioni locali (LCID) di Windows per la lingua.|  
|**nome**|**sysname**|Valore dell'alias in [sys.syslingue](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) corrispondenti al valore **LCID** o alla rappresentazione di stringa dell'identificatore LCID numerico.|  
  
## <a name="values-returned-for-default-languages"></a>Valori restituiti per le lingue predefinite  
 Nella tabella seguente sono mostrati i valori per le lingue i cui word breaker sono registrati per impostazione predefinita.  
  
|Linguaggio|LCID|  
|--------------|----------|  
|Arabo|1025|  
|Bengalese (India)|1093|  
|Inglese (Regno Unito)|2057|  
|Bulgaro|1026|  
|Catalano|1027|  
|Cinese (Hong Kong - R.A.S., Repubblica popolare cinese)|3076|  
|Cinese (RAS di Macao)|5124|  
|Cinese (Singapore)|4100|  
|Croato|1050|  
|Ceco|1029|  
|Danese|1030|  
|Olandese|1043|  
|Inglese|1033|  
|Francese|1036|  
|Tedesco|1031|  
|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Greco|1032|  
|Gujarati|1095|  
|Ebraico|1037|  
|Hindi|1081|  
|Islandese|1039|  
|Indonesiano|1057|  
|Italiano|1040|  
|Giapponese|1041|  
|Kannada|1099|  
|Coreano|1042|  
|Lettone|1062|  
|Lituano|1063|  
|Malese (Malesia)|1086|  
|Malayalam|1100|  
|Marathi|1102|  
|Neutralità|0|  
|Norvegese (Bokmål)|1044|  
|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Polacco|1045|  
|Portoghese (Brasile)|1046|  
|Portoghese (Portogallo)|2070|  
|Punjabi|1094|  
|Romeno|1048|  
|Russo|1049|  
|Serbo (alfabeto cirillico)|3098|  
|Serbo (alfabeto latino)|2074|  
|Cinese semplificato|2052|  
|Slovacco|1051|  
|Sloveno|1060|  
|Spagnolo|3082|  
|Svedese|1053|  
|Tamil|1097|  
|Telugu|1098|  
|Thai|1054|  
|Cinese tradizionale|1028|  
|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Turco|1055|  
|Ucraino|1058|  
|Urdu|1056|  
|Vietnamita|1066|  
  
## <a name="remarks"></a>Commenti  
 Per aggiornare l'elenco delle lingue registrate per la ricerca full-text, utilizzare [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)'**update_languages**'.  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Vedere anche  
 [sp_fulltext_load_thesaurus_file &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)   
 [sp_fulltext_service &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)   
 [Configurazione e gestione di word breaker e stemmer per la ricerca](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [Configurare e gestire i file del thesaurus per la ricerca Full-Text](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)   
 [Configurare e gestire parole non significative ed elenchi di parole non significative per la ricerca full-text](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [Aggiornamento della ricerca full-text](../../relational-databases/search/upgrade-full-text-search.md)  
  
  
