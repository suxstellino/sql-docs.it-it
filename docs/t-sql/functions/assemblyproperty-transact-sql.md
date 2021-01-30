---
description: ASSEMBLYPROPERTY (Transact-SQL)
title: ASSEMBLYPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASSEMBLYPROPERTY_TSQL
- ASSEMBLYPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- ASSEMBLYPROPERTY statement
- assemblies [CLR integration], properties
ms.assetid: cf03d1b1-724c-48bf-a8df-3fe2586b150a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5a5acce18561975c29a3e5f22f74227e079f422f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210005"
---
# <a name="assemblyproperty-transact-sql"></a>ASSEMBLYPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questa funzione restituisce informazioni su una proprietà di un assembly.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ASSEMBLYPROPERTY('assembly_name', 'property_name')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*assembly_name*  
Nome dell'assembly.
  
*property_name*  
Nome di una proprietà su cui recuperare informazioni. *property_name* può avere uno dei valori seguenti:
  
|valore|Descrizione|  
|---|---|
|**CultureInfo**|Impostazioni locali dell'assembly.|  
|**PublicKey**|Chiave pubblica o token di chiave pubblica dell'assembly.|  
|**MvID**|Numero completo di identificazione della versione dell'assembly generato dal compilatore.|  
|**VersionMajor**|Parte principale (prima parte) del numero di identificazione della versione dell'assembly composto da quattro parti.|  
|**VersionMinor**|Parte secondaria (seconda parte) del numero di identificazione della versione dell'assembly composto da quattro parti.|  
|**VersionBuild**|Parte relativa alla build (terza parte) del numero di identificazione della versione dell'assembly composto da quattro parti.|  
|**VersionRevision**|Parte relativa alla revisione (quarta parte) del numero di identificazione della versione dell'assembly composto da quattro parti.|  
|**SimpleName**|Nome semplice dell'assembly.|  
|**Architettura**|Architettura del processore dell'assembly.|  
|**CLRName**|Stringa canonica che codifica il nome semplice, il numero di versione, le impostazioni internazionali, la chiave pubblica e l'architettura dell'assembly. Questo valore identifica in modo univoco l'assembly sul lato CLR (Common Language Runtime).|  
  
## <a name="return-type"></a>Tipo restituito
**sql_variant**
  
## <a name="examples"></a>Esempi  
Questo esempio presuppone che un assembly `HelloWorld` sia registrato nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Per altre informazioni, vedere [Esempio Hello World](/previous-versions/sql/sql-server-2016/ff878250(v=sql.130)).
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ASSEMBLYPROPERTY ('HelloWorld' , 'PublicKey');  
```  
  
## <a name="see-also"></a>Vedere anche
[CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)  
[DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)
  
