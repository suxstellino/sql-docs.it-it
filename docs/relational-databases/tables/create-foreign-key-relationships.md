---
description: Creare relazioni di chiave esterna
title: Creare relazioni di chiave esterna | Microsoft Docs
ms.custom: ''
ms.date: 06/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- relationships [SQL Server], creating
ms.assetid: 867a54b8-5be4-46e6-9702-49ae6dabf67c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 230a1a00d66f1839be7a8135ff9c45022f0a2511
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765652"
---
# <a name="create-foreign-key-relationships"></a>Creare relazioni di chiave esterna


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Questo articolo descrive come creare relazioni di chiave esterna in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Una relazione tra due tabelle consente di stabilire un'associazione tra le righe di una tabella e le righe di un'altra tabella.

## <a name="permissions"></a>Autorizzazioni

Per la creazione di una nuova tabella con una chiave esterna è richiesta l'autorizzazione [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) per il database e l'autorizzazione [ALTER](../../t-sql/statements/alter-schema-transact-sql.md) per lo schema in cui viene creata la tabella.

Per la creazione di una chiave esterna in una tabella esistente è richiesta l'autorizzazione [ALTER](../../t-sql/statements/alter-table-transact-sql.md) per la tabella.

## <a name="limits-and-restrictions"></a><a name="BeforeYouBegin"></a> Limiti e restrizioni

- Un vincolo di chiave esterna non deve necessariamente essere collegato solo a un vincolo di chiave primaria in un'altra tabella. È anche possibile definire le chiavi esterne per fare riferimento alle colonne di un vincolo UNIQUE in un'altra tabella.
- I valori diversi da NULL immessi nella colonna di un vincolo FOREIGN KEY devono essere presenti nella colonna a cui viene fatto riferimento. In caso contrario, viene restituito un messaggio di errore di violazione della chiave esterna. Per assicurarsi che tutti i valori di un vincolo di chiave esterna composto vengano verificati, specificare NOT NULL in tutte le colonne coinvolte.
- I vincoli FOREIGN KEY possono fare riferimento solo a tabelle di un singolo database nello stesso server. L'integrità referenziale tra database diversi deve essere implementata tramite trigger. Per altre informazioni, vedere [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md).
- I vincoli FOREIGN KEY possono fare riferimento a un'altra colonna nella stessa tabella. Questo tipo di vincolo viene definito autoreferenziale.
- Un vincolo FOREIGN KEY specificato a livello di colonna può includere una sola colonna di riferimento. Il tipo di dati di tale colonna deve essere uguale al tipo di dati della colonna in cui viene definito il vincolo.
- Un vincolo FOREIGN KEY specificato a livello di tabella deve includere lo stesso numero di colonne di riferimento di quelle presenti nell'elenco di colonne del vincolo. Il tipo di dati di ogni colonna di riferimento deve inoltre essere uguale a quello della colonna corrispondente nell'elenco di colonne.
- [!INCLUDE[ssDE](../../includes/ssde-md.md)] non ha un limite predefinito per il numero di vincoli FOREIGN KEY che possono essere inclusi in una tabella che fanno riferimento ad altre tabelle. [!INCLUDE[ssDE](../../includes/ssde-md.md)] non limita inoltre il numero di vincoli FOREIGN KEY di proprietà di altre tabelle che fanno riferimento a una tabella specifica. Tuttavia, il numero effettivo di vincoli FOREIGN KEY che è possibile usare è limitato dalla configurazione hardware e dalla progettazione del database e dell'applicazione. Una tabella può fare riferimento a un massimo di 253 altre tabelle e colonne come chiavi esterne (riferimenti in uscita). [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive incrementa da 253 a 10.000 il limite per il numero di altre tabelle e colonne che possono fare riferimento alle colonne in una singola tabella (riferimenti in ingresso). (richiede almeno il livello di compatibilità 130). All'incremento vengono applicate le seguenti restrizioni:

  - I riferimenti di chiave esterna maggiori di 253 sono supportati per le operazioni DELETE e UPDATE DML. Le operazioni MERGE non sono supportate.
  - Una tabella con un riferimento di chiave esterna a se stessa è comunque limitata a 253 riferimenti di chiave esterna.
  - I riferimenti di chiave esterna maggiori di 253 non sono attualmente disponibili per gli indici columnstore, le tabelle ottimizzate per la memoria o Stretch Database.

