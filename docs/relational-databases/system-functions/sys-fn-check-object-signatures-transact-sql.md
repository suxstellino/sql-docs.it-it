---
description: sys.fn_check_object_signatures (Transact-SQL)
title: sys.fn_check_object_signatures (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_check_object_signatures_TSQL
- fn_check_object_signatures_TSQL
- fn_check_object_signatures
- sys.fn_check_object_signatures
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_check_object_signatures function
ms.assetid: 47509566-d3d7-46a9-89c1-91b4895d56b9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25c4774812d900bcbc0b9979b966f7ba7a3945bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206132"
---
# <a name="sysfn_check_object_signatures-transact-sql"></a>sys.fn_check_object_signatures (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Restituisce un elenco di tutti gli oggetti che è possibile firmare e indica se un oggetto viene firmato con una chiave asimmetrica o con un certificato specificato. In questo caso, indica anche se la firma dell'oggetto è valida.  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn_ check_object_signatures (   
    { '@class' } , { @thumbprint }   
  )   
```  
  
## <a name="arguments"></a>Argomenti  
 { '\@ *classe*'}  
 Specifica il tipo di identificazione digitale fornito:  
  
-   'certificate'  
  
-   'asymmetric key'  
  
 \@*classe* è di **tipo sysname**.  
  
 { \@ *identificazione digitale* }  
 Hash SHA-1 del certificato con il quale è crittografata la chiave, o GUID della chiave asimmetrica con il quale è crittografata la chiave. \@*identificazione personale* è di tipo **varbinary (20)**.  
  
## <a name="tables-returned"></a>Tabelle restituite  
 Nella tabella seguente sono elencate le colonne restituite da **fn_check_object_signatures** .  
  
|Colonna|Tipo|Descrizione|  
|------------|----------|-----------------|  
|type|**nvarchar(120)**|Restituisce la descrizione del tipo o l'assembly.|  
|entity_id|**int**|Restituisce l'identificatore dell'oggetto valutato.|  
|is_signed|**int**|Restituisce 0 quando l'oggetto non viene firmato con l'identificazione digitale fornita. Restituisce 1 quando l'oggetto viene firmato con l'identificazione digitale fornita.|  
|is_signature_valid|**int**|Quando il valore is_signed è 1, restituisce 0 se la firma non è valida e 1 se è valida.<br /><br /> Quando il valore is_signed è 0, restituisce sempre 0.|  
  
## <a name="remarks"></a>Commenti  
 Usare **fn_check_object_signatures** per verificare che gli utenti malintenzionati non abbiano alterato gli oggetti.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW DEFINITION per il certificato o la chiave asimmetrica.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene individuato il certificato di firma dello schema per il database `master` e viene restituito il valore `is_signed` 1 e il valore `is_signature_valid` 1 per gli oggetti firmati con il certificato di firma dello schema che dispongono di firme valide.  
  
```  
USE master;  
-- Declare a variable to hold the thumbprint.  
DECLARE @thumbprint varbinary(20) ;  
-- Populate the thumbprint variable with the master database schema signing certificate.  
SELECT @thumbprint = thumbprint   
FROM sys.certificates   
WHERE name LIKE '%SchemaSigningCertificate%' ;  
-- Evaluates the objects signed by the schema signing certificate  
SELECT type, entity_id, OBJECT_NAME(entity_id) AS [object name], is_signed, is_signature_valid  
FROM sys.fn_check_object_signatures ('certificate', @thumbprint) ;  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [IS_OBJECTSIGNED &#40;Transact-SQL&#41;](../../t-sql/functions/is-objectsigned-transact-sql.md)  
  
  
