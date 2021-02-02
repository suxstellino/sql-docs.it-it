---
title: Eseguire una stored procedure | Microsoft Docs
description: Informazioni su come eseguire una stored procedure in SQL Server 2019 (15.x) usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
f1_keywords:
- sql13.swb.executeprocedure.general.f1
- sql13.swb.executeprocedure.f1
helpviewer_keywords:
- stored procedures [SQL Server], parameters
- extended stored procedures [SQL Server], executing
- system stored procedures [SQL Server], executing
- stored procedures [SQL Server], executing
- user-defined stored procedures [SQL Server]
ms.assetid: a0b1337d-2059-4872-8c62-3f967d8b170f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 115220ef0fabbe98a0c4a1bf5f185409c5a82829
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250272"
---
# <a name="execute-a-stored-procedure"></a>Eseguire una stored procedure
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In questo argomento viene illustrato come eseguire una stored procedure in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Sono disponibili due modi diversi per eseguire una stored procedure. Il primo e più comune approccio consiste nella chiamata della stored procedure da parte di un'applicazione o un utente. Il secondo approccio consiste nell'impostare la stored procedure per l'esecuzione automatica all'avvio di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando una stored procedure viene chiamata da un'applicazione o da un utente, la parola chiave [!INCLUDE[tsql](../../includes/tsql-md.md)] EXECUTE o EXEC viene dichiarata in modo esplicito nella chiamata. In alternativa, è possibile chiamare ed eseguire la stored procedure senza la parola chiave se la stored procedure è la prima istruzione nel batch [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per eseguire una stored procedure tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Le regole di confronto del database chiamante vengono utilizzate per la ricerca dei nomi delle stored procedure di sistema corrispondenti. Nei nomi delle stored procedure di sistema nelle chiamate alle stored procedure è pertanto necessario utilizzare sempre la corretta combinazione di maiuscole e minuscole. Ad esempio, il codice seguente, se eseguito nel contesto di un database con regole di confronto con distinzione tra maiuscole e minuscole, genererà un errore:  
  
    ```sql  
    EXEC SP_heLP; -- Will fail to resolve because SP_heLP does not equal sp_help  
    ```  
  
     Per visualizzare i nomi esatti delle stored procedure di sistema, eseguire query nelle viste del catalogo [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) e [sys.system_parameters](../../relational-databases/system-catalog-views/sys-system-parameters-transact-sql.md) .  
  
-   Se una stored procedure definita dall'utente ha lo stesso nome di una stored procedure di sistema, potrebbe non essere possibile eseguire la prima.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Esecuzione di stored procedure di sistema  
  
     Le stored procedure di sistema iniziano con il prefisso **sp_** . Poiché sono visualizzate logicamente in ogni database definito dall'utente e dal sistema, possono essere eseguite da qualsiasi database senza che sia necessario specificare il nome completo della stored procedure. È tuttavia consigliabile specificare lo schema in tutti i nomi di stored procedure di sistema con il nome dello schema **sys** per evitare conflitti. Nell'esempio seguente viene illustrato il metodo consigliato per la chiamata a una stored procedure di sistema.  
  
    ```sql  
    EXEC sys.sp_who;  
    ```  
  
