---
description: Modalità SQL (MySQLToSQL)
title: Modalità SQL (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d840ee51-b863-4e77-84aa-37d3f094bfed
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e09a38ce70b4f144f6da324525a7fee925d586a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074472"
---
# <a name="sql-modes-mysqltosql"></a>Modalità SQL (MySQLToSQL)
SSMA per MySQL può funzionare in modalità SQL diverse e può applicare queste modalità in modo diverso per client diversi.  
  
Le modalità definiscono la sintassi SQL che MySQL deve supportare e il tipo di controlli di convalida dei dati da eseguire. Questo rende più semplice l'uso di MySQL in ambienti diversi e l'uso di MySQL con SQL Server.  
  
## <a name="sql-modes-grid"></a>Griglia modalità SQL:  
  
-   La griglia modalità SQL a livello di radice contiene le colonne seguenti: **nome della modalità SQL**, **modalità SQL caricate** e **modalità SQL valide**.  
  
-   Griglia modalità SQL alla categoria database, database, categoria tabella, categoria istruzioni, categoria viste, tabella, vista, funzioni, procedure, UDF e livello oggetto evento contiene le colonne seguenti: **nome modalità SQL**, **modalità SQL ereditate** e **modalità SQL valide**.  
  
-   La griglia modalità SQL alla stored procedure, la funzione archiviata e il livello del trigger contengono le colonne seguenti: **nome della modalità SQL**,  **modalità SQL originali** e **modalità SQL valide**.  
  
> [!NOTE]  
> Le modalità gruppo verranno visualizzate in grassetto, sotto la colonna "nome modalità SQL".  
  
## <a name="loaded-sql-modes"></a>Modalità SQL caricate  
Queste sono le modalità SQL, che vengono impostate a livello di sessione o di radice. Le modalità SQL, una volta caricate nel database di destinazione, non possono essere modificate o modificate.  
  
## <a name="inherited-sql-modes"></a>Modalità SQL ereditate  
Si tratta delle modalità SQL, che vengono ereditate dal nodo padre corrispondente.  
  
Ad eccezione di funzioni categoria, categoria procedure, categoria eventi e trigger, queste modalità SQL sono presenti a tutti i livelli (database, categoria tabella, categoria istruzioni, categoria viste, tabella, vista, funzioni, procedure, UDF e oggetto evento).  
  
> [!NOTE]  
> Selezionando la casella di controllo **eredita da padre** , le modalità SQL ereditate possono essere ereditate dal nodo padre. Per impostazione predefinita, questa casella di controllo rimane selezionata.  
  
## <a name="original-sql-modes"></a>Modalità SQL originali  
Si tratta delle modalità SQL presenti solo a livello di funzione, procedura e trigger.  
  
> [!NOTE]  
> Selezionando la casella di controllo **Usa originale** , è possibile usare le modalità SQL originariamente utilizzate nella funzione o nella procedura o nel trigger corrispondente. Per impostazione predefinita, questa casella di controllo rimane selezionata.  
  
## <a name="effective-sql-modes"></a>Modalità SQL valide  
È possibile definire modalità SQL valide a diversi livelli nel modo seguente:  
  
-   **A livello di sessione:**  
  
    1.  È possibile chiamare tutte le modalità SQL caricate, ovvero "modalità SQL valide".  
  
    2.  A questo livello, le modalità SQL effettive possono essere modificate direttamente e in modo esplicito.  
  
    3.  La modalità SQL effettiva impostata in modo esplicito non viene riflessa come modalità SQL caricata e viene infine applicata all'oggetto.  
  
-   **A livello di funzione o di procedura o di trigger:**  
  
    1.  Tutte le modalità SQL originali possono essere chiamate "modalità SQL valide".  
  
    2.  A questo livello, la modalità SQL effettiva può essere modificata in modo esplicito solo quando la casella di controllo **Usa originale** è deselezionata.  
  
    3.  La modalità SQL effettiva impostata in modo esplicito non viene riflessa come modalità SQL originale e viene infine applicata all'oggetto.  
  
-   **In nodi diversi da funzione o routine o livello di trigger:**  
  
    1.  È possibile chiamare tutte le modalità SQL ereditate, ovvero "modalità SQL valide".  
  
    2.  A questo livello, la modalità SQL effettiva può essere modificata in modo esplicito solo quando la casella **di controllo eredita da padre** è deselezionata.  
  
    3.  La modalità SQL effettiva impostata in modo esplicito non viene riflessa come modalità SQL ereditata e viene infine applicata all'oggetto.  
  
