---
title: Usare il confronto schema per confrontare definizioni di database diverse
description: Informazioni su come confrontare le definizioni di database con Confronto schemi. Scoprire come escludere differenze specifiche e aggiornare la destinazione o creare uno script di aggiornamento.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.schemacompare.SchemaCompareOptionsDialog
- sql.data.tools.schemacompare.watermark.f1
- sql.data.tools.schemacompare.f1
- sql.data.tools.schemacompare.connectiondialog.f1
- sql.data.tools.schemacompare.connectiondialog.error.f1
ms.assetid: 7f0905a4-081c-46e2-bd7d-325b63e5c675
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 414a6eb4f6e63d2bb0c02e831f60ebcff97dc590
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018031"
---
# <a name="how-to-use-schema-compare-to-compare-different-database-definitions"></a>Procedura: Usare il confronto schema per confrontare definizioni di database diverse

In SQL Server Data Tools (SSDT) è inclusa un'utilità Confronto schema che è possibile usare per confrontare due definizioni di database.  L'origine e la destinazione del confronto possono essere qualsiasi combinazione di un database connesso, un progetto di database di SQL Server, uno snapshot o un file con estensione dacpac.  I risultati del confronto vengono visualizzati come un set di azioni da intraprendere sulla destinazione per renderla uguale all'origine.  Una volta completato il confronto, è possibile aggiornare direttamente la destinazione (se la destinazione è un progetto o un database) oppure generare uno script di aggiornamento che produce gli stessi effetti.  
  
Le differenze tra l'origine e la destinazione sono visualizzate in una griglia per facilitarne la revisione.  È possibile esaminare e rivedere tutte le differenze nella griglia dei risultati o sotto forma di script.  È possibile quindi escludere in maniera selettiva specifiche differenze.  
  
È possibile salvare i confronti come parte di un progetto di database SQL Server o come un file autonomo.  È inoltre possibile impostare le opzioni che controllano l'ambito del confronto e gli aspetti dell'aggiornamento.  È quindi possibile salvare il confronto in modo da poterlo ripetere con facilità in un secondo momento o utilizzarlo come punto di partenza per un nuovo confronto.  
  
Nella procedura seguente viene confrontato lo schema di un progetto di database con un database connesso.  
  
