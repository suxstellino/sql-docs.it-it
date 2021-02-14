---
title: Creare condizioni di test per la finestra di progettazione unit test di SQL Server
description: Informazioni su come estendere la classe TestCondition per creare una condizione di test personalizzata per la finestra di progettazione unit test di SQL Server. Visualizzare un esempio di una condizione di test personalizzata.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 48076062-1ef5-419a-8a55-3c7b4234cc35
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d74fe60ff94d2893dc5a2dd899643cabfd0a4f30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018281"
---
# <a name="how-to-create-test-conditions-for-the-sql-server-unit-test-designer"></a>Procedura: Creare condizioni di test per la finestra di progettazione unit test di SQL Server

È possibile usare la classe [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) estendibile per creare nuove condizioni di test. Ad esempio, è possibile creare una nuova condizione di test che verifica il numero delle colonne o i valori restituiti in un set di risultati.  
  
## <a name="to-create-a-test-condition"></a>Per creare una condizione di test  
In questa procedura viene spiegato come creare una condizione di test da visualizzare nella finestra di progettazione unit test di SQL Server.  
  
1.  In Visual Studio creare un progetto Libreria di classi.  
  
2.  Scegliere **Aggiungi riferimento** dal menu **Progetto**.  
  
3.  Fare clic sulla scheda **.NET**.  
  
4.  Nell'elenco **Nome componente** selezionare **System.ComponentModel.Composition** e quindi fare clic su **OK**.  
  
5.  Aggiungere i riferimenti di assembly necessari. Fare clic con il pulsante destro del mouse sul nodo del progetto e quindi scegliere **Aggiungi riferimento**. Fare clic su **Sfoglia** e passare alla cartella C:\Programmi (x86)\\MicrosoftSQL Server\110\DAC\Bin. Scegliere Microsoft.Data.Tools.Schema.Sql.dll e fare clic su Aggiungi, quindi fare clic su OK.  
  
6.  Scegliere **Scarica progetto** dal menu **Progetto**.  
  
7.  Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Modifica <project name>.csproj**.  
  
8.  Aggiungere le seguenti istruzioni Import dopo l'importazione di Microsoft.CSharp.targets:  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
9. Salvare il file e chiuderlo. Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Ricarica progetto**.  
  
10. Derivare la classe dalla classe [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)).  
  
11. Firmare l'assembly con un nome sicuro. Per altre informazioni, vedere [Procedura: Firmare un assembly con un nome sicuro](/dotnet/standard/assembly/sign-strong-name).  
  
12. Compilare la libreria di classi.  
  
13. Prima di poter usare la nuova condizione di test, è necessario copiare l'assembly firmato nella cartella %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions. Se la cartella non esiste, è necessario crearla. È necessario disporre dei privilegi amministrativi del computer in uso per eseguire la copia in questa directory.  
  
14. Installare la condizione di test. Per altre informazioni, vedere [Condizioni di test personalizzate per unit test di SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
15. Aggiungere un nuovo unit test di SQL Server al progetto per creare un riferimento alla condizione di test da aggiungere al progetto. È possibile aggiungere manualmente un riferimento all'assembly della condizione di test nel progetto. Ricaricare la finestra di progettazione dopo questo passaggio.  
  
    > [!NOTE]  
    > Per creare il riferimento è necessario aggiungere una classe di test. È possibile eliminare la classe di test una volta aggiunto il riferimento.  
  
Nel seguente esempio, viene creata una condizione di test semplice che verifica il numero delle colonne restituite in ResultSet. È possibile utilizzare questa condizione di test semplice per essere certi che il contratto per una stored procedure sia corretto.  
  
```  
using System;  
using System.ComponentModel;  
using System.Data;  
using System.Data.Common;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
namespace Ssdt.Samples.SqlUnitTesting  
{  
  
    [ExportTestCondition("ResultSet Column Count", typeof(ResultSetColumnCountCondition))]  
    public class ResultSetColumnCountCondition : TestCondition  
    {  
        private int _resultSet;  
        private int _count;  
        private int _batch;  
  
        public ResultSetColumnCountCondition()  
        {  
            _resultSet = 1;  
            _count = 0;  
            _batch = 1;  
        }  
  
        // method you need to override  
        // to perform the condition verification  
        public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
        {  
            // call base for parameter validation  
            base.Assert(validationConnection, results);  
  
            // verify batch exists  
            if (results.Length < _batch)  
                throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
            SqlExecutionResult result = results[_batch - 1];  
  
            // verify resultset exists  
            if (result.DataSet.Tables.Count < ResultSet)  
                throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
            DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
            // actual condition verification  
            // verify resultset column count matches expected  
            if (table.Columns.Count != Count)  
                throw new DataException(String.Format(  
                    "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                    ResultSet, table.Columns.Count, Count));  
        }  
  
        // this method is called to provide the string shown in the  
        // test conditions panel grid describing what the condition tests  
        public override string ToString()  
        {  
            return String.Format(  
                "Condition fails if ResultSet {0} does not contain {1} columns",  
                ResultSet, Count);  
        }  
  
        // below are the test condition properties  
        // that are exposed to the user in the property browser  
        #region Properties  
  
        // property specifying the resultset for which  
        // you want to check the column count  
        [Category("Test Condition")]  
        [DisplayName("ResultSet")]  
        [Description("ResultSet Number")]  
        public int ResultSet  
        {  
            get { return _resultSet; }  
  
            set  
            {  
                //basic validation  
                if (value < 1)  
                    throw new ArgumentException("ResultSet cannot be less than 1");  
  
                _resultSet = value;  
            }  
        }  
  
        // property specifying  
        // expected column count  
        [Category("Test Condition")]  
        [DisplayName("Count")]  
        [Description("Column Count")]  
        public int Count  
        {  
            get { return _count; }  
  
            set  
            {  
                //basic validation  
                if (value < 0)  
                    throw new ArgumentException("Count cannot be less than 0");  
  
                _count = value;  
            }  
        }  
  
        #endregion  
    }  
}  
```  
  
La classe per la condizione di test personalizzata eredita dalla classe base [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)). Grazie alle proprietà aggiuntive della condizione di test personalizzata, gli utenti possono configurare la condizione dalla finestra Proprietà, una volta installata la condizione.  
  
