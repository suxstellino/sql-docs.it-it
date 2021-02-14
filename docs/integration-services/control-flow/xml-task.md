---
description: Attività XML
title: Attività XML | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.xmltask.f1
- sql13.dts.designer.xmltask.general.f1
helpviewer_keywords:
- XML [Integration Services]
- XML task [Integration Services]
ms.assetid: 9f761846-390e-46d5-9db7-858943d40849
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f3b2b309d07e14347aa2d89038c897b3db804c9b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354402"
---
# <a name="xml-task"></a>Attività XML

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività XML consente di eseguire operazioni su dati in formato XML. Tramite questa attività un pacchetto può recuperare documenti XML, applicare operazioni ai documenti utilizzando fogli di stile XSLT (Extensible Stylesheet Language Transformation) ed espressioni XPath, unire più documenti oppure convalidare, confrontare e salvare i documenti aggiornati in file e variabili.  
  
 Questa attività consente ai pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] di modificare dinamicamente documenti XML in fase di esecuzione. È possibile utilizzare l'attività XML per gli scopi seguenti:  
  
-   Riformattare un documento XML. L'attività può ad esempio accedere a un report che risiede in un file XML e applicare dinamicamente un foglio di stile XSLT per personalizzare la presentazione del documento.  
  
-   Selezionare sezioni di un documento XML. L'attività può ad esempio accedere a un report che risiede in un file XML e applicare dinamicamente un'espressione XPath per selezionare una sezione del documento. L'operazione può inoltre ottenere ed elaborare valori nel documento.  
  
-   Unire più documenti da numerose origini. L'attività può ad esempio scaricare report da più origini e unirli dinamicamente in un documento XML riepilogativo.  
  
