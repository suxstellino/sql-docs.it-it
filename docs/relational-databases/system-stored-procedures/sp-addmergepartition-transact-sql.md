---
description: sp_addmergepartition (Transact-SQL)
title: sp_addmergepartition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepartition
- sp_addmergepartition_TSQL
helpviewer_keywords:
- sp_addmergepartition
ms.assetid: 02a5f46b-e5ff-4932-a3ff-7f0fd82d0981
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5129bcd9ee63b0c1e074294c8b69f997432240f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192324"
---
# <a name="sp_addmergepartition-transact-sql"></a>sp_addmergepartition (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una partizione filtrata in modo dinamico per una sottoscrizione filtrata in base ai valori di [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) o [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) nel Sottoscrittore. Questa stored procedure viene eseguita nel server di pubblicazione nel database da pubblicare e viene utilizzata per la generazione manuale di partizioni.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addmergepartition [ @publication = ] 'publication'  
        , [ @suser_sname = ] 'suser_sname'  
        , [ @host_name = ] 'host_name'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Pubblicazione di tipo merge in cui viene creata la partizione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito. Se *SUSER_SNAME* è specificato, il valore del *nome host* deve essere null.  
  
`[ @suser_sname = ] 'suser_sname'` Valore utilizzato per la creazione della partizione per una sottoscrizione filtrata in base al valore della funzione [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) nel Sottoscrittore. *SUSER_SNAME* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @host_name = ] 'host_name'` Valore utilizzato per la creazione della partizione per una sottoscrizione filtrata in base al valore della funzione [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) nel Sottoscrittore. *HOST_NAME* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addmergepartition** viene utilizzata nella replica di tipo merge.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/sp-addmergepartition-tra_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addmergepartition**.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di uno snapshot per una pubblicazione di tipo merge con filtri con parametri](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)   
 [Filtri di riga con parametri](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
