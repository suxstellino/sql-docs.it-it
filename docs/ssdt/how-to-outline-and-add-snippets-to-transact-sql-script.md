---
title: Strutturare e aggiungere frammenti di codice allo script Transact-SQL
description: Informazioni sui frammenti di codice forniti da SSDT. Scoprire come inserire frammenti di codice nelle applicazioni e come nascondere ed espandere il codice nell'editor Transact-SQL.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 543e7ce7-8639-4281-8a91-85314755e5de
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: be1adf9c385273d5cac8afbe972fd3f2f594ca34
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018141"
---
# <a name="how-to-outline-and-add-snippets-to-transact-sql-script"></a>Procedura: Strutturare e aggiungere frammenti di codice allo script Transact-SQL

In SQL Server Data Tools è disponibile una libreria del codice contenente frammenti di codice che è possibile inserire direttamente nell'applicazione. Ogni frammento consente di eseguire un'attività di script completa, quale la creazione di una funzione, di una tabella, di un trigger, di un indice, di una vista, di un tipo di dati definito dall'utente e così via. L'inserimento di un frammento nel codice di origine può essere eseguito con pochi clic del mouse. Questi frammenti consentono di aumentare la produttività riducendo il tempo necessario per digitare.  
  
Se è necessario cercare un frammento appropriato, è possibile utilizzare Selezione frammento di codice per visualizzare gli elenchi suddivisi per categoria in cui operare una scelta. Una volta aggiunto il frammento al codice, può essere necessario personalizzare alcune delle parti, ad esempio sostituire nomi di variabili con nomi più adatti o inserire la logica effettiva di una stored procedure. Si noterà che il frammento di codice inserito dispone di uno o più punti di sostituzione evidenziati nel codice a tale scopo. Se si posiziona il puntatore del mouse sul punto di sostituzione, viene visualizzata una descrizione comando che illustra come poter modificare il codice.  
  
Per impostazione predefinita, nell'Editor Transact\-SQL viene visualizzato tutto il testo, ma è possibile scegliere di nascondere parti di codice. L'Editor Transact\-SQL consente di selezionare un'area di codice e renderla comprimibile, in modo che venga visualizzata sotto un segno di addizione (+). È quindi possibile espandere o nascondere l'area facendo clic sul segno di addizione (+) accanto al simbolo. Il codice di struttura non è stato eliminato, ma semplicemente nascosto.  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nelle sezioni [Sviluppo del database connesso](../ssdt/connected-database-development.md) e [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-insert-snippets"></a>Per inserire frammenti di codice  
  
1.  Fare clic con il pulsante destro del mouse sul progetto **TradeDev** in **Esplora soluzioni** e selezionare **Aggiungi**, quindi **Script**. Nella finestra di dialogo **Aggiungi nuovo elemento** fare clic su **Aggiungi**.  
  
2.  Fare clic con il pulsante destro del mouse sull'Editor Transact\-SQL e selezionare **Inserisci frammento di codice**. Verrà visualizzata Selezione frammento di codice.  
  
3.  Fare doppio clic su **Tabella** in Selezione frammento di codice, quindi fare doppio clic su **Crea tabella**.  
  
4.  Si noti che i punti di sostituzione sono evidenziati in giallo. Passare il mouse su `Sample_Table`. Viene visualizzata una finestra popup contenente una descrizione della sostituzione. Fare doppio clic su `Sample_Table` e impostarlo su `Shipper2`.  
  
5.  Utilizzare il tasto TAB per spostarsi al punto di sostituzione successivo, ovvero `column_1`. Rinominarlo in `Id`. Seguire gli stessi passaggi per rinominare `column_2` in `name`, impostare il relativo tipo di dati su `nvarchar(128)` e consentire valori `NULL`.  
  
### <a name="to-outline-code"></a>Per strutturare il codice  
  
1.  Si noti il segno **-** accanto all'istruzione CREATE TABLE. Fare clic sul segno **-** accanto a una sezione nello script per nasconderla.  
  
2.  Fare clic con il pulsante destro del mouse sull'Editor \-SQL e selezionare **Struttura**, quindi **Rimuovi struttura** per rimuovere le informazioni sulla struttura senza influire sul codice sottostante nell'editor.  
  
3.  Per iniziare di nuovo la struttura del codice, fare clic con il pulsante destro del mouse sull'Editor Transact\-SQL e selezionare **Struttura**, quindi **Inizia struttura automatica**. Inoltre, è possibile selezionare **Attiva/Disattiva tutte le strutture** per passare da una sezione espansa a una nascosta.  
  
