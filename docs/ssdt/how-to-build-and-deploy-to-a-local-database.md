---
title: Compilare e distribuire in un database locale
description: Informazioni sull'istanza del server locale fornita da SQL Server 2012. Scoprire come usare questa istanza per la compilazione, il test e il debug di progetti di sviluppo.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ebca8ff8-9a09-4207-8979-9d577af7c1d5
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 598c75f5ef502d1679bd8ed7f17cb44c0f536abc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063536"
---
# <a name="how-to-build-and-deploy-to-a-local-database"></a>Procedura: Compilare e distribuire in un database locale

In Microsoft SQL Server 2012 è disponibile un'istanza del server su richiesta locale, denominata Runtime del database locale di SQL Server Express, che viene attivata quando si esegue il debug di un progetto di database di SQL Server. Questa istanza del server locale può essere utilizzata come sandbox per la compilazione, il test e il debug del progetto. È indipendente da tutte le istanze di SQL Server installate e non è accessibile al di fuori di SQL Server Data Tools (SSDT). Tale soluzione è ideale per gli sviluppatori che dispongono di accesso limitato ai database di produzione, o a cui non possono accedere affatto, ma che desiderano eseguire il test dei progetti in locale prima che vengano distribuiti nella produzione da persone autorizzate. Inoltre, quando si sviluppa una soluzione database per SQL Azure, è possibile avvalersi dei vantaggi forniti da questo server locale per sviluppare ed eseguire il test del progetto di database in locale, prima di distribuirlo nel cloud.  
  
> [!WARNING]  
> Un database nel nodo del database locale in Esplora oggetti di SQL Server rispecchia il progetto di database corrispondente e non è correlato al database avente lo stesso nome in un'istanza del server connessa.  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nelle sezioni [Sviluppo del database connesso](../ssdt/connected-database-development.md) e [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-use-the-local-database"></a>Per utilizzare il database locale  
  
1.  Si noti che nel nodo **SQL Server** in **Esplora oggetti di SQL Server** viene visualizzato un nuovo nodo denominato **Locale**. Si tratta dell'istanza del database locale.  
  
2.  Espandere i nodi **Locale** e **Database**. Si noti l'aspetto di un database con lo stesso nome del progetto TradeDev. Espandere i nodi in questo database. Nella finestra **Operazioni degli strumenti dati** viene visualizzato lo stato delle operazioni di espansione/importazione in corso su qualsiasi database nel nodo **Locale**. Si noti che non contengono alcuna tabella o entità create nelle procedure precedenti.  
  
3.  Premere F5 per eseguire il debug del progetto di database TradeDev.  
  
    Per impostazione predefinita, in SSDT verrà utilizzata l'istanza del server di database locale per l'esecuzione del debug di progetti di database. In questo caso, in SSDT si tenterà innanzitutto di compilare il progetto che, se non presenta errori, verrà distribuito insieme alle relative entità nel database locale. Se si esegue il debug dello stesso progetto in un secondo momento, tramite SSDT verranno rilevate tutte le modifiche dall'ultima sessione di debug e solo queste verranno distribuite nel database locale.  
  
4.  Espandere di nuovo i nodi in **TradeDev** nel server di database **Locale**. Questa volta, si noti che le tabelle, le viste e le funzioni sono state distribuite nel server di database locale.  
  
5.  Fare clic con il pulsante destro del mouse sul nodo **TradeDev** e selezionare **Nuova query**.  
  
6.  Nel riquadro di script incollare questo codice e fare clic sul pulsante **Esegui query** per eseguire la query.  
  
    ```  
    select * from dbo.GetProductsBySupplier(1)  
    ```  
  
7.  Nel riquadro **Messaggio** viene mostrato (0 righe interessate), mentre nel riquadro dei **risultati** non viene restituita alcuna riga. Questa situazione si verifica perché si esegue una query sul database locale anziché sul database connesso in cui sono contenuti effettivamente i dati real.  
  
    È possibile confermare questa condizione facendo clic sul pulsante destro del mouse sulla tabella **Products** in questo database **TradeDev** locale e selezionando **Visualizza dati**. Si noti che la tabella è vuota.  
  
### <a name="to-replicate-real-data-to-the-local-database"></a>Per replicare i dati real nel database locale  
  
1.  In **Esplora oggetti di SQL Server** espandere l'istanza di SQL Server connessa e individuare il database **TradeDev**.  
  
    Fare clic con il pulsante destro del mouse sulla tabella **Suppliers** e selezionare **Visualizza dati**.  
  
2.  Fare clic sul pulsante **Script** (il secondo da destra) nella parte superiore dell'editor dei dati. Copiare le istruzioni `INSERT` dallo script.  
  
3.  Espandere l'istanza del server **Locale**, fare clic con il pulsante destro del mouse sul nodo **TradeDev** e selezionare **Nuova query**.  
  
4.  Incollare le istruzioni `INSERT` in questa finestra Query ed eseguire la query.  
  
5.  Ripetere i passaggi elencati in precedenza per replicare i dati dalle tabelle **Products** e **Fruits** del database **TradeDev** connesso nel database **TradeDev** locale.  
  
6.  Fare clic con il pulsante destro del mouse sull'istanza del server **Locale** e selezionare **Aggiorna**. Esaminare le tabelle usando **Visualizza dati** per verificare che il database locale sia stato popolato.  
  
7.  Fare clic con il pulsante destro del mouse sul nodo **TradeDev** dell'istanza del server Locale e selezionare **Nuova query**.  
  
8.  Nel riquadro di script incollare questo codice e fare clic sul pulsante **Esegui query** per eseguire la query.  
  
    ```  
    select * from dbo.GetProductsBySupplier(1)  
    ```  
  
9. Nel riquadro dei **risultati** sotto il riquadro Editor Transact\-SQL si noterà che sono state restituite le righe Apples e Potato Chips della tabella `Products`.  
  
