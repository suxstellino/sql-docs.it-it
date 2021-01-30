---
description: MSSQLSERVER_2814
title: MSSQLSERVER_2814 | Microsoft Docs
ms.custom: ''
ms.date: 07/11/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2814 (Database Engine error)
ms.assetid: 22800748-9be9-4511-9428-6b8b40e5bef9
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6d64879e4ee4bc54edc566c2109ecdd3e28598f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198202"
---
# <a name="mssqlserver_2814"></a>MSSQLSERVER_2814
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|2814|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|PR_POSSIBLE_INFINITE_RECOMPILE|  
|Testo del messaggio|Rilevata una possibile ricompilazione infinita per SQLHANDLE %hs, PlanHandle %hs, offset iniziale %d, offset finale %d. Motivo dell'ultima ricompilazione: % d.|  
  
## <a name="explanation"></a>Spiegazione  
Una o più istruzioni ha determinato la ricompilazione del batch di query almeno 50 volte. Per evitare ulteriori ricompilazioni, è necessario correggere l'istruzione specificata.  
  
I motivi della ricompilazione sono elencati nella tabella seguente.  
  
|Codice motivo|Descrizione|  
|---------------|---------------|  
|1|Schema modificato|  
|2|Statistiche modificate|  
|3|Compilazione posticipata|  
|4|Opzione impostata modificata|  
|5|Tabella temporanea modificata|  
|6|Set di righe remoto modificato|  
|7|Autorizzazioni FOR BROWSE modificate|  
|8|Ambiente di notifica query modificato|  
|9|Vista partizionata modificata|  
|10|Opzioni cursore modificate|  
|11|Opzione (recompile) richiesta|  
  
## <a name="user-action"></a>Azione dell'utente  
  
1.  Visualizzare l'istruzione che determina la ricompilazione mediante l'esecuzione della query seguente. Sostituire i segnaposto *sql_handle*, *starting_offset*, *ending_offset* e *plan_handle* con i valori specificati nel messaggio di errore. Le colonne **database_name** e **object_name** sono NULL per le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc e preparate.  
  
    ```sql   
    SELECT DB_NAME(st.dbid) AS database_name,  
        OBJECT_NAME(st.objectid) AS object_name,  
        st.text  
    FROM sys.dm_exec_query_stats AS qs  
    CROSS APPLY sys.dm_exec_sql_text (*sql_handle*) AS st  
    WHERE qs.statement_start_offset = *starting_offset*  
    AND qs.statement_end_offset = *ending_offset*  
    AND qs.plan_handle = *plan_handle*;
    ```
  
2.  In base alla descrizione del codice motivo, modificare l'istruzione, il batch o la procedura per evitare ricompilazioni. Una stored procedure può contenere, ad esempio, uno o più istruzioni SET. Queste istruzioni devono essere rimosse dalla procedura. Per esempi aggiuntivi sulle cause e sulle risoluzioni dei problemi di ricompilazione, vedere [Problematiche di compilazione batch, ricompilazione e memorizzazione dei piani nella cache in SQL Server 2005](/previous-versions/sql/sql-server-2005/administrator/cc966425(v=technet.10)).  
  
3.  Se il problema persiste, contattare il Servizio Supporto Tecnico Clienti Microsoft.  
  
## <a name="see-also"></a>Vedere anche  
[Classe di evento SQL:StmtRecompile](../event-classes/sql-stmtrecompile-event-class.md)  
  
