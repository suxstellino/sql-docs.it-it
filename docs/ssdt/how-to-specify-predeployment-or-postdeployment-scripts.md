---
title: Specificare script di pre-distribuzione o di post-distribuzione
description: Informazioni su come usare gli script pre-distribuzione e post-distribuzione per eseguire istruzioni Transact-SQL prima e dopo lo script di distribuzione principale.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 7f78f517-f13d-4f4b-84b9-e804cb490b2c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d4565e9b6ab4efc133aef84ff9d4d6dbbd6c501a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018111"
---
# <a name="how-to-specify-predeployment-or-postdeployment-scripts"></a>Procedura: Specificare script pre-distribuzione o post-distribuzione

Gli script pre-distribuzione e post-distribuzione consentono di eseguire le istruzioni Transact\-SQL prima e dopo lo script di distribuzione principale generato dal progetto di database. Lo script di pre-distribuzione non verrà eseguito quando si aggiornano le destinazioni dai risultati di confronto dello schema in Visual Studio. Un progetto può contenere un solo script pre-distribuzione e un solo script post-distribuzione. Tali script possono essere utilizzati per molti scopi. Ad esempio:  
  
-   Uno script pre-distribuzione può essere utilizzato per copiare i dati da una tabella da modificare in una tabella temporanea prima di riformattare e applicare i dati nella tabella modificata in uno script post-distribuzione  
  
-   È possibile inserire i dati di riferimento che devono essere presenti in una tabella in uno script post-distribuzione. Prima di inserire dati, è possibile verificare se sono già presenti nella tabella. Se la tabella non è vuota, è necessario cancellare i dati esistenti o specificare che si desidera sempre ricreare il database prima di distribuirlo. È possibile aggiungere un'istruzione come la seguente allo script post-distribuzione:  
  
```  
IF (EXISTS(SELECT * FROM [dbo].[MyReferenceTable]))  
BEGIN  
    DELETE FROM [dbo].[MyReferenceTable]  
END  
```  

## <a name="to-add-and-modify-a-pre--or-post-deployment-script"></a>Per aggiungere e modificare uno script pre-distribuzione o post-distribuzione  
  
1.  In **Esplora soluzioni** espandere il progetto di database per visualizzare la cartella Script.  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella Script, quindi scegliere Aggiungi.  
  
3.  Selezionare Script dal menu di scelta rapida.  
  
4.  Selezionare Script pre-distribuzione o Script post-distribuzione. Facoltativamente, specificare un nome non predefinito. Fare clic su Aggiungi per completare l'operazione.  
  
5.  Fare doppio clic sul file nella cartella Script.  
  
    Verrà aperto l'Editor Transact\-SQL in cui è visualizzato il contenuto del file.  
  
È possibile utilizzare la sintassi e le variabili SQLCMD negli script e impostare tali valori nelle proprietà del progetto di database. Ad esempio:  
  
-   È possibile utilizzare la sintassi SQLCMD per includere il contenuto di un file in uno script pre-distribuzione o post-distribuzione. I file vengono inclusi ed eseguiti nell'ordine in cui vengono definiti: `:r .\myfile.sql`  
  
-   È possibile utilizzare la sintassi SQLCMD per fare riferimento a una variabile nello script post-distribuzione. È possibile impostare la variabile SQLCMD nelle proprietà del progetto o in un profilo di pubblicazione.  
  
    ```  
    :setvar TableName MyTable  
    SELECT * FROM [$(TableName)]  
    ```  
  
Per altre informazioni sull'uso di SQLCMD negli script, vedere [Impostazioni del progetto di database](../ssdt/database-project-settings.md).  
  
## <a name="see-also"></a>Vedere anche  
[Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md)  
  
