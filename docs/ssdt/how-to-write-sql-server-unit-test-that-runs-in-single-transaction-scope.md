---
title: Scrivere uno unit test di SQL Server che esegue un ambito di una singola transazione
description: Informazioni su come avviare il servizio Distributed Transaction Coordinator, scrivere un unit test di SQL Server per una singola transazione ed eseguire il rollback delle modifiche apportate al test.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cb241e94-d81c-40e9-a7ae-127762a6b855
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6c03450778defd73b21c19d72aa1e0acc6af342b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017941"
---
# <a name="how-to-write-a-sql-server-unit-test-that-runs-within-the-scope-of-a-single-transaction"></a>Procedura: Scrivere uno unit test di SQL Server in esecuzione nell'ambito di una singola transazione

È possibile modificare unit test in modo che vengano eseguiti nell'ambito di una singola transazione. Se si adotta questo approccio, al termine del test è possibile eseguire il rollback di tutte le modifiche apportate dal test. Nelle procedure seguenti viene descritto come procedere:  
  
-   Creare una transazione nello script di test Transact\-SQL che usa **BEGIN TRANSACTION** e **ROLLBACK TRANSACTION**.  
  
-   Creare una transazione per un singolo metodo di test in una classe di test.  
  
-   Creare una transazione per tutti i metodi di test in una determinata classe di test.  
  
**Prerequisiti**  
  
Per alcune procedure di questo argomento è necessario che il servizio Distributed Transaction Coordinator sia in esecuzione nel computer in cui vengono eseguiti gli unit test. Per ulteriori informazioni, vedere la procedura alla fine di questo argomento.  
  
## <a name="to-create-a-transaction-using-transact-sql"></a>Per creare una transazione tramite Transact\-SQL  
  
#### <a name="to-create-a-transaction-using-transact-sql"></a>Per creare una transazione tramite Transact\-SQL  
  
1.  Aprire uno unit test nella finestra di progettazione unit test di SQL Server. Per visualizzare la finestra di progettazione, fare doppio clic sul file del codice sorgente dello unit test.  
  
2.  Specificare il tipo di script per il quale si desidera creare la transazione. È ad esempio possibile specificare pre-test, test o post-test.  
  
3.  Immettere uno script di test nell'editor Transact\-SQL.  
  
4.  Inserire le istruzioni `BEGIN TRANSACTION` e `ROLLBACK TRANSACTION`, come illustrato in questo semplice esempio. Nell'esempio viene utilizzata una tabella di database denominata OrderDetails che contiene 50 righe di dati:  
  
    ```  
    BEGIN TRANSACTION TestTransaction  
    UPDATE "OrderDetails" set Quantity = Quantity + 10  
    IF @@ROWCOUNT!=50  
    RAISERROR('Row count does not equal 50',16,1)  
    ROLLBACK TRANSACTION TestTransaction  
    ```  
  
    > [!NOTE]  
    > Non è possibile eseguire il rollback di una transazione dopo l'esecuzione dell'istruzione COMMIT TRANSACTION.  
  
    Per altre informazioni sul funzionamento di ROLLBACK TRANSACTION con stored procedure e trigger, vedere la pagina nel sito Web Microsoft: [ROLLBACK TRANSACTION (Transact-SQL)](../t-sql/language-elements/rollback-transaction-transact-sql.md).  
  
## <a name="to-create-a-transaction-for-a-single-test-method"></a>Per creare una transazione per un singolo metodo di test  
In questo esempio viene usata una transazione di ambiente quando si usa il tipo [System.Transactions.TransactionScope](/dotnet/api/system.transactions.transactionscope). Per impostazione predefinita, nelle connessioni di esecuzione e con privilegi non verrà utilizzata la transazione di ambiente, poiché le connessioni sono state create prima dell'esecuzione del metodo. In SqlConnection è incluso un metodo [System.Data.SqlClient.SqlConnection.EnlistTransaction](/dotnet/api/system.data.sqlclient.sqlconnection.enlisttransaction) che associa una connessione attiva a una transazione. Quando si crea una transazione di ambiente, questa viene registrata automaticamente come la transazione corrente ed è possibile accedervi tramite la proprietà [System.Transactions.Transaction.Current](/dotnet/api/system.transactions.transaction.current). In questo esempio viene eseguito il rollback della transazione quando la transazione di ambiente viene eliminata. Se si vuole eseguire il commit delle modifiche apportate dopo l'esecuzione dello unit test, è necessario chiamare il metodo [System.Transactions.TransactionScope.Complete](/dotnet/api/system.transactions.transactionscope.complete).  
  
#### <a name="to-create-a-transaction-for-a-single-test-method"></a>Per creare una transazione per un singolo metodo di test  
  
1.  In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul nodo **Riferimenti** nel progetto di test e scegliere **Aggiungi riferimento**.  
  
    Verrà visualizzata la finestra di dialogo **Aggiungi riferimento**.  
  
2.  Fare clic sulla scheda **.NET**.  
  
3.  Nell'elenco degli assembly fare clic su **System.Transactions** e quindi scegliere **OK**.  
  
4.  Aprire il file Visual Basic o C# per lo unit test.  
  
