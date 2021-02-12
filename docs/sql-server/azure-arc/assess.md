---
title: Configurare Valutazione SQL su richiesta in un'istanza di SQL Server con abilitazione di Azure Arc
description: Configurare Valutazione SQL su richiesta in un'istanza di SQL Server con abilitazione di Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 2fb25ca30a3b55bfcedb470addad8680f4914e2f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063293"
---
# <a name="configure-sql-assessment-on-an-azure-arc-enabled-sql-server-instance"></a>Configurare Valutazione SQL in un'istanza di SQL Server con abilitazione di Azure Arc

Valutazione SQL offre un meccanismo per valutare la configurazione di SQL Server. Questo articolo include istruzioni per l'uso di Valutazione SQL in un'istanza di SQL Server abilitata per Azure Arc.

## <a name="prerequisites"></a>Prerequisiti

* L'istanza di SQL Server deve essere connessa ad Azure Arc. Per istruzioni, vedere l'articolo [Connettere SQL Server ad Azure Arc](connect.md).

* L'estensione Microsoft Monitoring Agent (MMA) deve essere installata e configurata nel computer. Per istruzioni, vedere l'articolo [Installare MMA](configure-advanced-data-security.md#install-microsoft-monitoring-agent-mma). È possibile ottenere ulteriori informazioni anche nell'articolo [Agente di Log Analytics](/azure/azure-monitor/platform/log-analytics-agent).

* Per l'istanza di SQL Server deve essere [abilitato il protocollo TCP/IP](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).

* Se si usa un'istanza denominata di SQL Server, deve essere in esecuzione il [servizio SQL Server Browser](../../tools/configuration-manager/sql-server-browser-service.md).

* Verificare di aver esaminato il documento relativo a SQL Server nei [prerequisiti per le valutazioni su richiesta dell'hub Servizi](/services-hub/health/assessment-prereq-docs#on-demand-assessment-prerequisite-documents).

## <a name="run-on-demand-sql-assessment"></a>Eseguire Valutazione SQL su richiesta

1. Aprire la risorsa SQL Server - Azure Arc e selezionare **Integrità ambiente** nel riquadro a sinistra.

   > [!div class="mx-imgBorder"]
   > [ ![Screenshot che visualizza la schermata Integrità ambiente di una risorsa SQL Server - Azure Arc.](media/assess/sql-assessment-heading-sql-server-arc.png) ](media/assess/sql-assessment-heading-sql-server-arc.png#lightbox)

> [!IMPORTANT]
> Se l'estensione MMA non è installata, non sarà possibile avviare la Valutazione SQL su richiesta.

2. Selezionare il tipo di account. Se è disponibile un account del servizio gestito, sarà possibile avviare Valutazione SQL direttamente dal portale. Specificare il nome dell'account.

> [!NOTE]
> Se si specifica un *account del servizio gestito* verrà attivato il pulsante **Configure SQL Assessment** (Configura Valutazione SQL) per avviare la valutazione dal portale distribuendo una *CustomScriptExtension*. Poiché è possibile distribuire una sola *CustomScriptExtension* alla volta, l'estensione dello script per Valutazione SQL verrà automaticamente rimossa dopo l'esecuzione. Se nel computer di hosting è già stata distribuita una *CustomScriptExtension*, il pulsante **Configure SQL Assessment** (Configura Valutazione SQL) non verrà attivato.

3. Se si vuole modificare l'impostazione predefinita, specificare una directory di lavoro nel computer della raccolta dati. Per impostazione predefinita si usa `C:\sql_assessment\work_dir`. Durante la raccolta e l'analisi, i dati vengono archiviati temporaneamente in tale cartella. Se la cartella non esiste, viene creata automaticamente.

4. Se si avvia Valutazione SQL dal portale facendo clic su **Configure SQL Assessment** (Configura Valutazione SQL), verrà visualizzata una notifica di distribuzione standard.

> [!div class="mx-imgBorder"]
   > [ ![Screenshot che mostra la distribuzione di CustomScriptExtension.](media/assess/sql-assessment-custom-script-deployment.png) ](media/assess/sql-assessment-custom-script-deployment.png#lightbox)

5. Se si preferisce avviare Valutazione SQL dal computer di destinazione, fare clic su **Scarica script di configurazione del dispositivo**, copiare lo script scaricato nel computer di destinazione ed eseguire uno dei blocchi di codice seguenti in un'istanza di amministrazione di **powershell.exe**:

   * _Account di dominio_:  verrà richiesto di specificare account utente e password.

      ```powershell
      Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
      & '.\AddSqlAssessment.ps1'
      ```

   * _Account del servizio gestito (MSA)_

      ```powershell
      Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
      & '.\AddSqlAssessment.ps1' -ManagedServiceAccountName <MSA account name>
      ```

> [!NOTE]
> Lo script pianifica un'attività denominata *SQLassessment* che attiva la raccolta dei dati. Questa attività viene eseguita entro un'ora dall'esecuzione dello script. Quindi si ripete ogni sette giorni.

> [!TIP]
> È possibile modificare l'attività per eseguirla in una data e ora diverse oppure per forzarne l'esecuzione immediata. Nella libreria dell'utilità di pianificazione trovare **Microsoft** > **Operations Management Suite** > **AOI\*\*\***  > **Assessments (Valutazioni)**  > **SQLAssessment**.

## <a name="view-sql-assessment-results"></a>Visualizzare i risultati di Valutazione SQL

* Nel riquadro _Integrità ambiente_ selezionare il pulsante **View SQL Assessment results** (Visualizza risultati di Valutazione SQL).

   > [!NOTE]
   > Il pulsante **View SQL Assessment results** (Visualizza risultati di Valutazione SQL) rimane disattivato fino a quando i risultati non sono pronti in Log Analytics. Questo processo può richiedere fino a due ore dopo il completamento dell'elaborazione dei file di dati nel computer di destinazione.

   > [!div class="mx-imgBorder"]
   > [ ![Screenshot che visualizza i risultati di Valutazione SQL.](media/assess/sql-assessment-results.png) ](media/assess/sql-assessment-results.png#lightbox)

* È possibile visualizzare lo stato di elaborazione dei dati nel computer della raccolta controllando i file nella cartella di lavoro. Al termine dell'attività pianificata, verranno visualizzati diversi file con il prefisso _new._ nella directory di lavoro.

   > [!div class="mx-imgBorder"]
   > [ ![Screenshot con una finestra di File Manager che visualizza i nuovi file di dati nella cartella di lavoro.](media/assess/sql-assessment-data-files-ready.png) ](media/assess/sql-assessment-data-files-ready.png#lightbox)

* Microsoft Monitoring Agent analizza la cartella di lavoro ogni 15 minuti. Cerca i file _new.*_ e invia i dati all'area di lavoro Log Analytics. Dopo aver caricato il file, MMA modifica il prefisso da _new._ a _processed._

   > [!div class="mx-imgBorder"]
   > ![Screenshot con una finestra di File Manager che visualizza i file di dati elaborati.](media/assess/sql-assessment-data-files-processed.png)

## <a name="next-steps"></a>Passaggi successivi

* Per altre informazioni, vedere i documenti in [Prerequisiti delle valutazioni on demand dell'Hub Servizi](/services-hub/health/assessment-prereq-docs#on-demand-assessment-prerequisite-documents).

* Per ottenere il supporto completo della funzionalità Valutazione SQL on demand è necessaria la sottoscrizione a un contratto di supporto Premier o Unified. Per informazioni dettagliate, vedere [Supporto tecnico Premier di Microsoft Azure](https://azure.microsoft.com/support/plans/premier).
