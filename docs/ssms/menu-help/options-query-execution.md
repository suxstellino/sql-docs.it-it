---
title: Pagina delle opzioni di SSMS-esecuzione delle query
description: Opzioni di SSMS (esecuzione di query)
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
ms.openlocfilehash: 860011bf1ad6ad28a8dac5a1b5f86dd8a01e711a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069856"
---
# <a name="options-query-execution---general"></a>Opzioni (esecuzione query-generale)

Usare questa pagina per specificare le opzioni per l'esecuzione di query di Microsoft SQL Server. Per accedere a questa finestra di dialogo, fare clic con il pulsante destro del mouse sul corpo di una finestra dell'editor di query e quindi scegliere **Opzioni query** oppure passare a **strumenti > opzioni > esecuzione query** dalla barra dei menu superiore.

- **SET ROWCOUNT** Il valore predefinito 0 indica che SQL Server rimarrà in attesa di risultati fino a quando non verranno ricevuti tutti i risultati. Specificare un valore maggiore di 0 se si desidera che in SQL Server la query venga interrotta dopo aver ottenuto il numero di righe specificato. Per disattivare questa opzione, in modo che vengano restituite tutte le righe, specificare SET ROWCOUNT 0.

- **SET TEXTSIZE** Il valore predefinito 2.147.483.647 byte indica che in SQL Server sarà disponibile un campo dati completo fino al limite dei campi dati text, ntext, nvarchar(max) e varchar(max). Non influisce sul tipo di dati XML. Specificare un numero più piccolo per limitare i risultati di valori di grandi dimensioni. Le colonne le cui dimensioni sono maggiori del numero specificato verranno troncate.

- **Timeout esecuzione** Indica il numero di secondi di attesa prima dell'annullamento della query. Il valore 0 indica un'attesa infinita, ovvero nessun timeout.

- **Separatore batch** Digitare una parola che verrà usatta per separare le istruzioni Transact-SQL in batch. Il valore predefinito è GO.

- **Per impostazione predefinita, apri le nuove query in modalità SQLCMD.** Selezionare questa casella di controllo per aprire le nuove query in modalità SQLCMD. Questa casella di controllo è disponibile solo se la finestra di dialogo è stata aperta tramite il menu Strumenti.

    Quando si seleziona questa opzione, tenere presente le limitazioni seguenti:

    - Nell'editor di query del motore di database, IntelliSense è disattivata.

    - Poiché l'editor di query non viene eseguito dalla riga di comando, non è possibile passare parametri della riga di comando, ad esempio variabili.

    - Poiché l'editor di query non è in grado di rispondere ai prompt del sistema operativo, è necessario prestare attenzione a non eseguire istruzioni interattive.

- **Ripristina predefiniti** Reimposta le impostazioni predefinite originali per tutti i valori nella pagina.