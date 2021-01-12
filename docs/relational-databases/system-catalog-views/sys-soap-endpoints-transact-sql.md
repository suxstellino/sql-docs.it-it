---
description: sys.soap_endpoints (Transact-SQL)
title: sys.soap_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- soap_endpoints_TSQL
- sys.soap_endpoints
- soap_endpoints
- sys.soap_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.soap_endpoints catalog view
ms.assetid: f50dcbfc-02ed-4a19-9c07-c78a5a1b3224
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a0a41f08d7600ae25541924953c407ef12a2ab20
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096726"
---
# <a name="syssoap_endpoints-transact-sql"></a>sys.soap_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Contiene una riga per ogni endpoint nel server con payload di tipo SOAP. Per ogni riga di questa vista, esiste una riga corrispondente con lo stesso **endpoint_id** nella vista del catalogo **sys.http_endpoints** che contiene i metadati di configurazione http.  
  
 
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**< colonne ereditate>**||Per un elenco delle colonne ereditate da questa vista, vedere [sys. endpoints &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**is_sql_language_enabled**|**bit**|1 = BATCHES = è stata specificata l'opzione ENABLED, ovvero sono consentiti batch SQL ad hoc sull'endpoint.|  
|**wsdl_generator_procedure**|**nvarchar (776)**|Nome in tre parti della stored procedure che implementa questo metodo.<br /><br /> I nomi dei metodi richiedono strettamente una sintassi in tre parti. Nomi in una, due e quattro parti non sono consentiti.|  
|**default_database**|**sysname**|Nome del database predefinito indicato nell'opzione DATABASE =.<br /><br /> NULL = è specificato DEFAULT.|  
|**default_namespace**|**nvarchar (384)**|Lo spazio dei nomi predefinito specificato nell'opzione NAMESPACE = oppure `https://tempuri.org` se è stato specificato default.|  
|**default_result_schema**|**tinyint**|Valore predefinito dell'opzione SCHEMA =.<br /><br /> 0 = NONE<br /><br /> 1 = STANDARD|  
|**default_result_schema_desc**|**nvarchar(60)**|Descrizione del valore predefinito dell'opzione SCHEMA =.<br /><br /> NONE<br /><br /> STANDARD|  
|**is_xml_charset_enforced**|**bit**|0 = è specificata l'opzione CHARACTER_SET = SQL.<br /><br /> 1 = è specificata l'opzione CHARACTER_SET = XML.|  
|**is_session_enabled**|**bit**|0 = è specificata l'opzione SESSION = DISABLE.<br /><br /> 1 = è specificata l'opzione SESSION = ENABLED.|  
|**session_timeout**|**int**|Valore specificato nell'opzione SESSION_TIMEOUT =.|  
|**login_type**|**nvarchar(60)**|Tipo di autenticazione consentito su questo endpoint.<br /><br /> WINDOWS<br /><br /> MIXED|  
|**header_limit**|**int**|Dimensione massima consentita dell'intestazione SOAP.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo degli endpoint &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
