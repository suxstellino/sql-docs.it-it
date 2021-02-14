---
title: Condizione di test personalizzata per verificare i risultati di una stored procedure
description: Procedura dettagliata per la configurazione di una condizione di test personalizzata che controlla se una stored procedure restituisce il numero corretto di colonne.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 4c33b494-a85e-4dd2-97b6-c88ee858a99c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d434df88cae0a5fb28add27cc9785e659f059ced
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042491"
---
# <a name="walkthrough-using-a-custom-test-condition-to-verify-the-results-of-a-stored-procedure"></a>Procedura dettagliata: Uso di una condizione di test personalizzata per verificare i risultati di una stored procedure

In questa procedura dettagliata dell'estensione di funzionalità verrà creata una condizione di test di cui ne verrà verificata la funzionalità tramite la creazione di uno unit test di SQL Server. Il processo include la creazione di un progetto libreria di classi per la condizione di test, nonché la firma e l'installazione della condizione di test. Se è già disponibile una condizione di test che si vuole aggiornare, vedere [Procedura: Aggiornare una condizione di test personalizzata di Visual Studio 2010 da una versione precedente a SQL Server Data Tools](../ssdt/how-to-upgrade-visual-studio-2010-custom-test-condition-to-ssdt.md).  
  
Vengono illustrate le attività seguenti:  
  
-   Creazione di una condizione di test.  
  
-   Firma dell'assembly con un nome sicuro.  
  
-   Aggiunta dei riferimenti necessari al progetto.  
  
-   Compilazione di una condizione di test.  
  
-   Installazione della nuova condizione di test.  
  
-   Test della nuova condizione di test.  
  
È necessario avere a disposizione Visual Studio 2010 o Visual Studio 2012 con la versione più recente di SQL Server Data Tools per completare questa procedura dettagliata. Per altre informazioni, vedere [Installare SQL Server Data Tools](./download-sql-server-data-tools-ssdt.md).  
  
## <a name="creating-a-custom-test-condition"></a>Creazione di una condizione di test personalizzata  
Creare per prima cosa una libreria di classi.  
  
1.  Nel menu **File** fare clic su **Nuovo** e quindi su **Progetto**.  
  
2.  Nella finestra di dialogo **Nuovo progetto** fare clic su Visual C\# in **Tipi di progetto**.  
  
3.  In **Modelli** selezionare **Libreria di classi**.  
  
4.  Nella casella di testo **Nome** digitare **ColumnCountCondition** e quindi fare clic su **OK**.  
  
Nella prossima esercitazione verrà illustrato come firmare il progetto.  
  
1.  Scegliere **Proprietà di ColumnCountCondition** dal menu **Progetto**.  
  
2.  Nella scheda **Firma** selezionare la casella di controllo **Firma assembly**.  
  
3.  Nella casella **Scegli un file chiave con nome sicuro** fare clic su **\<New...>** .  
  
    Verrà visualizzata la finestra di dialogo **Crea chiave con nome sicuro**.  
  
4.  Nella casella **Nome file di chiave** digitare **SampleKey**.  
  
5.  Digitare e confermare una password, quindi fare clic su **OK**. Quando si compila la soluzione, il file di chiave viene utilizzato per firmare l'assembly.  
  
6.  Scegliere **Salva tutti** dal menu **File**.  
  
7.  Nel menu **Compila** scegliere **Compila soluzione**.  
  
Nella prossima esercitazione verrà illustrato come aggiungere i riferimenti necessari al progetto.  
  
1.  In **Esplora soluzioni** selezionare il progetto **ColumnCountCondition**.  
  
2.  Scegliere **Aggiungi riferimento** dal menu **Progetto** per visualizzare la finestra di dialogo **Aggiungi riferimento**.  
  
3.  Fare clic sulla scheda **.NET**.  
  
4.  Nella colonna **Nome componente** individuare e selezionare il componente **System.ComponentModel.Composition**. Fare clic su **OK** dopo aver selezionato il componente.  
  
5.  Aggiungere i riferimenti di assembly necessari. Fare clic con il pulsante destro del mouse sul nodo del progetto e quindi scegliere **Aggiungi riferimento**. Fare clic su **Sfoglia** e passare alla cartella C:\Programmi (x86)\\MicrosoftSQL Server\110\DAC\Bin. Scegliere Microsoft.Data.Tools.Schema.Sql.dll e fare clic su Aggiungi, quindi fare clic su OK.  
  
6.  Scegliere **Scarica progetto** dal menu **Progetto**.  
  
7.  Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Modifica <project name>.csproj**.  
  
8.  Aggiungere le istruzioni Import seguenti dopo l'importazione di **Microsoft.CSharp.targets**:  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
9. Salvare il file e chiuderlo. Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Ricarica progetto**.  
  
    I riferimenti necessari verranno visualizzati nel nodo **Riferimenti** del progetto in **Esplora soluzioni**.  
  