-   Convalidare un documento XML e, facoltativamente, ottenere un output dettagliato degli errori. Per ulteriori informazioni, vedere [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
 È possibile includere dati XML in un flusso di dati utilizzando un'origine XML per estrarre valori da un documento XML. Per altre informazioni, vedere [Origine XML](../../integration-services/data-flow/xml-source.md).  
  
## <a name="xml-operations"></a>Operazioni XML  
 La prima azione eseguita dall'attività XML consiste nel recupero di un documento XML specifico. Tale azione è incorporata nell'attività XML e viene eseguita automaticamente. Il documento XML recuperato viene utilizzato come origine dei dati per l'operazione eseguita dall'attività XML.  
  
 Le operazioni XML Diff, Merge e Patch richiedono due operandi. Il primo operando specifica il documento XML di origine. Il secondo specifica un documento XML il cui contenuto dipende dai requisiti dell'operazione. L'operazione Diff, ad esempio, confronta due documenti. Il secondo operando specifica pertanto un altro documento XML simile, con cui confrontare il documento XML di origine.  
  
 L'attività XML può utilizzare una variabile o una gestione connessione file come origine oppure includere i dati XML in una proprietà dell'attività.  
  
 Se l'origine è una variabile, la variabile specificata conterrà il percorso del documento XML.  
  
 Se l'origine è una gestione connessione file, le informazioni sull'origine verranno fornite dalla gestione connessione file specificata, configurata separatamente, a cui viene fatto riferimento dall'attività XML. La stringa di connessione della gestione connessione file specifica il percorso del file XML. Per altre informazioni, vedere [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
 L'attività XML può essere configurata in modo da salvare il risultato dell'operazione in una variabile o in un file. Se il risultato viene salvato in un file, l'attività XML utilizzerà una gestione connessione file per accedervi. Anche i risultati del Diffgram generato dall'operazione Diff possono essere salvati in file e variabili.  
  
## <a name="predefined-xml-operations"></a>Operazioni XML predefinite  
 L'attività XML include un set predefinito di operazioni applicabili ai documenti XML, descritte nella tabella seguente.  
  
|Operazione|Descrizione|  
|---------------|-----------------|  
|Diff|Consente di confrontare due documenti XML. Utilizzando il documento XML di origine come documento di base, l'operazione Diff esegue un confronto con un secondo documento XML, rileva le differenze e le scrive in un documento Diffgram in formato XML. Questa operazione include proprietà per la personalizzazione del confronto.|  
|Unione|Consente di unire due documenti XML. Utilizzando il documento XML di origine come documento di base, l'operazione Merge aggiunge il contenuto di un secondo documento al documento di base. L'operazione può specificare una posizione di unione nel documento di base.|  
|Patch|Applica l'output dell'operazione Diff, detto documento Diffgram, a un documento XML, per creare un nuovo documento padre che include il contenuto del documento Diffgram.|  
|Convalida|Consente di convalidare il documento XML in base a una schema di definizione DTD (Document Type Definition) o XSD (XML Schema Definition).|  
|XPath|Consente di eseguire valutazioni e query XPath.|  
|XSLT|Consente di eseguire trasformazioni XSL in documenti XML.|  
  
### <a name="diff-operation"></a>Operazione Diff  
 L'operazione Diff può essere configurata in modo da utilizzare un algoritmo di confronto diverso a seconda che il confronto debba essere rapido o preciso. L'operazione può essere inoltre configurata in modo da selezionare automaticamente il confronto rapido o preciso, in base alle dimensioni dei documenti da confrontare.  
  
 L'operazione Diff include un set di opzioni per la personalizzazione del confronto XML, Nella tabella seguente vengono descritte le opzioni disponibili.  
  
|Opzione|Descrizione|  
|------------|-----------------|  
|**IgnoreComments**|Valore che specifica se i nodi dei commenti devono essere confrontati.|  
|**IgnoreNamespaces**|Valore che specifica se l'URI (Uniform Resource Identifier) dello spazio dei nomi di un elemento e i nomi dei suoi attributi devono essere confrontati. Se questa opzione è impostata su **true**, due elementi con lo stesso nome locale e spazio dei nomi diverso verranno considerati identici.|  
|**IgnorePrefixes**|Valore che specifica se i prefissi degli elementi e i nomi degli attributi vengono confrontati. Se questa opzione è impostata su **true,** due elementi con lo stesso nome locale ma prefisso e URI dello spazio dei nomi diversi verranno considerati identici.|  
|**IgnoreXMLDeclaration**|Valore che specifica se le dichiarazioni XML devono essere confrontate.|  
|**IgnoreOrderOfChildElements**|Valore che specifica se l'ordine degli elementi figli viene confrontato. Se questa opzione è impostata su **true**, gli elementi figli che differiscono solo per la posizione in un elenco di elementi di pari livelli vengono considerati identici.|  
|**IgnoreWhiteSpaces**|Valore che specifica se gli spazi devono essere confrontati.|  
|**IgnoreProcessingInstructions**|Valore che specifica se le istruzioni di elaborazione devono essere confrontate.|  
|**IgnoreDTD**|Valore che specifica se il DTD deve essere ignorato.|  
  
### <a name="merge-operation"></a>Operazione Merge  
 Quando si utilizza un'istruzione XPath per identificare la posizione di unione nel documento di origine, è previsto che tale istruzione restituisca un unico nodo. Se l'istruzione restituisce più nodi, viene utilizzato solo il primo nodo. Il contenuto del secondo documento viene unito sotto il primo nodo restituito dalla query XPath.  
  
### <a name="xpath-operation"></a>Operazione XPath  
 L'operazione XPath può essere configurata in modo da utilizzare diversi tipi di funzionalità XPath.  
  
-   Per implementare funzioni XPath come sum(), selezionare l'opzione **Valutazione** .  
  
-   Per restituire come frammento XML i nodi selezionati, selezionare l'opzione **Elenco dei nodi** .  
  
-   Per restituire il valore del testo interno di tutti i nodi selezionati, concatenato in modo da formare una stringa, selezionare l'opzione **Valori** .  
  
### <a name="validation-operation"></a>Operazione Validation  
 L'operazione Validate può essere configurata in modo da utilizzare un file DTD (Document Type Definition) o uno schema XSD (XML Schema Definition).  
  
 Abilitare **ValidationDetails** per ottenere output dettagliato degli errori. Per ulteriori informazioni, vedere [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
## <a name="xml-document-encoding"></a>Codifica dei documenti XML  
 L'attività XML supporta solo l'unione di documenti Unicode. È pertanto possibile applicare l'operazione Merge solo a documenti con codifica Unicode. Se si utilizzano altre codifiche, l'attività XML non verrà completata.  
  
> [!NOTE]  
>  Le operazioni Diff e Patch includono un'opzione che consente di ignorare la dichiarazione XML nei dati XML del secondo operando e questo consente di utilizzare documenti con altre codifiche in tali operazioni.  
  
 Per verificare se il documento XML può essere utilizzato, esaminare la dichiarazione XML. La dichiarazione deve specificare esplicitamente UTF-8, che indica la codifica Unicode a 8 bit.  
  
 Nel tag seguente è specificata la codifica Unicode a 8 bit.  
  
 `<?xml version="1.0" encoding="UTF-8"?>`  
  
## <a name="custom-logging-messages-available-on-the-xml-task"></a>Messaggi di registrazione personalizzati disponibili nell'attività XML  
 Nella tabella seguente è indicata la voce di log personalizzata disponibile per l'attività XML. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**XMLOperation**|Fornisce informazioni sull'operazione eseguita dall'attività.|  
  
## <a name="configuration-of-the-xml-task"></a>Configurazione dell'attività XML  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per ulteriori informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic su uno degli argomenti seguenti:  
  
-   [Eseguire la convalida XML con l'attività XML](../../integration-services/control-flow/validate-xml-with-the-xml-task.md)  
  
-   [Pagina Espressioni](../../integration-services/expressions/expressions-page.md)  
  
 Per ulteriori informazioni sull'impostazione delle proprietà in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-xml-task"></a>Configurazione a livello di codice dell'attività XML  
 Per ulteriori informazioni sull'impostazione di queste proprietà a livello di codice, fare clic sull'argomento seguente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.XMLTask.XMLTask>  
  
## <a name="related-tasks"></a>Attività correlate  
 [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="xml-task-editor-general-page"></a>XML Task Editor (General Page)
  Utilizzare il nodo **Generale** della finestra di dialogo **Editor attività XML** per specificare il tipo di operazione e configurarla.  
  
 Per altre informazioni su questa attività, vedere [Eseguire la convalida XML con Attività XML](../../integration-services/control-flow/validate-xml-with-the-xml-task.md). Per ulteriori informazioni sull'utilizzo di documenti e dati XML, vedere "[Employing XML in the .NET Framework](/previous-versions/aa720019(v=vs.71))" (Utilizzo di codice XML in .NET Framework) in MSDN Library.  
  
### <a name="static-options"></a>Opzioni statiche  
 **Tipo operazione**  
 Selezionare il tipo di operazione dall'elenco. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Convalida**|Consente di convalidare il documento XML in base a una schema di definizione DTD (Document Type Definition) o XSD (XML Schema Definition). Selezionando questa opzione, verranno visualizzate le opzioni dinamiche nella sezione **Convalida**.|  
|**XSLT**|Consente di eseguire trasformazioni XSL in documenti XML. Selezionando questa opzione, verranno visualizzate le opzioni dinamiche nella sezione **XSLT**.|  
|**XPATH**|Consente di eseguire valutazioni e query XPath. Selezionando questa opzione, verranno visualizzate le opzioni dinamiche nella sezione **XPATH**.|  
|**Merge**|Consente di unire due documenti XML. Selezionando questa opzione, verranno visualizzate le opzioni dinamiche nella sezione **Merge**.|  
|**Diff**|Consente di confrontare due documenti XML. Selezionando questa opzione, verranno visualizzate le opzioni dinamiche nella sezione **Diff**.|  
|**Patch**|Consente di applicare l'output di un'operazione Diff per la creazione di un nuovo documento. Selezionando questa opzione, verranno visualizzate le opzioni dinamiche nella sezione **Patch**.|  
  
 **SourceType**  
 Consente di selezionare il tipo di origine del documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Input diretto**|Consente di impostare l'origine su un documento XML.|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **Origine**  
 Se l'opzione **Origine** è impostata su **Input diretto**, specificare il codice XML oppure fare clic sul pulsante con i puntini di sospensione **(...)** e quindi indicare il codice XML usando la finestra di dialogo **Editor origine documento**.  
  
 Se l'opzione **Origine** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **Origine** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su **\<New variable...>** per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md).  
  
### <a name="operationtype-dynamic-options"></a>Opzioni dinamiche OperationType  
  
#### <a name="operationtype--validate"></a>OperationType = Convalida  
 Consente di specificare le opzioni per l'operazione di convalida.  
  
 **SaveOperationResult**  
 Consente di indicare se l'attività XML deve salvare l'output dell'operazione di convalida.  
  
 **OverwriteDestination**  
 Consente di indicare se sovrascrivere il file o la variabile di destinazione.  
  
 **Destinazione**  
 Selezionare una gestione connessione file nell'elenco oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 **DestinationType**  
 Consente di selezionare il tipo di destinazione del documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **ValidationType**  
 Consente di selezionare il tipo di convalida. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**DTD**|Consente di utilizzare una definizione DTD.|  
|**XSD**|Consente di utilizzare una definizione XSD. Selezionando questa opzione, verranno visualizzate le opzioni dinamiche nella sezione **ValidationType**.|  
  
 **FailOnValidationFail**  
 Consente di indicare se l'operazione deve essere interrotta in caso di errore della convalida.  
  
 **ValidationDetails**  
 Fornisce output avanzato degli errori quando il valore di questa proprietà è True. Per ulteriori informazioni, vedere [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
### <a name="validationtype-dynamic-options"></a>Opzioni dinamiche ValidationType  
  
#### <a name="validationtype--xsd"></a>ValidationType = XSD  
 **SecondOperandType**  
 Consente di selezionare il tipo di origine del secondo documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Input diretto**|Consente di impostare l'origine su un documento XML.|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperand**  
 Se la proprietà **SecondOperandType** è impostata su **Input diretto**, specificare il codice XML oppure fare clic sul pulsante con i puntini di sospensione **(...)** e quindi indicare il codice XML usando la finestra di dialogo **Editor origine**.  
  
 Se la proprietà **SecondOperandType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **XPathStringSourceType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md).  
  
#### <a name="operationtype--xslt"></a>OperationType = XSLT  
 Consente di specificare le opzioni per l'operazione di trasformazione XSLT.  
  
 **SaveOperationResult**  
 Consente di indicare se l'attività XML deve salvare l'output dell'operazione di trasformazione XSLT.  
  
 **OverwriteDestination**  
 Consente di indicare se sovrascrivere il file o la variabile di destinazione.  
  
 **Destinazione**  
 Se la proprietà **DestinationType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **DestinationType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Consente di selezionare il tipo di destinazione del documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperandType**  
 Consente di selezionare il tipo di origine del secondo documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Input diretto**|Consente di impostare l'origine su un documento XML.|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperand**  
 Se la proprietà **SecondOperandType** è impostata su **Input diretto**, specificare il codice XML oppure fare clic sul pulsante con i puntini di sospensione **(...)** e quindi indicare il codice XML usando la finestra di dialogo **Editor origine**.  
  
 Se la proprietà **SecondOperandType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **XPathStringSourceType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md).  
  
#### <a name="operationtype--xpath"></a>OperationType = XPATH  
 Consente di specificare le opzioni per l'operazione XPath.  
  
 **SaveOperationResult**  
 Consente di indicare se l'attività XML deve salvare l'output dell'operazione XPath.  
  
 **OverwriteDestination**  
 Consente di indicare se sovrascrivere il file o la variabile di destinazione.  
  
 **Destinazione**  
 Se la proprietà **DestinationType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **DestinationType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Consente di selezionare il tipo di destinazione del documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperandType**  
 Consente di selezionare il tipo di origine del secondo documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Input diretto**|Consente di impostare l'origine su un documento XML.|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperand**  
 Se la proprietà **SecondOperandType** è impostata su **Input diretto**, specificare il codice XML oppure fare clic sul pulsante con i puntini di sospensione **(...)** e quindi indicare il codice XML usando la finestra di dialogo **Editor origine**.  
  
 Se la proprietà **SecondOperandType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **XPathStringSourceType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md).  
  
 **PutResultInOneNode**  
 Consente di indicare se il risultato deve essere scritto in un singolo nodo.  
  
 **XPathOperation**  
 Consente di selezionare il tipo di risultato XPath. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Versione di valutazione**|Restituisce i risultati di una funzione XPath.|  
|**Elenco dei nodi**|Restituisce i nodi selezionati come frammento XML.|  
|**Valori**|Restituisce il valore di testo interno di tutti i nodi selezionati, concatenato all'interno di una stringa.|  
  
#### <a name="operationtype--merge"></a>OperationType = Merge  
 Consente di specificare le opzioni per l'operazione di unione.  
  
 **XPathStringSourceType**  
 Consente di selezionare il tipo di origine del documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Input diretto**|Consente di impostare l'origine su un documento XML.|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **XPathStringSource**  
 Se la proprietà **XPathStringSourceType** è impostata su **Input diretto**, specificare il codice XML oppure fare clic sul pulsante con i puntini di sospensione **(...)** e quindi indicare il codice XML usando la finestra di dialogo **Editor origine documento**.  
  
 Se la proprietà **XPathStringSourceType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **XPathStringSourceType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md)  
  
 Quando si utilizza un'istruzione XPath per identificare la posizione di unione nel documento di origine, è previsto che tale istruzione restituisca un unico nodo. Se l'istruzione restituisce più nodi, viene utilizzato solo il primo nodo. Il contenuto del secondo documento viene unito sotto il primo nodo restituito dalla query XPath.  
  
 **SaveOperationResult**  
 Consente di indicare se l'attività XML deve salvare l'output dell'operazione di unione.  
  
 **OverwriteDestination**  
 Consente di indicare se sovrascrivere il file o la variabile di destinazione.  
  
 **Destinazione**  
 Se la proprietà **DestinationType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **DestinationType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Consente di selezionare il tipo di destinazione del documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperandType**  
 Consente di selezionare il tipo di destinazione del secondo documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Input diretto**|Consente di impostare l'origine su un documento XML.|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperand**  
 Se la proprietà **SecondOperandType** è impostata su **Input diretto**, specificare il codice XML oppure fare clic sul pulsante con i puntini di sospensione **(...)** e quindi indicare il codice XML usando la finestra di dialogo **Editor origine documento**.  
  
 Se la proprietà **SecondOperandType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **SecondOperandType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md)  
  
