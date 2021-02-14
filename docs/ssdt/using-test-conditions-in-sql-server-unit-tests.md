---
title: Utilizzo di condizioni di test in unit test di SQL Server
description: Informazioni sulle condizioni di test in unit test di SQL Server. Scoprire come usare le condizioni predefinite e i test negativi e visualizzare informazioni sulle condizioni personalizzate.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.testconditions
ms.assetid: e3d1c86c-1e58-4d2c-b625-d1b591b221aa
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 18021d85c9b490664411b7bb98f1b55972a13022
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082412"
---
# <a name="using-test-conditions-in-sql-server-unit-tests"></a>Utilizzo di condizioni di test in unit test di SQL Server

In uno unit test di SQL Server vengono eseguiti uno o più script di test Transact\-SQL. I risultati possono essere valutati nello script Transact\-SQL e nell'istruzione THROW o RAISERROR usata per restituire un errore e interrompere il test. In alternativa, per valutare i risultati è possibile definire nel test le relative condizioni. Tramite il test viene restituita un'istanza della classe [SqlExecutionResult](/previous-versions/sql/sql-server-data-tools/jj856590(v=vs.103)). Nell'istanza di questa classe sono contenuti uno o più set di dati, il tempo di esecuzione e le righe interessate dallo script. Tutte queste informazioni vengono raccolte durante l'esecuzione dello script. Questi risultati possono essere valutati usando le condizioni di test. SQL Server Data Tools fornisce un set di condizioni di test predefinite. È anche possibile creare e usare condizioni personalizzate. Vedere [Condizioni di test personalizzate per unit test di SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
## <a name="predefined-test-conditions"></a>Condizioni di test predefinite  
Nella tabella seguente sono elencate le condizioni di test predefinite che è possibile aggiungere tramite il riquadro Condizioni di test nella finestra di progettazione unit test di SQL Server.  
  
|**Condizione di test**|**Descrizione della condizione di test**|  
|----------------------|----------------------------------|  
|Checksum di dati|Esito negativo se il valore di checksum del set di risultati restituito dallo script Transact\-SQL non corrisponde al valore di checksum previsto. Per ulteriori informazioni, vedere [Specifica di un checksum di dati](#SpecifyDataChecksum).<br /><br />**NOTA** È consigliabile non usare questa condizione di test se vengono restituiti dati che variano tra le esecuzioni dei test. Se ad esempio il set di risultati contiene date o ore generate oppure colonne Identity, i test avranno esito negativo perché il valore di checksum sarà diverso per ogni esecuzione.|  
|Set di risultati vuoto|Esito negativo se il set di risultati restituito dallo script Transact\-SQL non è vuoto.|  
|Tempo di esecuzione|Esito negativo se l'esecuzione dello script di test Transact\-SQL richiede più tempo del previsto. Il tempo di esecuzione predefinito è di 30 secondi.<br /><br />Il tempo di esecuzione si applica solo allo script di test, non allo script di pre-test o post-test.|  
|Schema previsto|Esito negativo se i tipi di dati e colonne del set di risultati non corrispondono a quelli specificati per la condizione di test. È necessario specificare uno schema tramite le proprietà della condizione di test. Per ulteriori informazioni, vedere [Specifica di uno schema previsto](#SpecifyExpectedSchema).|  
|Senza risultati|Genera sempre un test con un risultato Senza risultati. Si tratta della condizione predefinita aggiunta a ogni test. Questa condizione di test viene inclusa per indicare che la verifica del test non è stata implementata. Eliminare tale condizione dal test dopo l'aggiunta di altre condizioni di test.|  
|Set di risultati non vuoto|Esito negativo se il set di risultati è vuoto. È possibile usare questa condizione di test o EmptyResultSet con la funzione Transact\-SQL @@RAISERROR nello script di test per verificare se un aggiornamento è stato completato. È possibile ad esempio salvare i valori pre-aggiornamento, eseguire l'aggiornamento, confrontare i valori post-aggiornamento e generare un errore se non si ottengono i risultati previsti.|  
|Conteggio righe|Esito negativo se il set di risultati non contiene il numero di righe previsto.|  
|Valore scalare|Esito negativo se un determinato valore nel set di risultati non è uguale al valore specificato. Il **Valore previsto** predefinito è Null.|  
  
> [!NOTE]  
> La condizione di test Tempo di esecuzione consente di specificare un limite di tempo entro il quale lo script di test Transact\-SQL deve essere eseguito. Se tale limite di tempo viene superato, il test ha esito negativo. I risultati di test includono inoltre una statistica relativa alla durata, diversa dalla condizione di test Tempo di esecuzione. Nella statistica relativa alla durata sono inclusi non solo il tempo di esecuzione, ma anche il tempo per connettersi due volte al database, ovvero il tempo per eseguire gli eventuali altri script di test, ad esempio gli script di pre-test e post-test, e il tempo per eseguire le condizioni di test. È quindi possibile che un test venga completato anche se la durata è maggiore del tempo di esecuzione.  
>   
> La durata segnalata non include il tempo utilizzato per la generazione di dati e la distribuzione dello schema, perché queste operazioni si verificano prima dell'esecuzione dei test. Per visualizzare la durata del test, selezionare un'esecuzione di test nella finestra **Risultati test**, fare clic con il pulsante destro del mouse e scegliere **Visualizza dettagli risultati test**.  
  
È possibile aggiungere condizioni di test agli unit test di SQL Server tramite il riquadro Condizioni di test della finestra di progettazione unit test di SQL Server. Per altre informazioni, vedere [Procedura: Aggiungere condizioni di test a unit test di SQL Server](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md).  
  
È inoltre possibile modificare direttamente il codice del metodo di test per aggiungere ulteriori funzionalità. Per altre informazioni, vedere [Procedura: Aprire uno unit test di SQL Server per la modifica](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md) e [Procedura: Scrivere uno unit test di SQL Server in esecuzione nell'ambito di una singola transazione](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md). È possibile ad esempio aggiungere funzionalità a un metodo di test tramite l'aggiunta di istruzioni Assert. Per altre informazioni, vedere [Uso di asserzioni Transact-SQL in unit test di SQL Server](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md).  
  
## <a name="expected-failures"></a>Errori previsti  
È possibile creare unit test di SQL Server per verificare il comportamento che dovrebbe avere esito negativo. Questi errori previsti vengono a volte definiti test negativi. Di seguito sono riportati alcuni esempi comuni:  
  
-   Verificare che una stored procedure tramite cui vengono eliminati i dati di un cliente abbia esito negativo se si specifica un ID cliente non valido.  
  
-   Verificare che una stored procedure tramite cui viene compilato un ordine abbia esito negativo se l'ordine non è mai stato effettuato o se è già stato compilato.  
  
-   Verificare che mediante una stored procedure tramite cui viene annullato un ordine non sia possibile annullare ordini completati oppure già annullati.  
  
È possibile definire unit test di SQL Server per stored procedure che generano eccezioni previste. È possibile aggiungere un attributo al metodo di unit test per indicare l'eccezione o le eccezioni previste. In questo modo, si impedisce che il test abbia esito negativo quando si verifica un'eccezione.  
  
Per contrassegnare un metodo di unit test di SQL Server con le eccezioni previste, aggiungere l'attributo seguente:  
  
```  
[ExpectedSqlException(MessageNumber = nnnnn, Severity = x, MatchFirstError = false, State = y)]  
```  
  
Dove:  
  
-   *nnnnn* è il numero del messaggio previsto, ad esempio 14025  
  
-   *x* è il livello di gravità dell'eccezione prevista  
  
-   *y* è lo stato dell'eccezione prevista.  
  
Tutti i parametri non specificati vengono ignorati. Passare questi parametri all'istruzione **THROW** nel codice di database. Se si specifica MatchFirstError = false, l'attributo corrisponderà a uno qualsiasi dei valori SqlErrors nell'eccezione. Il comportamento predefinito (MatchFirstError = true) troverà una corrispondenza solo per il primo errore che si verifica.  
  
Per un esempio di come usare le eccezioni previste e uno unit test negativo di SQL Server, vedere [Procedura dettagliata: Creazione ed esecuzione di uno unit test di SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
## <a name="specifying-a-data-checksum"></a><a name="SpecifyDataChecksum"></a>Specifica di un checksum di dati  
Per visualizzare la finestra di progettazione unit test di SQL Server, fare doppio clic sul file del codice sorgente dello unit test in **Esplora soluzioni**.  
  
Dopo avere aggiunto una condizione di test Checksum di dati allo unit test del database, è necessario configurare il valore di checksum previsto utilizzando la procedura seguente:  
  
#### <a name="to-specify-an-expected-checksum"></a>Per specificare un valore di checksum previsto  
  
1.  Nell'elenco delle condizioni di test fare clic sulla condizione di test Checksum di dati per cui si desidera specificare un checksum.  
  
2.  Premere F4 per aprire la finestra **Proprietà** . È inoltre possibile aprire il menu **Visualizza** e fare clic sulla finestra **Proprietà** .  
  
3.  (Facoltativo) È possibile modificare la proprietà **(Nome)** della condizione di test per renderla più descrittiva.  
  
4.  Nella proprietà **Configurazione** fare clic sul pulsante Sfoglia ( **...** ).  
  
    Verrà visualizzata la finestra di dialogo **Configurazione per NomeCondizioneTest** .  
  
5.  Specificare una connessione al database che si desidera testare. Per altre informazioni, vedere [Procedura: Creare una connessione al database](/previous-versions/visualstudio/visual-studio-2010/aa833420(v=vs.100)).  
  
6.  Per impostazione predefinita, il corpo Transact\-SQL del test viene visualizzato nel riquadro di modifica. È possibile modificare il codice per fornire i risultati previsti, se necessario. Se ad esempio nel pre-test è incluso codice, potrebbe essere necessario aggiungere tale codice.  
  
    > [!IMPORTANT]  
    > Se si modifica una condizione di checksum per la quale in precedenza è stato specificato un valore di checksum, le eventuali modifiche apportate nel riquadro di modifica non verranno salvate. È necessario effettuare nuovamente le modifiche prima di fare clic **Recupera**.  
  
7.  Fare clic su **Recupera**.  
  
    Il codice Transact\-SQL viene eseguito sulla connessione al database specificata e i risultati vengono visualizzati nella finestra di dialogo.  
  
8.  Se i risultati corrispondono ai risultati previsti del test, fare clic su **OK**. In caso contrario modificare il corpo Transact\-SQL e ripetere i passaggi 6, 7 e 8 fino a quando i risultati non corrisponderanno a quelli previsti.  
  
    Nella colonna **Valore** relativa alla condizione di test viene visualizzato il valore di checksum previsto.  
  
## <a name="specifying-an-expected-schema"></a><a name="SpecifyExpectedSchema"></a>Specifica di uno schema previsto  
Dopo aver aggiunto una condizione di test Schema previsto allo unit test di SQL Server, è necessario configurare lo schema previsto usando la procedura seguente:  
  
#### <a name="to-specify-an-expected-schema"></a>Per specificare uno schema previsto  
  
1.  Nell'elenco delle condizioni di test fare clic sulla condizione di test Schema previsto per cui si desidera specificare uno schema.  
  
2.  Premere F4 per aprire la finestra **Proprietà** . È inoltre possibile aprire il menu **Visualizza** e fare clic sulla finestra **Proprietà** .  
  
3.  (Facoltativo) È possibile modificare la proprietà **(Nome)** della condizione di test per renderla più descrittiva.  
  
4.  Nella proprietà **Configurazione** fare clic sul pulsante Sfoglia ( **...** ).  
  
    Verrà visualizzata la finestra di dialogo **Configurazione per NomeCondizioneTest** .  
  
5.  Specificare una connessione al database che si desidera testare. Per altre informazioni, vedere [Procedura: Creare una connessione al database](/previous-versions/visualstudio/visual-studio-2010/aa833420(v=vs.100)).  
  
6.  Per impostazione predefinita, il corpo Transact\-SQL del test viene visualizzato nel riquadro di modifica. È possibile modificare il codice per fornire i risultati previsti, se necessario. Se ad esempio nel pre-test è incluso codice, potrebbe essere necessario aggiungere tale codice.  
  
    > [!IMPORTANT]  
    > Se si modifica una condizione di schema previsto per la quale in precedenza è stato specificato uno schema, le eventuali modifiche apportate nel riquadro di modifica non verranno salvate. È necessario effettuare nuovamente le modifiche prima di fare clic **Recupera**.  
  
7.  Fare clic su **Recupera**.  
  
    Il codice Transact\-SQL viene eseguito sulla connessione al database specificata e i risultati vengono visualizzati nella finestra di dialogo. Poiché si verifica lo schema, o la forma, del set di risultati e non i valori dei risultati, non è necessario visualizzare dati nei risultati restituiti, a condizione che le colonne siano riprodotte nel modo previsto.  
  
8.  Se i risultati corrispondono ai risultati previsti del test, fare clic su **OK**. In caso contrario modificare il corpo Transact\-SQL e ripetere i passaggi 6, 7 e 8 fino a quando i risultati non corrisponderanno a quelli previsti.  
  
    Nella colonna **Valore** relativa alla condizione di test vengono visualizzate informazioni sullo schema previsto. Ad esempio, potrebbe essere indicato "Previsto: 2 tabelle".  
  
## <a name="extensible-test-conditions"></a>Condizioni di test estendibili  
Oltre alle sei condizioni di test predefinite, è possibile scrivere nuove condizioni di test personalizzate. Queste condizioni di test verranno visualizzate nel riquadro Condizioni di test della finestra di progettazione unit test di SQL Server. Per altre informazioni, vedere [Condizioni di test personalizzate per unit test di SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Uso di asserzioni Transact-SQL in unit test di SQL Server](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md)  
[Script in unit test di SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md)  
