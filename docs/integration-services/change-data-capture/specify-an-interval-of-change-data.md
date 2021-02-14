---
description: Definizione di un intervallo dei dati delle modifiche
title: Definire un intervallo dei dati delle modifiche | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],specifying interval
ms.assetid: 17899078-8ba3-4f40-8769-e9837dc3ec60
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6e3d2ef011a95d2024479656fe083629890186b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338471"
---
# <a name="specify-an-interval-of-change-data"></a>Definizione di un intervallo dei dati delle modifiche

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Nel flusso di controllo di un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che esegue un carico incrementale dei dati delle modifiche, la prima attività consiste nel calcolare gli endpoint dell'intervallo di modifiche. Tali endpoint sono valori **datetime** e verranno archiviati in variabili del pacchetto per l'uso successivo nel pacchetto.  
  
> [!NOTE]  
>  Per una descrizione del processo complessivo di progettazione del flusso di controllo, vedere [Change Data Capture &#40;SSIS&#41;](../../integration-services/change-data-capture/change-data-capture-ssis.md).  
  
## <a name="set-up-package-variables-for-the-endpoints"></a>Configurare le variabili del pacchetto per gli endpoint  
 Prima di configurare l'attività Esegui SQL per il calcolo degli endpoint, è necessario definire le variabili del pacchetto in cui verranno archiviati gli endpoint.  
  
#### <a name="to-set-up-package-variables"></a>Per configurare le variabili del pacchetto  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire un nuovo progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  Nella finestra **Variabili** creare le variabili seguenti:  
  
    1.  Creare una variabile con il tipo di dati **datetime** per memorizzare il punto iniziale per l'intervallo.  
  
         In questo esempio viene utilizzato il nome di variabile ExtractStartTime.  
  
    2.  Creare un'altra variabile con il tipo di dati **datetime** per memorizzare il punto finale per l'intervallo.  
  
         In questo esempio viene utilizzato il nome di variabile ExtractEndTime.  
  
 Se si calcolano gli endpoint in un pacchetto master che esegue più pacchetti figlio, è possibile utilizzare le configurazioni Variabile pacchetto padre per passare i valori di tali variabili a ciascun pacchetto figlio. Per altre informazioni, vedere [Attività Esegui pacchetto](../../integration-services/control-flow/execute-package-task.md) e [Utilizzare i valori di variabili e parametri in un pacchetto figlio](../../integration-services/packages/legacy-package-deployment-ssis.md#child).  
  
## <a name="calculate-a-starting-point-and-an-ending-point-for-change-data"></a>Calcolare un punto iniziale e un punto finale per i dati delle modifiche  
 Dopo avere configurato le variabili del pacchetto per gli endpoint dell'intervallo, è possibile calcolare i valori effettivi per tali endpoint ed eseguirne il mapping alle variabili del pacchetto corrispondenti. Poiché gli endpoint sono valori **datetime** , sarà necessario usare funzioni in grado di calcolare o usare valori **datetime** . Sia il linguaggio delle espressioni [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sia Transact-SQL includono funzioni che usano valori **datetime** :  
  
 Funzioni nel linguaggio delle espressioni [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che usano valori **datetime**  
 -   [DATEADD &#40;espressione SSIS&#41;](../../integration-services/expressions/dateadd-ssis-expression.md)  
  
-   [DATEDIFF &#40;espressione SSIS&#41;](../../integration-services/expressions/datediff-ssis-expression.md)  
  
-   [DATEPART &#40;espressione SSIS&#41;](../../integration-services/expressions/datepart-ssis-expression.md)  
  
-   [DAY &#40;espressione SSIS&#41;](../../integration-services/expressions/day-ssis-expression.md)  
  
-   [GETDATE &#40;espressione SSIS&#41;](../../integration-services/expressions/getdate-ssis-expression.md)  
  
-   [GETUTCDATE &#40;espressione SSIS&#41;](../../integration-services/expressions/getutcdate-ssis-expression.md)  
  
-   [MONTH &#40;espressione SSIS&#41;](../../integration-services/expressions/month-ssis-expression.md)  
  
-   [YEAR &#40;espressione SSIS&#41;](../../integration-services/expressions/year-ssis-expression.md)  
  
 Funzioni in Transact-SQL che usano valori **datetime**  
 [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 Prima di usare una di queste funzioni **datetime** per calcolare gli endpoint, è necessario determinare se l'intervallo è fisso e si verifica regolarmente. In genere, le modifiche verificatesi nelle tabelle di origine vengono applicate alle tabelle di destinazione a intervalli regolari. Potrebbe essere necessario, ad esempio, applicare le modifiche su base oraria, giornaliera o settimanale.  
  
 Dopo avere determinato se l'intervallo di modifiche è fisso o più casuale, è possibile calcolare gli endpoint:  
  
-   **Calcolo della data e dell'ora di inizio**. Utilizzare la data e l'ora di fine del caricamento precedente come data e ora di inizio correnti. Se si usa un intervallo fisso per i caricamenti incrementali, è possibile calcolare questo valore usando le funzioni **datetime** di Transact-SQL o del linguaggio delle espressioni [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . In caso contrario, potrebbe essere necessario impostare la persistenza degli endpoint tra le esecuzioni e utilizzare un'attività Esegui SQL o un'attività Script per caricare l'endpoint precedente.  
  
-   **Calcolo della data e dell'ora di fine**. Se si utilizza un intervallo fisso per i carichi incrementali, calcolare la data e l'ora di fine correnti come offset della data e dell'ora di inizio. Anche in questo caso, è possibile calcolare questo valore usando le funzioni **datetime** di Transact-SQL o del linguaggio delle espressioni [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Nella procedura seguente l'intervallo di modifiche è un intervallo fisso e si presuppone che il pacchetto del caricamento incrementale venga eseguito ogni giorno senza eccezione. In caso contrario, i dati delle modifiche per gli intervalli in cui non viene eseguito il caricamento andranno perduti. Il punto iniziale per l'intervallo è costituito dalla mezzanotte del giorno precedente a ieri, ovvero tra le 24 e le 48 ore precedenti. Il punto finale per l'intervallo è costituito dalla mezzanotte di ieri, ovvero la notte precedente, tra le 0 e le 24 ore precedenti.  
  
#### <a name="to-calculate-the-starting-point-and-ending-point-for-the-capture-interval"></a>Per calcolare il punto iniziale e il punto finale per l'intervallo di acquisizione  
  
1.  Nella scheda **Flusso di controllo** di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] aggiungere un'attività Esegui SQL al pacchetto.  
  
2.  Nella pagina **Generale** in **Editor attività Esegui SQL** selezionare le opzioni seguenti:  
  
    1.  Per **ResultSet**, selezionare **Riga singola**.  
  
    2.  Configurare una connessione valida al database di origine.  
  
    3.  Per **SQLSourceType**, selezionare **Input diretto**.  
  
    4.  Per **SQLStatement** immettere l'istruzione SQL seguente:  
  
        ```sql
        SELECT DATEADD(dd,0, DATEDIFF(dd,0,GETDATE()-1)) AS ExtractStartTime,  
          DATEADD(dd,0, DATEDIFF(dd,0,GETDATE())) AS ExtractEndTime  
  
        ```  
  
3.  Nella pagina **Set dei risultati** di **Editor attività Esegui SQL** eseguire il mapping tra il risultato di ExtractStartTime e la variabile del pacchetto ExtractStartTime e tra il risultato di ExtractEndTime e la variabile del pacchetto ExtractEndTime.  
  
    > [!NOTE]  
    >  Quando si usa un'espressione per impostare il valore di una variabile in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], l'espressione viene valutata ogni volta che si accede al valore della variabile.  
  
## <a name="next-step"></a>passaggio successivo  
 Dopo avere calcolato il punto iniziale e il punto finale per un intervallo di modifiche, il passaggio successivo consiste nel determinare se i dati delle modifiche sono pronti.  
  
 **Argomento successivo:** [Come determinare se i dati delle modifiche sono pronti](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo di variabili nei pacchetti](../integration-services-ssis-variables.md)   
 [Espressioni di Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md)   
 [Attività Esegui SQL](../../integration-services/control-flow/execute-sql-task.md)   
 [Attività Script](../../integration-services/control-flow/script-task.md)  
  
