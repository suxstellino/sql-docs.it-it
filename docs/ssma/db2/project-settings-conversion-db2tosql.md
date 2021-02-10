---
description: Impostazioni progetto (conversione) (DB2ToSQL)
title: Impostazioni progetto (conversione) (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 538c93cf-c5bb-43d5-b758-186d9fb00c19
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 5d01a893a2284a85fd4540d532500b7915a97326
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072345"
---
# <a name="project-settings-conversion-db2tosql"></a>Impostazioni progetto (conversione) (DB2ToSQL)
La pagina conversione della finestra di dialogo **Impostazioni progetto** contiene impostazioni che consentono di personalizzare il modo in cui SSMA converte la sintassi DB2 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi.  
  
Il riquadro conversione è disponibile nelle finestre di dialogo **Impostazioni** progetto e **Impostazioni progetto predefinite** :  
  
-   Per specificare le impostazioni per tutti i progetti SSMA, nel menu **strumenti** fare clic su **Impostazioni progetto predefinite**, selezionare tipo di progetto di migrazione per cui è necessario visualizzare o modificare le impostazioni dall'elenco a discesa **versione destinazione migrazione** , quindi fare clic su **generale** nella parte inferiore del riquadro a sinistra e quindi su **conversione**.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , quindi fare clic su **generale** nella parte inferiore del riquadro a sinistra e quindi su **conversione**.  
  
## <a name="conversion-messages"></a>Messaggi di conversione  
  
### <a name="generate-messages-about-issues-applied"></a>Genera messaggi relativi ai problemi applicati  
Specifica se SSMA genera messaggi informativi durante la conversione, li Visualizza nel riquadro di output e li aggiunge al codice convertito.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica:** No  
  
**Modalità completa:** No  
  
## <a name="miscellaneous-options"></a>Opzioni varie  
  
### <a name="cast-rownum-expressions-as-integers"></a>Cast di espressioni ROWNUM come numeri interi  
Quando SSMA converte le espressioni ROWNUM, l'espressione viene convertita in una clausola TOP, seguita dall'espressione. Nell'esempio seguente viene illustrato ROWNUM in un'istruzione di eliminazione DB2:  
  
`DELETE FROM Table1`  
  
`WHERE ROWNUM < expression and Field1 >= 2`  
  
Nell'esempio seguente viene illustrato il risultato [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
`DELETE TOP (expression-1)`  
  
`FROM Table1`  
  
`WHERE Field1>=2`  
  
Per la parte superiore è necessario che l'espressione di clausole TOP restituisca un valore integer. Se il valore integer è negativo, l'istruzione genererà un errore.  
  
-   Se si seleziona **Sì**, SSMA esegue il cast dell'espressione come Integer.  
  
-   Se si seleziona **No**, SSMA contrassegna tutte le espressioni non integer come errore nel codice convertito.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/completa:** No  
  
**Modalità ottimistica:** Sì  
  
### <a name="default-schema-mapping"></a>Mapping dello schema predefinito  
Questa impostazione specifica come viene eseguito il mapping degli schemi DB2 a SQL Server schemi. In questa impostazione sono disponibili due opzioni:  
  
1.  **Schema per database:** In questo modo, per impostazione predefinita, lo schema DB2' Sch1' verrà mappato a' dbo ' SQL Server schema nel database SQL Server ' Sch1'.  
  
2.  **Schema per schema:** In questo modo, per impostazione predefinita, lo schema DB2' Sch1' verrà mappato a' Sch1' SQL Server schema nel database predefinito SQL Server specificato nella finestra di dialogo di connessione.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Schema per database  
  
### <a name="conversion-ways-of-merge-statement"></a>Modalità di conversione dell'istruzione MERGE  
  
-   Se si seleziona **utilizzando l'istruzione INSERT, Update, Delete**, SSMA converte l'istruzione MERGE in istruzioni INSERT, Update e DELETE.  
  
-   Se si seleziona **Using Merge Statement**, SSMA converte l'istruzione MERGE in un'istruzione MERGE in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!WARNING]  
> Questa opzione di impostazione del progetto è disponibile solo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Utilizzo dell'istruzione MERGE  
  
