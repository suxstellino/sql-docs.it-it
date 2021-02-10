---
description: Generazione di report (AccessToSQL)
title: Generazione di report (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: abb4264a-622e-4215-af5b-14e309b8a399
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1e80761c63aec6461af497af696a059ab7b86f60
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044539"
---
# <a name="generating-reports-accesstosql"></a>Generazione di report (AccessToSQL)
I report di determinate attività eseguite usando i comandi vengono generati nella console di SSMA a livello di albero degli oggetti.  
  
Utilizzare la procedura seguente per generare report:  
  
1.  Specificare il parametro **Write-Summary-Report-to** . Il report correlato viene archiviato come nome file (se specificato) o nella cartella specificata. Il nome del file è predefinito dal sistema come indicato nella tabella seguente, dove **&lt; n &gt;** è il numero di file univoco che incrementa con una cifra a ogni esecuzione dello stesso comando.  
  
    I report Vis-à-Vis sono i seguenti:  
  
    |SL. No.|Comando|Titolo del report|  
    |-|-|-|  
    |1|generate-assessment-report|AssessmentReport &lt; n &gt; . XML|  
    |2|Convert-schema|SchemaConversionReport &lt; n &gt; . XML|  
    |3|migrate-dati|DataMigrationReport &lt; n &gt; . XML|  
    |4|sincronizzazione-destinazione|TargetSynchronizationReport &lt; n &gt; . XML|  
    |5|aggiornamento da database|SourceDBRefreshReport &lt; n &gt; . XML|  
  
    > [!IMPORTANT]  
    > Un report di output è diverso dal rapporto di valutazione. Il primo è un report sulle prestazioni di un comando eseguito mentre, quest'ultimo è un report XML per l'utilizzo a livello di codice.  
  
    Per le opzioni di comando per i report di output (da SL. No. 2-4 sopra), fare riferimento alla sezione [esecuzione della console SSMA &#40;AccessToSQL&#41;](../../ssma/access/executing-the-ssma-console-accesstosql.md) .  
  
2.  Indica il livello di dettaglio desiderato nel report di output usando le impostazioni di dettaglio del report:  
  
    |SL. No.|Comando e parametro|Descrizione output|  
    |-|-|-|  
    |1|Verbose = "false"|Genera un report riepilogato dell'attività.|  
    |2|Verbose = "true"|Genera un report di stato riepilogativo e dettagliato per ogni attività.|  
  
    > [!NOTE]  
    > Le impostazioni del livello di dettaglio del report specificate in precedenza sono applicabili ai comandi generate-assessment-report, Convert-schema, Migrate-data.  
  
3.  Indica il livello di dettaglio desiderato nei report degli errori usando le impostazioni di segnalazione errori:  
  
    |SL. No.|Comando e parametro|Descrizione output|  
    |-|-|-|  
    |1|report-errori = "false"|Nessun dettaglio sui messaggi di errore, di avviso/informazione.|  
    |2|report-errori = "true"|Messaggi dettagliati di errore, avviso o informazioni.|  
  
    > [!NOTE]  
    > Le impostazioni di segnalazione errori specificate in precedenza sono applicabili ai comandi generate-assessment-report, Convert-schema, Migrate-data.  
  
**Esempio:**  
  
```xml  
<generate-assessment-report  
  
    object-name="testschema"  
  
    object-type="Schemas"  
  
    verbose="yes"  
  
    report-erors="yes"  
  
    write-summary-report-to="$AssessmentFolder$\Report1.xml"  
  
    assessment-report-folder="$AssessmentFolder$\assesment_report"  
  
    assessment-report-overwrite="true"  
  
/>  
```  
  
### <a name="synchronize-target"></a>sincronizzazione-destinazione:  
Il comando **Synchronize-target** dispone del parametro **Report-Errors-to** , che specifica il percorso del report degli errori per l'operazione di sincronizzazione. Quindi, un file in base al nome **TargetSynchronizationReport &lt; n &gt; . Il codice XML** viene creato nel percorso specificato, dove **&lt; n &gt;** è il numero di file univoco che incrementa con una cifra a ogni esecuzione dello stesso comando.  
  
**Nota:** Se viene specificato il percorso della cartella, il parametro ' Report-Errors-to ' diventa un attributo facoltativo per il comando ' Synchronize-target '.  
  
```xml  
<!-- Example: Synchronize target entire Database with all attributes-->  
  
<synchronize-target  
  
    object-name="$TargetDB$.dbo"  
  
    on-error="fail-script"  
  
    report-errors-to="$SynchronizationReports$"  
  
/>  
```  
**nome oggetto:** Specifica gli oggetti considerati per la sincronizzazione. può anche avere nomi di oggetti indivdual o un nome di oggetto gruppo.  
  
**in errore:** Specifica se specificare gli errori di sincronizzazione come avvisi o errori. Opzioni disponibili per l'errore:  
  
-   report-totale-come-avviso  
  
-   report-each-As-Warning  
  
-   script di errore  
  
### <a name="refresh-from-database"></a>aggiornamento da database:  
Il comando **Refresh-from-database** presenta un parametro **Report-Errors-to** , che specifica la posizione della segnalazione errori per l'operazione di aggiornamento. Quindi, un file in base al nome **SourceDBRefreshReport &lt; n &gt; . Il codice XML** viene creato nel percorso specificato, dove **&lt; n &gt;** è il numero di file univoco che incrementa con una cifra a ogni esecuzione dello stesso comando.  
  
**Nota:** Se viene specificato il percorso della cartella, il parametro ' Report-Errors-to ' diventa un attributo facoltativo per il comando ' Synchronize-target '.  
  
```xml  
<!-- Example: Refresh entire Schema (with all attributes)-->  
  
<refresh-from-database  
  
    object-name="$SourceDatabaseStandard$"  
  
    object-type ="Databases"  
  
    on-error="fail-script"  
  
    report-errors-to="$RefreshDBFolder$\RefreshReport.xml"  
  
/>  
```  
**nome oggetto:** Specifica gli oggetti considerati per l'aggiornamento. può anche avere nomi di oggetti indivdual o un nome di oggetto gruppo.  
  
**in errore:** Specifica se specificare gli errori di aggiornamento come avvisi o errori. Opzioni disponibili per l'errore:  
  
-   report-totale-come-avviso  
  
-   report-each-As-Warning  
  
-   script di errore  
  
## <a name="see-also"></a>Vedere anche  
[Esecuzione della console SSMA (accesso)](./executing-the-ssma-console-accesstosql.md)  
