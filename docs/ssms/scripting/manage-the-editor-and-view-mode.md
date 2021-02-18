---
title: Gestione dell'editor e della modalità di visualizzazione
description: "Informazioni su come selezionare una delle due modalità di visualizzazione di SQL Server Management Studio: modalità Documenti a schede e modalità interfaccia a documenti multipli. Informazioni sulle doppie visualizzazioni, l'a capo automatico, la modalità spazio virtuale, la visualizzazione di numeri di riga, la modalità schermo intero e Nascondi tutto automaticamente."
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Query Editor [SQL Server Management Studio], managing window behavior
- workbench view modes [SQL Server Management Studio]
- full screen mode [SQL Server Management Studio]
- fonts [SQL Server Management Studio]
- word wraps [SQL Server Management Studio]
- virtual space mode [SQL Server Management Studio]
- splitting document views
- displaying line numbers
- view modes [SQL Server Management Studio]
ms.assetid: 25c58a14-9f94-4296-9770-7d84c6bc3969
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8b70e9f5a954a3b763d8861afe3be0d83aaf7079
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270892"
---
# <a name="manage-the-editor-and-view-mode"></a>Gestione dell'editor e della modalità di visualizzazione

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Nell'editor sono disponibili vari modi per gestire la visualizzazione del codice.  

## <a name="changing-the-view-mode"></a>Modifica della modalità di visualizzazione  

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] offre una modalità di visualizzazione denominata **Documenti a schede** che consente di aprire contemporaneamente più editor e documenti e di accedervi tramite schede visualizzate nella parte superiore dell'editor. In alternativa, è possibile aprire l'ambiente [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] in modalità interfaccia a più documenti (MDI, Multiple Document Interface), nella quale le finestre sono visualizzate senza schede e possono essere ancorate, affiancate, ridotte a icona e così via.  
  
#### <a name="to-switch-between-view-modes"></a>Per passare da una modalità di visualizzazione all'altra  
  
1.  Scegliere **Opzioni** dal menu **Strumenti** .  
  
2.  Fare clic su **Ambiente**. Fare clic su **Generale**.  
  
3.  Selezionare **Documenti a schede** o **Ambiente MDI**.  
  
    > [!NOTE]  
    >  Le modifiche verranno applicate solo dopo il riavvio di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
## <a name="splitting-the-view"></a>Divisione di una finestra di visualizzazione  
 È possibile dividere una finestra dell'editor in due parti distinte per semplificare le operazioni di modifica.  
  
#### <a name="to-split-a-window"></a>Per dividere una finestra  
  
1.  Fare clic sulla barra di divisione visualizzata sopra la barra di scorrimento.  
  
2.  Trascinare la barra di divisione verso il basso.  
  
3.  Per visualizzare di nuovo un solo riquadro, fare doppio clic sulla barra di divisione che separa i due riquadri.  
  
 Nel nuovo riquadro sarà visualizzato lo stesso documento e qualsiasi modifica apportata in un riquadro si rifletterà nell'altro, purché si sia posizionati nello stesso punto del documento.  
  
## <a name="word-wrap"></a>A capo automatico  
 Quando si attiva l'opzione A capo automatico, la barra di scorrimento orizzontale viene rimossa e le righe di codice con una larghezza superiore alle dimensioni della finestra dell'editor vengono automaticamente mandate a capo sulla riga successiva visualizzata anziché scorrere nella parte non visibile della finestra.  
  
#### <a name="to-activate-word-wrap"></a>Per attivare il ritorno a capo automatico  
  
1.  Scegliere **Opzioni** dal menu **Strumenti** .  
  
2.  Fare clic su **Editor di testo**.  
  
3.  Aprire la cartella del linguaggio desiderato, oppure la cartella **Tutti i linguaggi** per eseguire l'impostazione per tutti i linguaggi.  
  
4.  Selezionare **A capo automatico**.  
  
## <a name="enabling-virtual-space-mode"></a>Attivazione della modalità spazio virtuale  
 Nella modalità **Spazio virtuale** le righe di codice possono continuare oltre l'area visibile dello schermo come se lo spazio alla fine di ogni riga fosse riempito da un numero infinito di spazi.  
  
#### <a name="to-enable-virtual-space-mode"></a>Per attivare la modalità spazio virtuale  
  
1.  Scegliere **Opzioni** dal menu **Strumenti** .  
  
2.  Fare clic su **Editor di testo**.  
  
3.  Aprire la cartella del linguaggio desiderato, oppure la cartella **Tutti i linguaggi** per eseguire l'impostazione per tutti i linguaggi.  
  
4.  Selezionare **Attiva spazio virtuale**.  
  
 Quando la modalità spazio virtuale non è attivata, il cursore si sposta dalla fine di una riga al primo carattere della riga successiva e viceversa.  
  
## <a name="displaying-line-numbers"></a>visualizzazione di numeri di riga  
 È possibile attivare la numerazione delle righe del codice. I numeri di riga sono utili per spostarsi all'interno del codice. Per altre informazioni, vedere [Spostarsi nel codice e nel testo](./navigate-code-and-text.md).  
  
> [!NOTE]  
>  L'attivazione dei numeri di riga non attiva la stampa dei numeri di riga nel documento. Per stampare i numeri di riga è necessario selezionare la casella di controllo **Numeri di riga** nella finestra di dialogo **Imposta pagina** a cui si accede dal menu **File** .  
  
#### <a name="to-display-line-numbers-in-code"></a>Per visualizzare i numeri di riga nel codice  
  
1.  Scegliere **Opzioni** dal menu **Strumenti** .  
  
2.  Fare clic su **Editor di testo**.  
  
3.  Fare clic su **Tutti i linguaggi**.  
  
4.  Fare clic su **Generale**.  
  
5.  Selezionare **Numeri di riga**.  
  
 Per impostare la numerazione delle righe solo per alcuni linguaggi di programmazione, selezionare **Numeri di riga** nella cartella appropriata.  
  
## <a name="enabling-full-screen-mode"></a>Attivazione della modalità Schermo intero  
 È possibile attivare la modalità Schermo intero per nascondere tutte le finestre degli strumenti e visualizzare solo le finestre dei documenti.  
  
#### <a name="to-enable-full-screen-mode"></a>Per attivare la modalità Schermo intero  
  
1.  Per attivare (e successivamente disattivare) la modalità Schermo intero, premere ALT+MAIUSC+INVIO.  
  
## <a name="using-auto-hide-all"></a>Utilizzo dell'opzione Nascondi tutto automaticamente  
  
#### <a name="to-hide-all-the-tool-windows-at-once"></a>Per nascondere tutte le finestre degli strumenti contemporaneamente  
  
1.  Scegliere **Nascondi tutto automaticamente** dal menu **Finestra** .  
  
