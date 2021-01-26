---
description: Modificare una funzione di partizione
title: Modificare una funzione di partizione | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: ae5bfc09-f27a-4ea9-9518-485278b11674
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b200814dedac25809d1295146ff71d8908bce4da
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766117"
---
# <a name="modify-a-partition-function"></a>Modificare una funzione di partizione
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile modificare la modalità di partizionamento di una tabella o di un indice in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] aggiungendo o sottraendo, con incrementi di 1, il numero di partizioni specificate nella funzione di partizione della tabella o dell'indice partizionato tramite [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'aggiunta di una partizione consiste nel "suddividere" una partizione esistente in due partizioni e nel ridefinire i limiti delle nuove partizioni. L'eliminazione di una partizione consiste nell'"unire" i limiti di due partizioni in modo da ottenerne una. L'ultima operazione consiste nel ripopolare una partizione lasciando l'altra non assegnata.  
  
> [!CAUTION]  
>  La stessa funzione di partizione può essere utilizzata da più tabelle o indici. La modifica di una funzione di partizione viene applicata a tutti gli elementi in un'unica transazione. Controllare le dipendenze della funzione di partizione prima di modificarla.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per modificare una funzione di partizione utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   L'istruzione ALTER PARTITION FUNCTION può essere utilizzata solo per suddividere una partizione in due o per unire due partizioni in una. Per modificare la modalità di partizionamento di una tabella o un indice, ad esempio da 10 partizioni a 5, è possibile utilizzare una delle opzioni seguenti:  
  
    -   Creare una nuova tabella partizionata utilizzando la funzione di partizione desiderata e quindi inserire i dati della vecchia tabella in quella nuova utilizzando un'istruzione INSERT INTO ... SELECT FROM [!INCLUDE[tsql](../../includes/tsql-md.md)] o la **Gestione guidata partizione** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
    -   Creazione di un indice cluster partizionato su un heap.  
  
        > [!NOTE]  
        >  L'eliminazione di un indice cluster partizionato ha come risultato un heap partizionato.  
  
    -   Eliminazione e ricompilazione di un indice partizionato esistente mediante l'utilizzo dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE INDEX con la clausola DROP EXISTING = ON.  
  
    -   Esecuzione di una sequenza di istruzioni ALTER PARTITION FUNCTION.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non fornisce il supporto di replica per la modifica di una funzione di partizione. Se si desidera apportare modifiche a una funzione di partizione nel database di pubblicazione, è necessario procedere manualmente nel database di sottoscrizione.  
  
-   Tutti i filegroup interessati dall'istruzione ALTER PARTITION FUNCTION devono essere online.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Per eseguire l'istruzione ALTER PARTITION FUNCTION, è necessario utilizzare le autorizzazioni seguenti:  
  
-   Autorizzazione ALTER ANY DATASPACE. Questa autorizzazione viene concessa per impostazione predefinita al ruolo predefinito del server **sysadmin** e ai ruoli predefiniti del database **db_owner** e **db_ddladmin** .  
  
-   Autorizzazione CONTROL o ALTER nel database in cui la funzione di partizione è stata creata.  
  
-   Autorizzazione CONTROL SERVER o ALTER ANY DATABASE nel server del database in cui la funzione di partizione è stata creata.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 **Per modificare una funzione di partizione:**  
  
 Non è possibile eseguire questa azione specifica tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per modificare una funzione di partizione, è innanzitutto necessario eliminare la funzione e crearne quindi una nuova con le proprietà desiderate tramite la Creazione guidata partizione. Per ulteriori informazioni, vedere  
  
#### <a name="to-delete-a-partition-function"></a>Per eliminare una funzione di partizione  
  
1.  Espandere il database in cui si desidera eliminare la funzione di partizione ed espandere quindi la cartella **Archiviazione** .  
  
2.  Espandere la cartella **Funzioni di partizione** .  
  
3.  Fare clic con il pulsante destro del mouse sulla funzione di partizione che si vuole eliminare e scegliere **Elimina**.  
  
4.  Nella finestra di dialogo **Elimina oggetto** verificare che venga selezionata la funzione di partizione corretta, quindi fare clic su **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-split-a-single-partition-into-two-partitions"></a>Per suddividere una singola partizione in due partizioni  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- Look for a previous version of the partition function "myRangePF1" and deletes it if it is found.  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
        DROP PARTITION FUNCTION myRangePF1;  
    GO  
    -- Create a new partition function called "myRangePF1" that partitions a table into four partitions.  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    --Split the partition between boundary_values 100 and 1000  
    --to create two partitions between boundary_values 100 and 500  
    --and between boundary_values 500 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    SPLIT RANGE (500);  
    ```  
  
#### <a name="to-merge-two-partitions-into-one-partition"></a>Per unire due partizioni in una partizione  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- Look for a previous version of the partition function "myRangePF1" and deletes it if it is found.  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
        DROP PARTITION FUNCTION myRangePF1;  
    GO  
    -- Create a new partition function called "myRangePF1" that partitions a table into four partitions.  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    --Merge the partitions between boundary_values 1 and 100  
    --and between boundary_values 100 and 1000 to create one partition  
    --between boundary_values 1 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    MERGE RANGE (100);  
    ```  
  
 Per altre informazioni, vedere [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md).  
  
  