- I vincoli FOREIGN KEY non vengono applicati nelle tabelle temporanee.
- Se si definisce una chiave esterna su una colonna di tipo CLR definito dall'utente, è necessario che l'implementazione del tipo supporti l'ordinamento binario. Per altre informazioni, vedere [Tipi CLR definiti dall'utente](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).
- Una colonna di tipo **varchar(max)** può far parte di un vincolo FOREIGN KEY solo se anche la chiave primaria a cui fa riferimento è definita come tipo **varchar(max)** .

## <a name="create-a-foreign-key-relationship-in-table-designer"></a>Creare una relazione di chiave esterna in Progettazione tabelle

### <a name="using-sql-server-management-studio"></a>Utilizzare SQL Server Management Studio

1. In Esplora oggetti fare clic con il pulsante destro del mouse sulla tabella che si troverà sul lato chiave esterna della relazione e scegliere **Progetta**.

   La tabella verrà visualizzata in [**Progettazione tabelle**](../../ssms/visual-db-tools/design-tables-visual-database-tools.md).
2. Scegliere **Relazioni** dal menu **Progettazione tabelle**.
3. Nella finestra di dialogo **Relazioni chiavi esterne** fare clic su **Aggiungi**.

   La relazione verrà visualizzata nell'elenco **Relazione selezionata** con un nome specificato dal sistema nel formato FK_\<*tablename*>_\<*tablename*>, dove *tablename* è il nome della tabella di chiave esterna.
4. Fare clic sulla relazione nell'elenco **Relazione selezionata** .
5. Fare clic su **Specifica tabelle e colonne** nella griglia a destra e quindi sul pulsante con i puntini di sospensione ( **...** ) a destra della proprietà.
6. Nella finestra di dialogo **Tabelle e colonne** selezionare dall'elenco a discesa **Chiave primaria** la tabella che si troverà sul lato chiave primaria della relazione.
7. Nella griglia sottostante selezionare le colonne che contribuiranno alla chiave primaria della tabella. Nella cella adiacente a destra di ogni colonna selezionare la corrispondente colonna chiave esterna della tabella di chiave esterna.

   **Progettazione tabelle** suggerisce automaticamente un nome da assegnare alla relazione. Per specificare un nome diverso, modificare il contenuto della casella di testo **Nome relazione** .
8. Scegliere **OK** per creare la relazione.
9. Chiudere la finestra Progettazione tabelle e **salvare** le modifiche per rendere effettiva la modifica della relazione di chiave esterna.

## <a name="create-a-foreign-key-in-a-new-table"></a>Creare una chiave esterna in una nuova tabella

### <a name="using-transact-sql"></a>Uso di Transact-SQL

L'esempio seguente crea una tabella e definisce un vincolo di chiave esterna nella colonna `TempID` alla quale fa riferimento la colonna `SalesReasonID` nella tabella `Sales.SalesReason` nel database AdventureWorks. Le clausole ON DELETE CASCADE e ON UPDATE CASCADE vengono utilizzate per garantire che le modifiche apportate alla tabella `Sales.SalesReason` vengano propagate automaticamente alla tabella `Sales.TempSalesReason` .    

```sql
CREATE TABLE Sales.TempSalesReason 
   (
      TempID int NOT NULL, Name nvarchar(50)
      , CONSTRAINT PK_TempSales PRIMARY KEY NONCLUSTERED (TempID)
      , CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)
        REFERENCES Sales.SalesReason (SalesReasonID)
        ON DELETE CASCADE
        ON UPDATE CASCADE
   )
;
```

## <a name="create-a-foreign-key-in-an-existing-table"></a>Creare una chiave esterna in una tabella esistente

### <a name="using-transact-sql"></a>Uso di Transact-SQL
L'esempio seguente crea una chiave esterna nella colonna `TempID` e fa riferimento alla colonna `SalesReasonID` nella tabella `Sales.SalesReason` nel database AdventureWorks.

```sql
ALTER TABLE Sales.TempSalesReason
   ADD CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)
      REFERENCES Sales.SalesReason (SalesReasonID)
      ON DELETE CASCADE
      ON UPDATE CASCADE
;
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere:

- [Vincoli di chiavi primarie ed esterne](primary-and-foreign-key-constraints.md)
- [GRANT - Autorizzazioni per database](../../t-sql/statements/grant-database-permissions-transact-sql.md)
- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md).
