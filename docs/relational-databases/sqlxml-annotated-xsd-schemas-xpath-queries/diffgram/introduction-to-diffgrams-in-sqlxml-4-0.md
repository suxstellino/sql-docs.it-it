---
title: Introduzione ai DiffGram in SQLXML 4.0
description: Informazioni sul formato, le annotazioni e la logica di elaborazione dei DiffGram in SQLXML 4.0.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotations [SQLXML]
- DiffGrams [SQLXML], about DiffGrams
ms.assetid: 1902d67f-baf3-46e6-a36c-b24b5ba6f8ea
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 187c3bab080050c518f34e4c184849ce1b2d6e95
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490537"
---
# <a name="introduction-to-diffgrams-in-sqlxml-40"></a>Introduzione ai DiffGram in SQLXML 4.0
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene fornita una breve introduzione ai DiffGram.  
  
## <a name="diffgram-format"></a>Formato DiffGram  
 Il formato DiffGram generale è il seguente:  
  
```  
<?xml version="1.0"?>  
<diffgr:diffgram   
         xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"  
         xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1"  
         xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
   <DataInstance>  
      ...  
   </DataInstance>  
   [<diffgr:before>  
        ...  
   </diffgr:before>]  
  
   [<diffgr:errors>  
        ...  
   </diffgr:errors>]  
</diffgr:diffgram>  
```  
  
 Il formato DiffGram è costituito dai blocchi seguenti:  
  
 **\<DataInstance>**  
 Il nome di questo elemento, **DataInstance**, viene usato a scopo di spiegazione in questa documentazione. Ad esempio, se il DiffGram è stato generato da un set di dati nel .NET Framework, come nome di questo elemento verrà usato il valore della proprietà **Name** del set di dati. Questo blocco contiene tutti i dati rilevanti dopo la modifica, inclusi eventualmente i dati che non sono stati modificati. La logica di elaborazione DiffGram ignora gli elementi in questo blocco per cui non è specificato **l'attributo diffgr:hasChanges.**  
  
 **\<diffgr:before>**  
 Questo blocco facoltativo contiene le istanze dei record originali (elementi) che devono essere aggiornate o eliminate. Tutte le tabelle di database modificate (aggiornate o eliminate) da DiffGram devono essere visualizzate come elementi di primo livello nel **\<before>** blocco .  
  
 **\<diffgr:errors>**  
 Questo blocco facoltativo viene ignorato dalla logica di elaborazione DiffGram.  
  
## <a name="diffgram-annotations"></a>Annotazioni DiffGram  
 Queste annotazioni sono definite nello spazio dei nomi DiffGram **"urn:schemas-microsoft-com:xml-diffgram-01":**  
  
 **id**  
 Questo attributo viene usato per associare gli elementi nei **\<before>** blocchi e **\<DataInstance>** .  
  
 **hasChanges**  
 Per un'operazione di inserimento o aggiornamento, Il DiffGram deve specificare questo attributo con il valore **inserito** o **modificato.** Se questo attributo non è presente, l'elemento corrispondente in viene ignorato dalla logica di elaborazione e non **\<DataInstance>** viene eseguito alcun aggiornamento. Per esempi funzionanti, vedere [Esempi di DiffGram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).  
  
 **Parentid**  
 Questo attributo viene utilizzato per specificare relazioni padre-figlio tra gli elementi nel DiffGram. Questo attributo viene visualizzato solo nel \<before> blocco . e viene utilizzato da SQLXML durante l'applicazione di aggiornamenti. La relazione padre-figlio viene utilizzata per determinare l'ordine in cui gli elementi vengono elaborati nel DiffGram.  
  
## <a name="understanding-the-diffgram-processing-logic"></a>Informazioni sulla logica di elaborazione DiffGram  
 La logica di elaborazione DiffGram utilizza determinate regole per stabilire se un'operazione è di inserimento, aggiornamento o eliminazione. Tali regole sono descritte nella tabella seguente:  
  
|Operazione|Descrizione|  
|---------------|-----------------|  
|Insert|Un DiffGram indica un'operazione di inserimento quando un elemento viene visualizzato nel blocco ma non nel blocco corrispondente e viene specificato l'attributo **\<DataInstance>** **\<before>** **diffgr:hasChanges** (**diffgr:hasChanges=inserted**) nell'elemento. In questo caso, il DiffGram inserisce l'istanza del record specificata nel blocco **\<DataInstance>** nel database.<br /><br /> Se **l'attributo diffgr:hasChanges** non è specificato, l'elemento viene ignorato dalla logica di elaborazione e non viene eseguito alcun inserimento. Per esempi funzionanti, vedere [Esempi di DiffGram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).|  
|Aggiornamento|Il DiffGram indica un'operazione di aggiornamento quando nel blocco è presente un elemento per il quale è presente un elemento corrispondente nel blocco (ovvero entrambi gli elementi hanno un attributo diffgr:id con lo stesso valore) e l'attributo \<before> **\<DataInstance>** **diffgr:hasChanges**   viene specificato con il valore modificato nell'elemento nel **\<DataInstance>** blocco.<br /><br /> Se **l'attributo diffgr:hasChanges** non è specificato nell'elemento nel blocco , la logica di elaborazione restituirà **\<DataInstance>** un errore. Per esempi funzionanti, vedere [Esempi di DiffGram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).<br /><br /> Se **diffgr:parentID** viene specificato nel blocco , la relazione padre-figlio degli elementi specificati da parentID viene usata per determinare l'ordine in cui vengono aggiornati **\<before>** i record. |  
|Elimina|Un DiffGram indica un'operazione di eliminazione quando un elemento viene visualizzato nel blocco **\<before>** ma non nel blocco **\<DataInstance>** corrispondente. In questo caso, Il DiffGram elimina dal database l'istanza del record specificata **\<before>** nel blocco . Per esempi funzionanti, vedere [Esempi di DiffGram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).<br /><br /> Se **diffgr:parentID** viene specificato nel blocco , la relazione padre-figlio degli elementi specificati da parentID viene usata per determinare l'ordine in cui i record **\<before>** vengono eliminati. |  
  
> [!NOTE]  
>  Non è possibile passare parametri ai DiffGram.  
  
  
