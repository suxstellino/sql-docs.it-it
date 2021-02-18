---
title: Completare frammenti di Transact-SQL
description: Un frammento Transact-SQL è un modello di codice. Informazioni su come personalizzarne l'uso inserendo il contenuto in corrispondenza dei punti di sostituzione e aggiungendo elementi di sintassi al modello.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- IntelliSense [SQL Server], completing snippets
- snippets [Transact-SQL], completing
- Transact-SQL snippets, completing
ms.assetid: a8316a58-bb57-485e-845f-84c23360314c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a601f7fe6192a7a5fd1395cb95c2eabf6285dfc5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354721"
---
# <a name="complete-transact-sql-snippets"></a>Completare frammenti di Transact-SQL
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Dopo aver inserito un frammento di codice [!INCLUDE[tsql](../../includes/tsql-md.md)] in uno script, è necessario modificare il contenuto del frammento per compilare un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] completa.  
  
## <a name="completing-snippets"></a>Completamento di frammenti  
 Quando si aggiunge un frammento [!INCLUDE[tsql](../../includes/tsql-md.md)] allo script, l'istruzione del frammento inserito include uno o più punti di sostituzione, evidenziati. Se si posiziona il puntatore del mouse su un punto di sostituzione, viene visualizzata una descrizione comando con una descrizione dell'elemento di sintassi che è possibile specificare. L'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] riconosce il frammento come separato dallo script circostante fino a quando non si chiude il file di origine. I punti di sostituzione rimangono attivi fino a quando non si chiude il file di origine.  
  
 È inoltre possibile aggiungere altri elementi di sintassi al codice del modello inserito da un frammento. Il modello di frammento Crea tabella, ad esempio, genera due definizioni di colonna. È necessario aggiungere altre definizioni di colonna per creare una tabella con più di due colonne.  
  
#### <a name="completing-the-snippet-statement"></a>Completamento dell'istruzione del frammento  
  
1.  Utilizzare il tasto TAB per spostarsi da un punto di sostituzione al successivo. Utilizzare MAIUSC+TAB per spostarsi al punto di sostituzione precedente.  
  
2.  Premere CTRL+BARRA SPAZIATRICE per richiamare IntelliSense.  
  
3.  Selezionare un elemento nell'elenco o digitare una sostituzione desiderata.  
  
## <a name="see-also"></a>Vedere anche  
 [Inserimento di frammenti di Transact-SQL](./insert-transact-sql-snippets.md)   
 [Inserire frammenti Transact-SQL racchiusi](./insert-surround-with-transact-sql-snippets.md)  
  