## <a name="creating-the-resultsetcolumncountcondition-class"></a>Creazione della classe ResultSetColumnCountCondition  
A questo punto, **Class1** verrà ridenominata in **ResultSetColumnCountCondition** e derivata da [testcondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)). La classe **ResultSetColumnCountCondition** è una condizione di test semplice che verifica il numero delle colonne restituite in ResultSet. È possibile utilizzare questa condizione per essere certi che il contratto per una stored procedure sia corretto.  
  
1.  In **Esplora soluzioni** fare clic con il pulsante destro del mouse su Class1.cs, scegliere **Rinomina** e digitare **ResultSetColumnCountCondition.cs**.  
  
2.  Fare clic su **Sì** e confermare la volontà di rinominare tutti i riferimenti a Class1.  
  
3.  Aprire il file **ResultSetColumnCountCondition.cs** e aggiungere le istruzioni using seguenti al file:  
  
    ```  
    using System;  
    using System.ComponentModel;  
    using System.Data;  
    using System.Data.Common;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
    namespace ColumnCountCondition {  
        public class ResultSetColumnCountCondition  
    ```  
  
4.  Derivare la classe da [testcondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)):  
  
    ```  
    public class ResultSetColumnCountCondition : TestCondition  
    ```  
  
5.  Aggiungere [ExportTestConditionAttribute](/previous-versions/sql/sql-server-data-tools/jj856578(v=vs.103)). Vedere [Procedura: Creare condizioni di test per la finestra di progettazione unit test di SQL Server](../ssdt/how-to-create-test-conditions-for-the-sql-server-unit-test-designer.md) per altre informazioni su UnitTesting.Conditions.ExportTestConditionAttribute.  
  
    ```  
    [ExportTestCondition("ResultSet Column Count", typeof(ResultSetColumnCountCondition))]  
        public class ResultSetColumnCountCondition : TestCondition  
    ```  
  
6.  Creare le variabili membro e il costruttore:  
  
    ```  
            private int _resultSet;  
            private int _count;  
            private int _batch;  
  
            public ResultSetColumnCountCondition() {  
                _resultSet = 1;  
                _count = 0;  
                _batch = 1;  
            }  
    ```  
  
7.  Eseguire l'override del metodo **Assert**. Il metodo include gli argomenti per **IDbConnection**, che rappresentano la connessione al database, e **SqlExecutionResult**. Il metodo usa **DataSchemaException** per la gestione degli errori:  
  
    ```  
           //method you need to override  
            //to perform the condition verification  
            public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
            {  
                //call base for parameter validation  
                base.Assert(validationConnection, results);  
  
                //verify batch exists  
                if (results.Length < _batch)  
                    throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
                SqlExecutionResult result = results[_batch - 1];  
  
                //verify resultset exists  
                if (result.DataSet.Tables.Count < ResultSet)  
                    throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
                DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
                //actual condition verification  
                //verify resultset column count matches expected  
                if (table.Columns.Count != Count)  
                    throw new DataException(String.Format(  
                        "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                        ResultSet, table.Columns.Count, Count));  
            }  
  
    Add the following method, which overrides the ToString method:  
    C#  
            //this method is called to provide the string shown in the  
            //test conditions panel grid describing what the condition tests  
            public override string ToString()  
            {  
                return String.Format(  
                    "Condition fails if ResultSet {0} does not contain {1} columns",  
                    ResultSet, Count);  
            }  
    ```  
  
8.  Aggiungere le proprietà della condizione di test seguenti usando gli attributi **CategoryAttribute**, **DisplayNameAttribute** e **DescriptionAttribute**:  
  
    ```  
            //below are the test condition properties  
            //that are exposed to the user in the property browser  
            #region Properties  
  
            //property specifying the resultset for which  
            //you want to check the column count  
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
  
            //property specifying  
            //expected column count  
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
    ```  
  
Di seguito viene riportato il codice finale:  
  
