---
title: Introduzione al provider SQLXMLOLEDB (SQLXML)
description: Informazioni sul provider SQLXMLOLEDB, un provider OLE DB che espone la funzionalità SQLXML tramite ActiveX Data Objects (ADO).
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXMLOLEDB Provider, properties
- adExecuteStream flag
- SQLXMLOLEDB Provider, about SQLXMLOLEDB Provider
ms.assetid: 2e3f3817-4209-4bf4-9f46-248c95bc6f1b
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 585874edde793c1da0f6849089fdef558b2920ca
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490589"
---
# <a name="introduction-to-the-sqlxmloledb-provider-sqlxml-40"></a>Introduzione al provider SQLXMLOLEDB (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Il provider SQLXMLOLEDB è un provider OLE DB che espone la funzionalità [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML tramite gli oggetti ADO (ActiveX Data Objects). Il provider può tuttavia eseguire i comandi solo nella modalità di scrittura in un flusso di output di ADO. Il provider SQLXMLOLEDB non è un provider di set di righe. Quando si esegue un comando, è necessario specificare il flag adExecuteStream, che indica ad ADO di usare il flusso di output specificato.  
  
 L'esempio seguente illustra la sintassi per il comando Execute in cui è specificato il flag adExecuteStream:  
  
```  
Dim oTestCommand As New ADODB.Command  
...  
oTestCommand.Properties("Output Stream").Value = oTestStream  
oTestCommand.Execute , , adExecuteStream  
...  
```  
  
## <a name="sqlxmloledb-provider-specific-properties"></a>Proprietà specifiche del provider SQLXMLOLEDB  
 Il provider SQLXMLOLEDB espone le proprietà di connessione specifiche riportate di seguito.  
  
|Connessioni<br /><br /> proprietà|Predefinito<br /><br /> (se disponibile)|Descrizione|  
|-----------------------------|----------------------------|-----------------|  
|Provider di dati||Specifica il PROGID del provider OLE DB tramite il quale SQLXMLOLEDB esegue i comandi. A partire da SQLXML 4.0 e [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], questo provider è contenuto in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. Il valore di questa proprietà è pertanto limitato a "SQLNCLI11". Per altre informazioni, vedere [Programmazione in SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client-programming.md).|  
  
 Il provider SQLXMLOLEDB espone le proprietà dei comandi specifiche riportate di seguito.  
  
