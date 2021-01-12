---
description: sys.dm_db_xtp_merge_requests (Transact-SQL)
title: sys.dm_db_xtp_merge_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: c1224e88-af74-4c99-ae32-d5d2c552a1f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7a7f8f4a38bffb8abc6be1380a2225077130f143
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099857"
---
# <a name="sysdm_db_xtp_merge_requests-transact-sql"></a>sys.dm_db_xtp_merge_requests (Transact-SQL)

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Traccia le richieste di unione del database. È possibile che la richiesta merge sia stata generata da SQL Server o che la richiesta sia stata eseguita da un utente con [sys.sp_xtp_merge_checkpoint_files (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-merge-checkpoint-files-transact-sql.md).

> [!NOTE]
> Questa vista a gestione dinamica (DMV), sys.dm_db_xtp_merge_requests, esiste fino Microsoft SQL Server 2014.
> Tuttavia, a partire da SQL Server 2016 questa DMV non è più applicabile.

## <a name="columns-in-the-report"></a>Colonne del report

| Nome colonna | Tipo di dati | Descrizione |
| :-- | :-- | :-- |
| request_state | TINYINT | Stato della richiesta di unione:<br/>0 = Richiesta<br/>1 = In sospeso<br/>2 = installato<br/>3 = Abbandonata |
| request_state_desc | nvarchar(60) | Significati per lo stato corrente della richiesta:<br/><br/>Richiesto: esiste una richiesta di merge.<br/>In sospeso: l'Unione sta per essere elaborata.<br/>Installato: l'Unione è stata completata.<br/>Abbandonato: l'Unione non è stata completata, probabilmente a causa della mancanza di spazio di archiviazione. |
| destination_file_id | GUID | Identificatore univoco del file di destinazione per l'unione dei file di origine. |
| lower_bound_tsn | bigint | Timestamp minimo per il file di unione di destinazione. Il timestamp minimo per la transazione di tutti i file di origine da unire. |
| upper_bound_tsn | bigint | Timestamp massimo per il file di unione di destinazione. Il timestamp massimo per la transazione di tutti i file di origine da unire. |
| collection_tsn | bigint | Timestamp di raccolta della riga corrente.<br/><br/>Una riga nello stato Installata viene rimossa quando checkpoint_tsn è maggiore di collection_tsn.<br/><br/>Una riga nello stato Abbandonata viene rimossa quando checkpoint_tsn è minore di collection_tsn. |
| checkpoint_tsn | bigint | Ora di avvio del checkpoint.<br/><br/>Tutte le eliminazioni eseguite da transazioni con un timestamp minore di questo vengono incluse nel nuovo file di dati. Le eliminazioni rimanenti vengono spostate nel file differenziale di destinazione. |
| sourcenumber_file_id | GUID | Fino a 16 ID di file interni che identificano in modo univoco i file di origine nell'Unione. |

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione VIEW DATABASE STATE per il database corrente.

## <a name="see-also"></a>Vedere anche

[Viste a gestione dinamica correlate alle tabelle con ottimizzazione per la memoria (Transact-SQL)](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)
