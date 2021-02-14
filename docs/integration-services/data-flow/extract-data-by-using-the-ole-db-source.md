---
description: Estrazione dei dati tramite l'origine OLE DB
title: Estrarre i dati tramite l'origine OLE DB | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- extracting data [Integration Services]
- sources [Integration Services], OLE DB
- OLE DB source [Integration Services]
ms.assetid: 4ca6eeb5-b60e-4b81-86dd-0674be8ae8d8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 864463e65b5857c3d24710f286f17817d2f48a46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339770"
---
# <a name="extract-data-by-using-the-ole-db-source"></a>Estrazione dei dati tramite l'origine OLE DB

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  È possibile aggiungere e configurare un'origine OLE DB solo se il pacchetto include già almeno un'attività Flusso di dati.  
  
### <a name="to-extract-data-using-an-ole-db-source"></a>Per estrarre dati tramite un'origine OLE DB  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che contiene il pacchetto desiderato.  
  
2.  In Esplora soluzioni fare doppio clic sul pacchetto per aprirlo.  
  
3.  Fare clic sulla scheda **Flusso di dati** e quindi dalla **Casella degli strumenti** trascinare l'origine OLE DB sull'area di progettazione.  
  
4.  Fare doppio clic sull'origine OLE DB.  
  
5.  Nella pagina **Gestione connessione** della finestra di dialogo **Editor origine OLE DB** selezionare una gestione connessione OLE DB esistente oppure fare clic su **Nuova** per creare una nuova gestione connessione. Per altre informazioni, vedere [Gestione connessione OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
6.  Selezionare il metodo di accesso ai dati:  
  
    -   **Tabella o vista** Selezionare la tabella o vista del database a cui deve connettersi la gestione connessione OLE DB.  
  
    -   **Variabile nome vista o nome tabella** Selezionare la variabile definita dall'utente che contiene il nome della tabella o vista del database a cui deve connettersi la gestione connessione OLE DB.  
  
    -   **Comando SQL** Digitare un comando SQL oppure fare clic su **Compila query** per scrivere un comando SQL usando **Generatore query**.  
  
        > [!NOTE]  
        >  Il comando può includere parametri. Per altre informazioni, vedere [Mapping dei parametri di query a variabili in un componente di un flusso di dati](../../integration-services/data-flow/map-query-parameters-to-variables-in-a-data-flow-component.md).  
  
    -   **Comando SQL da variabile** Selezionare la variabile definita dall'utente che contiene il comando SQL.  
  
        > [!NOTE]  
        >  La variabile deve essere definita nell'ambito della stessa attività Flusso di dati che contiene l'origine OLE DB, oppure nell'ambito dello stesso pacchetto, e deve avere un tipo di dati string.  
  
7.  Per aggiornare il mapping tra colonne esterne e colonne di output, fare clic su **Colonne** e selezionare colonne diverse nell'elenco **Colonna esterna** .  
  
8.  Facoltativamente, aggiornare i nomi delle colonne di output modificando i valori nell'elenco **Colonna di output** .  
  
9. Per configurare l'output degli errori, fare clic su **Output errori**. Per altre informazioni, vedere [Debug di un flusso di dati](../../integration-services/troubleshooting/debugging-data-flow.md).  
  
10. Facendo clic su **Anteprima** è possibile visualizzare fino a 200 righe di dati estratti dall'origine OLE DB.  
  
11. Fare clic su **OK**.  
  
12. Per salvare il pacchetto aggiornato, scegliere **Salva elementi selezionati** dal menu **File** .  
  
## <a name="see-also"></a>Vedere anche  
 [Origine OLE DB](../../integration-services/data-flow/ole-db-source.md)   
 [Trasformazioni di Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Percorsi in Integration Services](../../integration-services/data-flow/integration-services-paths.md)   
 [Attività Flusso di dati](../../integration-services/control-flow/data-flow-task.md)  
  
  
