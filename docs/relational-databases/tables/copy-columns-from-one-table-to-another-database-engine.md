---
description: Copia di colonne da una tabella a un'altra (Motore di database)
title: Copiare colonne da una tabella a un'altra (motore di database) | Microsoft Docs
ms.custom: ''
ms.date: 09/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- copying columns
- columns [SQL Server], copying
ms.assetid: 5f5e70dc-69f9-44b8-bc48-b5d51ac20d77
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba2b4f4e106f1edb9d9f3fe57a0f42cdf20fec27
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748631"
---
# <a name="copy-columns-from-one-table-to-another-database-engine"></a>Copia di colonne da una tabella a un'altra (Motore di database)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  In questo argomento viene illustrato come copiare colonne di una tabella a un'altra copiando solo la definizione di colonna oppure la definizione e i dati in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per copiare le colonne tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 Quando si copia una colonna contenente un tipo di dati alias da un database a un altro, il tipo di dati alias potrebbe non essere disponibile nel database di destinazione. In questo caso, alla colonna verrà assegnato il tipo di dati di base più simile tra quelli disponibili nel database.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER per la tabella.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-copy-column-definitions-from-one-table-to-another"></a>Per copiare le definizioni delle colonne tra tabelle  
  
1.  Aprire la tabella contenente le colonne da copiare e quella in cui verranno copiate le colonne facendo clic con il pulsante destro del mouse sulle tabelle, quindi scegliendo **Progetta**.  
  
2.  Fare clic sulla scheda relativa alla tabella contenente le colonne da copiare e selezionarle.  
  
3.  Scegliere **Copia** dal menu **Modifica**.  
  
4.  Fare clic sulla scheda relativa alla tabella in cui copiare le colonne.  
  
5.  Selezionare la colonna prima della quale si desidera che vengano inserite le colonne appena copiate, quindi scegliere **Incolla** dal menu **Modifica**.  

#### <a name="to-copy-data-from-one-table-to-another"></a>Per copiare dati tra tabelle  
  
1.  Seguire le istruzioni sopra riportate per copiare le definizioni delle colonne.  
  
    > [!NOTE]  
    >  Prima di iniziare a copiare i dati da una tabella a un'altra, assicurarsi che i tipi di dati delle colonne di destinazione siano compatibili con quelli delle colonne di origine.  
  
2.  Aprire una nuova finestra dell'editor di query. 

3.  Fare clic con il pulsante destro sull’editor di query e quindi scegliere **Progetta query nell'editor**. 

4.  Nella finestra di dialogo **Aggiungi tabella** selezionare la tabella di origine e destinazione, fare clic su **Aggiungi**, quindi chiudere la finestra di dialogo **Aggiungi tabella** . 

5.  Fare clic con il pulsante destro del mouse su un'area vuota dell'editor di query, scegliere **Modifica tipo** e quindi fare clic su **Accodamento**.  

6.  Nella finestra di dialogo **Scegliere la tabella di destinazione per Accodamento** selezionare la tabella di destinazione. 

7.  Nella parte superiore di Progettazione query fare clic sulla colonna di origine nella tabella di origine.

8. In Progettazione query è stata ora creata una query INSERT. Fare clic su OK per inserire la query nella finestra dell'editor di query originale.  

9.  Eseguire la query per inserire i dati dalla tabella di origine alla tabella di destinazione.

  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-copy-column-definitions-from-one-table-to-another"></a>Per copiare le definizioni delle colonne tra tabelle  
  
1.  Non è possibile copiare singole colonne da una tabella a un'altra tramite istruzioni Transact-SQL. È tuttavia possibile creare una nuova tabella nel filegroup predefinito e inserirvi le righe restituite dalla query. Per altre informazioni, vedere [Clausola INTO &#40;Transact-SQL&#41;](../../t-sql/queries/select-into-clause-transact-sql.md).  
  
#### <a name="to-copy-data-from-one-table-to-another"></a>Per copiare dati tra tabelle  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE TABLE dbo.EmployeeSales  
    ( BusinessEntityID   varchar(11) NOT NULL,  
      SalesYTD money NOT NULL  
    );  
    GO  
    INSERT INTO dbo.EmployeeSales  
        SELECT BusinessEntityID, SalesYTD   
        FROM Sales.SalesPerson;  
    GO  
    ```  
  
  
