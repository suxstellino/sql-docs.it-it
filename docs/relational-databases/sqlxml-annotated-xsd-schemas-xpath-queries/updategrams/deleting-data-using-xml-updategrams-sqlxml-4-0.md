---
title: Eliminazione di dati tramite updategram XML (SQLXML)
description: Informazioni sull'eliminazione di dati tramite un updategram XML in SQLXML 4.0.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- <after> block
- updategrams [SQLXML], deleting data
- <before> block
- mapping-schema attribute
- record deletions [SQLXML]
ms.assetid: 4fb116d7-7652-474a-a567-cb475a20765c
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d6923cbe4ce1968992a7944e3a4aedce41bcd7ac
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491541"
---
# <a name="deleting-data-using-xml-updategrams-sqlxml-40"></a>Eliminazione di dati mediante updategram XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Un updategram indica un'operazione di eliminazione quando un'istanza di record viene visualizzata nel blocco **\<before>** senza record corrispondenti nel **\<after>** blocco . In questo caso, l'updategram elimina il record nel **\<before>** blocco dal database.  
  
 Di seguito viene illustrato il formato dell'updategram per un'operazione di eliminazione:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema="SampleSchema.xml"]  >  
   <updg:before>  
       <ElementName />  
      [<ElementName .../>... ]  
   </updg:before>  
    [<updg:after>  
    </updg:after>]  
  </updg:sync>  
</ROOT>  
```  
  
 È possibile omettere il **\<after>** tag se l'updategram esegue solo un'operazione di eliminazione. Se non si specifica l'attributo **facoltativo mapping-schema,** l'oggetto specificato nell'updategram viene mappato a una tabella di database e gli elementi figlio o gli attributi vengono mappati alle **\<ElementName>** colonne della tabella.  
  
 Se un elemento specificato nell'updategram corrisponde a più righe della tabella o non corrisponde ad alcuna riga, l'updategram restituisce un errore e annulla l'intero **\<sync>** blocco. Un elemento dell'updategram può eliminare un solo record per volta.  
  
## <a name="examples"></a>Esempio  
 Negli esempi presentati in questa sezione viene utilizzato il mapping predefinito, ovvero non viene specificato alcuno schema di mapping nell'updategram. Per altri esempi di updategram che usano schemi di mapping, vedere Specifica di uno schema di mapping con annotazioni in un [updategram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
 Per creare esempi funzionanti usando gli esempi seguenti, è necessario soddisfare i requisiti specificati in [Requisiti per l'esecuzione di esempi SQLXML](../../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-deleting-a-record-by-using-an-updategram"></a>R. Eliminazione di un record mediante un updategram  
 Negli updategram seguenti vengono eliminati due record dalla tabella HumanResources.Shift.  
  
 In questi esempi l'updategram non specifica uno schema di mapping, pertanto utilizza il mapping predefinito nel quale il nome dell'elemento esegue il mapping a un nome di tabella e gli attributi o i sottoelementi eseguono il mapping alle colonne.  
  
 Questo primo updategram è incentrato sugli attributi e identifica due turni (giorno-sera e sera-notte) nel **\<before>** blocco. Poiché nel blocco non è presente alcun record **\<after>** corrispondente, si tratta di un'operazione di eliminazione.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
       <HumanResources.Shift ShiftID="4"  
                        Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
       <HumanResources.Shift ShiftID="5"  
                        Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
  </updg:before>  
  <updg:after>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>Per testare l'updategram  
  
1.  Esempio completo B ("Inserimento di più record tramite un updategram") in Inserimento di dati tramite [updategram XML &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md).  
  
2.  Copiare l'updategram precedente nel Blocco note e salvarlo come Updategram-RemoveShifts.xml nella stessa cartella usata per completare ("Inserimento di più record tramite un updategram") in Inserimento di dati tramite [updategram XML &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md).  
  
3.  Creare e utilizzare lo script di test SQLXML 4.0 (Sqlxml4test.vbs) per eseguire l'updategram.  
  
     Per altre informazioni, vedere [Uso di ADO per eseguire query SQLXML 4.0.](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Considerazioni sulla sicurezza di Updategram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