|Comando<br /><br /> proprietà|Predefinito<br /><br /> (se disponibile)|Descrizione|  
|--------------------------|----------------------------|-----------------|  
|Percorso di base|""|Specifica il percorso file di base. Tale percorso viene utilizzato per specificare la posizione dei file XSL (XML Stylesheet Language) o dei file dello schema di mapping. Il percorso del file di base viene usato anche per risolvere i percorsi relativi dei file XSL o dello schema di mapping specificati nelle proprietà XSL o Schema di mapping.<br /><br /> Per un esempio in cui viene usata questa proprietà, vedere [Executing XPath Queries &#40;SQLXMLOLEDB Provider&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-sqlxmloledb-provider.md).|  
|ClientSideXML|Falso|Impostare questa proprietà su False se si desidera che il processo di conversione del set di righe in XML venga eseguito sul client anziché sul server. Questa operazione si rivela utile quando si desidera spostare il carico delle prestazioni al livello intermedio.<br /><br /> Per un esempio in cui viene usata questa proprietà, vedere Esecuzione di query SQL &#40;provider [SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-sql-queries-sqlxmloledb-provider.md) o Esecuzione di modelli che contengono query SQL &#40;provider [SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-templates-that-contain-sql-queries-sqlxmloledb-provider.md).|  
|Tipo di contenuto||Restituisce il tipo di contenuto di output. Si tratta di una proprietà READ ONLY.<br /><br /> Questa proprietà fornisce informazioni al browser sul tipo di contenuto, ad esempio TEXT/XML, TEXT/HTML, image/jpeg e così via. Il valore di questa proprietà diventa il campo **content-type** inviato al browser come parte dell'intestazione HTTP, che contiene il tipo MIME (Multipurpose Internet Mail Extensions) del documento inviato come corpo.|  
|Schema di mapping|NULL|Se un'applicazione client esegue una query XPath su uno schema di mapping (XDR o XSD), questa proprietà viene utilizzata per specificare il nome dello schema di mapping.<br /><br /> Il percorso specificato può essere relativo (xyz/abc/Schema.xml) o assoluto (C:\Cartella\abc\Schema.xml).<br /><br /> Se viene specificato un percorso relativo, il percorso di base specificato dalla proprietà Percorso di base viene usato per risolvere il percorso relativo. Se non è stato specificato alcun percorso nella proprietà Percorso di base, il percorso relativo è relativo alla directory corrente.<br /><br /> Quando si specifica un valore per la proprietà Schema di mapping, è possibile specificare un percorso di directory locale o un URL (https://...). Se si specifica un URL, è necessario configurare WinHTTP per accedere ai server HTTP e HTTPS tramite un server proxy. A tale scopo, eseguire l'utilità Proxycfg.exe. Per ulteriori informazioni, vedere l'argomento relativo all'utilizzo dell'utilità di configurazione proxy WinHTTP in MSDN Library.<br /><br /> Per un esempio in cui viene usata questa proprietà, vedere [Executing XPath Queries &#40;SQLXMLOLEDB Provider&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-sqlxmloledb-provider.md).|  
|spazi dei nomi||Questa proprietà consente l'esecuzione delle query XPath che utilizzano gli spazi dei nomi. Per un esempio in cui viene usata questa proprietà, vedere Esecuzione di query XPath con spazi dei nomi &#40;[provider SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-with-namespaces-sqlxmloledb-provider.md).|  
|ss Stream Flags||Questa proprietà viene utilizzata per specificare tipi particolari di restrizioni di sicurezza. È ad esempio possibile che non si desideri consentire la specifica di riferimenti URL a file o di percorsi assoluti di file, ad esempio siti esterni, o che non si desideri consentire l'inserimento di query nei modelli.<br /><br /> È possibile assegnare alla proprietà i valori seguenti:<br /><br /> 1 = STREAM_FLAGS_DISALLOW_URL 2 = STREAM_FLAGS_DISALLOW_ABSOLUTE_PATH 4 = STREAM_FLAGS_DISALLOW_QUERY 8 = STREAM_FLAGS_       DONTCACHEMAPPINGSCHEMA 16 = STREAM_FLAGS_DONTCACHETEMPLATE 32 = STREAM_FLAGS_DONTCACHEXSL<br /><br /> Nella tabella che segue sono contenute informazioni aggiuntive su tali valori.|  
|xml root||Questa proprietà viene utilizzata per definire un tag radice per il codice XML risultante. Se, ad esempio, si eseguono query SQL sul database e il documento XML risultante non contiene un singolo elemento radice, il valore della proprietà viene utilizzato per aggiungere tale elemento al documento.<br /><br /> Per un esempio in cui viene usata questa proprietà, vedere [Executing SQL Queries &#40;SQLXMLOLEDB Provider&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-sql-queries-sqlxmloledb-provider.md).|  
|xsl||Questa proprietà viene utilizzata per specificare il nome file XSL quando si desidera applicare la trasformazione XSL al documento XML restituito dalla query.<br /><br /> Il percorso specificato può essere relativo (xyz/abc/XSL.xsl) o assoluto (C:\Cartella\abc\XSL.xsl).<br /><br /> Se viene specificato un percorso relativo, il percorso di base specificato dalla proprietà Percorso di base viene usato per risolvere il percorso relativo. Se non è stato specificato alcun percorso nella proprietà Percorso di base, il percorso relativo è relativo alla directory corrente.<br /><br /> Per un esempio in cui viene usata questa proprietà, vedere Applicazione di una trasformazione XSL (provider SQLXMLOLEDB).|  
  
 La tabella seguente contiene le descrizioni dei valori della proprietà ss Stream Flags.  
  
|Valore proprietà|Descrizione|  
|--------------------|-----------------|  
|STREAM_FLAGS_DISALLOW_URL|Gli URL non sono consentiti per gli schemi di mapping o XSL.|  
|STREAM_FLAGS_DISALLOW_ABSOLTE_PATH|Un percorso specificato per uno schema di mapping o per XSL deve essere relativo al percorso di base del modello stesso.|  
|STREAM_FLAGS_DISALLOW_QUERY|Le query non sono consentite in un modello.|  
|STREAM_FLAGS_DONTCACHEMAPPINGSCHEMA|Lo schema di mapping non viene memorizzato nella cache. Il valore di questa proprietà è utile durante la fase di sviluppo del database, quando gli schemi del database sono soggetti a modifiche.|  
|STREAM_FLAGS_DONTCACHETEMPLATE|I modelli non vengono memorizzati nella cache.|  
|STREAM_FLAGS_DONTCACHEXSL|XSL non viene memorizzato nella cache.|  
  
  