### <a name="convert-calls-to-subprograms-that-use-default-arguments"></a>Converte le chiamate a sottoprogrammi che usano argomenti predefiniti  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le funzioni non supportano l'omissione dei parametri nella chiamata di funzione. Inoltre, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le funzioni e le routine non supportano espressioni come valori di parametro predefiniti.  
  
-   Se si seleziona **Sì** e una chiamata di funzione omette i parametri, SSMA inserisce la parola chiave **default** nella funzione e chiama nella posizione corretta. Quindi, la chiamata verrà contrassegnata con un avviso.  
  
-   Se si seleziona **No**, SSMA contrassegna le chiamate di funzione come errori.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
### <a name="convert-count-function-to-count_big"></a>Converte la funzione COUNT in COUNT_BIG  
Se è probabile che le funzioni COUNT restituiscano valori maggiori di 2.147.483.647, ovvero 2<sup>31</sup>-1, è necessario convertire le funzioni in COUNT_BIG.  
  
-   Se si seleziona **Sì**, SSMA convertirà tutti gli usi di COUNT in COUNT_BIG.  
  
-   Se si seleziona **No**, le funzioni rimarranno come Count. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituirà un errore se la funzione restituisce un valore maggiore di 2<sup>31</sup>-1.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/completa:** Sì  
  
**Modalità ottimistica:** No  
  
### <a name="convert-forall-statement-to-while-statement"></a>Converte l'istruzione FORALL nell'istruzione WHILE  
Definisce il modo in cui SSMA considererà i cicli FORALL sugli elementi della raccolta PL/SQL.  
  
-   Se si seleziona **Sì**, SSMA crea un ciclo while in cui gli elementi della raccolta vengono recuperati uno alla volta.  
  
-   Se si seleziona **No**, SSMA genera un set di righe dalla raccolta utilizzando il metodo nodes () e lo utilizza come una singola tabella. Questa operazione è più efficiente, ma rende il codice di output meno leggibile.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica:** No  
  
**Modalità completa:** Sì  
  
### <a name="convert-foreign-keys-with-set-null-referential-action-on-column-that-is-not-null"></a>Converte le chiavi esterne con l'operazione referenziale SET NULL sulla colonna che non è NULL  
DB2 consente la creazione di vincoli FOREIGN KEY, in cui non è possibile eseguire un'azione SET NULL perché nella colonna a cui si fa riferimento non sono consentiti valori NULL. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non consente la configurazione della chiave esterna.  
  
-   Se si seleziona **Sì**, SSMA genererà azioni referenziali come in DB2, ma sarà necessario apportare modifiche manuali prima di caricare il vincolo a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ad esempio, è possibile scegliere nessuna azione anziché impostare NULL.  
  
-   Se si seleziona **No**, il vincolo verrà contrassegnato come un errore.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** No  
  
### <a name="convert-function-calls-to-procedure-calls"></a>Converte le chiamate di funzione a chiamate di routine  
Alcune funzioni DB2 sono definite come transazioni autonome o contengono istruzioni che non sarebbero valide in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . In questi casi, SSMA crea una stored procedure e una funzione che è un wrapper per la routine. La funzione convertita chiama la procedura di implementazione.  
  
SSMA è in grado di convertire le chiamate alla funzione wrapper in chiamate alla routine. Questo consente di creare codice più leggibile e può migliorare le prestazioni. Tuttavia, il contesto non è sempre consentito. ad esempio, non è possibile sostituire una chiamata di funzione in SELECT list con una chiamata di routine. In SSMA sono disponibili alcune opzioni per coprire i casi comuni:  
  
-   Se si seleziona **Always**, SSMA tenta di convertire le chiamate alle funzioni wrapper in chiamate di procedura. Se il contesto corrente non consente questa conversione, viene generato un messaggio di errore. In questo modo, nessuna chiamata di funzione viene lasciata nel codice generato.  
  