[ExportTestConditionAttribute](/previous-versions/sql/sql-server-data-tools/jj856578(v=vs.103)) deve essere aggiunto alle classi che estendono [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)). Questo attributo consente a SQL Server Data Tools di individuare la classe e di usarla durante la progettazione e l'esecuzione di unit test. L'attributo accetta due parametri:  
  
|Parametro attributo|Posizione|Descrizione|  
|-----------------------|------------|---------------|  
|DisplayName|1|Identifica la stringa nella casella combinata "Condizioni di test". Il nome deve essere univoco. Se due condizioni hanno lo stesso nome visualizzato, verrà visualizzata la prima condizione trovata e verrà restituito un avviso nella gestione degli errori di Visual Studio.|  
|ImplementingType|2|Viene utilizzato per identificare in modo univoco l'estensione. È necessario modificare questo parametro in modo che corrisponda al tipo su cui si imposta l'attributo. In questo esempio viene usato il tipo **ResultSetColumnCountCondition**, quindi usare **typeof(ResultSetColumnCountCondition)** . Se il tipo è **NewTestCondition**, usare **typeof(NewTestCondition)** .|  
  
Nel seguente esempio vengono aggiunte due proprietà. Gli utenti della condizione di test personalizzata possono utilizzare la proprietà ResultSet per specificare per quale set di risultati si deve verificare il totale delle colonne. Quindi, gli utenti possono utilizzare la proprietà Count per specificare il totale delle colonne previsto.  
  
Vengono aggiunti tre attributi per ogni proprietà:  
  
-   Nome della categoria che consente di organizzare le proprietà.  
  
-   Nome visualizzato della proprietà.  
  
-   Descrizione della proprietà.  
  
Viene eseguita la convalida sulle proprietà al fine di verificare che il valore della proprietà ResultSet non sia inferiore a 1 e che il valore della proprietà Count sia maggiore di zero.  
  
Il metodo Assert esegue l'attività primaria della condizione di test. Eseguire l'override del metodo Assert per convalidare che la condizione prevista è soddisfatta. Il metodo fornisce due parametri:  
  
-   Il primo parametro è la connessione al database utilizzata per la convalida della condizione di test.  
  
-   Il secondo e più importante parametro è la matrice dei risultati che restituisce un solo elemento di matrice per ogni batch eseguito.  
  
È supportato un solo batch per ogni script di test. Di conseguenza, le condizioni di test esamineranno sempre il primo elemento della matrice. L'elemento della matrice contiene un set di dati che, a sua volta, contiene i set di risultati restituiti per lo script di test. In questo esempio, il codice verifica che la tabella dei dati del set di dati contenga il numero di colonne appropriato. Per ulteriori informazioni, vedere Set di dati.  
  
È necessario impostare la libreria di classi contenente la condizione di test da firmare. Tale operazione può essere eseguita nella proprietà del progetto della scheda Firma.  
  
## <a name="see-also"></a>Vedere anche  
[Condizioni di test personalizzate per unit test di SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
