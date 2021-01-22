---
description: Analisi dei dati
title: Analisi dei dati | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- parsing [Integration Services]
- data parsing [Integration Services]
ms.assetid: 8893ea9d-634c-4309-b52c-6337222dcb39
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 905c5988860aef1a2e495bb17d98a48eaf91c5ce
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594830"
---
# <a name="parsing-data"></a>Analisi dei dati

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  I flussi di dati nei pacchetti consentono di estrarre e caricare dati da archivi dati eterogenei, in cui possono venire utilizzati numerosi diversi tipi di dati standard e personalizzati. In un flusso di dati le origini di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sono responsabili dell'estrazione dei dati, dell'analisi dei dati stringa e della conversione dei dati in un tipo di dati di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Le trasformazioni successive possono analizzare dati per convertirli in un tipo di dati diverso oppure creare copie delle colonne con tipi di dati diversi. Anche le espressioni utilizzate nei componenti possono eseguire il cast di argomenti e operandi a tipi di dati diversi. Quando infine i dati vengono caricati in un archivio dati, la destinazione può analizzare i dati per convertirli in un tipo di dati utilizzato dalla destinazione. Per altre informazioni, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="two-types-of-parsing"></a>Due tipi di analisi  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] offre due tipi di analisi per la conversione dei dati: l'analisi veloce e l'analisi standard.  
  
-   L'analisi veloce è un semplice set di routine di analisi veloci, che non supportano conversioni di tipi di dati specifiche delle impostazioni locali e supportano solo i formati di data e ora utilizzati più di frequente. 
  
-   L'analisi standard è un set completo di routine di analisi che supportano tutte le conversioni dei tipi di dati offerte dalle API per la conversione dei tipi di dati di automazione disponibili in Oleaut32.dll e Ole2dsip.dll.   
  
## <a name="fast-parse"></a>Analisi veloce
L'analisi veloce offre un set di routine semplici e veloci per l'analisi dei dati. Tali routine sono indipendenti dalle impostazioni locali e supportano solo un subset dei formati di data, ora e integer.  
  
### <a name="requirements-and-limitations"></a>Requisiti e limitazioni  
 I pacchetti che implementano l'analisi veloce hanno una capacità limitata di interpretare le informazioni di data, ora e numeriche nei formati che dipendono dalle impostazioni locali e nei formati ISO 8601 di base ed estesi maggiormente utilizzati, ma offrono prestazioni superiori. L'analisi veloce supporta ad esempio solo le rappresentazioni dei formati di data utilizzate più di frequente, quali AAAAMMGG e AAAA-MM-GG, non esegue analisi specifiche delle impostazioni locali, non riconosce i caratteri speciali nei dati di valuta e non è in grado di convertire la rappresentazione esadecimale o scientifica dei valori interi.  
  
 L'analisi veloce è disponibile solo quando si utilizza l'origine file flat o la trasformazione Conversione dati. Poiché l'aumento di prestazioni può essere significativo, se possibile in tali componenti del flusso di dati è consigliabile utilizzare l'analisi veloce.  
  
 Se il flusso di dati nel pacchetto richiede analisi che dipendono dalle impostazioni locali, è consigliabile utilizzare l'analisi standard anziché l'analisi veloce. L'analisi veloce non riconosce ad esempio dati dipendenti dalle impostazioni locali che includono simboli quali il separatore decimale, formati di data diversi dai formati anno-mese-giorno e simboli di valuta.  
  
 Le rappresentazioni troncate che includono in modo implicito una o più parti della data, ad esempio il secolo, l'anno o il mese, non vengono riconosciute dall'analisi veloce. L'analisi veloce non riconosce né il formato "**-AAMM**", che specifica l'anno e il mese in un secolo implicito, né il formato "**--MM**", che specifica un mese in un anno implicito. Vengono tuttavia riconosciute alcune rappresentazioni con precisione ridotta, ad esempio il formato "hhmm;" che indica solo le ore e i minuti, e il formato "**AAAA**", che indica solo l'anno.  
  
 L'analisi veloce è specificata a livello di colonna. Nell'origine file flat e nella trasformazione Conversione dati è possibile specificare l'analisi veloce nelle colonne di output. Input e output possono includere sia colonne dipendenti dalle impostazioni locali che colonne indipendenti dalle impostazioni locali.  
 