-   Se si seleziona **quando possibile**, SSMA esegue una chiamata di procedura solo se la funzione ha parametri di output. Quando lo spostamento non è possibile, l'attributo di output del parametro viene rimosso. In tutti gli altri casi SSMA lascia chiamate di funzione.  
  
-   Se si seleziona **Never**, SSMA lascerà tutte le chiamate di funzione come chiamate di funzione. A volte questa scelta può essere inaccettabile per motivi di prestazioni.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Quando possibile  
  
### <a name="convert-lock-table-statements"></a>Converti istruzioni della tabella di blocco  
SSMA è in grado di convertire molte istruzioni LOCK TABLE in hint di tabella. SSMA non è in grado di convertire le istruzioni della tabella di blocco contenenti clausole PARTITION, subpartition, @dblink e NOWAIT e contrassegnare tali istruzioni con messaggi di errore di conversione.  
  
-   Se si seleziona **Sì**, SSMA convertirà le istruzioni della tabella di blocco supportate in hint di tabella.  
  
-   Se si seleziona **No**, SSMA contrassegna tutte le istruzioni lock Table con messaggi di errore di conversione.  
  
La tabella seguente illustra il modo in cui SSMA converte le modalità di blocco DB2:  
  
|Modalità di blocco DB2|SQL Server hint di tabella|  
|-|-|  
|CONDIVISIONE DI RIGA|ROWLOCK, HOLDLOCK|  
|RIGA ESCLUSIVA|ROWLOCK, XLOCK, HOLDLOCK|  
|AGGIORNAMENTO CONDIVISIONE = CONDIVISIONE RIGHE|ROWLOCK, HOLDLOCK|  
|CONDIVIDI|TABLOCK, HOLDLOCK|  
|CONDIVIDI RIGA ESCLUSIVA|TABLOCK, XLOCK, HOLDLOCK|  
|ESCLUSIVO|TABLOCKX, HOLDLOCK|  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
### <a name="convert-open-for-statements-for-ref-cursor-out-parameters"></a>Converte le istruzioni OPEN-FOR per i parametri OUT REF CURSOR  
In DB2 è possibile utilizzare l'istruzione OPEN-FOR per restituire un set di risultati al parametro OUT di un sottoprogramma di tipo REF CURSOR. In le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stored procedure restituiscono direttamente i risultati delle istruzioni SELECT.  
  
SSMA è in grado di convertire molte istruzioni OPEN-FOR in istruzioni SELECT.  
  
-   Se si seleziona **Sì**, SSMA converte l'istruzione Open-for in un'istruzione SELECT, che restituisce il set di risultati al client.  
  
-   Se si seleziona **No**, SSMA genererà un messaggio di errore nel codice convertito e nel riquadro di output.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
### <a name="convert-record-as-a-list-of-separates-variables"></a>Converti record come elenco di variabili separate  
SSMA è in grado di convertire i record DB2 in variabili separate e in variabili XML con una struttura specifica.  
  
-   Se si seleziona **Sì**, SSMA converte il record in un elenco di variabili separate, quando possibile.  
  
-   Se si seleziona **No**, SSMA converte il record in variabili XML con una struttura specifica.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
### <a name="convert-substr-function-calls-to-substring-function-calls"></a>Converte le chiamate di funzione substr a chiamate di funzione SUBSTRING  
SSMA è in grado di convertire le chiamate della funzione substr di DB2 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chiamate di funzione **substring** , a seconda del numero di parametri. Se SSMA non è in grado di convertire una chiamata di funzione substr o il numero di parametri non è supportato, SSMA convertirà la chiamata della funzione substr in una chiamata di funzione SSMA personalizzata.  
  
-   Se si seleziona **Sì**, SSMA convertirà le chiamate di funzione substr che usano tre parametri in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **substring**. Altre funzioni substr verranno convertite per chiamare la funzione SSMA personalizzata.  
  
