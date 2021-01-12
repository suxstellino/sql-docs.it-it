---
description: CHECKPOINT (Transact-SQL)
title: CHECKPOINT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKPOINT_TSQL
- CHECKPOINT
dev_langs:
- TSQL
helpviewer_keywords:
- events [SQL Server], checkpoints
- automatic checkpoints
- writing dirty pages to disk
- pages [SQL Server], dirty
- checkpoints [SQL Server]
- CHECKPOINT statement
- log truncate mode [SQL Server]
- dirty pages
- logs [SQL Server], checkpoints
- manual checkpoints [SQL Server]
- pages [SQL Server], checkpoints
ms.assetid: ccdfc689-ad4e-44c0-83f7-0f2cfcfb6406
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 257bdc81d1f5f2ab6a5599cb155f71453ac27c34
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097149"
---
# <a name="checkpoint-transact-sql"></a>CHECKPOINT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Genera un checkpoint manuale nel database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al quale l'utente è attualmente connesso.  
  
> [!NOTE]  
>  Per informazioni sui tipi diversi di checkpoint del database e sull'operazione di checkpoint in generale, vedere [Checkpoint di database &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CHECKPOINT [ checkpoint_duration ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *checkpoint_duration*  
 Specifica il numero di secondi disponibili per il completamento manuale del checkpoint. Se si specifica *checkpoint_duration*, il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] cerca di eseguire il checkpoint nell'arco di tempo specificato. Il valore di *checkpoint_duration* deve essere un'espressione di tipo **int** e maggiore di zero. Se si omette questo parametro, tramite il [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene adeguata automaticamente la durata del checkpoint per ridurre al minimo l'impatto sulle prestazioni nelle applicazioni di database. L'opzione *checkpoint_duration* è un'opzione avanzata.  
  
## <a name="factors-affecting-the-duration-of-checkpoint-operations"></a>Fattori che incidono sulla durata delle operazioni di checkpoint  
 In generale il tempo necessario per l'esecuzione di un'operazione di checkpoint aumenta con il numero di pagine dirty che l'operazione deve scrivere. Per impostazione predefinita, per ridurre l'impatto sulle prestazioni su altre applicazioni in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene regolata la frequenza di scritture che un'operazione di checkpoint esegue. Se si riduce la frequenza delle scritture, aumenta il tempo necessario per completare l'operazione di checkpoint. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa strategia viene usata per un checkpoint manuale, a meno che nel comando CHECKPOINT non venga specificato un valore *checkpoint_duration*.  
  
 L'impatto sulle prestazioni derivante dall'uso di *checkpoint_duration* dipende dal numero di pagine dirty, dall'attività del sistema e dalla durata effettiva specificata. Se ad esempio il checkpoint viene normalmente completato in 120 secondi e si imposta *checkpoint_duration* su 45 secondi, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve dedicare al checkpoint più risorse di quante verrebbero assegnate per impostazione predefinita. Se invece si imposta *checkpoint_duration* su 180 secondi, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assegna meno risorse di quante ne verrebbero assegnate per impostazione predefinita. In generale, un valore breve di *checkpoint_duration* aumenterà le risorse dedicate al checkpoint, mentre un valore elevato di *checkpoint_duration* le ridurrà. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un checkpoint, se possibile, viene sempre completato e l'istruzione CHECKPOINT restituisce immediatamente un risultato al completamento del checkpoint. In alcuni casi, quindi, un checkpoint può venire completato prima o dopo il periodo di tempo indicato.  
  
##  <a name="security"></a><a name="Security"></a> Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni per l'istruzione CHECKPOINT vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** e dei ruoli predefiniti del database **db_owner** e **db_backupoperator**. Non sono trasferibili.  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Checkpoint di database &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [Configurare l'opzione di configurazione del server intervallo di recupero](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md)   
 [SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md)  
  
  
