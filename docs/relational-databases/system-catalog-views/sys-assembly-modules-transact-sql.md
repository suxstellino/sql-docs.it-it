---
description: sys.assembly_modules (Transact-SQL)
title: sys.assembly_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.assembly_modules
- sys.assembly_modules_TSQL
- assembly_modules
- assembly_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assembly_modules catalog view
ms.assetid: 5f9e644e-8065-49a2-b53d-db7df98f70d8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5d46e4eca31f16a9f244046f05abe6dda44df3fb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099298"
---
# <a name="sysassembly_modules-transact-sql"></a>sys.assembly_modules (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni funzione, procedura o trigger definito da un assembly CLR (Common Language Runtime). Questa vista del catalogo esegue il mapping di stored procedure CLR, trigger CLR o funzioni CLR all'implementazione sottostante corrispondente. Gli oggetti di tipo TA, AF, PC, FS e FT sono associati a un modulo in assembly. Per trovare l'associazione tra oggetto e assembly, è possibile unire questa vista del catalogo ad altre viste. Ad esempio, quando si crea un stored procedure CLR, questo viene rappresentato da una riga in **sys. Objects**, una riga in **sys. Procedures** (che eredita da **sys. Objects**) e una riga in **sys.assembly_modules**. Il stored procedure stesso è rappresentato dai metadati in **sys. Objects** e **sys. Procedures**. I riferimenti all'implementazione CLR sottostante della stored procedure sono disponibili in **sys.assembly_modules**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Numero di identificazione dell'oggetto SQL. Valore univoco all'interno di un database.|  
|**assembly_id**|**int**|ID dell'assembly da cui è stato creato questo modulo.|  
|**assembly_class**|**sysname**|Nome della classe nell'assembly che definisce il modulo corrente.|  
|**assembly_method**|**sysname**|Nome del metodo all'interno del **assembly_class** che definisce il modulo.<br /><br /> Restituisce NULL per le funzioni di aggregazione (AF).|  
|**null_on_null_input**|**bit**|Il modulo è stato dichiarato in modo da produrre un output NULL per qualsiasi input NULL.|  
|**execute_as_principal_id**|**int**|ID dell'entità di database nella quale si verifica l'esecuzione del contesto nella modalità specificata dalla clausola EXECUTE AS della funzione CLR, della stored procedure CLR o del trigger CLR.<br /><br /> NULL = EXECUTE AS CALLER Questo è il valore predefinito.<br /><br /> ID dell'entità di database specificata = EXECUTE AS SELF, EXECUTE AS *user_name* o execute As *login_name*.<br /><br /> -2 = EXECUTE AS OWNER.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
