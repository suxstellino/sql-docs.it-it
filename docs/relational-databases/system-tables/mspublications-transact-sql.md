---
description: MSpublications (Transact-SQL)
title: MSpublications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpublications
- MSpublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpublications system table
ms.assetid: 7a0b3457-7265-4f24-a255-7f055d908f20
author: cawrites
ms.author: chadam
ms.openlocfilehash: 089270c4c95b35e25f44339a7f001b5ac194ec01
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098177"
---
# <a name="mspublications-transact-sql"></a>MSpublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSpublications** contiene una riga per ogni pubblicazione replicata da un server di pubblicazione. Questa tabella è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|ID del server di pubblicazione.|  
|**publisher_db**|**sysname**|Nome del database del server di pubblicazione.|  
|**pubblicazione**|**sysname**|Nome della pubblicazione.|  
|**publication_id**|**int**|ID della pubblicazione.|  
|**publication_type**|**int**|Tipo di pubblicazione:<br /><br /> **0** = transazionale.<br /><br /> **1** = snapshot.<br /><br /> **2** = Unione.|  
|**thirdparty_flag**|**bit**|Indica se una pubblicazione è un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **1** = origine dati diversa da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**independent_agent**|**bit**|Indica se per questa pubblicazione è disponibile un agente di distribuzione autonomo.|  
|**immediate_sync**|**bit**|Indica se i file di sincronizzazione vengono creati o aggiornati a ogni esecuzione dell'agente snapshot.|  
|**allow_push**|**bit**|Indica se è possibile creare sottoscrizioni di tipo push per la pubblicazione specificata.|  
|**allow_pull**|**bit**|Indica se è possibile creare sottoscrizioni di tipo pull per la pubblicazione specificata.|  
|**allow_anonymous**|**bit**|Indica se è possibile creare sottoscrizioni anonime per la pubblicazione specificata.|  
|**description**|**nvarchar(255)**|Descrizione della pubblicazione.|  
|**vendor_name**|**nvarchar (100)**|Nome del produttore se il server di pubblicazione non è un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**conservazione**|**int**|Periodo di memorizzazione della pubblicazione in ore.|  
|**sync_method**|**int**|Metodo di sincronizzazione:<br /><br /> **0** = nativo (genera l'output della copia bulk in modalità nativa di tutte le tabelle).<br /><br /> **1** = character (genera un output per la copia bulk in modalità carattere per tutte le tabelle).<br /><br /> **3** = simultanea (genera l'output della copia bulk in modalità nativa di tutte le tabelle, ma non blocca la tabella durante lo snapshot).<br /><br /> **4** = concurrent_c (genera un output per la copia bulk in modalità carattere di tutte le tabelle, ma non blocca la tabella durante lo snapshot)<br /><br /> I valori **3** e **4** sono disponibili per la replica transazionale e per la replica di tipo merge, ma non per la replica snapshot.|  
|**allow_subscription_copy**|**bit**|Abilita o disabilita la funzione di copia dei database di sottoscrizione che sottoscrivono la pubblicazione. **0** indica che la copia è disabilitata e **1** indica che è abilitata.|  
|**thirdparty_options**|**int**|Specifica se la visualizzazione di una pubblicazione nella cartella replica in è stata annullata [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] :<br /><br /> **0** = Visualizza una pubblicazione eterogenea nella cartella replica in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .<br /><br /> **1** = non visualizza una pubblicazione eterogenea nella cartella replica in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .|  
|**allow_queued_tran**|**bit**|Specifica se la pubblicazione consente l'aggiornamento in coda:<br /><br /> **0 =** La pubblicazione non è in coda.<br /><br /> **1** = la pubblicazione è accodata.|  
|**options**|**int**|Informazioni non disponibili in questa versione.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