-   Se si seleziona **No**, SSMA convertirà la chiamata della funzione substr in una chiamata di funzione SSMA personalizzata.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica:** Sì  
  
**Modalità completa:** No  
  
### <a name="convert-subtypes"></a>Converti sottotipi  
SSMA può convertire i sottotipi PL/SQL in due modi:  
  
-   Se si seleziona **Sì**, SSMA creerà [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un tipo definito dall'utente da un sottotipo e lo utilizzerà per ogni variabile di questo sottotipo.  
  
-   Se si seleziona **No**, SSMA sostituirà tutte le dichiarazioni di origine del sottotipo con il tipo sottostante e convertirà il risultato come di consueto. In questo caso, non viene creato alcun tipo aggiuntivo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** No  
  
### <a name="convert-synonyms"></a>Converti sinonimi  
È possibile eseguire la migrazione dei sinonimi per gli oggetti DB2 seguenti a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Tabelle e tabelle di oggetti  
  
-   Viste e visualizzazioni di oggetti  
  
-   Stored procedure e funzioni  
  
-   Viste materializzate  
  
I sinonimi per gli oggetti DB2 seguenti possono essere sostituiti da riferimenti diretti agli oggetti:  
  
-   Sequenze  
  
-   Pacchetti  
  
-   Oggetti dello schema di classe Java  
  
-   Tipi di oggetti definiti dall'utente  
  
Non è possibile eseguire la migrazione di altri sinonimi. SSMA genererà i messaggi di errore per il sinonimo e tutti i riferimenti che usano il sinonimo.  
  
-   Se si seleziona **Sì**, SSMA creerà [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sinonimi e riferimenti a oggetti diretti secondo gli elenchi precedenti.  
  
-   Se si seleziona **No**, SSMA creerà riferimenti a oggetti diretti per tutti i sinonimi elencati qui.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
### <a name="convert-to_chardate-format"></a>Converti TO_CHAR (data, formato)  
SSMA è in grado di convertire DB2 TO_CHAR (date, Format) in procedure dal database di sysdb.  
  
-   Se si seleziona **utilizzando TO_CHAR_DATE funzione**, SSMA converte il TO_CHAR (data, formato) in TO_CHAR_DATE funzione utilizzando la lingua inglese per la conversione.  
  
-   Se si seleziona l' **uso di TO_CHAR_DATE_LS function (NLS care)**, SSMA converte il TO_CHAR (date, Format) in TO_CHAR_DATE_LS funzione utilizzando il linguaggio di sessione per la conversione  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica:** Uso di TO_CHAR_DATE funzione  
  
**Modalità completa:** Uso della funzione TO_CHAR_DATE_LS (NLS care)  
  
### <a name="convert-transaction-processing-statements"></a>Converti istruzioni di elaborazione delle transazioni  
SSMA è in grado di convertire le istruzioni di elaborazione delle transazioni DB2:  
  
-   Se si seleziona **Sì**, SSMA converte le istruzioni di elaborazione delle transazioni DB2 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istruzioni.  
  
-   Se si seleziona **No**, SSMA contrassegna le istruzioni di elaborazione delle transazioni come errori di conversione.  
  
> [!NOTE]  
> DB2 apre in modo implicito le transazioni. Per emulare questo comportamento in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario aggiungere BEGIN TRANSACTION istruzioni manualmente in cui si desidera avviare le transazioni. In alternativa, è possibile eseguire il comando SET IMPLICIT_TRANSACTIONS ON all'inizio della sessione. SSMA aggiunge automaticamente SET IMPLICIT_TRANSACTIONS quando si convertono subroutine con transazioni autonome.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
### <a name="emulate-db2-null-behavior-in-order-by-clauses"></a>Emulare il comportamento null DB2 nelle clausole ORDER BY  
I valori NULL vengono ordinati in modo diverso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e DB2:  
  
-   In i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valori null sono i valori più bassi in un elenco ordinato. In un elenco crescente, i valori NULL verranno visualizzati per primi.  
  
-   In DB2 i valori NULL sono i valori più alti in un elenco ordinato. Per impostazione predefinita, i valori NULL vengono visualizzati per ultimi in un elenco in ordine crescente.  
  
-   DB2 presenta prima di tutto i valori NULL e Annulla le ultime clausole, che consente di modificare il modo in cui DB2 Ordina i valori NULL.  
  
SSMA è in grado di emulare il comportamento di ORDER BY DB2 controllando la presenza di valori NULL. Viene quindi prima ordinata in base ai valori NULL nell'ordine specificato, quindi gli ordini vengono ordinati in base ad altri valori.  
  
-   Se si seleziona **Sì**, SSMA convertirà l'istruzione DB2 in modo da emulare il comportamento DB2 order by.  
  
-   Se si seleziona **No**, SSMA ignorerà le regole DB2 e genererà un messaggio di errore quando rileverà prima i valori NULL e nullerà le ultime clausole.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica:** No  
  
**Modalità completa:** Sì  
  
### <a name="emulate-row-count-exceptions-in-select"></a>Emulare le eccezioni per il conteggio delle righe in SELECT  
Se un'istruzione SELECT con una clausola INTO non restituisce alcuna riga, DB2 genera un'eccezione NO_DATA_FOUND. Se l'istruzione restituisce due o più righe, viene generata l'eccezione TOO_MANY_ROWS. L'istruzione convertita in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non genera alcuna eccezione se il conteggio delle righe è diverso da uno.  
  
-   Se si seleziona **Sì**, SSMA aggiunge la chiamata a sysdb procedure db_error_exact_one_row_check dopo ogni istruzione SELECT. Questa procedura emula le eccezioni NO_DATA_FOUND e TOO_MANY_ROWS. Si tratta dell'impostazione predefinita e consente di riprodurre il comportamento DB2 il più vicino possibile. È sempre necessario scegliere **Sì** se il codice sorgente dispone di gestori di eccezioni che elaborano questi errori. Si noti che se l'istruzione SELECT si verifica all'interno di una funzione definita dall'utente, questo modulo verrà convertito in un stored procedure, perché l'esecuzione di stored procedure e la generazione di eccezioni non sono compatibili con il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contesto della funzione.  
  
-   Se si seleziona **No**, non verrà generata alcuna eccezione. Questo può essere utile quando SSMA converte una funzione definita dall'utente e si desidera che rimanga una funzione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
### <a name="generate-error-for-dbms_sqlparse"></a>Genera un errore per DBMS_SQL. ANALIZZARE  
  
-   Se si seleziona **errore**, SSMA genera un errore durante la conversione DBMS_SQL. Analizzare.  
  
-   Se si seleziona **avviso**, SSMA genera un avviso durante la conversione DBMS_SQL. Analizzare.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Errore  
  
### <a name="generate-rowid-column"></a>Genera colonna ROWID  
Quando SSMA crea tabelle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , può creare una colonna ROWID. Quando si esegue la migrazione dei dati, ogni riga ottiene un nuovo valore UNIQUEIDENTIFIER generato dalla funzione NEWID ().  
  
-   Se si seleziona **Sì**, la colonna ROWID viene creata in tutte le tabelle e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera GUID quando si inseriscono i valori. Se si prevede di usare SSMA tester, scegliere sempre **Sì** .  
  
-   Se si seleziona **No**, le colonne ROWID non vengono aggiunte alle tabelle.  
  
-   **Aggiungere la colonna ROWID per le tabelle con trigger** Aggiungi ROWID per le tabelle contenenti trigger.  
  
> [!WARNING]  
> L'impostazione predefinita nel caso di SQL Server 2005, SQL Server 2008 e SQL Server 2012 e 2014 è **Aggiungi colonna ROWID per le tabelle con trigger**.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica:** Aggiungere la colonna ROWID per le tabelle con trigger  
  
**Modalità completa:** Sì  
  
### <a name="generate-unique-index-on-rowid-column"></a>Genera indice univoco nella colonna ROWID  
Specifica se SSMA genera una colonna di indice univoca nella colonna ROWID generata. Se l'opzione è impostata su "YES", viene generato un indice univoco e se è impostato su "NO", nella colonna ROWID non viene generato un indice univoco.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
### <a name="local-modules-conversion"></a>Conversione di moduli locali  
Definisce il tipo di sottoprogramma nidificato DB2 (dichiarato nella conversione di stored procedure o funzione autonoma).  
  
-   Se si seleziona **inline**, le chiamate al sottoprogramma nidificate verranno sostituite dal relativo corpo.  
  
-   Se si seleziona **stored procedure**, il sottoprogramma nidificato verrà convertito in un SQL Server stored procedure e le relative chiamate verranno sostituite in questa chiamata di procedura.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Inline  
  
### <a name="use-isnull-in-string-concatenation"></a>USA ISNULL nella concatenazione di stringhe  
DB2 e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituiscono risultati diversi quando le concatenazioni di stringhe includono valori null. DB2 considera il valore NULL come un set di caratteri vuoto. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce NULL.  
  
-   Se si seleziona **Sì**, SSMA sostituisce il carattere di concatenazione DB2 (| |) con il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] carattere di concatenazione (+). SSMA controlla inoltre le espressioni su entrambi i lati della concatenazione per i valori NULL.  
  
-   Se si seleziona **No**, SSMA sostituisce i caratteri di concatenazione, ma non verifica la presenza di valori null.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
### <a name="use-isnull-in-replace-function-calls"></a>USA ISNULL nelle chiamate di funzione REPLACE  
L'istruzione ISNULL viene utilizzata nelle chiamate di funzione REPLACE per emulare il comportamento di DB2. Per questa impostazione sono presenti le opzioni seguenti:  
  
-   YES  
  
-   NO  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica:** No  
  
**Modalità completa:** Sì  
  
### <a name="use-isnull-in-concat-function-calls"></a>USA ISNULL nelle chiamate di funzione CONCAt  
L'istruzione ISNULL viene usata nelle chiamate di funzione CONCAt per emulare il comportamento di DB2. Per questa impostazione sono presenti le opzioni seguenti:  
  
-   YES  
  
-   NO  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica:** No  
  
**Modalità completa:** Sì  
  
### <a name="use-native-convert-function-when-possible"></a>Usa la funzione Convert nativa quando possibile  
  
-   Se si seleziona **Sì**, SSMA converte la TO_CHAR (data, formato) nella funzione di conversione nativa, quando possibile.  
  
-   Se si seleziona **No**, SSMA converte il TO_CHAR (data, formato) in TO_CHAR_DATE o TO_CHAR_DATE_LS (è definito dalle opzioni "Converti TO_CHAR (data, formato)").  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica:** Sì  
  
**Modalità completa:** No  
  
### <a name="use-selectfor-xml-when-converting-selectinto-for-record-variable"></a>USA seleziona... FOR XML durante la conversione di SELECT... INTO per la variabile di record  
Specifica se generare un set di risultati XML quando si seleziona in una variabile di record.  
  
-   Se si seleziona **Sì**, l'istruzione SELECT restituisce il codice XML.  
  
-   Se si seleziona **No**, l'istruzione SELECT restituisce un set di risultati.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** No  
  
## <a name="returning-clause-conversion"></a>Conversione della clausola RETURNING  
  
### <a name="convert-returning-clause-in-delete-statement-to-output"></a>Converte la clausola RETURNING nell'istruzione DELETE nell'OUTPUT  
DB2 fornisce una clausola RETURNING come metodo per ottenere immediatamente i valori eliminati. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce tale funzionalità con la clausola OUTPUT.  
  
-   Se si seleziona **Sì**, SSMA convertirà le clausole restituzione nelle istruzioni DELETE nelle clausole output. Poiché i trigger in una tabella possono modificare i valori, il valore restituito potrebbe essere diverso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rispetto a DB2.  
  
-   Se si seleziona **No**, SSMA genererà un'istruzione SELECT prima delle istruzioni DELETE per recuperare i valori restituiti.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
### <a name="convert-returning-clause-in-insert-statement-to-output"></a>Converte la clausola RETURNING nell'istruzione INSERT nell'OUTPUT  
DB2 fornisce una clausola RETURNING come metodo per ottenere immediatamente i valori inseriti. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce tale funzionalità con la clausola OUTPUT.  
  
-   Se si seleziona **Sì**, SSMA convertirà una clausola returning in un'istruzione INSERT in output. Poiché i trigger in una tabella possono modificare i valori, il valore restituito potrebbe essere diverso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rispetto a DB2.  
  
-   Se si seleziona **No**, SSMA emula la funzionalità DB2 inserendo e selezionando i valori da una tabella di riferimento.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
### <a name="convert-returning-clause-in-update-statement-to-output"></a>Converte la clausola RETURNING nell'istruzione UPDATE nell'OUTPUT  
DB2 fornisce una clausola RETURNING come metodo per ottenere immediatamente i valori aggiornati. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce tale funzionalità con la clausola OUTPUT.  
  
-   Se si seleziona **Sì**, SSMA convertirà le clausole restituzione nelle istruzioni Update in clausole output. Poiché i trigger in una tabella possono modificare i valori, il valore restituito potrebbe essere diverso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rispetto a DB2.  
  
-   Se si seleziona **No**, SSMA genererà le istruzioni SELECT dopo Update per recuperare i valori restituiti.  
  
Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:  
  
**Modalità predefinita/ottimistica/completa:** Sì  
  
## <a name="sequence-conversion"></a>Conversione sequenza  
  
### <a name="convert-sequence-generator"></a>Converti generatore sequenza  
In DB2 è possibile utilizzare una sequenza per generare identificatori univoci.  
  
SSMA è in grado di convertire le sequenze come segue.  
  
-   Uso di SQL Server Generatore sequenza (questa opzione è disponibile solo quando si esegue la conversione in SQL Server 2012 e SQL Server 2014).  
  
-   Uso del generatore di sequenze SSMA.  
  
-   Utilizzando l'identità della colonna.  
  
Per impostazione predefinita, quando si esegue la conversione in SQL Server 2012 o SQL Server 2014 è necessario utilizzare SQL Server Generatore di sequenze. Tuttavia, SQL Server 2012 e SQL Server 2014 non supporta il recupero del valore di sequenza corrente (ad esempio, il metodo CURRVAL di DB2 Sequence). Vedere il sito Blog del team di SSMA per informazioni aggiuntive sulla migrazione del metodo CURRVAL della sequenza DB2.  
  
SSMA fornisce anche un'opzione per convertire la sequenza DB2 in SSMA Sequence Emulator. Questa è l'opzione predefinita quando si esegue la conversione a SQL Server precedente alla 2012  
  
Infine, è anche possibile convertire la sequenza assegnata a una colonna nella tabella per SQL Server valori Identity. È necessario specificare il mapping tra le sequenze a una colonna Identity nella scheda **tabella** DB2  
  
### <a name="convert-currval-outside-triggers"></a>Converte CURRVAL all'esterno di trigger  
Visibile solo quando il generatore di sequenze di conversione è impostato sull' **utilizzo dell'identità della colonna**. Poiché le sequenze DB2 sono oggetti separati dalle tabelle, molte tabelle che utilizzano sequenze utilizzano un trigger per generare e inserire un nuovo valore di sequenza. SSMA Invia commenti a queste istruzioni o le contrassegna come errori quando il commento genera errori.  
  
-   Se si seleziona **Sì**, SSMA contrassegna tutti i riferimenti ai trigger esterni nella sequenza convertita CURRVAL con un avviso.  
  
-   Se si seleziona **No**, SSMA contrassegna tutti i riferimenti ai trigger esterni sulla sequenza convertita CURRVAL con un errore.  
  
## <a name="see-also"></a>Vedere anche  
[Informazioni di riferimento sull'interfaccia utente &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