## <a name="numeric-data-formats-fast-parse"></a>Formati di dati numerici (analisi veloce)
L'analisi veloce offre un set di routine semplici e veloci per l'analisi dei dati, indipendenti dalle impostazioni locali. Supporta solo un limitato set di formati per i tipi di dati integer.  
  
### <a name="integer-data-type"></a>Dati di tipo Integer
 In [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sono disponibili i tipi di dati integer DT_I1, DT_UI1, DT_I2, DT_UI2, DT_I4, DT_UI4, DT_I8 e DT_UI8. Per altre informazioni, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 L'analisi veloce supporta i formati seguenti per i tipi di dati integer:  
  
-   Zero o più spazi o tabulazioni iniziali e finali o arresti di tabulazioni. Ad esempio, il valore "  123  " è valido. Un valore costituito da soli spazi viene considerato equivalente a zero.  
  
-   Un segno più (+), un segno meno (-) o nessun segno iniziale. Ad esempio, i valori +123, -123 e 123 sono validi.  
  
-   Una o più cifre arabe (0-9). Ad esempio, il valore 345 è valido. Gli altri tipi di cifre non sono supportati.  
  
 Di seguito sono elencati alcuni formati di dati non supportati:  
  
-   Caratteri speciali. Il carattere di valuta $, ad esempio, non è supportato e il valore $20 non può essere analizzato.  
  
-   Caratteri spazio quali i caratteri di avanzamento riga, ritorno a capo e spazio unificatore. Il valore " 123", ad esempio, non può essere analizzato.  
  
-   Rappresentazioni esadecimali di valori interi. Il valore 2EE, ad esempio, non può essere analizzato.  
  
-   Valori interi in notazione scientifica. Il valore 1E+10, ad esempio, non può essere analizzato.  
  
 Di seguito sono elencati alcuni formati di output per i dati integer:  
  
-   Un segno meno (-) per i numeri negativi e nessun segno per i numeri positivi.  
  
-   Nessun carattere spazio.  
  
-   Una o più cifre arabe (0-9).  

## <a name="date-and-time-formats-fast-parse"></a>Formati di data e ora (analisi veloce)
L'analisi veloce offre un set di routine semplici e veloci per l'analisi dei dati. L'analisi veloce supporta i formati seguenti per i tipi di dati data e ora.  
  
### <a name="date-data-type"></a>Tipo di dati date 
 L'analisi veloce supporta i formati stringa seguenti per i dati di data:  
  
-   Formati di data che includono spazi vuoti iniziali. Il valore " 2004- 02-03", ad esempio, è valido.  
  
-   Formati ISO 8601, elencati nella tabella seguente:  
  
    |Formato|Descrizione|  
    |------------|-----------------|  
    |YYYYMMDD<br /><br /> YYYY-MM-DD|Formati di base ed esteso con anno a quattro cifre, a due cifre e giorno a due cifre. Nel formato esteso, le parti della data sono separate da un segno meno (-).|  
    |AAAA-MM|Formati di base ed esteso a precisione ridotta, con anno a quattro cifre e mese a due cifre. Nel formato esteso, le parti della data sono separate da un segno meno (-).|  
    |AAAA|Formato a precisione ridotta con anno a quattro cifre.|  
  
 L'analisi veloce non supporta i formati seguenti per i dati di data:  
  
-   Valore del mese in forma alfabetica. Ad esempio, il formato di data 31-ott-2003 non è valido.  
  
-   Formati ambigui, ad esempio GG-MM-AAAA e MM-GG-AAAA. Ad esempio, le date 03-04-1995 e 04-03-1995 non sono valide.  
  
-   Formati troncati di base ed esteso con anno di calendario a quattro cifre e giorno dell'anno a tre cifre, AAAAGGG e AAAA-GGG.  
  
-   Formati di base ed esteso con anno a quattro cifre, numero di due cifre per la settimana dell'anno e numero di una cifra per il giorno della settimana, AAAASssG e AAAA-Sss-G.  
  
-   Formati troncati di base ed esteso per date che includono anno e settimana, con anno a quattro cifre e numero di due cifre per la settimana, AAASss e AAAA-Sss.  
  
 I dati di output dell'analisi veloce sono di tipo DT_DBDATE. Ai valori di data con formati troncati viene applicato un riempimento. Ad esempio, AAAA diventa AAAA0101.  
  
 Per altre informazioni, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
### <a name="time-data-type"></a>Dati di tipo Time
 L'analisi veloce supporta i formati stringa seguenti per i dati di ora:  
  
-   Formati di ora che includono spazi vuoti iniziali. Ad esempio, il valore " 10.24" è valido.  
  
-   Formato di 24 ore. L'analisi veloce non supporta la notazione con AM e PM.  
  
-   Formati di ora ISO 8601, elencati nella tabella seguente:  
  
    |Formato|Descrizione|  
    |------------|-----------------|  
    |HHMISS<br /><br /> HH:MI:SS|Formati di base ed esteso con ora a due cifre, minuto a due cifre e giorno a due cifre. Nel formato esteso, le parti dell'ora sono separate da un punto (.).|  
    |HHMI<br /><br /> HH:MI|Formati troncati di base ed esteso con ora a due cifre e minuto a due cifre. Nel formato esteso, le parti dell'ora sono separate da un punto (.).|  
    |HH|Formato troncato con ora a due cifre.|  
    |00:00:00<br /><br /> 000000<br /><br /> 0000<br /><br /> 00<br /><br /> 240000<br /><br /> 24:00:00<br /><br /> 2400<br /><br /> 24|Formato per la mezzanotte.|  
  
-   Formati ora che specificano un fuso orario, elencati nella tabella seguente:  
  
    |Formato|Descrizione|  
    |------------|-----------------|  
    |+HH:MI<br /><br /> +HHMI|Formati di base ed estesi che indicano il numero di ore e minuti da aggiungere all'ora UTC (Coordinated Universal Time) per ottenere l'ora locale.|  
    |-HH:MI<br /><br /> -HHMI|Formati di base ed estesi che indicano il numero di ore e minuti da sottrarre all'ora UTC per ottenere l'ora locale.|  
    |+HH|Formato troncato che indica il numero di ore da aggiungere all'ora UTC per ottenere l'ora locale.|  
    |-HH|Formato troncato che indica il numero di ore da sottrarre all'ora UTC per ottenere l'ora locale.|  
    |Z|Valore 0 che indica che l'ora è rappresentata in formato UTC.|  
  
     I formati per tutti i dati di ora e di data e ora possono includere un elemento relativo al fuso orario. Il valore del fuso orario viene tuttavia ignorato dal sistema, ad eccezione del caso in cui i dati sono di tipo DT_DBTIMESTAMPOFFSET. Per altre informazioni, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
     Nei formati che includono un elemento del fuso orario, non vi sono spazi tra l'elemento relativo all'ora e quello relativo al fuso orario, come illustrato nell'esempio seguente:  
  
     HH:MI:SS[+HH:MI]  
  
     Le parentesi quadrate nell'esempio precedente indicano che il valore del fuso orario è facoltativo.  
  
-   Formati di ora che includono una frazione decimale, elencati nella tabella seguente:  
  
    |Formato|Descrizione|  
    |------------|-----------------|  
    |HH[.nnnnnnn]|n è un valore compreso tra 0 e 9999999 che rappresenta una frazione di ore. Le parentesi quadrate indicano che tale valore è facoltativo.<br /><br /> Il valore 12.750 indica ad esempio le 12:45.|  
    |HHMI[.nnnnnnn]<br /><br /> HH:MI[.nnnnnnn]|n è un valore compreso tra 0 e 9999999 che rappresenta una frazione di minuti. Le parentesi quadrate indicano che tale valore è facoltativo.<br /><br /> Il valore 1220,500 indica ad esempio le 12:20:30.|  
    |HHMISS[.nnnnnnn]<br /><br /> HH:MI:SS[.nnnnnnn]|n è un valore compreso tra 0 e 9999999 che rappresenta una frazione di secondi. Le parentesi quadrate indicano che tale valore è facoltativo.<br /><br /> Il valore 122040,250 indica ad esempio le 12:20:40.15.|  
  
    > [!NOTE]  
    >  Il separatore della parte frazionaria per i formati di ora nella tabella precedente può essere un punto o una virgola.  
  
-   Valori di ora che includono un secondo intercalare, come illustrato negli esempi seguenti:  
  
     23:59:60[.0000000]  
  
     235960[.0000000]  
  
 Le stringhe di output dell'analisi veloce sono di tipo DT_DBTIME e DT_DBTIME2. Ai valori di ora con formati troncati viene applicato un riempimento. HH:MI diventa ad esempio HH:MM:00.000.  
  
 Per altre informazioni, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
### <a name="datetime-data-type"></a>Tipi di dati per i valori di data e ora  
 L'analisi veloce supporta i formati stringa seguenti per i dati di data e ora:  
  
-   Formati che includono spazi vuoti iniziali. Il valore "  2003-01-10T203910", ad esempio, è valido.  
  
-   Combinazioni di formati di data validi e formati di ora validi separati da una T maiuscola e di formati di fuso orario validi. Ad esempio, AAAAMMGGT[HHMISS][+HH:MI]. I valori relativi a ora e fuso orario non sono obbligatori. Il valore "2003-10-14", ad esempio, è valido.  
  
 L'analisi veloce non supporta gli intervalli di tempo. Non è ad esempio possibile analizzare un intervallo di tempo identificato da una data e ora di inizio e da una data e ora di fine, nel formato AAAAMMGGThhmmss/AAAAMMGGThhmmss.  
  
 Le stringhe di output dell'analisi veloce sono di tipo DT_DATE, DT_DBTIMESTAMP, DT_DBTIMESTAMP2 e DT_DBTIMESTAMPOFFSET. Ai valori di data e ora con formati troncati viene applicato un riempimento. Nella tabella seguente sono elencati i valori aggiunti per le parti mancanti relative a data e ora.  
  
|Parte della data o dell'ora|Riempimento|  
|---------------------|-------------|  
|Secondi|Viene aggiunto 00.|  
|Minuti|Aggiungi 00:00.|  
|Ora|Viene aggiunto 00.00.00.|  
|Giorno|Viene aggiunto 01 come giorno del mese.|  
|Month|Viene aggiunto 01 come mese dell'anno.|  
  
 Per altre informazioni, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="enable-fast-parse"></a>Abilitare l'analisi veloce
È necessario impostare la proprietà relativa all'analisi veloce per ogni colonna dell'origine o della trasformazione in cui questa viene utilizzata. Per impostare la proprietà, utilizzare l'editor avanzato dell'origine file flat e della trasformazione Conversione dati.  
  
1.  Fare clic con il pulsante destro del mouse sull'origine file flat o sulla trasformazione Conversione dati e quindi scegliere **Visualizza editor avanzato**.  
  
2.  Nella finestra di dialogo **Editor avanzato** fare clic sulla scheda **Proprietà input e output** .  
  
3.  Nel riquadro **Input e output** fare clic sulla colonna per la quale si desidera abilitare l'analisi veloce.  
  
4.  Nella finestra Proprietà espandere il nodo **Proprietà personalizzate** e quindi impostare la proprietà **FastParse** su **True**.  
  
5.  Fare clic su **OK**.  

## <a name="standard-parse"></a>Analisi standard
L'analisi standard è un set di routine di analisi, dipendenti dalle impostazioni locali, da cui sono supportate tutte le conversioni previste dalle API per la conversione dei tipi di dati di automazione disponibili in Oleaut32.dll e Ole2dsip.dll. L'analisi standard è equivalente alle API di analisi di OLE DB.  
  
 L'analisi standard consente di eseguire conversioni tra tipi di dati utilizzati per dati internazionali e deve essere utilizzata quando il formato dei dati non è supportato dall'analisi veloce. Per ulteriori informazioni sull'API di conversione del tipo di dati di automazione, vedere la sezione relativa alle API di conversione dei tipi di dati nel sito Web [MSDN Library](/documentation/). 
