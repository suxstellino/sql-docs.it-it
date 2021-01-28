---
description: Finestra di dialogo Tabelle e colonne (Visual Database Tools)
title: Finestra di dialogo Tabelle e colonne
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.tablesandcolumns
ms.assetid: 8cf27be1-e66d-4735-a428-9ab4b33af4f5
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 444691b0ec7196c2dcb09426b7e7cb3063445826
ms.sourcegitcommit: 0bee2b4392d50151c3839a937371588cc2ce2d3e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2021
ms.locfileid: "98669128"
---
# <a name="tables-and-columns-dialog-box-visual-database-tools"></a>Finestra di dialogo Tabelle e colonne (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Questa finestra di dialogo consente di eseguire il mapping di una chiave primaria in una tabella a una chiave esterna in un'altra tabella. Per accedere a tale finestra di dialogo, scegliere **Relazioni** dal menu **Progettazione tabelle**. Nella finestra di dialogo **Relazioni chiavi esterne** fare clic sul campo **Specifica tabelle e colonne** e quindi sui puntini di sospensione **(...)** a destra della proprietà.  
  
> [!NOTE]  
> Se la tabella viene pubblicata per la replica, è necessario apportare modifiche allo schema usando l'istruzione [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) di Transact-SQL oppure SMO (SQL Server Management Objects). Quando si apportano modifiche allo schema utilizzando Progettazione tabelle o Progettazione diagrammi di database, viene effettuato il tentativo di rimuovere e rigenerare la tabella. La modifica allo schema non riuscirà, poiché non è consentita la rimozione di oggetti pubblicati.  
  
## <a name="options"></a>Opzioni  
**Nome relazione**  
Visualizza il nome della relazione.  
  
**Tabella chiave primaria**  
Elenca le tabelle del database. Scegliere la tabella che si troverà sul lato chiave primaria della relazione.  
  
**Tabella chiave esterna**  
Visualizza la tabella sul lato chiave esterna della relazione. Corrisponde alla tabella attualmente selezionata in Progettazione tabelle.  
  
**Griglia al di sotto della tabella di chiave primaria**  
Elenca le colonne della tabella selezionata nell'elenco **Tabella chiave primaria** . Immettere le colonne che contribuiscono alla chiave primaria della tabella.  
  
**Griglia al di sotto della tabella di chiave esterna**  
Elenca le colonne della tabella selezionata nell'elenco **Tabella chiave esterna** . Immettere la colonna chiave esterna della tabella di chiave esterna che corrisponde alla colonna chiave primaria.  
  
> [!NOTE]  
> Le colonne selezionate per la chiave esterna devono avere lo stesso tipo di dati delle colonne primarie a cui corrispondono. In ogni chiave deve esistere un numero uguale di colonne. Se ad esempio la chiave primaria della tabella sul lato primario della relazione è composta da due colonne, ognuna di queste colonne dovrà corrispondere a una colonna della tabella sul lato chiave esterna della relazione.  
  
## <a name="see-also"></a>Vedere anche  
[Procedura: creare relazioni tra tabelle](https://msdn.microsoft.com/867a54b8-5be4-46e6-9702-49ae6dabf67c)  
  
