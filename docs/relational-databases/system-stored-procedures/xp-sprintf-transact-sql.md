---
description: xp_sprintf (Transact-SQL)
title: xp_sprintf (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_sprintf_TSQL
- xp_sprintf
dev_langs:
- TSQL
helpviewer_keywords:
- xp_sprintf
ms.assetid: 1eedd65c-03cc-4eab-b76e-04684fdfec52
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c4cccbc09cfeb79284dd17a0b107a754c548f5c4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755171"
---
# <a name="xp_sprintf-transact-sql"></a>xp_sprintf (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Formatta e archivia una serie di caratteri e valori nel parametro di output della stringa. Ogni argomento di formato viene sostituito con l'argomento corrispondente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
xp_sprintf { string OUTPUT , format }  
     [ , argument [ ,...n ] ]  
```  
  
## <a name="arguments"></a>Argomenti  
 *string*  
 Variabile **varchar** che riceve l'output.  
  
 OUTPUT  
 Se si specifica questo argomento, il valore della variabile viene inserito nel parametro di output.  
  
 *format*  
 È una stringa di caratteri di formato con segnaposto per i valori di *argomento* , simile a quello supportato dalla funzione **sprintf** del linguaggio C. Attualmente è supportato solo l'argomento di formato %.  
  
 *argument*  
 Stringa di caratteri che rappresenta il valore dell'argomento di formato corrispondente.  
  
 *n*  
 Segnaposto che indica la possibilità di specificare un massimo di 50 argomenti.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 **xp_sprintf** restituisce il messaggio seguente:  
  
 `The command(s) completed successfully.`  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure estese generali &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_sscanf &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/xp-sscanf-transact-sql.md)  
  
  