```  
using System;  
using System.ComponentModel;  
using System.Data;  
using System.Data.Common;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
namespace ColumnCountCondition  
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
  
        //method you need to override  
        //to perform the condition verification  
        public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
        {  
            //call base for parameter validation  
            base.Assert(validationConnection, results);  
  
            //verify batch exists  
            if (results.Length < _batch)  
                throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
            SqlExecutionResult result = results[_batch - 1];  
  
            //verify resultset exists  
            if (result.DataSet.Tables.Count < ResultSet)  
                throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
            DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
            //actual condition verification  
            //verify resultset column count matches expected  
            if (table.Columns.Count != Count)  
                throw new DataException(String.Format(  
                    "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                    ResultSet, table.Columns.Count, Count));  
        }  
  
        //this method is called to provide the string shown in the  
        //test conditions panel grid describing what the condition tests  
        public override string ToString()  
        {  
            return String.Format(  
                "Condition fails if ResultSet {0} does not contain {1} columns",  
                ResultSet, Count);  
        }  
  
        //below are the test condition properties  
        //that are exposed to the user in the property browser  
        #region Properties  
  
        //property specifying the resultset for which  
        //you want to check the column count  
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
  
        //property specifying  
        //expected column count  
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
  
Nella prossima esercitazione verrà illustrato come compilare il progetto.  
  
## <a name="compiling-the-project-and-installing-your-test-condition"></a><a name="xxx"></a> Compilazione del progetto e installazione della condizione di test  
Nel menu **Compila** scegliere **Compila soluzione**.  
  
Nella prossima esercitazione verrà illustrato come copiare le informazioni sull'assembly nella directory Estensioni. Quando si avvia Visual Studio, le estensioni vengono identificate nella directory %Programmi%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions e relative sottodirectory, quindi vengono rese disponibili per l'utilizzo:  
  
Copiare il file di assembly **ColumnCountCondition.dll** dalla directory di output nella directory %Programmi%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions.  
  
Per impostazione predefinita, il percorso del file DLL compilato è *PercorsoSoluzione*\\*PercorsoProgetto*\bin\Debug o *PercorsoSoluzione*\\*PercorsoProgetto* bin\Release.  
  
Nella prossima esercitazione verrà illustrato come avviare una nuova sessione di Visual Studio e creare un progetto di database. Per avviare una nuova sessione di Visual Studio e creare un progetto di database:  
  
1.  Avviare una seconda sessione di Visual Studio.  
  
2.  Nel menu **File** fare clic su **Nuovo** e quindi su **Progetto**.  
  
3.  Nella finestra di dialogo **Nuovo progetto**, selezionare il nodo **SQL Server** nell'elenco dei modelli installati.  
  
4.  Nel riquadro dei dettagli fare clic su **Progetto di database SQL Server**.  
  
5.  Nella casella di testo **Nome** digitare **SampleConditionDB** e quindi fare clic su **OK**.  
  
Nella prossima esercitazione verrà illustrato come creare uno unit test. Per creare uno unit test di SQL Server all'interno di una nuova classe di test:  
  
1.  Scegliere **Nuovo test** dal menu **Test** per visualizzare la finestra di dialogo **Aggiungi nuovo test**.  
  
    È anche possibile aprire **Esplora soluzioni**, fare clic con il pulsante destro del mouse su un progetto di test, scegliere **Aggiungi** e quindi fare clic su **Nuovo test**.  
  
2.  Nell'elenco dei modelli fare clic su **Unit test di SQL Server**.  
  
3.  In **Nome test** digitare **SampleUnitTest**.  
  
4.  In **Aggiungi a progetto di test** fare clic su **Crea nuovo progetto di test Visual C\#** . Fare clic su **OK** per visualizzare la finestra di dialogo **Nuovo progetto di test**.  
  
5.  Digitare **SampleUnitTest** per il nome del progetto.  
  
6.  Fare clic su **Annulla** per creare lo unit test senza configurare il progetto di test per l'uso di una connessione del database. Il test vuoto viene visualizzato nella finestra di progettazione unit test di SQL Server. Viene aggiunto un file di codice sorgente Visual C\# al progetto di test.  
  
    Per altre informazioni sulla creazione e sulla configurazione degli unit test del database con connessioni del database, vedere [Procedura: Creare uno unit test di SQL Server vuoto](../ssdt/how-to-create-an-empty-sql-server-unit-test.md).  
  
7.  Fare clic su **Fare clic qui per creare** per completare la creazione dello unit test. Verrà visualizzata la nuova condizione di test nel progetto SQL Server.  
  
> [!NOTE]  
> Per usare la condizione di test personalizzata con i progetti di unit test esistenti, è necessario creare almeno una nuova classe di unit test di SQL Server. Il riferimento necessario all'assembly della condizione di test viene aggiunto al progetto di test durante la creazione della classe di test.  
  
Per visualizzare la nuova condizione di test:  
  
1.  Nella **Finestra di progettazione unit test di SQL Server** scegliere **Condizioni di test** e quindi fare clic sul test inconclusiveCondition1 nella colonna **Nome**.  
  
2.  Fare clic sul pulsante **Elimina condizione di test** sulla barra degli strumenti per rimuovere il test inconclusiveCondition1.  
  
3.  Fare clic sull'elenco a discesa **Condizioni di test** e selezionare **Totale colonne ResultSet**.  
  
4.  Fare clic sul pulsante **Aggiungi condizione di test** sulla barra degli strumenti per aggiungere la condizione di test personalizzata.  
  
5.  Nella finestra **Proprietà** configurare le proprietà Count, Enabled e ResultSet.  
  
    Per altre informazioni, vedere [Procedura: Aggiungere condizioni di test a unit test di SQL Server](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md).  
  
## <a name="see-also"></a>Vedere anche  
[Condizioni di test personalizzate per unit test di SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
