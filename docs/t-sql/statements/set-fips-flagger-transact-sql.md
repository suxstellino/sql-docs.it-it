---
description: SET FIPS_FLAGGER (Transact-SQL)
title: SET FIPS_FLAGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FIPS_FLAGGER
- SET_FIPS_FLAGGER_TSQL
- FIPS_FLAGGER_TSQL
- SET FIPS_FLAGGER
dev_langs:
- TSQL
helpviewer_keywords:
- SET FIPS_FLAGGER statement
- FIPS 127-2 standard
- FIPS_FLAGGER option
ms.assetid: e82f6bee-6cf6-4061-be22-9ad2e8e9d3d6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6e0fc2dedf045d08f077f7417bd70a8eaf926fcd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093354"
---
# <a name="set-fips_flagger-transact-sql"></a>SET FIPS_FLAGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Specifica il controllo della conformità con lo standard FIPS 127-2, basato sullo standard ISO. Per informazioni sulla conformità FIPS in SQL Server, vedere [How to use SQL Server 2016 in FIPS 140-2-compliant mode](https://support.microsoft.com/help/4014354/how-to-use-sql-server-2016-in-fips-140-2-compliant-mode) (Come usare SQL Server 2016 in modalità di conformità con FIPS 140-2). 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SET FIPS_FLAGGER ( 'level' |  OFF )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **'** *level* **'**  
 Livello di conformità con lo standard FIPS 127-2 in base a cui vengono controllate tutte le operazioni del database. Se un'operazione di database è in conflitto con il livello degli standard ISO scelti, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un avviso.  
  
 Il valore di *level* deve essere uno dei seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|ENTRY|Controllo degli standard per la conformità di base con ISO.|  
|FULL|Controllo degli standard per la conformità totale con ISO.|  
|INTERMEDIATE|Controllo degli standard per la conformità a livello intermedio con ISO.|  
|OFF|Nessun controllo di conformità degli standard.|  
  
## <a name="remarks"></a>Commenti  
 L'opzione `SET FIPS_FLAGGER` viene impostata in fase di analisi e non in fase di esecuzione. L'impostazione in fase di analisi indica che, se l'istruzione SET è inclusa nel batch o nella stored procedure, l'istruzione viene attivata indipendentemente dal fatto che l'esecuzione del codice raggiunga effettivamente tale punto. L'istruzione `SET` diventa anche attiva prima dell'esecuzione di qualsiasi istruzione. Ad esempio, anche se l'istruzione `SET` è inclusa in un blocco di istruzione `IF...ELSE` che non viene mai raggiunto in fase di esecuzione, l'istruzione `SET` viene comunque eseguita perché il blocco `IF...ELSE` viene analizzato.  
  
 Se l'opzione `SET FIPS_FLAGGER` viene impostata in una stored procedure, il valore dell'opzione `SET FIPS_FLAGGER` viene ripristinato al termine della stored procedure. Un'istruzione `SET FIPS_FLAGGER` specificata nel codice SQL dinamico pertanto non ha alcun effetto sulle istruzioni successive.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="see-also"></a>Vedere anche  
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
