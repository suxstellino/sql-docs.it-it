---
title: Oggetto SqlXmlCommand (SQLXML)
description: Informazioni sui metodi e sulle proprietà dell'oggetto SqlXmlCommand.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- void ExecuteNonQuery() method
- namespaces property
- Stream ExecuteStream() method
- CommandType property
- RootTag property
- OutputEncoding property
- XmlReader ExecuteXmlReader() method
- Managed Classes [SQLXML], SqlXmlCommand object
- SQLXML Managed Classes, SqlXmlCommand object
- Base Path property
- void ClearParameters() method
- public void ExecuteToStream(Stream outputStream) method
- SqlXmlCommand object
- CommandText property
- XslPath property
- SchemaPath property
- SqlXmlParameter CreateParameter() method
- ClientSideXML property
- CommandStream property
ms.assetid: c1f9e0bb-a89d-4d6a-a96e-289ef516a3a6
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c5736ffab10bc0be104f06dbd6627d48aa45cd98
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439662"
---
# <a name="sqlxml-managed-classes---sqlxmlcommand-object"></a>Classi gestite SQLXML - Oggetto SqlXmlCommand
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Si tratta del costruttore per l'oggetto SqlXmlCommand:  
  
```  
public SqlXmlCommand(string cnString)  
```  
  
 Dove `cnString` è la stringa di connessione ADO o OLEDB che identifica il server, il database e le informazioni di accesso, ad esempio `Provider=SQLOLEDB; Server=(local); database=AdventureWorks; Integrated Security=SSPI"` .  
  
 Nella stringa di connessione `Provider` deve essere SQLOLEDB e `Data Provider` non deve essere incluso nella stringa del provider.  
  
 Per un esempio funzionante, vedere [esecuzione di query SQL &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
## <a name="methods"></a>Metodi  
 L'oggetto SqlXmlCommand supporta diversi metodi, inclusi i metodi seguenti per l'esecuzione di un comando:  
  
 void ExecuteNonQuery ()  
 Esegue il comando, ma non restituisce alcun valore. Questo metodo è utile se si desidera eseguire un comando senza query, ovvero un comando che non restituisca alcun valore. Un esempio di questo comportamento consiste nell'eseguire un updategram o un Diffgram che aggiorna record ma non restituisce alcun valore.  
  
 Flusso ExecuteStream ()  
 Restituisce un nuovo oggetto flusso. Questo metodo è utile quando si desidera che i risultati della query vengano restituiti in un nuovo flusso. Per un esempio funzionante, vedere [esecuzione di query SQL &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 public void ExecuteToStream (Stream outputStream)  
 Scrive i risultati della query in un flusso esistente. Questo metodo è utile quando si dispone di un flusso a cui è necessario aggiungere i risultati, ad esempio per fare in modo che i risultati della query vengano scritti in System. Web. HttpResponse. OutputStream. Per un esempio funzionante, vedere [esecuzione di query SQL &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 ExecuteXmlReader XmlReader ()  
 Restituisce un oggetto XmlReader. È possibile utilizzare questo metodo per modificare direttamente i dati nell'oggetto XmlReader o per collegare l'architettura concatenabile di System.Xml. Per ulteriori informazioni, vedere la documentazione di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework. Per un esempio funzionante, vedere [esecuzione di query SQL tramite il metodo ExecuteXmlReader](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-by-using-the-executexmlreader-method.md).  
  
 L'oggetto SqlXmlCommand supporta inoltre questi metodi aggiuntivi:  
  
 SqlXmlParameter CreateParameter ()  
 Crea un oggetto SqlXmlParameter. È possibile impostare i valori per i parametri *Name* e *value* di questo oggetto. Questo metodo è utile se si desidera passare parametri a un comando. Per un esempio funzionante, vedere [esecuzione di query SQL &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 void ClearParameters ()  
 Cancella i parametri creati per un oggetto comando specifico. Questo metodo è utile se si desidera eseguire più query sullo stesso oggetto comando.  
  
## <a name="properties"></a>Proprietà  
 L'oggetto SqlXmlCommand supporta anche queste proprietà:  
  
 ClientSideXml  
 Se impostata su True, specifica che la conversione del set di righe in XML deve verificarsi nel client anziché nel server. Questa proprietà è utile quando si desidera spostare il carico delle prestazioni nel livello intermedio. La proprietà consente inoltre di eseguire il wrapping delle stored procedure esistenti con FOR XML per ottenere output XML.  
  
 SchemaPath  
 Nome dello schema di mapping insieme al percorso di directory, ad esempio C:\x\y\Schema.xml. Questa proprietà è utile per specificare uno schema di mapping per query XPath. Il percorso specificato può essere assoluto o relativo. Se il percorso è relativo, il percorso di base specificato nel percorso di base viene usato per risolvere il percorso relativo. Se non è specificato alcun percorso di base, il percorso relativo è tale rispetto alla directory corrente. Per un esempio funzionante, vedere [accesso alla funzionalità SQLXML nell'ambiente .NET](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md).  
  
 XslPath  
 Nome del file XSL insieme al percorso di directory. Il percorso specificato può essere assoluto o relativo. Se il percorso è relativo, il percorso di base specificato nel percorso di base viene usato per risolvere il percorso relativo. Se non è specificato alcun percorso di base, il percorso relativo è tale rispetto alla directory corrente. Per un esempio funzionante, vedere [applying a XSL Transformation &#40;SQLXML Managed classes&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/applying-an-xsl-transformation-sqlxml-managed-classes.md).  
  
 Percorso di base  
 Percorso di base (un percorso di directory). Questa proprietà è utile per la risoluzione di un percorso relativo specificato per un file XSL (tramite la proprietà XslPath), un file di schema di mapping (tramite la proprietà SchemaPath) o un riferimento allo schema esterno in un modello XML (specificato utilizzando l'attributo **mapping-schema** ).  
  
 OutputEncoding  
 Specifica la codifica per il flusso restituito all'esecuzione del comando. Questa proprietà è utile per richiedere una codifica specifica per il flusso restituito. Alcune codifiche di uso comune sono UTF-8, ANSI e Unicode. La codifica predefinita è UTF-8.  
  
 Spazi dei nomi  
 Consente l'esecuzione di query XPath che utilizzano spazi dei nomi. Per ulteriori informazioni sulle query XPath con spazi dei nomi, vedere [esecuzione di query XPath con spazi dei nomi &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-with-namespaces-sqlxml-managed-classes.md). Per un esempio funzionante, vedere [esecuzione di query XPath &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-sqlxml-managed-classes.md).  
  
 RootTag  
 Fornisce il singolo elemento radice per un documento XML generato dall'esecuzione di comandi. Un documento XML valido richiede un singolo tag di livello radice. Se il comando eseguito genera un frammento XML, senza singolo elemento di livello principale, è possibile specificare un elemento radice per il documento XML restituito. Per un esempio funzionante, vedere [applying a XSL Transformation &#40;SQLXML Managed classes&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/applying-an-xsl-transformation-sqlxml-managed-classes.md).  
  
 CommandText  
 Testo del comando. Questa proprietà viene utilizzata per specificare il testo del comando che si desidera eseguire. Per un esempio funzionante, vedere [esecuzione di query SQL &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 CommandStream  
 Flusso del comando. Questa proprietà è utile se si desidera eseguire un comando da un file, ad esempio un modello XML. Quando si usa CommandStream, sono supportati solo i valori CommandType **"template"**, **"updategram"** e **"DiffGram"** . Per un esempio funzionante, vedere [esecuzione di file modello tramite la proprietà CommandStream](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandstream-property.md).  
  
 CommandType  
 Identifica il tipo di comando. Questa proprietà viene utilizzata per specificare il tipo di comando che si desidera eseguire. I valori nella tabella seguente determinano il tipo del comando. Per un esempio funzionante, vedere [accesso alla funzionalità SQLXML nell'ambiente .NET](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md).  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|SqlXmlCommandType. SQL|Esegue un comando SQL, ad esempio `SELECT * FROM Employees FOR XML AUTO`.|  
|SqlXmlCommandType. XPath|Esegue un comando XPath, ad esempio `Employees[@EmployeeID=1]`.|  
|SqlXmlCommandType. template|Esegue un modello XML.|  
|SqlXmlCommandType. TemplateFile|Esegue un file di modello nel percorso specificato.|  
|SqlXmlCommandType. UpdateGram|Esegue un updategram.|  
|SqlXmlCommandType. DiffGram|Esegue un Diffgram.|  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto SqlXmlParameter &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlparameter-object.md)   
 [Oggetto SqlXmlAdapter &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmladapter-object.md)  
  
  
