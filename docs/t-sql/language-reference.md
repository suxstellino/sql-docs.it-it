---
description: Guida di riferimento a Transact-SQL (Motore di database)
title: Guida di riferimento a Transact-SQL (motore di database) | Microsoft Docs
ms.custom: ''
ms.date: 04/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- sql13.tsqlref.f1
- devlang-tsql
helpviewer_keywords:
- Transact-SQL
ms.assetid: dbba47d7-e08e-4435-b876-35dced1f325d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 00d01b9b0646848ceb8b1deff2ab7cbe16518d0f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095851"
---
# <a name="transact-sql-reference-database-engine"></a>Guida di riferimento a Transact-SQL (Motore di database)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]

Questo argomento offre le informazioni di base sulla ricerca e l'uso degli argomenti della Guida di riferimento a Microsoft [!INCLUDE[tsql](../includes/tsql-md.md)] (T-SQL). T-SQL è fondamentale per l'uso dei prodotti e dei servizi Microsoft SQL. Tutti gli strumenti e le applicazioni che comunicano con un database SQL inviano comandi T-SQL.  

## <a name="t-sql-compliance-to-sql-standard"></a>Conformità di T-SQL agli standard SQL
Per documenti tecnici dettagliati sul modo in cui vengono implementati determinati standard in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vedere la [documentazione relativa al supporto degli standard di Microsoft SQL Server](/openspecs/sql_standards/ms-sqlstandlp/89fb00b1-4b9e-4296-92ce-a2b3f7ca01d2).

## <a name="tools-that-use-t-sql"></a>Strumenti che usano T-SQL
Gli strumenti Microsoft che usano comandi T-SQL includono:

- [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [sqlcmd](../tools/sqlcmd-utility.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)
  
## <a name="locate-the-transact-sql-reference-topics"></a>Cercare gli argomenti della Guida di riferimento a Transact-SQL  
Per trovare gli argomenti relativi a T-SQL, usare la ricerca nella parte superiore destra della pagina oppure usare il sommario sul lato sinistro della pagina. È anche possibile digitare una parola chiave T-SQL nella finestra dell'editor di query di Management Studio e premere F1. 
  
## <a name="find-system-views"></a>Trovare le visualizzazioni di sistema
Per trovare le tabelle, le visualizzazioni, le funzioni e le procedure di sistema, vedere i collegamenti seguenti disponibili nella sezione relativa all'[uso dei database relazionali](../relational-databases/databases/databases.md) della documentazione di SQL.

- [Viste del catalogo di sistema](../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Viste di compatibilità di sistema](../relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)
- [Viste a gestione dinamica (DMV) di sistema](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [Funzioni di sistema](../relational-databases/system-functions/system-functions-category-transact-sql.md)
- [Viste degli schemi delle informazioni di sistema](../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [Stored procedure di sistema](../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [Tabelle di sistema](../relational-databases/system-tables/system-tables-transact-sql.md)

## <a name="applies-to-references"></a>Riferimenti "Si applica a"  
 Gli argomenti della Guida di riferimento a T-SQL riguardano più versioni di SQL Server, a partire dalla versione 2008, inclusi altri servizi SQL di Azure. Nella parte superiore di ogni argomento è presente una sezione che indica i prodotti e i servizi che supportano l'oggetto dell'argomento. 

Ad esempio, questo argomento che si applica a tutte le versioni ha l'etichetta seguente. 
  
 [!INCLUDE[tsql-appliesto-ss2008-all_md](../includes/tsql-appliesto-ss2008-all-md.md)]   

L'etichetta seguente invece indica un argomento che si applica solo ad Azure Synapse Analytics e a Parallel Data Warehouse.

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw_md](../includes/applies-to-version/asa-pdw.md)]

In alcuni casi l'argomento si applica a un prodotto o servizio, ma non sono supportati tutti gli argomenti. In questo caso alcune sezioni **Si applica a** aggiuntive vengono inserite nelle descrizioni di argomento appropriate nel corpo dell'argomento.  
 
## <a name="get-help-from-microsoft-q--a"></a>Ottenere assistenza da Domande e risposte Microsoft  
Per il supporto online, vedere il [forum di Transact-SQL in Domande e risposte Microsoft](/answers/topics/sql-server-transact-sql.html).  
 
## <a name="see-other-language-references"></a>Vedere altre guide di riferimento al linguaggio
La documentazione di SQL include anche le guide di riferimento al linguaggio seguenti:
  
- [Guida di riferimento al linguaggio XQuery](../xquery/xquery-language-reference-sql-server.md)
- [Guida di riferimento al linguaggio Integration Services](../integration-services/integration-services-language-reference.md)
- [Guida di riferimento al linguaggio della replica](../relational-databases/replication/replication-language-reference.md)
- [Guida di riferimento al linguaggio di Analysis Services](../mdx/multidimensional-expressions-mdx-reference.md)  

## <a name="next-steps"></a>Passaggi successivi
Dopo aver compreso come trovare gli argomenti della Guida di riferimento a T-SQL, è possibile:

- Eseguire una breve esercitazione sulla scrittura in T-SQL. Vedere [Esercitazione: Scrittura di istruzioni Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md). 
- Visualizzare le [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  

