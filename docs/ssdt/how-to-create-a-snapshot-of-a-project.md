---
title: Creare uno snapshot di un progetto
description: Acquisire familiarità con i file dell'applicazione livello dati, o snapshot, e scoprire come usarli. Informazioni su come creare o importare snapshot e come confrontarli.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.SqlProjectImportSnapshotSummaryDialog.dialog
- sql.data.tools.SqlProjectImportSnapshotDialog.dialog
ms.assetid: bed670a3-13bd-4d88-91a1-58d5b9524a97
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0a3c615e98f16c5894924817fc2d4d5acf37b962
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018342"
---
# <a name="how-to-create-a-snapshot-of-a-project"></a>Procedura: Creare uno snapshot di un progetto

Un file di **applicazione livello dati** fornisce una rappresentazione di sola lettura dello schema del database al momento della creazione. È considerato essenzialmente uno schema del database da cui è possibile importare nuovamente gli oggetti dello schema in un progetto. Inoltre, è possibile confrontarlo con lo schema di un database o di un progetto e aggiornare il database o il progetto affinché rifletta lo schema definito nello snapshot.  
  
In caso di errore da parte di un utente in un progetto di database di origine, è possibile ripristinare lo stato in cui si trovava il progetto di origine al momento della creazione dello snapshot. Inoltre, è possibile creare snapshot in varie fasi dello sviluppo per una base di riferimento.  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nelle sezioni [Sviluppo del database connesso](../ssdt/connected-database-development.md) e [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-create-a-snapshot"></a>Per creare uno snapshot  
  
1.  Fare clic con il pulsante destro del mouse sul progetto **TradeDev** in **Esplora soluzioni** e selezionare **Applicazione livello dati (\*.dacpac)** .  
  
2.  In SSDT si tenterà innanzitutto di compilare il progetto. Se non si verificano errori di compilazione, in **Esplora soluzioni** viene creata una cartella **Snapshot**, all'interno della quale SSDT crea un file con estensione dacpac con il formato di nome "<Project Name>_AAAAMMGG_HH-MM-SS.dacpac".  
  
3.  Fare clic con il pulsante destro del mouse sul file con estensione dacpac e selezionare **Rinomina**. Modificare il nome file predefinito impostandolo su "TradeDev1.dacpac".  
  
4.  Fare clic con il pulsante destro del mouse sulla funzione **GetProductsBySupplier** in **Esplora soluzioni** e selezionare **Elimina** per rimuoverla dal progetto.  
  
5.  Seguire i passaggi precedenti per creare un nuovo snapshot denominato **TradeDev2.dacpac**.  
  
### <a name="to-import-a-snapshot"></a>Per importare uno snapshot  
  
1.  Fare clic con il pulsante destro del mouse sul progetto **TradeDev** in **Esplora soluzioni** e selezionare **Importa** e quindi **Applicazione livello dati (\*.dacpac)** dai menu di scelta rapida.  
  
2.  Nella finestra di dialogo **Importa applicazione di livello dati** fare clic su **Sfoglia** per selezionare **TradeDev1.dacpac** da usare come origine dell'importazione.  
  
    Si noti che la sezione **Progetto di destinazione** è stata disabilitata, dal momento che il progetto corrente è la destinazione predefinita. Fare clic su **Avvia** per avviare l'importazione.  
  
3.  Fare clic su **Fine** nella pagina **Riepilogo**. In **Esplora soluzioni** si noti che la tabella eliminata è stata ripristinata nel progetto.  
  
    > [!WARNING]  
    > Lo snapshot di importazione consentirà di importare tutte le entità del database dello schema dello snapshot nel progetto. Di conseguenza, in questo modo si potrebbero creare entità duplicate. In ognuna delle tabelle e viste, ad esempio, è ora contenuta una copia aggiuntiva della stessa denominata <NomeOggetto_1>. Fare clic con il pulsante destro del mouse su ognuno di questi oggetti duplicati in **Esplora soluzioni** e selezionare **Elimina** per rimuoverlo dal progetto.  
  
### <a name="to-compare-snapshots"></a>Per confrontare gli snapshot  
  
1.  Fare clic con il pulsante destro del mouse su **TradeDev1.dacpac** in Esplora soluzioni e selezionare **Confronto schema**. Verrà visualizzata la finestra **Confronto schema**.  
  
2.  Per impostare gli schemi di origine e di destinazione, usare le opzioni del **file di applicazione livello dati**. Assicurarsi che **Schema di origine** sia impostato su **TradeDev1.dacpac** nel file **Applicazione livello dati** e che **Schema di destinazione** sia impostato su **TradeDev2.dacpac**.  
  
3.  Fare clic su **OK** per avviare il confronto. Si noti che la funzione eliminata viene evidenziata come differenza tra lo snapshot precedente e quello nuovo.  
  
    È possibile trovare facilmente il delta di snapshot differenti utilizzando Confronto schema. In questo caso, è possibile ottenere informazioni sull'evoluzione del progetto durante il processo di sviluppo.  
  
## <a name="see-also"></a>Vedere anche  
[Procedura: Usare il confronto schema per confrontare definizioni di database diverse](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
