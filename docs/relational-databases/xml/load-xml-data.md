---
title: Caricare dati XML | Microsoft Docs
description: Vengono illustrati diversi metodi per il trasferimento di dati XML nei database di SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- XML data [SQL Server], loading
- loading XML data
ms.assetid: d1741e8d-f44e-49ec-9f14-10208b5468a7
author: rothja
ms.author: jroth
ms.openlocfilehash: 640007b28617c9bc703c9850be2be2a3a9251bf5
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488212"
---
# <a name="load-xml-data"></a>Caricamento dati XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile trasferire dati XML in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] in diversi modi. Ad esempio:  
  
-   Se i dati si trovano in una colonna di tipo [n]text o image in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile importare la tabella utilizzando [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Modificare il tipo di colonna in XML utilizzando l'istruzione ALTER TABLE.  
  
-   È possibile eseguire una copia bulk dei dati da un altro database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando bcp out e quindi eseguire un inserimento bulk dei dati nel database di una versione successiva usando bcp in.  
  
-   Se i dati si trovano in colonne relazionali in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , creare una nuova tabella con una colonna di tipo [n]text e, facoltativamente, una colonna chiave primaria per un identificatore di riga. Usare la programmazione sul lato client per recuperare i dati XML generati nel server con FOR XML e scriverli nella colonna **[n]text** . Usare quindi le tecniche illustrate in precedenza per trasferire i dati in un database di una versione successiva. È possibile scegliere di scrivere i dati XML direttamente in una colonna XML nel database di una versione successiva.  
  
## <a name="bulk-loading-xml-data"></a>Caricamento bulk di dati XML  
 È possibile eseguire un caricamento bulk dei dati XML nel server utilizzando le funzionalità per il caricamento bulk disponibili in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio bcp. OPENROWSET consente di caricare dati in una colonna XML da uno o più file, come illustrato nell'esempio seguente.  
  
##### <a name="example-loading-xml-from-files"></a>Esempio: Caricamento di dati XML da un file  
 In questo esempio viene illustrato l'inserimento di una riga nella tabella T. Il valore della colonna XML viene caricato dal file C:\MyFile\xmlfile.xml come CLOB e alla colonna di tipo integer viene fornito il valore 10.  
  
```  
INSERT INTO T  
SELECT 10, xCol  
FROM    (SELECT *      
    FROM OPENROWSET (BULK 'C:\MyFile\xmlfile.xml', SINGLE_CLOB)   
 AS xCol) AS R(xCol)  
```  
  
## <a name="text-encoding"></a>Codifica testo  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] archivia i dati XML in formato Unicode (UTF-16). I dati XML recuperati dal server vengono restituiti con la codifica UTF-16. Se si desidera una codifica diversa, sarà necessario eseguire la conversione appropriata sui dati recuperati. Per i dati XML viene talvolta utilizzata una codifica diversa. In questo caso è necessario prestare particolare attenzione durante il caricamento dei dati. Ad esempio:  
  
-   Se il testo XML è in formato Unicode (UCS-2, UTF-16), sarà possibile assegnarlo senza problemi a una colonna, una variabile o un parametro XML.  
  
-   Se viene utilizzata una codifica implicita diversa da Unicode, a causa della tabella codici di origine, la tabella codici per le stringhe nel database dovrà essere uguale o compatibile con i punti di codice da caricare. Se necessario, utilizzare COLLATE. Se tale tabella codici non esiste sul server, sarà necessario aggiungere una dichiarazione XML esplicita con la codifica appropriata.  
  
-   Per una codifica esplicita usare il tipo di dati **varbinary()** , che non interagisce in alcun modo con le tabelle codici, oppure usare un tipo stringa della tabella codici appropriata. Assegnare quindi i dati a una colonna, a una variabile o a un parametro XML.  
  
### <a name="example-explicitly-specifying-an-encoding"></a>Esempio: Impostazione di una codifica in modo esplicito  
 Si consideri un documento XML di nome vcdoc, archiviato come **varchar(max)** e che non include una dichiarazione XML esplicita. L'istruzione seguente aggiunge una dichiarazione XML con la codifica "iso8859-1", concatena il documento XML, esegue il cast del risultato a **varbinary(max)** , in modo da mantenere la rappresentazione dei byte e infine esegue il cast al tipo di dati XML. Questo consente al processore XML di analizzare i dati in base alla codifica specificata, "iso8859-1", e di generare la rappresentazione UTF-16 corrispondente per i valori stringa.  
  
```  
SELECT CAST(   
CAST (('<?xml version="1.0" encoding="iso8859-1"?>'+ vcdoc) AS VARBINARY (MAX))   
 AS XML)  
```  
  
### <a name="string-encoding-incompatibilities"></a>Stringa che codifica le incompatibilità  
 Se si copia e si incolla XML come un valore letterale stringa nella finestra dell'editor di query di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], potrebbero verificarsi incompatibilità di codifica delle stringhe [N]VARCHAR, che dipendono dalla codifica dell'istanza XML utilizzata. In molti casi, è possibile rimuovere la dichiarazione XML. Ad esempio:  
  
```  
<?xml version="1.0" encoding="UTF-8"?>  
  <xsd:schema ...  
```  
  
 È quindi consigliabile inserire una N per rendere l'istanza XML un'istanza di Unicode. Ad esempio:  
  
```  
-- Assign XML instance to a variable.  
DECLARE @X XML  
SET @X = N'...'  
-- Insert XML instance into an xml type column.  
INSERT INTO T VALUES (N'...')  
-- Create an XML schema collection  
CREATE XML SCHEMA COLLECTION XMLCOLL1 AS N'<xsd:schema ... '  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