-   Esecuzione di stored procedure definite dall'utente  
  
     Quando si esegue una stored procedure definita dall'utente, si consiglia di specificare il nome completo della stored procedure con il nome di schema. In questo modo, le prestazioni risulteranno leggermente migliorate poiché si evita che debbano essere eseguite ricerche in più schemi tramite il [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Si evita inoltre che venga eseguita la stored procedure errata se un database dispone di stored procedure con lo stesso nome in più schemi.  
  
     Nell'esempio seguente viene illustrato il metodo consigliato per l'esecuzione di una stored procedure definita dall'utente. Si noti che la stored procedure accetta un parametro di input. Per informazioni su come specificare i parametri di input e di output, vedere [Specificare i parametri](../../relational-databases/stored-procedures/specify-parameters.md).  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    EXEC dbo.uspGetEmployeeManagers @BusinessEntityID = 50;  
    ```  
  
     Oppure  
  
    ```sql  
    EXEC AdventureWorks2012.dbo.uspGetEmployeeManagers 50;  
    GO  
    ```  
  
     Se si specifica una stored procedure non qualificata definita dall'utente, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] cerca la stored procedure nell'ordine seguente:  
  
    1.  Schema **sys** del database corrente.  
  
    2.  Schema predefinito del chiamante se eseguito in un batch o in SQL dinamico. In alternativa, se il nome della stored procedure non completo viene visualizzato all'interno del corpo di un'altra definizione di stored procedure, la ricerca viene eseguita nello schema che contiene quest'ultima subito dopo.  
  
    3.  Schema **dbo** nel database corrente.  
  
-   Esecuzione automatica di stored procedure  
  
     Le stored procedure contrassegnate per l'esecuzione automatica vengono eseguite a ogni avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il database **master** viene recuperato durante il processo di avvio. L'impostazione di stored procedure per l'esecuzione automatica può essere utile per le operazioni di manutenzione del database o per l'esecuzione continua delle stored procedure come processi di background. È inoltre possibile utilizzare l'esecuzione automatica delle stored procedure per eseguire attività di sistema o di manutenzione in **tempdb**, ad esempio per creare una tabella temporanea globale. In questo modo si è sempre certi della disponibilità di una tabella temporanea quando **tempdb** viene ricreato durante l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Una stored procedure eseguita automaticamente utilizza le stesse autorizzazioni dei membri del ruolo predefinito del server **sysadmin** . I messaggi di errore generati dalla stored procedure vengono scritti nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Non è previsto alcun limite per le procedure di avvio, ma è necessario tenere presente che ogni procedura richiede un thread di lavoro per l'esecuzione. Se è necessario eseguire più procedure all'avvio, ma non necessariamente in parallelo, è possibile impostare una delle procedure come procedura di avvio che richiama le altre procedure. In questo modo è sufficiente un solo thread di lavoro.  
  
    > [!TIP]  
    >  Evitare di restituire set di risultati da una stored procedure eseguita automaticamente. Poiché la stored procedure viene eseguita da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anziché da un'applicazione o un utente, non è disponibile una destinazione per i set di risultati.  
  
-   Impostazione, annullamento e controllo dell'esecuzione automatica  
  
     Solo l'amministratore di sistema (**sa**) può contrassegnare una stored procedure per l'esecuzione automatica. La stored procedure, inoltre, deve essere nel database **master** , il proprietario deve essere **sa** e deve essere priva di parametri di input o output.  
  
     Usare [sp_procoption](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md) per:  
  
    1.  Designare una stored procedure esistente come procedura di avvio.  
  
    2.  Arrestare l'esecuzione di una stored procedure all'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 Per altre informazioni, vedere [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md) e [Clausola EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Per altre informazioni, vedere la sezione Autorizzazioni in [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-execute-a-stored-procedure"></a>Per eseguire una stored procedure  
  
1.  In **Esplora oggetti** connettersi a un'istanza di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], espandere tale istanza, quindi espandere **Database**.  
  
2.  Espandere il database desiderato, espandere **Programmabilità**, quindi **Stored procedure**.  
  
3.  Fare clic con il pulsante destro del mouse sulla stored procedure definita dall'utente desiderata e scegliere **Esegui stored procedure**.  
  
4.  Nella finestra di dialogo **Esegui stored procedure** specificare un valore per ogni parametro e indicare se deve essere passato un valore Null.  
  
     **Parametro**  
     Indica il nome del parametro.  
  
     **Tipo di dati**  
     Indica il tipo di dati del parametro.  
  
     **Parametro di output**  
     Indica se il parametro è un parametro di output.  
  
     **Passa valore Null**  
     Consente di passare NULL come valore del parametro.  
  
     **Valore**  
     Digitare il valore del parametro al momento della chiamata alla procedura.  
  
5.  Per eseguire la stored procedure, fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-execute-a-stored-procedure"></a>Per eseguire una stored procedure  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio viene illustrato come eseguire una stored procedure che prevede un parametro. Nell'esempio viene eseguita la stored procedure `uspGetEmployeeManagers` con il valore  `6` specificato come parametro `@EmployeeID` .  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC dbo.uspGetEmployeeManagers 6;  
GO  
```  
  
#### <a name="to-set-or-clear-a-procedure-for-executing-automatically"></a>Per impostare o annullare l'esecuzione automatica di una stored procedure  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Questo esempio mostra come usare [sp_procoption](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md) per impostare una stored procedure per l'esecuzione automatica.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_procoption @ProcName = '<procedure name>'   
    , @OptionName = 'startup'   
    , @OptionValue = 'on';  
```  
  
#### <a name="to-stop-a-procedure-from-executing-automatically"></a>Per arrestare l'esecuzione automatica di una stored procedure  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Questo esempio mostra come usare [sp_procoption](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md) per arrestare l'esecuzione automatica di una stored procedure.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_procoption @ProcName = '<procedure name>'   
    , @OptionValue = 'off';  
```  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
  
## <a name="see-also"></a>Vedere anche  
 [Specificare i parametri](../../relational-databases/stored-procedures/specify-parameters.md)   
 [Configurare l'opzione di configurazione del server scan for startup procs](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [Stored procedure &#40;Motore di database&#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)  
  
  