#### <a name="operationtype--diff"></a>OperationType = Diff  
 Consente di specificare le opzioni per l'operazione Diff.  
  
 **DiffAlgorithm**  
 Consente di selezionare l'algoritmo Diff da utilizzare per il confronto dei documenti. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Auto**|Fa sì che l'attività XML determini automaticamente se utilizzare l'algoritmo veloce o esatto.|  
|**Veloce**|Consente di utilizzare un algoritmo Diff veloce, ma meno preciso.|  
|**Preciso**|Consente di utilizzare un algoritmo Diff preciso.|  
  
 **Opzioni Diff**  
 Consente di impostare le opzioni Diff da applicare all'operazione Diff. Nella tabella seguente vengono elencate le opzioni.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**IgnoreXMLDeclaration**|Consente di indicare se confrontare la dichiarazione XML.|  
|**IgnoreDTD**|Consente di indicare se ignorare la definizione DTD.|  
|**IgnoreWhiteSpaces**|Consente di specificare se ignorare le differenze riguardanti la quantità di spazio bianco nel confronto tra documenti.|  
|**IgnoreNamespaces**|Consente di indicare se confrontare l'URL dello spazio dei nomi di un elemento e i nomi dei relativi attributi.<br /><br /> Nota: se questa opzione è impostata su **True**, due elementi che hanno lo stesso nome locale ma spazi dei nomi diversi verranno considerati identici.|  
|**IgnoreProcessingInstructions**|Consente di indicare se confrontare le istruzioni di elaborazione.|  
|**IgnoreOrderOfChildElements**|Consente di indicare se confrontare l'ordine degli elementi figlio.<br /><br /> Nota: se questa opzione è impostata su **True**, gli elementi figlio che differiscono solo per la posizione in un elenco di elementi di pari livello vengono considerati identici.|  
|**IgnoreComments**|Consente di indicare se confrontare i nodi di commento.|  
|**IgnorePrefixes**|Consente di indicare se confrontare i prefissi dei nomi degli elementi e degli attributi.<br /><br /> Nota: se questa opzione è impostata su **True**, due elementi che hanno lo stesso nome locale ma prefissi e URI dello spazio dei nomi diversi vengono considerati identici.|  
  
 **FailOnDifference**  
 Consente di indicare se l'attività deve essere interrotta in caso di errore dell'operazione Diff.  
  
 **SaveDiffGram**  
 Consente di indicare se salvare il risultato del confronto, un documento DiffGram.  
  
 **SaveOperationResult**  
 Consente di indicare se l'attività XML deve salvare l'output dell'operazione Diff.  
  
 **OverwriteDestination**  
 Consente di indicare se sovrascrivere il file o la variabile di destinazione.  
  
 **Destinazione**  
 Se la proprietà **DestinationType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **DestinationType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Consente di selezionare il tipo di destinazione del documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperandType**  
 Consente di selezionare il tipo di destinazione del documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Input diretto**|Consente di impostare l'origine su un documento XML.|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperand**  
 Se la proprietà **SecondOperandType** è impostata su **Input diretto**, specificare il codice XML oppure fare clic sul pulsante con i puntini di sospensione **(...)** e quindi indicare il codice XML usando la finestra di dialogo **Editor origine documento**.  
  
 Se la proprietà **SecondOperandType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **SecondOperandType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md)  
  
