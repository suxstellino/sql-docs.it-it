---
description: Generazione di report (MySQLToSQL)
title: Generazione di report (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Generating reports
ms.assetid: 1c0202e8-546d-4cb3-a37f-1d2e35d53839
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e0fb362926b88d65ed8b1959059e43465ce587f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078222"
---
# <a name="generating-reports-mysqltosql"></a>Generazione di report (MySQLToSQL)
I report di determinate attività eseguite usando i comandi vengono generati nella console di SSMA a livello di albero degli oggetti.  
  
Utilizzare la procedura seguente per generare report:  
  
1.  Specificare il parametro **Write-Summary-Report-to** . Il report correlato viene archiviato come nome file (se specificato) o nella cartella specificata. Il nome del file è predefinito dal sistema come indicato nella tabella seguente, dove **&lt; n &gt;** è il numero di file univoco che incrementa con una cifra a ogni esecuzione dello stesso comando.  
  
    I report Vis-à-Vis sono i seguenti:  
  
    |SL. No.|Comando|Titolo del report|  
    |-|-|-|  
    |1|generate-assessment-report|AssessmentReport &lt; n &gt; . XML|  
    |2|Convert-schema|SchemaConversionReport &lt; n &gt; . XML|  
    |3|migrate-dati|DataMigrationReport &lt; n &gt; . XML|  
    |4|Convert-SQL-statement|ConvertSQLReport &lt; n &gt; . XML|  
    |5|sincronizzazione-destinazione|TargetSynchronizationReport &lt; n &gt; . XML|  
    |6|aggiornamento da database|SourceDBRefreshReport &lt; n &gt; . XML|  
  
    > [!IMPORTANT]  
    > Un report di output è diverso dal rapporto di valutazione. Il primo è un report sulle prestazioni di un comando eseguito mentre, quest'ultimo è un report XML per l'utilizzo a livello di codice.  
  
    Per le opzioni di comando per i report di output (da SL. No. 2-4 sopra), fare riferimento alla sezione [esecuzione della console SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/executing-the-ssma-console-mysqltosql.md) .  
  
2.  Indica il livello di dettaglio desiderato nel report di output usando le impostazioni di dettaglio del report:  
  
    |SL. No.|Comando e parametro|Descrizione output|  
    |-|-|-|  
    |1|Verbose = "false"|Genera un report riepilogato dell'attività.|  
    |2|Verbose = "true"|Genera un report di stato riepilogativo e dettagliato per ogni attività.|  
  
    > [!NOTE]  
    > Le impostazioni di dettaglio del report specificate in precedenza sono applicabili ai comandi generate-assessment-report, Convert-schema, Migrate-data, Convert-SQL-statement.  
  
3.  Indica il livello di dettaglio desiderato nei report degli errori usando le impostazioni di segnalazione errori:  
  
    |SL. No.|Comando e parametro|Descrizione output|  
    |-|-|-|  
    |1|report-errori = "false"|Nessun dettaglio sui messaggi di errore, di avviso/informazione.|  
    |2|report-errori = "true"|Messaggi dettagliati di errore, avviso o informazioni.|  
  
    > [!NOTE]  
    > Le impostazioni di segnalazione errori specificate in precedenza sono applicabili ai comandi generate-assessment-report, Convert-schema, Migrate-data, Convert-SQL-statement.  
  
```xml  
<generate-assessment-report  
  
   object-name="<object-name>"  
  
   object-type="<object-type>"  
  
   verbose="<true/false>"  
  
   report-erors="<true/false>"  
  
   write-summary-report-to="<file-name/folder-name>"  
  
   assessment-report-folder="<folder-name>"  
  
   assessment-report-overwrite="<true/false>"  
  
/>  
```  
  
### <a name="synchronize-target"></a>sincronizzazione-destinazione:  
Il comando **Synchronize-target** dispone del parametro **Report-Errors-to** , che specifica il percorso del report degli errori per l'operazione di sincronizzazione. Quindi, un file in base al nome **TargetSynchronizationReport &lt; n &gt; . Il codice XML** viene creato nel percorso specificato, dove **&lt; n &gt;** è il numero di file univoco che incrementa con una cifra a ogni esecuzione dello stesso comando.  
  
**Nota:** Se viene specificato il percorso della cartella, il parametro ' Report-Errors-to ' diventa un attributo facoltativo per il comando ' Synchronize-target '.  
  
```xml  
<!-- Example: Synchronize target entire Database with all attributes-->  
  
<synchronize-target  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/report-each-as-warning/fail-script>"  
  
   report-errors-to="<file-name/folder-name>"  
  
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
  
   object-name="<object-name>"  
  
   object-type ="<object-type>"  
  
   on-error="<report-total-as-warning/report-each-as-warning/fail-script>"  
  
   report-errors-to="<file-name/folder-name>"  
  
/>  
```  
**nome oggetto:** Specifica gli oggetti considerati per l'aggiornamento. può anche avere nomi di oggetti indivdual o un nome di oggetto gruppo.  
  
**in errore:** Specifica se specificare gli errori di aggiornamento come avvisi o errori. Opzioni disponibili per l'errore:  
  
-   report-totale-come-avviso  
  
-   report-each-As-Warning  
  
-   script di errore  
  
## <a name="see-also"></a>Vedere anche  
[Esecuzione della console SSMA (MySQL)](./executing-the-ssma-console-mysqltosql.md)  
