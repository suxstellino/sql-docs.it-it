---
description: xp_msver (Transact-SQL)
title: xp_msver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_msver_TSQL
- xp_msver
dev_langs:
- TSQL
helpviewer_keywords:
- xp_msver
ms.assetid: 9264cf8c-92ba-45ad-b2d6-15d26d805a16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8a23619f5a1bf12ddcde718fc95db484f6d3cf6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99123847"
---
# <a name="xp_msver-transact-sql"></a>xp_msver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le informazioni sulla versione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . **xp_msver** restituisce inoltre informazioni sul numero di build effettivo del server e le informazioni sull'ambiente server. Le informazioni restituite da **xp_msver** possono essere utilizzate all'interno [!INCLUDE[tsql](../../includes/tsql-md.md)] di istruzioni, batch, stored procedure e così via, per migliorare la logica per il codice indipendente dalla piattaforma.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
xp_msver [ optname ]  
```  
  
## <a name="arguments"></a>Argomenti  
 *optname*  
 Nome di un'opzione. I possibili valori sono i seguenti.  
  
|Opzione/nome colonna|Descrizione|  
|-------------------------|-----------------|  
|**ProductName**|Nome prodotto; ad esempio, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**ProductVersion**|Versione del prodotto.|  
|**Lingua**|Lingua della versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Piattaforma**|Nome del sistema operativo, del produttore e del tipo di chip del computer che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Commenti**|Informazioni varie relative a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**CompanyName**|Nome del produttore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio [!INCLUDE[msCoName](../../includes/msconame-md.md)] Corporation.|  
|**FileDescription**|Sistema operativo.|  
|**FileVersion**|Versione dell'eseguibile di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**InternalName**|Nome interno [!INCLUDE[msCoName](../../includes/msconame-md.md)] di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio SQLSERVR.|  
|**LegalCopyright**|Informazioni legali sul copyright necessarie per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio Copyright© [!INCLUDE[msCoName](../../includes/msconame-md.md)] Corp. 1988-2005.|  
|**LegalTrademarks**|Informazioni legali sul marchio di fabbrica necessarie per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio [!INCLUDE[msCoName](../../includes/msconame-md.md)] è un marchio registrato di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Corporation.|  
|**OriginalFilename**|Nome del file eseguito all'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio Sqlservr.exe.|  
|**PrivateBuild**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**SpecialBuild**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**WindowsVersion**|Versione di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows installata nel computer in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione.|  
|**ProcessorCount**|Numero di processori del computer in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione.|  
|**ProcessorActiveMask**|Indica i processori installati nel computer in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione che sono avviati e utilizzabili da [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.|  
|**ProcessorType**|Tipo di processore. Simile alla **piattaforma**.|  
|**PhysicalMemory**|Quantità di RAM espressa in megabyte (MB) installata nel computer in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione.|  
|**ID prodotto**|Numero di ID del prodotto (PID). Questo numero viene specificato durante l'installazione ed è riportato sull'etichetta della custodia del CD originale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 1 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 **xp_msver**, senza parametri, restituisce un set di risultati di quattro colonne in cui sono elencati tutti i valori delle opzioni. **xp_msver**, per qualsiasi parametro, restituisce il set di risultati a quattro colonne con i valori per tale opzione.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure estese generali &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [@@VERSION &#40;Transact-SQL&#41;](../../t-sql/functions/version-transact-sql-configuration-functions.md)  
  
  
