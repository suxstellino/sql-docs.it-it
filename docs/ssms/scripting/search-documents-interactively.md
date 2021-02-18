---
title: Ricerca interattiva all'interno di documenti
description: Informazioni su come usare la finestra di dialogo Trova e sostituisci per cercare uno o più file o finestre aperti, sospendendo l'operazione dopo l'individuazione di ogni corrispondenza per esaminare gli elementi trovati nel contesto. È anche possibile eseguire un'operazione di ricerca bulk ed esaminare le corrispondenze trovate in formato report.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- interactive searches [SQL Server Management Studio]
- searches [SQL Server Management Studio], interactive
- Query Editor [SQL Server Management Studio], interactive search
ms.assetid: dae65ac5-67af-45c6-a6e0-952fea26d680
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e02625ead97ff1ee1e1e7924d2a97a4d5a2d331a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354522"
---
# <a name="search-documents-interactively"></a>Ricerca interattiva all'interno di documenti
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La finestra di dialogo **Trova e sostituisci** consente di eseguire una ricerca in uno o più file o finestre aperti e passare da un risultato della ricerca all'altro. Questa tecnica consente di esaminare ogni singola corrispondenza della ricerca contestualmente, ossia nel testo che la precede e la segue. È inoltre possibile eseguire operazioni di ricerca bulk ed esaminare i risultati corrispondenti in formato report usando la finestra di dialogo **Trova e sostituisci** .  
  
### <a name="to-search-all-open-documents"></a>Per eseguire la ricerca in tutti i documenti aperti  
  
1.  Aprire gli elementi nei quali si desidera eseguire la ricerca.  
  
2.  Scegliere **Trova e sostituisci** dal menu **Modifica** e quindi fare clic su **Ricerca veloce**.  
  
3.  Nella casella di testo **Trova e sostituisci** immettere il testo che si desidera cercare.  
  
4.  Nell'elenco **Cerca in** selezionare **Tutti i documenti aperti**.  
  
    > [!NOTE]  
    >  Quando si seleziona **Tutti i documenti aperti** , è possibile che la ricerca non venga eseguita in alcuni file aperti. Nella ricerca vengono inclusi solo i file attualmente aperti in una visualizzazione di testo, ad esempio in visualizzazione Codice. I file in visualizzazione di progettazione non vengono inclusi nella ricerca.  
  
5.  Per migliorare la precisione della ricerca, selezionare opzioni di ricerca aggiuntive.  
  
6.  Fare clic su **Trova successivo** per avviare la ricerca e continuare a fare clic su **Trova successivo** fino al termine della ricerca nell'ultimo file.  
  
 Quando la ricerca raggiunge l'inizio o la fine del documento, nella barra di stato viene visualizzato un messaggio. Quando viene raggiunto il punto iniziale della ricerca, viene visualizzata una finestra di messaggio.  
  
#### <a name="to-replace-in-all-active-files-interactively"></a>Per eseguire una sostituzione in tutti i file attivi in modo interattivo  
  
1.  Scegliere **Trova e sostituisci** dal menu **Modifica** e quindi fare clic su **Sostituzione veloce**.  
  
2.  Nella casella di testo **Trova** immettere il testo da cercare.  
  
3.  Nella casella di testo **Sostituisci con** immettere il testo con cui sostituire il testo trovato.  
  
4.  Nell'elenco **Cerca in** selezionare **Tutti i documenti aperti**.  
  
5.  Fare clic su **Sostituisci** e continuare a fare clic su **Sostituisci** fino all'ultima sostituzione nell'ultimo file. Fare clic su **Trova successivo** per ignorare una corrispondenza che non si desidera sostituire.  
  
     -oppure-  
  
     Scegliere **Sostituisci tutto** per sostituire tutte le corrispondenze. Verrà visualizzata una finestra di messaggio con il numero totale di sostituzioni.  
  
 Il comando **Sostituisci tutto** esegue la sostituzione di tutte le corrispondenze, comprese quelle che sono state ignorate usando il pulsante **Trova successivo** . Per annullare il comando **Sostituisci tutto**, scegliere **Annulla** dal menu **Modifica** prima di chiudere un file.  
  
## <a name="see-also"></a>Vedere anche  
 [Ricerca incrementale in un documento attivo](./search-an-active-document-incrementally.md)   
 [Ricerca e sostituzione](./search-and-replace.md)   
 [Ricerca nei documenti utilizzando gli elenchi dei risultati](./search-documents-using-results-lists.md)   
 [Testo di ricerca con caratteri jolly](./search-text-with-wildcards.md)   
 [Eseguire ricerche di testo con espressioni regolari](./search-text-with-regular-expressions.md)  
  
