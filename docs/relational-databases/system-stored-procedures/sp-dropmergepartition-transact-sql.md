---
description: sp_dropmergepartition (Transact-SQL)
title: sp_dropmergepartition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergepartition_TSQL
- sp_dropmergepartition
helpviewer_keywords:
- sp_dropmergepartition
ms.assetid: 1be511c1-79ff-4947-9379-78d83b7b8945
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 42d514bff059011564ca4ba0bca0077e8bd8205f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754701"
---
# <a name="sp_dropmergepartition-transact-sql"></a>sp_dropmergepartition (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Rimuove una partizione per un filtro di riga con parametri da una pubblicazione. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione. Consente inoltre di rimuovere il processo snapshot corrispondente e i file di snapshot per la partizione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dropmergepartition [ @publication = ] 'publication'  
        , [ @suser_sname = ] 'suser_sname'  
        , [ @host_name = ] 'host_name'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication] = 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @suser_sname = ] 'suser_sname'` Valore della funzione [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) nel Sottoscrittore utilizzata per definire la partizione. *SUSER_SNAME* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @host_name = ] 'host_name'` Valore della funzione [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) nel Sottoscrittore utilizzata per definire la partizione. *HOST_NAME* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_dropmergepartition** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_dropmergepartition**.  
  
## <a name="see-also"></a>Vedere anche  
 [Gestire le partizioni di una pubblicazione di tipo merge con filtri con parametri](../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)  
  
  
