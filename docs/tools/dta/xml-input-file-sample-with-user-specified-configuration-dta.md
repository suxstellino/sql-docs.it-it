---
title: Esempio di file di input XML con configurazione specificata dall'utente
description: Questo articolo contiene un esempio di file di input XML con configurazione specificata dall'utente che può essere usato per ottimizzare i carichi di lavoro da usare con Ottimizzazione guidata motore di database.
titleSuffix: DTA
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: b29c9716-e5c3-4003-9efb-3ade2197b630
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: fc4e0f7e4816a6aec1cdbe41d2d4c84c8af55711
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339999"
---
# <a name="xml-input-file-sample-with-user-specified-configuration-dta"></a>Esempio di file di input XML con configurazione specificata dall'utente (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Copiare e incollare questo esempio di file di input XML che contiene una configurazione specificata dall'utente con l'elemento **Configuration** nell'editor di testo o nell'editor XML preferito. In questo modo sarà possibile eseguire analisi di simulazione che comportano l'uso dell'elemento **Configuration** per specificare un set di strutture di progettazione fisica ipotetiche per il database che si vuole ottimizzare. Per sapere se è possibile migliorare le prestazioni di elaborazione delle query, verrà quindi utilizzata l'Ottimizzazione guidata motore di database per analizzare gli effetti dell'esecuzione di un carico di lavoro rispetto a questa configurazione ipotetica. Questo tipo di analisi ha il vantaggio di valutare la nuova configurazione senza l'overhead dovuto all'effettiva implementazione. Se la configurazione ipotetica non consente di ottenere i miglioramenti delle prestazioni desiderati, è possibile modificarla e analizzarla di nuovo finché non verranno prodotti i risultati necessari.  
  
 Dopo avere copiato questo esempio nello strumento di modifica desiderato, sostituire i valori specificati per gli elementi **Server**, **Database**, **Schema**, **Table**, **Workload**, **TuningOptions** e **Configuration** con i valori per la sessione di ottimizzazione specifica. Per altre informazioni su tutti gli attributi e gli elementi figlio che è possibile usare con questi elementi, vedere [Guida di riferimento ai file di input XML &#40;Ottimizzazione guidata motore di database&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md). Nell'esempio seguente viene utilizzato solo un subset delle opzioni relative agli attributi e agli elementi figlio disponibili.  
  
## <a name="code"></a>Codice  
  
```  
<?xml version="1.0" encoding="utf-16" ?>  
<DTAXML xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="https://schemas.microsoft.com/sqlserver/2004/07/dta">  
  <DTAInput>  
    <Server>  
      <Name>MyServerName</Name>  
<!-- To tune multiple databases, list them and their associated tables in the following section. -->  
      <Database>  
        <Name>MyDatabaseName</Name>  
        <Schema>  
          <Name>MyDatabaseSchemaName</Name>  
<!-- You can list as many tables as necessary in the following section. -->  
          <Table>  
            <Name>MyTableName1</Name>  
          </Table>  
          <Table>  
            <Name>MyTableName2</Name>  
          </Table>  
        </Schema>  
      </Database>  
    </Server>  
    <Workload>  
<!-- The following element specifies a workload file, which can be a trace file or a Transact-SQL script file. -->  
      <File>c:\PathToYourWorkloadFile</File>  
    </Workload>  
    <TuningOptions>  
      <TuningTimeInMin>180</TuningTimeInMin>  
      <StorageBoundInMB>10000</StorageBoundInMB>  
      <FeatureSet>IDX_IV</FeatureSet>  
      <Partitioning>NONE</Partitioning>  
      <KeepExisting>NONE</KeepExisting>  
      <OnlineIndexOperation>OFF</OnlineIndexOperation>  
    </TuningOptions>  
    <Configuration SpecificationMode="Absolute">  
      <Server>  
        <Name>MyServerName</Name>  
          <Database>  
            <Name>MyDatabaseName</Name>  
            <Schema>  
              <Name>MyDatabaseSchemaName</Name>  
                <Table>  
                  <Name>MyTableName1</Name>  
                  <Recommendation>  
                    <Create>  
                      <Index Clustered="true" Unique="false" Online="false" IndexSizeInMB="873.75">  
                        <Name>MyIndexName</Name>  
                        <Column Type="KeyColumn" SortOrder="Ascending">  
                          <Name>MyColumnName1</Name>  
                        </Column>  
                        <Filegroup>MyFileGroupName1</FileGroup>  
                      </Index>  
                    </Create>  
                  </Recommendation>  
                </Table>  
            </Schema>  
          </Database>  
      </Server>  
    </Configuration>  
  </DTAInput>  
</DTAXML>  
```  
  
## <a name="comments"></a>Commenti  
  
-   L'eliminazione di strutture di progettazione fisica non è supportata se si specifica la modalità **Absolute** per l'elemento **Configuration** (`Configuration SpecificationMode="Absolute"`).  
  
-   La stessa struttura di progettazione fisica non può essere creata ed eliminata in modalità **Relative** o **Absolute** dell'elemento **Configuration** .  
  
## <a name="see-also"></a>Vedere anche  
 [Avvio e utilizzo di Ottimizzazione guidata motore di database](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [Visualizzare e utilizzare l'output di Ottimizzazione guidata motore di database](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)   
 [Guida di riferimento ai file di input XML&#40; (Ottimizzazione guidata motore di database)&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
