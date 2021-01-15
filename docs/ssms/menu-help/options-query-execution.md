---
title: Pagina Opzioni di SQL Server - Esecuzione query
description: Opzioni (Esecuzione query)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Query_Execution.Sql_Server.General
dev_langs:
- TSQL
author: markingmyname
ms.author: maghan
ms.date: 01/13/2021
ms.openlocfilehash: 25ac37cdb9095151c90fdf81314d4c32044e6159
ms.sourcegitcommit: af64e2b8d498af26b973e86db5c00f8d72991295
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2021
ms.locfileid: "98193131"
---
# <a name="query-options-execution-general-page"></a>Opzioni query - Esecuzione (pagina Generale)

Usare questa pagina per specificare le opzioni per l'esecuzione di query di Microsoft SQL Server. Per accedere a questa finestra di dialogo, fare clic con il pulsante destro del mouse su una finestra dell'editor di query e fare clic su Opzioni query.

- **SET ROWCOUNT** Il valore predefinito 0 indica che SQL Server rimarrà in attesa di risultati fino a quando non verranno ricevuti tutti i risultati. Specificare un valore maggiore di 0 se si desidera che in SQL Server la query venga interrotta dopo aver ottenuto il numero di righe specificato. Per disabilitare questa opzione in modo che vengano restituite tutte le righe, specificare SET ROWCOUNT 0.

- **SET TEXTSIZE** Il valore predefinito 2.147.483.647 byte indica che in SQL Server sarà disponibile un campo dati completo fino al limite dei campi dati text, ntext, nvarchar(max) e varchar(max). Ciò non ha alcun effetto sul tipo di dati XML. Specificare un numero più piccolo per limitare i risultati in caso di valori elevati. Le colonne le cui dimensioni sono maggiori del numero specificato verranno troncate.

- **Timeout esecuzione** Indica il numero di secondi di attesa prima dell'annullamento della query. Il valore 0 indica un'attesa infinita, ovvero nessun timeout.

- **Separatore batch** Digitare una parola che verrà usatta per separare le istruzioni Transact-SQL in batch. Il valore predefinito è GO.

- **Per impostazione predefinita, apri le nuove query in modalità SQLCMD.** Selezionare questa casella di controllo per aprire le nuove query in modalità SQLCMD. Questa casella di controllo è disponibile solo se la finestra di dialogo è stata aperta tramite il menu Strumenti.

    Quando si seleziona questa opzione, tenere presente le limitazioni seguenti:

    - Nell'editor di query del motore di database, IntelliSense è disattivata.

    - Poiché l'editor di query non è in esecuzione dalla riga di comando, non è possibile passare parametri della riga di comando, ad esempio variabili.

    - Poiché l'editor di query non può rispondere ai prompt del sistema operativo, è necessario prestare attenzione a non eseguire istruzioni interattive.

- **Ripristina predefiniti** Reimposta le impostazioni predefinite originali per tutti i valori nella pagina.