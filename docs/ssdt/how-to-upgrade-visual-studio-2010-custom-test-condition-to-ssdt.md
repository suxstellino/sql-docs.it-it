---
title: Aggiornare una condizione di test personalizzata di Visual Studio 2010 da una versione precedente
description: Informazioni su come eseguire l'aggiornamento di una condizione di test personalizzata di Visual Studio 2010 per l'uso in SQL Server Data Tools. Scoprire quali modifiche apportare e come installare la condizione.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 44c895a3-dee0-4032-a60f-812f5fe3c713
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2e0f9813db19e06729f7164f0ca419801364a365
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018051"
---
# <a name="how-to-upgrade-a-visual-studio-2010-custom-test-condition-from-a-previous-release-to-sql-server-data-tools"></a>Procedura: Aggiornare una condizione di test personalizzata di Visual Studio 2010 da una versione precedente a SQL Server Data Tools

Per usare una condizione di test creata in una versione precedente a SQL Server Data Tools, è necessario aggiornarla:  
  
-   [Aggiornare i riferimenti](#UpdateReferences)  
  
-   [Aggiornare gli attributi della classe e i riferimenti di tipo](#UpdateClassAttributesandTypeReference)  
  
-   [Installare la condizione di test aggiornata](#ApplytheNewRegistrationProcess)  
  
## <a name="update-references"></a><a name="UpdateReferences"></a>Aggiornare i riferimenti  
Per aggiornare i riferimenti del progetto:  
  
1.  Solo per Visual Basic, in **Esplora soluzioni** scegliere **Mostra tutti i file**.  
  
2.  Espandere il nodo **Riferimenti** in **Esplora soluzioni**.  
  
3.  Fare clic con il pulsante destro del mouse sui seguenti riferimenti di assembly e fare clic su **Rimuovi**:  
  
    1.  Microsoft.Data.Schema.UnitTesting  
  
    2.  Microsoft.Data.Schema  
  
4.  Nel menu **Progetto**, o facendo clic con il pulsante destro del mouse sulla cartella del progetto in **Esplora soluzioni**, scegliere **Aggiungi riferimento**.  
  
5.  Fare clic sulla scheda **.NET**.  
  
6.  Nell'elenco **Nome componente** selezionare **System.ComponentModel.Composition** e fare clic su **OK**.  
  
7.  Aggiungere i riferimenti di assembly necessari. Fare clic con il pulsante destro del mouse sul nodo del progetto e quindi scegliere **Aggiungi riferimento**. Fare clic su **Sfoglia** e passare alla cartella C:\Programmi (x86)\\MicrosoftSQL Server\110\DAC\Bin. Scegliere Microsoft.Data.Tools.Schema.Sql.dll e fare clic su Aggiungi, quindi fare clic su OK.  
  
8.  Scegliere **Scarica progetto** dal menu **Progetto**.  
  
9. Fare clic con il pulsante destro del mouse sul **progetto** in **Esplora soluzioni** e scegliere **Modifica**`project_name` **.csproj**.  
  
10. Aggiungere la seguente istruzione Import dopo l'importazione di `Microsoft.CSharp.targets`:  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
11. Salvare il file e chiuderlo. Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Ricarica progetto**.  
  
12. Aprire la classe della condizione di test e rimuovere tutte le istruzioni Using che iniziano con **Microsoft.Data.Schema**. Il modo più semplice consiste nel fare clic con il pulsante destro del mouse sul file e scegliere **Organizza using** e **Rimuovi e ordina**. Le istruzioni using seguenti devono essere rimosse:  
  
    ```  
    using Microsoft.Data.Schema.UnitTesting;  
    using Microsoft.Data.Schema.UnitTesting.Conditions;  
    using Microsoft.Data.Schema.Extensibility;  
    using Microsoft.Data.Schema;  
    ```  
  
13. Aggiungere al file le istruzioni using riportate di seguito, se non sono presenti:  
  
    ```  
    using System.ComponentModel;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
    ```  
  
La condizione di test a questo punto usa i riferimenti dell'assembly di unit test di SQL Server.  
  
## <a name="update-class-attributes-and-type-references"></a><a name="UpdateClassAttributesandTypeReference"></a>Aggiornare gli attributi della classe e i riferimenti di tipo  
Sostituire gli attributi precedenti della classe di unit test con nuovi attributi. L'estendibilità degli unit test di SQL Server ora si basa su Managed Extensibility Framework (MEF). È necessario aggiornare alcuni i riferimenti di tipo.  
  
### <a name="update-class-attributes"></a>Aggiornare gli attributi della classe  
Aggiornare il codice come segue:  
  
1.  Rimuovere gli attributi `DatabaseSchemaProviderCompatibility`. Tali attributi erano richiesti della funzione Extensibility della versione precedente e non sono supportati negli unit test di SQL Server.  
  
2.  Rimuovere l'attributo `DisplayName`. Il nome visualizzato è incluso nel nuovo attributo.  
  
3.  Aggiungere il nuovo attributo `ExportTestCondition`. Tale attributo deve essere presente per individuare la condizione di test e usarla negli unit test di SQL Server. `ExportTestCondition` sostituisce l'attributo `DatabaseSchemaProviderCompatibility`. `ExportTestCondition` accetta due parametri:  
  
    -   `DisplayName`è il primo parametro. In tal modo l'attributo `DisplayName` viene sostituito e utilizzato per descrivere tutte le condizioni di test di questo tipo.  
  
    -   Il secondo parametro viene utilizzato per identificare in modo univoco l'estensione. È possibile passare il tipo utilizzando `typeof(NewTestCondition)`, poiché dovrebbe essere univoco. È possibile tuttavia passare anche l'ID stringa, se preferito.  
  
4.  La definizione di classe deve essere modificata come segue:  
  
    Prima di:  
  
    ```  
    [DatabaseSchemaProviderCompatibility(typeof(DatabaseSchemaProvider))]  
    [DatabaseSchemaProviderCompatibility(null)]  
    [DisplayName("NewTestCondition")]  
    public class NewTestCondition:TestCondition  
    {  
       // Additional implementation to be added here  
    }  
    ```  
  
    Dopo:  
  
    ```  
    [ExportTestCondition("NewTestCondition ", typeof(NewTestCondition))]  
    public class NewTestCondition:TestCondition  
    {  
       // Additional implementation to be added here  
    }  
    ```  
  
### <a name="update-type-references"></a>Aggiornare i riferimenti di tipo  
Alcuni nomi di tipo sono cambiati nel framework per unit test di SQL Server. Per aggiornare il codice in modo da usare i nuovi nomi di tipo, usare l'opzione **Trova e sostituisci** nel menu **Modifica**. I nomi di tipo ora iniziano con **Sql**. I nomi di classe devono essere aggiornati come segue:  
  
|Nome di tipo precedente|Nome di tipo nuovo|  
|-----------------|-----------------|  
|`ExecutionResult`|`SqlExecutionResult`|  
  
## <a name="install-the-upgraded-test-condition"></a><a name="ApplytheNewRegistrationProcess"></a>Installare la condizione di test aggiornata  
Nelle versioni precedenti degli unit test di database, è possibile che veniva richiesto di installare la condizione di test nella Global Assembly Cache o creare un file XML contenente le informazioni sull'assembly. Con gli unit test di SQL Server, questo processo aggiuntivo non è più necessario. Per altre informazioni, vedere [Compilazione del progetto e installazione della condizione di test](../ssdt/walkthrough-use-custom-test-condition-to-verify-stored-procedure-results.md#xxx).  
  
Una volta aggiornati i riferimenti, verificare che l'assembly sia firmato e compilato.  
  
Copiare il file di assembly dalla directory di output, per impostazione predefinita Documenti\Visual Studio 2010\Projects\\<yoursolutionname>\\<yourprojectname>\bin\Debug\\, nella directory %Programmi%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions. Quando si avvia Visual Studio, le estensioni vengono identificate nella directory TestConditions e rese disponibili per l'utilizzo nella sessione:  
  
## <a name="upgrade-existing-tests-that-need-to-use-the-new-test-condition"></a>Aggiornare i testi esistenti che devono utilizzare la nuova condizione di test  
Individuare tutti i progetti di test che utilizzano la vecchia condizione di test e che devono utilizzare la nuova condizione. Accertarsi che tali progetti di test vengano aggiornati. Per altre informazioni, vedere [Aggiornare un progetto di test precedente contenente unit test del database](../ssdt/upgrade-an-older-test-project-containing-database-unit-tests.md).  
  
Rimuovere il riferimento dell'assembly alla vecchia condizione di test.  
  
Aggiungere un nuovo unit test di SQL Server al progetto per creare un riferimento dell'assembly alla condizione di test aggiornata nel progetto. Per creare il riferimento è necessario aggiungere una classe di test. È possibile eliminare la classe di test una volta aggiunto il riferimento.  
  
## <a name="see-also"></a>Vedere anche  
[Condizioni di test personalizzate per unit test di SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
  
