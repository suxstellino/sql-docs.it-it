---
description: Utilizzo delle tabelle inserite ed eliminate
title: Usare le tabelle inserite ed eliminate | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- inserted tables
- UPDATE statement [SQL Server], DML triggers
- DELETE statement [SQL Server], DML triggers
- INSTEAD OF triggers
- deleted tables
- INSERT statement [SQL Server], DML triggers
- DML triggers, deleted or inserted tables
ms.assetid: ed84567f-7b91-4b44-b5b2-c400bda4590d
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6f78a502be14ffcf51937823d61bd61c144777f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187268"
---
# <a name="use-the-inserted-and-deleted-tables"></a>Utilizzo delle tabelle inserite ed eliminate
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Nelle istruzioni dei trigger DML vengono usate due tabelle speciali, ovvero la tabella inserted e la tabella deleted. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea e gestisce queste tabelle automaticamente. È possibile utilizzare queste tabelle temporanee residenti in memoria per verificare gli effetti di determinate modifiche apportate ai dati e impostare le condizioni per le azioni dei trigger DML. Non è possibile modificare i dati o eseguire operazioni DDL (Data Definition Language), quale CREATE INDEX, direttamente nelle tabelle.  
  
 Nei trigger DML le tabelle inserted e deleted vengono principalmente utilizzate per eseguire le operazioni seguenti:  
  
-   Estendere l'integrità referenziale tra le tabelle.  
  
-   Inserire o aggiornare i dati in tabelle di base sottostanti alla vista.  
  
-   Verificare la presenza di errori ed eseguire le azioni appropriate sulla base dell'errore rilevato.  
  
-   Individuare le differenze tra lo stato di una tabella prima della modifica dei dati e lo stato della tabella stessa dopo la modifica, per eseguire le azioni appropriate sulla base di tali differenze.  
  
 Nella tabella deleted vengono archiviate copie delle righe interessate dall'esecuzione delle istruzioni DELETE e UPDATE. Durante l'esecuzione di un'istruzione DELETE o UPDATE, le righe vengono eliminate dalla tabella di trigger e trasferite nella tabella deleted. In genere, la tabella deleted e la tabella di trigger non hanno righe in comune.  
  
 Nella tabella inserted vengono archiviate copie delle righe interessate dall'esecuzione delle istruzioni INSERT e UPDATE. Durante una transazione INSERT o UPDATE, le nuove righe vengono aggiunte sia alla tabella inserted che alla tabella di trigger. Le righe della tabella inserted sono copie delle nuove righe della tabella di trigger.  
  
 Una transazione UPDATE corrisponde a un'eliminazione seguita da un inserimento. Per prima cosa le righe precedenti vengono copiate nella tabella deleted e quindi le nuove righe vengono copiate nella tabella inserted.  
  
 Quando si impostano le condizioni di trigger, utilizzare correttamente le tabelle inserted e deleted in base all'azione che ha attivato il trigger. Sebbene il riferimento alla tabella deleted durante la verifica di un'istruzione INSERT o alla tabella inserted durante la verifica di un'istruzione DELETE non causi alcun errore, in questi casi le tabelle di verifica dei trigger non conterranno alcuna riga.  
  
