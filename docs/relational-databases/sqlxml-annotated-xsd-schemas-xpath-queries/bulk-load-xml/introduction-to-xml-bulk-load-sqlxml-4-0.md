---
title: Introduzione al caricamento bulk XML (SQLXML)
description: Informazioni sull'utilità caricamento bulk XML, un oggetto COM autonomo in SQLXML 4.0 che consente di caricare dati XML semistrutturati in Microsoft SQL Server tabelle.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- nontransacted XML Bulk Load operations
- XML Bulk Load [SQLXML], about XML Bulk Load
- bulk load [SQLXML], about bulk load
- transacted XML Bulk Load operations
- streaming XML data
ms.assetid: 38bd3cbd-65ef-4c23-9ef3-e70ecf6bb88a
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c865c0120497d6a6a67858735f6ee96bdf3539e5
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490597"
---
# <a name="introduction-to-xml-bulk-load-sqlxml-40"></a>Introduzione al caricamento bulk XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Il caricamento bulk XML è un oggetto COM autonomo che consente di caricare dati XML semistrutturati nelle tabelle di Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 È possibile inserire dati XML in un database [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizzando un'istruzione INSERT e la funzione OPENXML. L'utilità di caricamento bulk fornisce tuttavia prestazioni migliori quando è necessario inserire grandi quantità di dati XML.  
  
 Il metodo Execute del modello a oggetti di caricamento bulk XML accetta due parametri:  
  
-   Uno schema XSD (XML Schema Definition) o uno schema XDR (XML-Data Reduced) con annotazioni. L'utilità di caricamento bulk XML interpreta questo schema di mapping e le annotazioni specificate nello schema per identificare le tabelle di SQL Server nelle quali devono essere inseriti i dati XML.  
  
-   Un documento o un frammento di documento XML (un frammento di documento è un documento privo di un singolo elemento di livello superiore). È possibile specificare un nome file o un flusso dal quale il caricamento bulk XML può leggere.  
  
 Il caricamento bulk XML interpreta lo schema di mapping e identifica le tabelle nelle quali devono essere inseriti i dati XML.  
  
 Si presuppone che l'utente abbia familiarità con le caratteristiche [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] seguenti:  
  
-   Schemi XSD e XDR annotati. Per altre informazioni sugli schemi XSD con annotazioni, vedere [Introduction to Annotated XSD Schemas &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md). Per informazioni sugli schemi XDR con annotazioni, vedere [Annotated XDR Schemas &#40;Deprecated in SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
-   Meccanismi di inserimento bulk [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], quali l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] BULK INSERT e l'utilità bcp. Per altre informazioni, vedere BULK INSERT &#40;[Transact-SQL&#41;](../../../t-sql/statements/bulk-insert-transact-sql.md) [e utilità bcp](../../../tools/bcp-utility.md).  
  
## <a name="streaming-of-xml-data"></a>Flusso dei dati XML  
 Poiché le dimensioni del documento XML di origine possono essere elevate, l'intero documento non viene letto in memoria per l'elaborazione del caricamento bulk. Il caricamento bulk XML interpreta invece i dati XML come un flusso e li legge. Durante la lettura dei dati, l'utilità identifica le tabelle di database, genera i record appropriati dall'origine dati XML, quindi invia i record a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per l'inserimento.  
  
 Ad esempio, il documento XML di origine seguente è costituito **\<Customer>** da elementi ed elementi **\<Order>** figlio:  
  
```  
<Customer ...>  
    <Order.../>  
    <Order .../>  
     ...  
</Customer>  
...  
```  
  
 Durante la lettura dell'elemento, il caricamento bulk XML **\<Customer>** genera un record per Customertable. Quando legge il **\</Customer>** tag di fine, il caricamento bulk XML inserisce tale record nella tabella in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Allo stesso modo, quando legge l'elemento , il caricamento bulk XML genera un record per Ordertable e quindi lo inserisce nella tabella durante la lettura del **\<Order>** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tag di **\</Order>** fine.  
  
## <a name="transacted-and-nontransacted-xml-bulk-load-operations"></a>Operazioni di caricamento bulk XML in transazioni e non in transazioni  
 Il caricamento bulk XML può funzionare in modalità transazionale e non transazionale. Le prestazioni sono in genere ottimali se si sta caricando in blocco in modalità non transazionale, ad esempio se la proprietà Transaction è impostata su FALSE e si verifica una delle condizioni seguenti:  
  
-   Le tabelle nelle quali si esegue il caricamento bulk dei dati sono vuote e senza indici.  
  
-   Le tabelle includono dati e indici univoci.  
  
 L'approccio non transazionale non garantisce un rollback in caso di errore del processo di caricamento bulk, anche se possono verificarsi rollback parziali. Il caricamento bulk non transazionale è consigliabile quando il database è vuoto. In caso di errore, pertanto, è possibile eseguire una pulizia del database e riavviare il caricamento bulk XML.  
  
> [!NOTE]  
>  In modalità non transazionale il caricamento bulk XML utilizza una transazione interna predefinita e ne esegue il commit. Quando la proprietà Transaction è impostata su TRUE, il caricamento bulk XML non chiama il commit su questa transazione.  
  
 Se la proprietà Transaction è impostata su TRUE, il caricamento bulk XML crea file temporanei, uno per ogni tabella identificata nello schema di mapping. Il caricamento bulk XML archivia innanzitutto i record del documento XML di origine in questi file temporanei. Quindi, un'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] BULK INSERT recupera tali record dai file e li archivia nelle tabelle corrispondenti. È possibile specificare il percorso di questi file temporanei usando la proprietà TempFilePath. In questo caso, è necessario assicurarsi che l'account di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizzato con il caricamento bulk XML disponga di accesso a tale percorso. Se la proprietà TempFilePath non è specificata, per creare i file temporanei viene usato il percorso di file predefinito specificato nella variabile di ambiente TEMP.  
  
 Se la proprietà Transaction è impostata su FALSE (impostazione predefinita), il caricamento bulk XML usa l'interfaccia OLE DB IRowsetFastLoad per eseguire il caricamento bulk dei dati.  
  
 Se la proprietà ConnectionString imposta la stringa di connessione e la proprietà Transaction è impostata su TRUE, il caricamento bulk XML opera nel proprio contesto di transazione. ad esempio avvia la propria transazione ed esegue il commit o il rollback nel modo appropriato.  
  
 Se la proprietà ConnectionCommand imposta la connessione con un oggetto connessione esistente e la proprietà Transaction è impostata su TRUE, il caricamento bulk XML non esegue rispettivamente un'istruzione COMMIT o ROLLBACK in caso di esito positivo o negativo. In presenza di un errore, il caricamento bulk XML restituisce il messaggio di errore appropriato. La decisione di eseguire un'istruzione COMMIT o ROLLBACK è delegata al client che ha avviato il caricamento bulk. L'oggetto connessione usato per il caricamento bulk XML deve essere di tipo ICommand o essere un oggetto comando ADO.  
  
 In SQLXML 4.0 non è possibile usare un oggetto ConnectionObject con la proprietà Transaction impostata su FALSE. La modalità non traslata non è supportata con un oggetto ConnectionObject perché non è possibile aprire più di un'interfaccia IRowsetFastLoad in una sessione passata.  
  
  