#### <a name="operationtype--patch"></a>OperationType = Patch  
 Consente di specificare le opzioni per l'operazione Patch.  
  
 **SaveOperationResult**  
 Consente di indicare se l'attività XML deve salvare l'output dell'operazione Patch.  
  
 **OverwriteDestination**  
 Consente di indicare se sovrascrivere il file o la variabile di destinazione.  
  
 **Destinazione**  
 Se la proprietà **DestinationType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **DestinationType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Consente di selezionare il tipo di destinazione del documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperandType**  
 Consente di selezionare il tipo di destinazione del documento XML. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Input diretto**|Consente di impostare l'origine su un documento XML.|  
|**Connessione file**|Consente di selezionare un file contenente il documento XML.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il documento XML.|  
  
 **SecondOperand**  
 Se la proprietà **SecondOperandType** è impostata su **Input diretto**, specificare il codice XML oppure fare clic sul pulsante con i puntini di sospensione **(...)** e quindi indicare il codice XML usando la finestra di dialogo **Editor origine documento**.  
  
 Se la proprietà **SecondOperandType** è impostata su **Connessione file**, selezionare una gestione connessione file oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
 Se l'opzione **SecondOperandType** è impostata su **Variabile**, selezionare una variabile esistente oppure fare clic su \<**New variable...**> per creare una nuova variabile.  
  
 **Argomenti correlati**: [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md)  
  
## <a name="related-content"></a>Contenuto correlato  

-   Esempio di CodePlex sull' [esempio di elaborazione di pacchetti dati XML](https://msftisprodsamples.codeplex.com/wikipage?title=SS2008!Process%20XML%20Data%20Package%20Sample&version=10&ProjectName=msftisprodsamples), su www.codeplex.com  
  
