---
title: Correggere gli errori
description: Acquisire familiarità con il riquadro Elenco errori e i tipi di errori visualizzati. Scoprire come ordinare e filtrare le informazioni nell'elenco errori e identificare le origini degli errori.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 0d504e00-4ff0-4fdf-b874-85280bbd8668
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: caca368f1556e7b9a875525e4c2d84a0c1cabc9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018191"
---
# <a name="how-to-fix-errors"></a>Procedura: Correggere gli errori

Nel riquadro Elenco errori vengono visualizzati tutti gli errori di distribuzione e di compilazione. Vengono inoltre visualizzati gli errori di sintassi e semantici causati dalle modifiche nell'Editor Transact\-SQL o in Progettazione tabelle quando si modificano entità del database e relative definizioni. L'Elenco errori viene aggiornato dinamicamente quando si modificano gli script nelle diverse schede. È quindi possibile seguire gli errori identificati per un'ulteriore risoluzione dei problemi.  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure nelle sezioni [Sviluppo del database connesso](../ssdt/connected-database-development.md) e [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-fix-errors"></a>Per correggere gli errori  
  
1.  Fare clic con il pulsante destro del mouse sulla tabella **Product** (Product.sql) in **Esplora soluzioni** e selezionare **Progettazione viste**.  
  
2.  Nella Griglia colonne della finestra di progettazione fare clic con il pulsante destro del mouse sulla colonna **ShelflLife** e selezionare **Elimina** per eliminare questa colonna dalla tabella.  
  
3.  Si noti che nel riquadro **Elenco errori**, nella parte inferiore della schermata, vengono visualizzati immediatamente un avviso e un errore simili ai seguenti.  
  
**Avviso SQL71502: Funzione: [dbo].[GetProductsBySupplier] contiene un riferimento non risolto a un oggetto. L'oggetto non esiste oppure il riferimento è ambiguo poiché potrebbe fare riferimento a uno dei seguenti oggetti: [dbo].[Product].[p]::[ShelfLife] o [dbo].[Product].[ShelfLife].Errore SQL71501: Vincolo CHECK: [dbo].[CK_Product_ShelfLife] contiene un riferimento non risolto all'oggetto [dbo].[Product].[ShelfLife].**  
  
4.  È possibile fare clic con il pulsante destro del mouse sull'**Elenco errori** e usare i menu contestuali per ordinare i risultati, filtrare le voci da visualizzare e le colonne di informazioni da mostrare per ogni voce.  
  
    Fare doppio clic sul primo avviso identificato e seguirlo fino al file di script da cui è stato generato. La sezione di codice con errori è evidenziata. Nell'esempio, questo problema si verifica perché la colonna `ShelfLife` è usata sia dall'istruzione `RETURN` sia dall'istruzione `SELECT` in una funzione con valori di tabella creata in precedenza.  
  
5.  Nell'Editor Transact\-SQL rimuovere `ShelfLife` dalla funzione.  
  
6.  Correggere il secondo errore in modo simile rimuovendo il vincolo CHECK.  
  
7.  Si noti che l'avviso e l'errore scompaiono dall'**Elenco errori** non appena vengono risolti i problemi.  
  
## <a name="see-also"></a>Vedere anche  
[Usare l'Editor Transact-SQL per modificare ed eseguire script](../ssdt/use-transact-sql-editor-to-edit-and-execute-scripts.md)  
  
