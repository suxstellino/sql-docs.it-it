---
description: MSmerge_conflicts_info (Transact-SQL)
title: MSmerge_conflicts_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_conflicts_info_TSQL
- MSmerge_conflicts_info
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_conflicts_info system table
ms.assetid: 6b76ae96-737a-4000-a6b6-fcc8772c2af4
author: cawrites
ms.author: chadam
ms.openlocfilehash: da7de7b1c8c18a74fc5a255027507853d195813b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091480"
---
# <a name="msmerge_conflicts_info-transact-sql"></a>MSmerge_conflicts_info (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSmerge_conflicts_info** tiene traccia dei conflitti che si verificano durante la sincronizzazione di una sottoscrizione di una pubblicazione di tipo merge. I dati della riga che perdono i conflitti vengono archiviati nella tabella [MSmerge_conflict_publication_article](../../relational-databases/system-tables/msmerge-conflict-publication-article-transact-sql.md) per l'articolo in cui si è verificato il conflitto. Questa tabella è archiviata nel database di pubblicazione del server di pubblicazione e nel database di sottoscrizione del Sottoscrittore.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|Nome alternativo della tabella pubblicata.|  
|**rowguid**|**uniqueidentifier**|Identificatore della riga in conflitto.|  
|**origin_datasource**|**nvarchar(255)**|Nome del database in cui ha avuto origine la modifica in conflitto.|  
|**conflict_type**|**int**|Tipo di conflitto che si è verificato. I possibili valori sono i seguenti.<br /><br /> **1** = conflitto di aggiornamento: il conflitto viene rilevato a livello di riga.<br /><br /> **2** = conflitto di aggiornamento della colonna: conflitto rilevato a livello di colonna.<br /><br /> **3** = conflitto di aggiornamento di eliminazione WINS: il conflitto viene eliminato.<br /><br /> **4** = conflitto di eliminazione di aggiornamento WINS: il ROWGUID eliminato che perde il conflitto viene registrato in questa tabella.<br /><br /> **5** = inserimento di caricamento non riuscito: Impossibile applicare l'inserimento dal Sottoscrittore al server di pubblicazione.<br /><br /> **6** = inserimento di download non riuscito: Impossibile applicare l'inserimento dal server di pubblicazione nel Sottoscrittore.<br /><br /> **7** = eliminazione del caricamento non riuscita: non è stato possibile caricare l'eliminazione nel Sottoscrittore nel server di pubblicazione.<br /><br /> **8** = eliminazione del download non riuscita: non è stato possibile scaricare l'eliminazione nel server di pubblicazione nel Sottoscrittore.<br /><br /> **9** = aggiornamento di caricamento non riuscito: non è stato possibile applicare l'aggiornamento nel Sottoscrittore al server di pubblicazione.<br /><br /> **10** = aggiornamento di download non riuscito: non è stato possibile applicare l'aggiornamento nel server di pubblicazione al Sottoscrittore.<br /><br /> **11** = risoluzione<br /><br /> **12** = eliminazione WINS aggiornamento record logico: il record logico eliminato che perde il conflitto viene registrato in questa tabella.<br /><br /> **13** = conflitto di inserimento dei record logici: l'inserimento in un record logico è in conflitto con un aggiornamento.<br /><br /> **14** = conflitto di aggiornamento WINS di eliminazione di record logici: il record logico aggiornato che perde il conflitto viene registrato in questa tabella.|  
|**reason_code**|**int**|Codice di errore che può essere sensibile al contesto. Nel caso dei conflitti Update-Update e Update-Delete, il valore usato per questa colonna è identico a quello del **conflict_type**. Per i conflitti di modifica non riuscita, tuttavia, il codice motivo è l'errore che ha impedito all'agente di merge l'applicazione della modifica. Se, ad esempio, l'agente di merge non può applicare un inserimento nel Sottoscrittore a causa di una violazione della chiave primaria, registrerà un **conflict_type** di 6 ("inserimento di download non riuscito") e un **reason_code** di 2627, ovvero il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] messaggio di errore interno per una violazione della chiave primaria: "violazione del vincolo% ls '%. * ls '. Impossibile inserire la chiave duplicata nell'oggetto '%. \* ls "."|  
|**reason_text**|**nvarchar (720)**|Descrizione dell'errore che può essere sensibile al contesto.|  
|**pubid**|**uniqueidentifier**|Identificatore della pubblicazione.|  
|**MSrepl_create_time**|**datetime**|Ora in cui si è verificato il conflitto.|  
|**origin_datasource_id**|**uniqueidentifier**|Identificatore del database in cui ha avuto origine la modifica in conflitto.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