5.  Eseguire il wrapping delle azioni pre-test, test e post-test come illustrato nell'esempio di codice Visual Basic seguente:  
  
    ```  
    <TestMethod()> _  
    Public Sub dbo_InsertTable1Test()  
  
        Using ts as New System.Transactions.TransactionScope( System.Transactions.TransactionScopeOption.Required)  
            ExecutionContext.Connection.EnlistTransaction(Transaction.Current)  
            PrivilegedContext.Connection.EnlistTransaction(Transaction.Current)  
  
            Dim testActions As DatabaseTestActions = Me.dbo_InsertTable1TestData  
            'Execute the pre-test script  
            '  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.PretestAction) Is Nothing), "Executing pre-test script...")  
            Dim pretestResults() As ExecutionResult = TestService.Execute(Me.PrivilegedContext, Me.PrivilegedContext, testActions.PretestAction)  
            'Execute the test script  
  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.TestAction) Is Nothing), "Executing test script...")  
            Dim testResults() As ExecutionResult = TestService.Execute(ExecutionContext, Me.PrivilegedContext, testActions.TestAction)  
  
            'Execute the post-test script  
            '  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.PosttestAction) Is Nothing), "Executing post-test script...")  
            Dim posttestResults() As ExecutionResult = TestService.Execute(Me.PrivilegedContext, Me.PrivilegedContext, testActions.PosttestAction)  
  
            'Because the transaction is not explicitly committed, it  
            'is rolled back when the ambient transaction is   
            'disposed.  
            'To commit the transaction, remove the comment delimiter  
            'from the following statement:  
            'ts.Complete()  
  
    End Sub  
    Private dbo_InsertTable1TestData As DatabaseTestActions  
    ```  
  
    > [!NOTE]  
    > Se si usa Visual Basic, è necessario aggiungere `Imports System.Transactions` (oltre a `Imports Microsoft.VisualStudio.TestTools.UnitTesting`, `Imports Microsoft.VisualStudio.TeamSystem.Data.UnitTesting` e `Imports Microsoft.VisualStudio.TeamSystem.Data.UnitTest.Conditions`). Se si usa Visual C#, è necessario aggiungere `using System.Transactions` (oltre alle istruzioni `using` per Microsoft.VisualStudio.TestTools, Microsoft.VisualStudio.TeamSystem.Data.UnitTesting e Microsoft.VisualStudio.TeamSystem.Data.UnitTesting.Conditions). È inoltre necessario aggiungere un riferimento al progetto a tali assembly.  
  
## <a name="to-create-a-transaction-for-all-test-methods-in-a-test-class"></a>Per creare una transazione per tutti i metodi di test in una classe di test  
  
#### <a name="to-create-a-transaction-for-all-test-methods-in-a-test-class"></a>Per creare una transazione per tutti i metodi di test in una classe di test  
  
1.  Aprire il file Visual Basic o C# per lo unit test.  
  
2.  Creare la transazione in TestInitialize ed eliminarla in TestCleanup, come illustrato nell'esempio di codice Visual C# seguente:  
  
    ```  
    TransactionScope _trans;  
  
            [TestInitialize()]  
            public void Init()  
            {  
                _trans = new TransactionScope();  
                base.InitializeTest();  
            }  
  
            [TestCleanup()]  
            public void Cleanup()  
            {  
                base.CleanupTest();  
                _trans.Dispose();  
            }  
  
            [TestMethod()]  
            public void TransactedTest()  
            {  
                DatabaseTestActions testActions = this.DatabaseTestMethod1Data;  
                // Execute the pre-test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.PretestAction != null), "Executing pre-test script...");  
                ExecutionResult[] pretestResults = TestService.Execute(this.PrivilegedContext, this.PrivilegedContext, testActions.PretestAction);  
                // Execute the test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.TestAction != null), "Executing test script...");  
                ExecutionResult[] testResults = TestService.Execute(this.ExecutionContext, this.PrivilegedContext, testActions.TestAction);  
                // Execute the post-test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.PosttestAction != null), "Executing post-test script...");  
                ExecutionResult[] posttestResults = TestService.Execute(this.PrivilegedContext, this.PrivilegedContext, testActions.PosttestAction);  
  
            }  
    ```  
  
## <a name="to-start-the-distributed-transaction-coordinator-service"></a>Per avviare il servizio Distributed Transaction Coordinator  
Per alcune procedure di questo argomento vengono utilizzati tipi dell'assembly System.Transactions. Prima di utilizzare queste procedure, è necessario assicurarsi che il servizio Distributed Transaction Coordinator sia in esecuzione nel computer in cui si eseguono gli unit test. In caso contrario, i test avranno esito negativo e verrà visualizzato il seguente messaggio di errore: "Il metodo di test *ProjectName*.*TestName*.*MethodName* ha generato un'eccezione: System.Data.SqlClient.SqlException: MSDTC sul server '*ComputerName*' non è disponibile".  
  
#### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Per avviare il servizio Distributed Transaction Coordinator  
  
1.  Aprire il **Pannello di controllo**.  
  
2.  Nel **Pannello di controllo** aprire **Strumenti di amministrazione**.  
  
3.  In **Strumenti di amministrazione** aprire **Servizi**.  
  
4.  Nel riquadro **Servizi** fare clic con il pulsante destro del mouse sul servizio **Distributed Transaction Controller** e scegliere **Avvia**.  
  
    Lo stato del servizio verrà aggiornato in **Avviato**. A questo punto dovrebbe essere possibile eseguire unit test che utilizzano System.Transactions.  
  
> [!IMPORTANT]  
> È possibile che il messaggio di errore seguente venga visualizzato anche se è stato avviato il servizio Distributed Transaction Controller: `System.Transactions.TransactionManagerCommunicationException: Network access for Distributed Transaction Manager (MSDTC) has been disabled. Please enable DTC for network access in the security configuration for MSDTC using the Component Services Administrative tool. ---> System.Runtime.InteropServices.COMException: The transaction manager has disabled its support for remote/network transactions. (Exception from HRESULT: 0x8004D024)`. Se viene visualizzato questo errore, è necessario configurare il servizio Distributed Transaction Controller per l'accesso alla rete. Per altre informazioni, vedere [Abilitare l'accesso DTC alla rete](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753510(v=ws.10)).  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
