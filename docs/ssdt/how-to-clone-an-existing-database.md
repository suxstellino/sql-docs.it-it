---
title: Clonare un database esistente
description: Informazioni su come clonare un database. Scoprire le procedure per la creazione di un nuovo database, la duplicazione dello schema e la replica dei dati.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: aad3594a-11cf-4e68-a622-071a93d43875
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 9dd341612f075c3a92cd01b5cfca1133910f2c57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018451"
---
# <a name="how-to-clone-an-existing-database"></a>Procedura: Clonare un database esistente

In questa attività vengono utilizzati alcuni dei passaggi di procedure precedenti per creare un nuovo database in cui trasferire i dati esistenti. Vengono inoltre usati i passaggi illustrati in [Procedura: Usare il confronto schema per confrontare definizioni di database diverse](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md) per sincronizzare lo schema di un database di origine e di un database del progetto.  
  
Se si utilizzano questi passaggi, è possibile creare facilmente un database di sviluppo o di test da un database di produzione con schema e dati identici. Inoltre, è possibile continuare a sviluppare il database di test in una modalità connessa o creare un progetto di database per lo sviluppo e il test offline, senza interrompere le operazioni del database di produzione.  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nella sezione [Sviluppo del database connesso](../ssdt/connected-database-development.md).  
  
### <a name="to-create-a-development-database"></a>Per creare un database di sviluppo  
  
1.  Nel nodo **SQL Server** in **Esplora oggetti di SQL Server** espandere l'istanza del server connessa.  
  
2.  Fare clic con il pulsante destro del mouse sul nodo **Database** e selezionare **Aggiungi nuovo database**.  
  
3.  Rinominare il nuovo database in **TradeDev**.  
  
4.  Fare clic con il pulsante destro del mouse sul database **Trade** in **Esplora oggetti di SQL Server** e selezionare **Confronto schema**. Seguire i passaggi nell'argomento [Procedura: Usare il confronto schema per confrontare definizioni di database diverse](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md), scegliendo il database **Trade** originale come origine e il nuovo database **TradeDev** come destinazione. In questo modo, **TradeDev** verrà aggiornato con lo schema da **Trade**.  
  
### <a name="to-replicate-data"></a>Per replicare i dati  
  
1.  Nel passaggio precedente è stato duplicato solo lo schema del database di produzione nel database di sviluppo. In questa procedura, i dati di produzione verranno duplicati nel database di sviluppo.  
  
    Fare clic con il pulsante destro del mouse sulla tabella **Suppliers** nel database **Trade** e selezionare **Visualizza dati**. Verrà aperto l'Editor dati.  
  
2.  Fare clic sul pulsante **Script** accanto a **Numero massimo righe** nella barra degli strumenti.  
  
3.  Quando viene aperta la finestra di script, assicurarsi che nella barra di stato sotto il riquadro di script Transact\-SQL sia visualizzato Connesso. Se viene visualizzato Disconnesso, fare clic sul pulsante **Connetti** (all'estrema sinistra nella barra degli strumenti) e immettere le informazioni sul server e le credenziali.  
  
4.  Nel menu a discesa **Database** accanto ai pulsanti **Connetti**/**Disconnetti** selezionare **TradeDev**. Il risultato è simile all'istruzione Transact\-SQL`USE` e garantirà che lo script nell'editor del codice verrà eseguito nel database **TradeDev**.  
  
5.  Fare clic sul pulsante **Esegui query** per eseguire le istruzioni `INSERT`. In questo modo verranno inserite tutte le righe dalla tabella `Suppliers` del database `Trade` nella tabella `Suppliers` del database `TradeDev`.  
  
6.  Ripetere i passaggi sopra elencati per tutte le tabelle nel database `Trade`, in modo che vengano replicate nel database `TradeDev`.  
  
7.  Utilizzare l'Editor dati per verificare che tutte le tabelle nel nuovo database `TradeDev` siano state popolate.  
  
## <a name="see-also"></a>Vedere anche  
[Procedura: Usare il confronto schema per confrontare definizioni di database diverse](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
