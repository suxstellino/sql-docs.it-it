---
description: MSSQL_ENG021286
title: MSSQL_ENG021286 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG021286 error
ms.assetid: b63620b7-1c6d-46f7-90ea-3a8e99af8de4
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 59066e248bd059376ec26e32d8541a3427366562
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460214"
---
# <a name="mssql_eng021286"></a>MSSQL_ENG021286
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|21286|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|La tabella dei conflitti '%s' non esiste.|  
  
## <a name="explanation"></a>Spiegazione  
 Questo errore si verifica se la tabella dei conflitti relativa a un articolo elencato in [sysmergearticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysmergearticles-transact-sql.md) in realtà non esiste. Questo errore può verificarsi quando si tenta di aggiungere o di eliminare una colonna da una tabella pubblicata per la replica di tipo merge.  
  
## <a name="user-action"></a>Azione dell'utente  
 Eseguire [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sul database con la tabella dei conflitti mancante per verificare che non siano presenti problemi di coerenza dei dati.  
  
 Se in un Sottoscrittore manca la tabella dei conflitti, eliminare la sottoscrizione e ricrearla. Se in un server di pubblicazione manca la tabella dei conflitti, eliminare tutte le sottoscrizioni, eliminare la pubblicazione e quindi ricreare la pubblicazione e tutte le sottoscrizioni. Per altre informazioni, vedere [Pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md) e [Sottoscrivere le pubblicazioni](../../relational-databases/replication/subscribe-to-publications.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
