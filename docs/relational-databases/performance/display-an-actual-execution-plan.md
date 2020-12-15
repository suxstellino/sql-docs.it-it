---
title: Visualizzare un piano di esecuzione effettivo | Microsoft Docs
description: Informazioni su come generare piani di esecuzione grafici effettivi usando SQL Server Management Studio. Un piano di esecuzione grafico effettivo contiene informazioni di runtime.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- displaying execution plans
- actual execution plans
- viewing execution plans
- execution plans [SQL Server], displaying
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bd72f25590c821a08037a41382f2375c21431330
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97418142"
---
# <a name="display-an-actual-execution-plan"></a>Visualizzazione di un piano di esecuzione effettivo
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene descritto come generare piani di esecuzione grafici effettivi utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. I piani di esecuzione effettivi vengono generati dopo l'esecuzione di query [!INCLUDE[tsql](../../includes/tsql-md.md)] o batch. Un piano di esecuzione effettivo include quindi informazioni di runtime, ad esempio le metriche relative all'utilizzo effettivo delle risorse e gli avvisi sul runtime, se disponibili. Il piano di esecuzione generato visualizza il piano di esecuzione query effettivo usato da [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] per eseguire le query.  
  
 Per utilizzare questa funzionalità, gli utenti devono disporre delle autorizzazioni appropriate per eseguire le query [!INCLUDE[tsql](../../includes/tsql-md.md)] per le quali viene generato un piano di esecuzione grafico e dell'autorizzazione SHOWPLAN per tutti i database a cui fa riferimento la query.  
  
## <a name="to-include-an-execution-plan-for-a-query-during-execution"></a>Per includere un piano di esecuzione per una query durante l'esecuzione  
  
1.  Scegliere [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] Query del motore di database **nella barra degli strumenti**. Per aprire una query esistente e visualizzare il piano di esecuzione stimato, è inoltre possibile fare clic sul pulsante **Apri file** della barra degli strumenti e individuare la query. 
  
2.  Immettere la query per quale si desidera visualizzare il piano di esecuzione effettivo.  
  
3.  Scegliere **Includi piano di esecuzione effettivo** dal menu **Query** oppure fare clic sul pulsante della barra degli strumenti **Includi piano di esecuzione effettivo**.

    ![Pulsante del piano di esecuzione effettivo sulla barra degli strumenti](../../relational-databases/performance/media/actualexecplantoolbar.png "Pulsante del piano di esecuzione effettivo sulla barra degli strumenti")   
  
4.  Fare clic sul pulsante della barra degli strumenti **Esegui** per eseguire la query. Il piano usato da Query Optimizer viene visualizzato nella scheda **Piano di esecuzione** del riquadro dei risultati. 

    ![Piano di esecuzione effettivo](../../relational-databases/performance/media/actualexecplan.png "Piano di esecuzione effettivo")   

5.  Passare il puntatore del mouse sugli operatori logici e fisici per visualizzarne la descrizione e le proprietà nella descrizione comando visualizzata, incluse le proprietà del piano di esecuzione complessivo, selezionando l'operatore del nodo radice (il nodo SELECT nell'immagine precedente).   
  
    In alternativa, è possibile visualizzare le proprietà dell'operatore nella finestra Proprietà. Se tale finestra non è visibile, fare clic con il pulsante destro del mouse su un operatore e scegliere **Proprietà**. Selezionare un operatore e visualizzare le relative proprietà.  

    ![Fare clic con il pulsante destro del mouse su Proprietà nell'operatore del piano](../../relational-databases/performance/media/planproperties.png "Fare clic con il pulsante destro del mouse su Proprietà nell'operatore del piano")    
  
6.  È possibile modificare la visualizzazione del piano di esecuzione facendo clic con il pulsante destro del mouse sul piano di esecuzione e scegliendo **Zoom avanti**, **Zoom indietro**, **Personalizza zoom** oppure **Adatta alla finestra**. Le opzioni **Zoom avanti** e **Zoom indietro** consentono rispettivamente di ingrandire e rimpicciolire il piano di esecuzione, mentre **Personalizza zoom** consente di definire un fattore di zoom personalizzato, ad esempio 80 percento. **Adatta alla finestra** consente di ingrandire il piano di esecuzione per adattarlo al riquadro Risultati. In alternativa, usare una combinazione di tasto CTRL e rotellina del mouse per attivare lo **zoom dinamico**.  

7.  Per esplorare la visualizzazione del piano di esecuzione, usare le barre di scorrimento verticale e orizzontale oppure **fare clic e tenere premuto il pulsante del mouse su qualsiasi area vuota** del piano di esecuzione e **trascinare il mouse**. In alternativa, fare clic e tenere premuto il pulsante del mouse sul segno più (+) nell'angolo inferiore destro della finestra del piano di esecuzione per visualizzare una mappa in miniatura dell'intero piano di esecuzione.

> [!NOTE] 
> In alternativa, usare [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md) per restituire le informazioni del piano di esecuzione per ogni istruzione dopo l'esecuzione. Se usata in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], la scheda *Risultati* includerà un collegamento per l'apertura del piano di esecuzione in formato grafico.   
> Per altre informazioni, vedere [Infrastruttura di profilatura query](../../relational-databases/performance/query-profiling-infrastructure.md).
  
## <a name="see-also"></a>Vedere anche  
 [Piani di esecuzione](../../relational-databases/performance/execution-plans.md)    
 [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md)  
