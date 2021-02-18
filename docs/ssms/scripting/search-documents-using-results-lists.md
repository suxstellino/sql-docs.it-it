---
title: Ricerca nei documenti utilizzando gli elenchi dei risultati
description: Informazioni su come cercare e sostituire il testo in tutti i file contenuti in un progetto, in una soluzione o in una cartella del file system. Le corrispondenze vengono visualizzate nella finestra Risultati ricerca 1 ed è possibile fare doppio clic su una corrispondenza per visualizzare la riga che le contiene.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- searches [SQL Server Management Studio], result lists
- result list searches [SQL Server Management Studio]
- searches [SQL Server Management Studio], multiple files
- Query Editor [SQL Server Management Studio], search multiple files
ms.assetid: 275e1b6c-fbd0-4408-af77-35903f90657c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 79bd3ff3e7998c92e1c3f6474480cb36c352a98a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354580"
---
# <a name="search-documents-using-results-lists"></a>Ricerca nei documenti utilizzando gli elenchi dei risultati
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
   Nella finestra di dialogo **Trova e sostituisci** è possibile eseguire ricerche e sostituzioni di testo in tutti i file di un progetto o di una soluzione o in una cartella di file system, anche se i file non sono aperti in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Le corrispondenze delle ricerche eseguite con la finestra di dialogo **Trova e sostituisci** vengono visualizzate nelle finestre Risultati ricerca 1 e Risultati ricerca 2, in cui è possibile vedere il testo esatto dalla riga contenente la corrispondenza.  
  
### <a name="to-search-in-multiple-files"></a>Per eseguire la ricerca in più file  
  
1.  Scegliere **Trova e sostituisci** dal menu **Modifica** e fare clic su **Cerca nei file**.  
  
2.  Nella casella di testo **Trova** immettere il testo da cercare.  
  
3.  Nell'elenco **Cerca in** fare clic su **Tutti i documenti aperti**, **Progetto corrente**, **Intera soluzione** o digitare un percorso di directory.  
  
4.  Nell'elenco **Tipi di file** selezionare uno dei set di estensioni di file elencati o immettere le estensioni relative ai tipi di file su cui eseguire la ricerca separandole da un punto e virgola. Usare \*.\* per eseguire la ricerca in tutti i file della directory indicata nell'elenco a discesa **Cerca in** .  
  
5.  Selezionare altre opzioni di ricerca per definire ulteriormente la precisione della ricerca.  
  
6.  Fare clic su **Trova** per iniziare la ricerca.  
  
 Per impostazione predefinita, le corrispondenze della ricerca vengono riportate nella finestra Risultati ricerca 1. È possibile esplorare le corrispondenze della ricerca facendo doppio clic su ogni voce della finestra Risultati ricerca 1. Per visualizzare i risultati della ricerca in una nuova finestra, selezionare **Visualizza in Risultati ricerca 2**.  
  
#### <a name="to-replace-across-multiple-files-or-folders"></a>Per eseguire sostituzioni in più file o cartelle  
  
1.  Scegliere **Trova e sostituisci** dal menu **Modifica** e fare clic su **Sostituisci nei file**.  
  
2.  Nella casella di testo **Trova** immettere il testo da cercare.  
  
3.  Nella casella di testo **Sostituisci con** immettere il testo con cui sostituire il testo trovato.  
  
4.  Nell'elenco **Cerca in** fare clic su **Tutti i documenti aperti**, **Progetto corrente**, **Intera soluzione** o digitare un percorso di directory.  
  
5.  Fare clic su **Sostituisci** per sostituire la corrispondenza di ricerca corrente con il testo contenuto nella casella **Sostituisci con** . Fare clic su **Trova successivo** per ignorare una corrispondenza e su **Ignora file** per ignorare un intero file.  
  
     \- - oppure -  
  
     Scegliere **Sostituisci tutto** per sostituire tutte le corrispondenze di ricerca con il testo contenuto nella casella **Sostituisci con** . Selezionare **Non chiudere i file modificati con Sostituisci tutto** se si vuole annullare alcune delle sostituzioni eseguite in un'altra sessione.  
  
    > [!NOTE]  
    >  **Sostituisci tutto** consente di sostituire tutte le corrispondenze di ricerca comprese quelle dei file ignorati con **Ignora file** o **Trova successivo**. È possibile usare **Annulla** solo per le sostituzioni eseguite nei file rimasti aperti dopo l'operazione di sostituzione.  
  
 Per impostazione predefinita, le informazioni di sostituzione vengono visualizzate nella finestra Risultati ricerca 1. È possibile esplorare le sostituzioni facendo doppio clic su ogni voce della finestra Risultati ricerca 1.  
  
## <a name="see-also"></a>Vedere anche  
 [Ricerca e sostituzione](./search-and-replace.md)   
 [Ricerca interattiva all'interno di documenti](./search-documents-interactively.md)   
 [Testo di ricerca con caratteri jolly](./search-text-with-wildcards.md)   
 [Eseguire ricerche di testo con espressioni regolari](./search-text-with-regular-expressions.md)  
  
