---
description: 'Lezione 4-3: Aggiungere il reindirizzamento del flusso degli errori'
title: 'Passaggio 3: Aggiungere il reindirizzamento del flusso degli errori | Microsoft Docs'
ms.custom: ''
ms.date: 01/07/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5683a45d-9e73-4cd5-83ca-fae8b26b488c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ac63d922423bcbb9c0c7f513d34a5834b3f0c1b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352027"
---
# <a name="lesson-4-3-add-error-flow-redirection"></a>Lezione 4-3: Aggiungere il reindirizzamento del flusso degli errori

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Nell'attività precedente, la trasformazione Lookup Currency Key non crea una corrispondenza quando tenta di elaborare il file flat di esempio danneggiato, che genera un errore. Dato che la trasformazione utilizza le impostazioni predefinite per l'output degli errori, qualsiasi errore determina l'esito negativo della trasformazione. Quando la trasformazione viene interrotta, si interrompe anche il resto del pacchetto.  
  
Anziché consentire la mancata riuscita della trasformazione, è possibile configurare il componente in modo da reindirizzare la riga con esito negativo a un altro percorso di elaborazione, usando l'output degli errori. L'utilizzo di un percorso di elaborazione degli errori separato offre più opzioni. Ad esempio, è possibile ripulire i dati e successivamente rielaborare la riga con esito negativo. È inoltre possibile salvare la riga con esito negativo insieme alle informazioni sugli errori in modo da consentire una verifica e una rielaborazione successive.  
  
In questa attività si configura la trasformazione Lookup Currency Key in modo che le righe con esito negativo vengano reindirizzate all'output degli errori. Nel ramo del flusso di dati relativo agli errori, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] scrive queste righe in un file.  
  
Per impostazione predefinita, le due colonne supplementari in un output degli errori [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], ovvero **ErrorCode** e **ErrorColumn**, contengono solo un codice numerico e l'ID della colonna in cui si è verificato l'errore. In questa attività, prima che il pacchetto scriva le righe con esito negativo nel file, viene usato un componente script per accedere all'API di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] e ottenere una descrizione dell'errore.  
  
## <a name="configure-an-error-output"></a>Configurare un output degli errori  
  
1.  Nella **Casella degli strumenti SSIS** espandere **Comune** e trascinare **Componente script** sull'area di progettazione della scheda **Flusso di dati** . Posizionare **Script** a destra della trasformazione **Lookup Currency Key** .  
  
2.  Nella finestra di dialogo **Seleziona tipo componente script**, selezionare **Trasformazione**, quindi selezionare **OK**.  
  
3.  Per collegare i due componenti, selezionare la trasformazione **Lookup Currency Key** e quindi trascinare la freccia rossa sulla nuova trasformazione **Script**.  
  
    La freccia rossa rappresenta l'output degli errori della trasformazione **Lookup Currency Key** . Usando la freccia rossa per collegare la trasformazione al componente script, reindirizzare qualsiasi errore di elaborazione al componente script, che elabora gli errori e li invia alla destinazione.  
  
4.  Nella colonna **Errore** della finestra di dialogo **Configura output errori** selezionare **Reindirizza riga** e quindi selezionare **OK**.  
  
5.  Nell'area di progettazione **Flusso di dati**, selezionare il nome **Componente script** nel nuovo **Componente script** e cambiarlo in **Get Error Description**.  
  
6.  Fare doppio clic sulla trasformazione **Get Error Description** .  
  
7.  Nella pagina **Colonne di input** della finestra di dialogo **Editor trasformazione Script** selezionare la colonna **ErrorCode** .  
  
8.  Nella pagina **Input e output** espandere **Output 0**, selezionare **Colonne di output**, quindi selezionare **Aggiungi colonna**.  
  
9. Nella proprietà **Name**, immettere *ErrorDescription* e impostare la proprietà **DataType** su **Stringa Unicode [DT_WSTR]**.  
  
10. Nella pagina **Script** verificare che la proprietà **LocaleID** sia impostata su **Inglese (Stati Uniti).**
  
11. Selezionare **Modifica script** per aprire [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] Tools for Applications (VSTA). Nel metodo **Input0_ProcessInputRow**, immettere o incollare il codice seguente:  
  
    [Visual Basic]  
  
    ```vb  
    Row.ErrorDescription =   
      Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
    ```  
  
    [Visual C#]  
  
    ```cs
    Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
    ```  
  
    La subroutine completa risulta analoga al codice seguente:  
  
    [Visual Basic]  
  
    ```vb
    Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
      Row.ErrorDescription =   
        Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
  
    End Sub  
    ```  
  
    [Visual C#]  
  
    ```cs
    public override void Input0_ProcessInputRow(Input0Buffer Row)  
        {  
  
            Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
  
        }  
    ```  
  
12. Nel menu **Compila**, selezionare **Compila soluzione** per compilare lo script e salvare le modifiche, quindi chiudere VSTA.  
  
13. Selezionare **OK** per chiudere la finestra di dialogo **Editor trasformazione Script**.  
  
## <a name="go-to-next-task"></a>Esecuzione del passaggio successivo
[Passaggio 4: Aggiungere una destinazione file flat](../integration-services/lesson-4-4-adding-a-flat-file-destination.md)  
  
  
  
