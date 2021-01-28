---
title: Utilizzo di asserzioni Transact-SQL in unit test di SQL Server
description: Informazioni sulle asserzioni Transact-SQL. Scoprire quando usare le asserzioni negli unit test di SQL Server, quando usare le condizioni di test e visualizzare esempi di uso delle asserzioni.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 55d8be9c-9282-47d3-be7f-e2c26f00c95e
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: f7b6eaf18166bd546e8eb3e89ee2176c39dfb49a
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564419"
---
# <a name="using-transact-sql-assertions-in-sql-server-unit-tests"></a>Utilizzo di asserzioni Transact-SQL in unit test di SQL Server

In uno unit test di SQL Server, uno script di test Transact\-SQL viene eseguito e restituisce un risultato. Talvolta, i risultati vengono restituiti come set di risultati. È possibile convalidare i risultati utilizzando le condizioni di test. È ad esempio possibile utilizzare una condizione di test per controllare il numero di righe restituite in un determinato set di risultati o per verificare il tempo necessario per l'esecuzione di un test specifico. Per altre informazioni sulle condizioni di test, vedere [Uso delle condizioni di test negli unit test di SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md).  
  
Invece di usare le condizioni di test, è possibile usare anche asserzioni Transact\-SQL, vale a dire le istruzioni THROW o RAISERROR in uno script Transact\-SQL. In determinate circostanze, è preferibile usare un'asserzione Transact\-SQL invece di una condizione di test.  
  
## <a name="using-transact-sql-assertions"></a>Utilizzo di asserzioni Transact-SQL  
Prima di decidere di convalidare i dati usando asserzioni Transact\-SQL o condizioni di test, è consigliabile tenere conto delle considerazioni seguenti.  
  
-   **Prestazioni**. È più rapido eseguire un'asserzione Transact\-SQL nel server anziché spostare prima i dati in un computer client e quindi modificarli localmente.  
  
-   **Familiarità con il linguaggio**. È possibile che si preferisca usare un particolare linguaggio in base alla propria esperienza e che quindi si scelgano le asserzioni Transact\-SQL oppure le condizione di test Visual C\# o Visual Basic.  
  
-   **Complessità della convalida**. In alcuni casi, è possibile compilare test più complessi in Visual C\# o in Visual Basic e convalidare i test nel client.  
  
-   **Semplicità**. Spesso è più semplice usare una condizione di test predefinita anziché scrivere lo script equivalente in Transact\-SQL.  
  
-   **Librerie di convalida legacy**. Se è già presente del codice per l'esecuzione della convalida, è possibile usarlo in uno unit test di SQL Server invece di usare condizioni di test.  
  
## <a name="mark-unit-test-methods-with-the-expected-exception"></a>Contrassegnare metodi di unit test con l'eccezione prevista  
Per contrassegnare un metodo di unit test di SQL Server con le eccezioni previste, aggiungere l'attributo seguente:  
  
```vb  
<ExpectedSqlException(MessageNumber=nnnnn, Severity=x, MatchFirstError=false, State=y)> _  
```  
  
```csharp  
[ExpectedSqlException(MessageNumber=nnnnn, Severity=x, MatchFirstError=false, State=y)]  
```  
  
Dove:  
  
-   *nnnnn* è il numero del messaggio previsto, ad esempio 14025  
  
-   *x* è il livello di gravità dell'eccezione prevista  
  
-   *y* è lo stato dell'eccezione prevista.  
  
Tutti i parametri non specificati vengono ignorati. Passare questi parametri all'istruzione RAISERROR nel codice di database. Se si specifica MatchFirstError = true, l'attributo corrisponderà a uno qualsiasi dei valori SqlErrors nell'eccezione. Il comportamento predefinito (MatchFirstError = true) troverà una corrispondenza solo per il primo errore che si verifica.  
  
Per un esempio di come usare le eccezioni previste e uno unit test negativo di SQL Server, vedere [Procedura dettagliata: Creazione ed esecuzione di uno unit test di SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
## <a name="the-raiserror-statement"></a>Istruzione RAISERROR  
  
> [!NOTE]  
> Utilizzare THROW anziché RAISERROR. RAISERROR è deprecata.  
  
È possibile usare direttamente asserzioni Transact\-SQL nel server usando l'istruzione RAISERROR nello script Transact\-SQL. La relativa sintassi è la seguente:  
  
**RAISERROR ( \@ ErrorMessage, \@ ErrorSeverity, \@ errorState)**  
  
dove:  
  
@ErrorMessage è qualsiasi messaggio di errore definito dall'utente. È possibile formattare questa stringa di messaggio in modo simile alla funzione printf_s.  
  
@ErrorSeverity è un livello di gravità definito dall'utente compreso tra 0 e 18.  
  
> [!NOTE]  
> I valori '0' e '10' per il livello di gravità non causano l'esito negativo dello unit test di SQL Server. È possibile utilizzare qualsiasi altro valore nell'intervallo compreso tra 0 e 18 per causare l'esito negativo del test.  
  
@ErrorState è un numero intero arbitrario compreso tra 1 e 127. È possibile utilizzare questo numero intero per differenziare le occorrenze di un singolo errore generato in punti diversi del codice.  
  
Per altre informazioni, vedere [RAISERROR (Transact-SQL)](../t-sql/language-elements/raiserror-transact-sql.md). Un esempio dell'uso di RAISERROR in uno unit test di SQL Server è disponibile nell'argomento [Procedura: Scrivere uno unit test di SQL Server in esecuzione nell'ambito di una singola transazione](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md).  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Uso di condizioni di test in unit test di SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
[Verifica del codice di database tramite unit test di SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[Procedura: Aprire uno unit test di SQL Server per la modifica](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md)  