> [!NOTE]  
>  Se le azioni dei trigger dipendono dal numero di righe interessate dalle modifiche apportate ai dati, usare le verifiche (ad esempio un esame di @@ROWCOUNT) nel caso di modifiche apportate ai dati di più righe (un'istruzione INSERT, DELETE o UPDATE basata su un'istruzione SELECT) ed eseguire le azioni appropriate. Per altre informazioni, vedere [Creazione di trigger DML per gestire più righe di dati](../../relational-databases/triggers/create-dml-triggers-to-handle-multiple-rows-of-data.md).
  
 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] non è possibile usare i riferimenti di colonna di tipo **text**, **ntext** o **image** nelle tabelle inserted e deleted per i trigger AFTER. Questi tipi di dati sono tuttavia disponibili per garantire la compatibilità con le versioni precedenti. L'archiviazione preferita per i dati di grandi dimensioni consiste nell'usare i tipi di dati **varchar(max)** , **nvarchar(max)** e **varbinary(max)** . I trigger AFTER e INSTEAD OF supportano i dati **varchar(max)** , **nvarchar(max)** e **varbinary(max)** nelle tabelle inserted e deleted. Per altre informazioni, vedere [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 **Esempio di utilizzo della tabella inserted in un trigger per l'applicazione delle regole business**  
  
 Poiché i vincoli CHECK possono fare riferimento solo alle colonne in cui è definito il vincolo a livello di colonna o di tabella, è necessario definire come trigger qualsiasi vincolo tra tabelle, in questo caso le regole business.  
  
 Nell'esempio seguente viene creato un trigger DML. Questo trigger verifica che la posizione creditizia del fornitore sia buona quando viene eseguito un tentativo di inserimento di un nuovo ordine di acquisto nella tabella `PurchaseOrderHeader` . Per ottenere la posizione creditizia del fornitore corrispondente all'ordine di acquisto inserito, la tabella `Vendor` deve essere una tabella con riferimenti e deve essere unita in join alla tabella inserted. Se la posizione creditizia è troppo bassa, viene visualizzato un messaggio e l'operazione di inserimento non viene eseguita.
  
 [!code-sql[TriggerDDL#CreateTrigger3](../../relational-databases/triggers/codesnippet/tsql/use-the-inserted-and-del_1.sql)]  
  
## <a name="using-the-inserted-and-deleted-tables-in-instead-of-triggers"></a>Utilizzo delle tabelle inserted e deleted nei trigger INSTEAD OF  
 Alle tabelle inserted e deleted passate ai trigger INSTEAD OF definiti nelle tabelle vengono applicate le stesse regole valide per le tabelle inserted e deleted passate ai trigger AFTER. Il formato delle tabelle inserted e deleted è uguale a quello della tabella in cui è stato definito il trigger INSTEAD OF. Ogni colonna delle tabelle inserted e deleted è mappata direttamente a una colonna della tabella di base.  
  
 Le regole seguenti relative al fatto che un'istruzione INSERT o UPDATE che fa riferimento a una tabella con un trigger INSTEAD OF debba fornire valori per le colonne sono applicabili anche alle tabelle senza trigger INSTEAD OF:  
  
-   Non è possibile specificare valori per colonne calcolate o colonne con tipo di dati **timestamp** .  
  
-   Non è possibile specificare valori per colonne con proprietà IDENTITY, a meno che IDENTITY_INSERT non sia impostata su ON per la tabella. Se IDENTITY_INSERT è impostata su ON, le istruzioni INSERT devono fornire un valore.  
  
-   Le istruzioni INSERT devono fornire valori per tutte le colonne NOT NULL a cui sono applicati vincoli DEFAULT.  
  
-   Ad eccezione delle colonne calcolate, identity o **timestamp** , i valori sono facoltativi per tutte le colonne che supportano valori Null o per le colonne NOT NULL con definizione DEFAULT.  
  
 Quando un'istruzione INSERT, UPDATE o DELETE fa riferimento a una vista con trigger INSTEAD OF, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] esegue una chiamata al trigger invece di eseguire azioni sulle tabelle. Il trigger deve utilizzare le informazioni visualizzate nelle tabelle inserted e deleted per compilare le istruzioni necessarie per implementare l'azione richiesta nelle tabelle di base, anche nel caso in cui il formato delle informazioni nelle tabelle inserted e deleted compilate per la vista sia diverso dal formato dei dati delle tabelle di base.  
  
 Il formato delle tabelle inserted e deleted passate a un trigger INSTEAD OF definito in una vista corrisponde all'elenco di selezione dell'istruzione SELECT definita per la vista. Ad esempio:  
  
```  
USE AdventureWorks2012;  
GO  
CREATE VIEW dbo.EmployeeNames (BusinessEntityID, LName, FName)  
AS  
SELECT e.BusinessEntityID, p.LastName, p.FirstName  
FROM HumanResources.Employee AS e   
JOIN Person.Person AS p  
ON e.BusinessEntityID = p.BusinessEntityID;  
```  
  
 Il set di risultati di questa vista ha tre colonne, ovvero una colonna **int** e due colonne **nvarchar** . Le tabelle inserted e deleted passate a un trigger INSTEAD OF definito nella vista hanno a loro volta una colonna **int** chiamata `BusinessEntityID`, una colonna **nvarchar** chiamata `LName`e una colonna **nvarchar** chiamata `FName`.  
  
 L'elenco di selezione di una vista può inoltre includere espressioni di cui non è stato eseguito il mapping diretto a una singola colonna della tabella di base. È possibile che alcune espressioni di vista, ad esempio una chiamata di funzione o costante, non facciano riferimento ad alcuna colonna e vengano pertanto ignorate. Le espressioni complesse possono fare riferimento a più colonne, ma nelle tabelle inserted e deleted è disponibile un solo valore per ogni riga inserita. Lo stesso vale per le espressioni semplici di una vista che fanno riferimento a una colonna calcolata con un'espressione complessa. Un trigger INSTEAD OF nella vista deve essere in grado di gestire questo tipo di espressioni.  
  
  
