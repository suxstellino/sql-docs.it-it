---
title: Analizzare le prestazioni degli script
description: Informazioni su come le statistiche dei client, i piani di esecuzione e l'analisi del codice in SQL Server Data Tools possono contribuire a migliorare le prestazioni di query, stored procedure e script.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f4bbdd31-12a5-4c57-b0fe-1c6683820f11
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5f8d17fa156cd144a87b2dc387564cd93556d8da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077192"
---
# <a name="analyze-script-performance"></a>Analizzare le prestazioni degli script

Gli strumenti forniti da SQL Server Data Tools consentono di determinare se è possibile migliorare le prestazioni di query, stored procedure o script. Il monitoraggio delle statistiche client quali i tempi di risposta delle query più frequenti consente, ad esempio, di determinare se sono necessarie modifiche alle query o agli indici nelle tabelle. Tali statistiche possono includere l'ora di esecuzione del client, il profilo delle query e i pacchetti/byte inviati e ricevuti.  
  
Inoltre, è consigliabile risolvere determinati problemi di prestazioni analizzando le query dell'applicazione e gli aggiornamenti inoltrati al database dall'applicazione, oltre al modo in cui tali query e aggiornamenti interagiscono con i dati inclusi nel database e nello schema del database. I piani di esecuzione consentono di visualizzare graficamente i metodi di recupero dei dati scelti da Query Optimizer di SQL Server e di mostrare i costi di esecuzione di istruzioni e query specifiche. Pertanto, aiutano a comprendere la modalità con cui una query SQL verrà elaborata da SQL Server e di determinare la causa di eventuali riduzioni delle prestazioni.  
  
## <a name="using-client-statistics"></a>Utilizzo delle statistiche client  
Quando si esegue uno script o una query nell'Editor Transact\-SQL, si può scegliere di raccogliere statistiche client, ad esempio statistiche temporali, della rete e del profilo dell'applicazione per l'esecuzione. Tale metrica consente di misurare l'efficienza dello script o di effettuare un benchmark di script differenti.  
  
Per attivare o disattivare la raccolta di statistiche client, quando l'Editor Transact\-SQL è aperto, nel menu **Dati** scegliere **Editor Transact\-SQL**, fare clic su **Impostazioni di esecuzione**, quindi su **Includi statistiche client**. In alternativa, fare clic sul pulsante **Includi statistiche client** (il quinto da destra) nella barra degli strumenti dell'Editor Transact\-SQL o fare clic con il pulsante destro del mouse sull'Editor Transact\-SQL e selezionare **Impostazioni di esecuzione** e **Includi statistiche client**. Si noti che per raggruppare le statistiche per una query, è necessario abilitare questa funzionalità prima di eseguirla.  
  
Se le statistiche client sono state abilitate, la scheda **Statistiche** viene visualizzata accanto alla scheda **Messaggio** durante l'esecuzione della query. Se sono state invece disabilitate, la scheda **Statistiche** non compare. Le statistiche delle successive esecuzioni delle query vengono elencate insieme ai valori medi.  
  
Per altre informazioni sulle statistiche raccolte, vedere [Riquadro statistiche nella finestra di query](https://msdn.microsoft.com/library/aa216969(SQL.80).aspx) e la sezione ["Scheda Statistiche client" di questo argomento](/previous-versions/visualstudio/visual-studio-2010/aa833205(v=vs.100)).  
  
## <a name="using-execution-plans"></a>Utilizzo dei piani di esecuzione  
I piani di esecuzione consentono di visualizzare la modalità di navigazione delle tabelle e di utilizzo degli indici da parte del motore di database per l'accesso o l'elaborazione dei dati di una query o di un'altra istruzione DML, ad esempio un aggiornamento. Questo approccio grafico è particolarmente utile per la comprensione delle caratteristiche relative alle prestazioni di una query.  
  
Aprire uno script Transact\-SQL contenente le query da analizzare nell'editor Transact\-SQL. È possibile evidenziare il codice che si vuole rivedere e scegliere di visualizzare un piano di esecuzione stimato facendo clic sul pulsante **Visualizza piano di esecuzione stimato** nella barra degli strumenti dell'editor. Se si fa clic su **Visualizza piano di esecuzione stimato**, le query o i batch Transact\-SQL non vengono eseguiti. Viene invece analizzato lo script e viene visualizzato il piano di esecuzione query che verrebbe utilizzato con maggiore probabilità dal motore di database in caso di effettiva esecuzione delle query.  
  
Dopo l'analisi o l'esecuzione dello script, fare clic sulla scheda **Piano di esecuzione** per visualizzare una rappresentazione grafica dell'output del piano di esecuzione.  
  
L'output del piano di esecuzione grafico viene letto da destra a sinistra e dall'alto verso il basso. Vengono visualizzate tutte le query del batch analizzato, con il costo di ogni query espresso come percentuale del costo totale del batch. Per visualizzare informazioni aggiuntive, come costi e operazioni per ogni singolo passaggio, passare il mouse sulle [icone degli operatori logici e fisici](/previous-versions/sql/sql-server-2008-r2/ms175913(v=sql.105)) nel piano grafico.  
  
Per modificare la visualizzazione del piano di esecuzione, fare clic con il pulsante destro del mouse sul **piano di esecuzione** e scegliere **Zoom avanti**, **Zoom indietro**, **Personalizza zoom** o **Adatta alla finestra**. **Zoom avanti** e **Zoom indietro** consentono rispettivamente di ingrandire o ridurre il piano di esecuzione in base a valori di percentuale predefiniti. **Personalizza zoom** consente di definire un ingrandimento personalizzato per la visualizzazione, ad esempio 80 percento.  **Adatta alla finestra** consente di regolare il piano di esecuzione per adattarlo al riquadro Risultati.  
  
I piani di esecuzione possono essere salvati e riaperti in un secondo momento per effettuare una verifica. A questo scopo, fare clic con il pulsante destro del mouse su **Piano di esecuzione** e selezionare **Salva piano di esecuzione con nome**. Successivamente, è possibile aprire il piano in Visual Studio come qualsiasi altro tipo di file.  
  
## <a name="using-code-analysis"></a>Utilizzo dell'analisi codice  
È possibile utilizzare l'analisi codice per individuare potenziali problemi negli script, ad esempio problemi di progettazione, denominazione e di prestazioni.  Le regole per i progetti di database sono organizzate in set di regole predefiniti destinati ad aree specifiche. Ogni singola regola può essere abilitata o disabilitata nella scheda **Analisi codice** della pagina delle proprietà **Proprietà progetto** . Nella stessa scheda è possibile specificare che l'analisi codice venga eseguita automaticamente ogni volta che viene compilato un progetto o nel caso in cui gli avvisi vengano considerati come errori.  
  
Per usare l'analisi codice manualmente, fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Esegui analisi del codice**. Nella finestra **Elenco errori** vengono visualizzati gli avvisi dell'analisi codice. È possibile fare doppio clic su un avviso per passare al codice di origine contenente il problema. Inoltre, si possono visualizzare informazioni aggiuntive e le possibili correzioni per un avviso usando il menu contestuale **Mostra guida errore**.  
  
Per altre informazioni sull'analisi codice, vedere [Analisi del codice di database per migliorare la qualità del codice](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  