> [!WARNING]  
> Se un progetto viene specificato come destinazione per il confronto, la lunghezza massima del percorso supportata (esclusi la lettera di unità, i due punti e la barra rovesciata iniziale) per il progetto è di 256 caratteri. Se il percorso di progetto supera questa lunghezza, sarà comunque possibile confrontare lo schema con un database o un altro progetto, ma non si potrà aggiornarlo.  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nelle sezioni [Sviluppo del database connesso](../ssdt/connected-database-development.md) e [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-compare-database-definitions"></a>Per confrontare le definizioni di database  
  
1.  Nel menu **Strumenti** selezionare **SQL Server** e quindi fare clic su **Nuovo confronto schema**.  
  
    In alternativa, fare clic con il pulsante destro del mouse sul progetto **TradeDev** in **Esplora soluzioni** e selezionare **Confronto schema**.  
  
    Verrà visualizzata la finestra **Confronto schema** e Visual Studio assegna automaticamente un nome, ad esempio `SqlSchemaCompare1`.  
  
    Vengono visualizzati due menu a discesa con una freccia verde tra di loro proprio sotto la barra degli strumenti **Confronto schema**. Questi menu consentono di selezionare le definizioni di database per l'origine e la destinazione del confronto.  
  
2.  Nell'elenco a discesa **Seleziona origine** scegliere **Seleziona origine**. Verrà visualizzata la finestra di dialogo **Seleziona schema di origine**.  
  
    Si noti che se si apre la finestra **Confronto schema** facendo clic con il pulsante destro del mouse sul nome del progetto, lo schema di origine è già popolato ed è possibile procedere al passaggio 4.  
  
3.  Selezionare il pulsante di opzione **Project**, quindi selezionare il progetto di database **TradeDev** creato nella procedura precedente.  
  
4.  Dall'elenco a discesa **Seleziona destinazione** nella finestra **Confronto schema** scegliere **Seleziona destinazione**. Verrà visualizzata la finestra di dialogo **Seleziona schema di destinazione**. Nella sezione **Schema** fare clic sul pulsante di opzione **Database**, quindi sul pulsante **Nuova connessione**.  
  
5.  Nella finestra di dialogo **Proprietà connessione** immettere il nome del server in cui si trova il database `TradeDev` e assicurarsi che vengano specificate le credenziali di autenticazione corrette. Selezionare quindi **TradeDev** in **Connessione a un database** e fare clic su **OK**.  
  
    È inoltre possibile fare clic sul pulsante **Opzioni** nella barra degli strumenti della finestra **Confronto schema** per specificare quali oggetti vengono confrontati, quali tipi di differenze vengono ignorate e altre impostazioni.  
  
6.  Fare clic sul pulsante **Confronta** nella barra degli strumenti della finestra **Confronto schema** per avviare il processo di confronto.  
  
    Al termine del confronto, le differenze strutturali tra il progetto e il database verranno visualizzate nel riquadro dei **risultati** nella parte superiore della finestra. Per impostazione predefinita, nei risultati del confronto tutte le differenze sono raggruppate in base all'azione (ad esempio, Elimina, Modifica o Aggiungi). Nel riquadro dei **risultati** viene visualizzata una riga per ogni oggetto di database che presenta differenze tra le definizioni di database. Ogni riga identifica l'oggetto nello schema di origine o di destinazione (o entrambi) e l'azione che verrà intrapresa sullo schema di destinazione per rendere l'oggetto di destinazione uguale a quello di origine.  Se un oggetto è stato sottoposto a refactoring e rinominato o spostato in un nuovo schema, i nomi di origine e di destinazione sono differenti e il nome dell'origine verrà visualizzato in carattere grassetto per evidenziare la differenza.  
  
    Per impostazione predefinita, gli oggetti uguali in entrambi gli schemi o non supportati per l'aggiornamento (ad esempio, oggetti predefiniti) risultano nascosti nell'elenco dei risultati.  È possibile fare clic sui pulsanti di filtro appropriati nella barra degli strumenti per visualizzare questi oggetti.  
  
    Per modificare la preferenza di raggruppamento, fare clic sull'elenco a discesa **Risultati del gruppo** nella barra degli strumenti.  Selezionare **Tipo** per raggruppare i risultati per tipo di oggetto (ad esempio, per tabelle, viste o stored procedure).  
  
7.  Individuare la tabella `Products` nel gruppo `Tables`. Fare clic sulla riga e notare che le definizioni di origine e di destinazione della tabella sono visualizzate nel riquadro **Definizioni di oggetto** con le differenze evidenziate. È inoltre possibile espandere la riga della tabella `Products` nel riquadro dei **risultati** per controllare gli elementi specifici della tabella che risultano differenti.  
  
8.  Per impostazione predefinita, tutte le differenze vengono incluse nell'ambito dell'azione Aggiorna destinazione. È possibile escludere le differenze che non si desidera sincronizzare. A tale scopo, deselezionare l'opzione nella colonna **Azione** al centro di ogni riga. In alternativa, fare clic con il pulsante destro del mouse su una riga nel riquadro Schema e selezionare **Escludi**. Si noti che la riga viene immediatamente disabilitata. Al momento di aggiornare il database di destinazione, questa riga non verrà considerata per le modifiche in sospeso.  
  
    È inoltre possibile fare clic con il pulsante destro del mouse sulla riga di un gruppo e selezionare **Escludi tutto** o **Includi tutto**, che equivale a deselezionare o selezionare tutte le differenze presenti in quel gruppo. Raggruppare i risultati per schema rappresenta un modo utile per includere o escludere tutte le modifiche apportate a uno schema specifico.  
  
    > [!WARNING]  
    > Se per la riga esclusa sono presenti oggetti dipendenti (ad esempio, una riga di una **tabella** a cui fa riferimento una riga di una **vista**), la riga esclusa verrà disabilitata ma la casella di controllo corrispondente non verrà deselezionata. Una volta deselezionate tutte le righe dipendenti, la riga disabilitata verrà deselezionata. Inoltre, se una riga è stata sottoposta a refactoring (rinominata o spostata in un altro schema), la casella di controllo viene disabilitata per quella riga e per tutte le righe figlio dipendenti.  
    >   
    > Si noti che se si aggiorna il confronto, le differenze che si è scelto di ignorare verranno escluse.  
  
Per aggiornare lo schema della destinazione, sono disponibili due opzioni. È possibile aggiornare direttamente la destinazione dalla finestra **Confronto schema** se la destinazione è un progetto o un database oppure generare uno script di aggiornamento se la destinazione è un database o un file di database.  Verrà visualizzato uno script generato nell'Editor Transact\-SQL in cui è possibile esaminare lo script per eseguirlo su un database. Le procedure seguenti descrivono ulteriormente queste opzioni.  
  
> [!WARNING]  
> L'aggiornamento non riuscirà correttamente poiché le modifiche implicano la modifica di una colonna da NOT NULL a NULL e ciò determina una perdita di dati. Se si vuole procedere con l'aggiornamento, fare clic sul pulsante **Opzioni** (il quinto da sinistra) nella barra degli strumenti per Confronto schema e deselezionare l'opzione **Blocca distribuzione incrementale in caso di perdita di dati**.  
  
### <a name="to-update-directly-in-the-schema-compare-window"></a>Per aggiornare direttamente nella finestra Confronto schema  
  
1.  Fare clic sul pulsante **Aggiorna** nella barra degli strumenti per la finestra Confronto schema.  
  
2.  Esaminare lo script delle modifiche generato. È possibile salvare lo script tramite il menu File/Nuovo. Questa operazione può essere utile nelle situazioni in cui non si è autorizzati ad aggiornare un database di produzione e in tal caso è possibile fornire lo script a un amministratore di database per una distribuzione successiva.  
  
3.  Se si dispone dell'autorizzazione necessaria per l'aggiornamento del database, fare clic sul pulsante **Esegui query** nella barra degli strumenti del riquadro di modifica per eseguire lo script.  
  
### <a name="to-update-by-script"></a>Per aggiornare in base allo script  
  
1.  Fare clic sul pulsante **Genera script** (il quarto da sinistra) nella barra degli strumenti per la finestra Confronto schema.  
  
    Lo script generato verrà visualizzato in una nuova finestra dell'Editor Transact\-SQL  
  
    > [!WARNING]  
    > Solo i file con estensione dacpac creati dal processo dello snapshot SSDT supportano questo comportamento.  Non è possibile al momento utilizzare come destinazione un file con estensione dacpac prodotto dagli strumenti di un'applicazione livello dati SQL o da un framework.  
  
2.  Esaminare lo script delle modifiche generato. È possibile salvare lo script tramite il comando del menu **File/Salva** o File/Salva con nome.  
  
    Uno script salvato può essere utile in situazioni in cui non si è autorizzati ad aggiornare un database di produzione. In questi casi, è possibile fornire lo script a un amministratore di database per una distribuzione successiva.  
  
    In alternativa, è possibile connettere l'Editor Transact\-SQL a un server appropriato ed eseguire lo script direttamente. Prima di poter eseguire questa procedura, è necessario disporre dell'autorizzazione necessaria per la creazione o l'aggiornamento del database. Se si dispone dell'autorizzazione necessaria per l'aggiornamento del database, fare clic sul pulsante **Esegui query** nella barra degli strumenti del riquadro di modifica per eseguire lo script.  
  
3.  Fare clic sul pulsante **Connetti**. Questa azione effettua la connessione al server corrente o richiede l'immissione o la selezione di un server nella finestra di dialogo Connetti al server.  Si noti che il nome database è definito nello script come una variabile di comando.  
  
4.  Esaminare lo script e, se necessario, apportare tutte le modifiche alle variabili di comando che definiscono il nome database di destinazione, il prefisso associato e i percorsi dei file.  
  
5.  Fare clic sul pulsante **Esegui** nella barra degli strumenti del riquadro di modifica per eseguire lo script.  
  
