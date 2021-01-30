---
description: Glossario di ADO
title: Glossario ADO | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: reference
helpviewer_keywords:
- ADO, glossary
ms.assetid: b0478836-4123-4357-969a-c5784fc28be5
author: rothja
ms.author: jroth
ms.openlocfilehash: 36ca4b14058b2728e60856f3c848452ad922107d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166242"
---
# <a name="ado-glossary"></a>Glossario di ADO
In questo argomento vengono definiti i termini rilevanti per ADO.  
  
## <a name="a"></a>A  
 URL assoluto  
 URL completo che specifica il percorso di una risorsa che risiede in Internet o in una rete Intranet. Vedere anche *URL* e *URL relativo*.  
  
 ActiveX (controllo)  
 Componente COM in-process con registrazione automatica che spesso dispone di un elemento visivo in fase di progettazione o di esecuzione. I controlli ActiveX sono inoltre in grado di comunicare con un contenitore di documenti attivi, ad esempio Microsoft Internet Explorer.  
  
 ADISAPI (Advanced Data Internet Server Application Programming Interface)  
 DLL ISAPI che fornisce analisi, controllo di automazione, marshalling del recordset e creazione di pacchetti MIME. Il componente ADISAPI funziona tramite l'API fornita da Internet Information Services (IIS). Vedere anche *ISAPI*.  
  
 funzione di aggregazione  
 In una query, funzione come COUNT, AVG o STDEV, che calcola un valore utilizzando tutte le righe in una colonna di una tabella. Per la scrittura di espressioni e la programmazione, è possibile utilizzare le funzioni di aggregazione SQL (incluse le tre elencate sopra) e le funzioni di aggregazione del dominio per determinare diverse statistiche.  
  
 alias  
 Nome alternativo fornito a una colonna o un'espressione in un'istruzione SQL SELECT, spesso più breve o più significativa. Ad esempio, BobSales è l'alias nell'istruzione SELECT seguente: "Select wr-Sales as BobSales from SalesDB". Un alias può essere utilizzato per assegnare dinamicamente le colonne per controllare le associazioni nell'oggetto DataControl.  
  
 threading apartment  
 Modello di threading COM in cui tutte le chiamate a un oggetto si verificano in un thread. Nel threading dell'Apartment, COM Sincronizza e esegue il marshalling delle chiamate. Vedere anche *COMmddefcom*.  
  
 operazione asincrona  
 Operazione che restituisce il controllo al programma chiamante senza attendere il completamento dell'operazione. Prima del completamento dell'operazione, l'esecuzione del codice continua. Vedere anche *operazione sincrona*.  
  
## <a name="b"></a>B  
 voce di associazione  
 Mapping tra un campo di una tabella e una variabile. Nelle estensioni ADO Visual C++, viene eseguito il mapping dei campi **Recordset** alle variabili C/C++.  
  
 maschera  
 Valore numerico destinato a un confronto di valori bit per bit con altri valori numerici, in genere per contrassegnare le opzioni nel parametro o nei valori restituiti. Questo confronto viene in genere eseguito con operatori logici bit per bit, ad esempio **and** e **or** in Visual Basic **&** e **&#124;** in C++.  
  
 Ad esempio, i valori ADO **FieldAttributeEnum** possono essere utilizzati come maschere di maschera per determinare gli attributi di un campo. Si supponga di voler determinare se un campo è aggiornabile. Per eseguire questa operazione, è possibile usare l'espressione seguente in Visual Basic:`Field.Attributes AND adFldUpdatable`  
  
 Se il risultato è TRUE, il campo è aggiornabile.  
  
 segnalibro  
 Marcatore che identifica in modo univoco una riga all'interno di un set di righe in modo che un utente possa accedervi rapidamente.  
  
 oggetto business  
 Oggetto che esegue un set definito di operazioni, ad esempio la convalida dei dati o la logica della regola business. Gli oggetti business in genere risiedono nel livello intermedio.  
  
 regola di business  
 Combinazione di modifiche di convalida, verifiche di accesso, ricerche nel database, criteri e trasformazioni algoritmiche che costituiscono il modo in cui le aziende operano. Noto anche come *logica di business*.  
  
## <a name="c"></a>C  
 espressione calcolata  
 Espressione non costante, ma il cui valore dipende da altri valori. Per la valutazione, un'espressione calcolata deve ottenere e calcolare i valori di altre origini, in genere in altri campi o righe.  
  
 capitolo  
 Riferimento a un intervallo di righe da un'origine dati. In ADO, un capitolo è in genere un riferimento a un altro **Recordset**.  
  
 Le colonne del capitolo consentono di definire una relazione *padre-figlio* in cui *l'elemento padre* è il **Recordset** contenente la colonna del capitolo e l' *elemento figlio* è il **Recordset** rappresentato dal capitolo.  
  
 capitolo-alias  
 Alias che fa riferimento alla colonna aggiunta all'elemento padre.  
  
 set di caratteri  
 Mapping di un set di caratteri ai valori numerici. Unicode, ad esempio, è un set di caratteri a 16 bit in grado di codificare tutti i caratteri noti e utilizzato come standard di codifica dei caratteri in tutto il mondo.  
  
 child  
 Lato dipendente di una relazione gerarchica. Un elemento figlio è un nodo in una struttura gerarchica con un altro nodo al di sopra di esso, più vicino alla radice. Vedere anche l' *alias figlio*, la *relazione padre-figlio*, l' *elemento padre*.  
  
 alias figlio  
 Alias che fa riferimento al figlio. Vedere anche *alias*, *figlio*.  
  
 CLSID (identificatore di classe)  
 Identificatore univoco universale (UUID) che identifica un componente COM. Ogni componente COM dispone del relativo CLSID nel registro di sistema di Windows, in modo che possa essere caricato da altre applicazioni. Vedere anche *ProgID*, *com*.  
  
 livello client  
 Livello logico di un sistema distribuito che in genere presenta i dati ed elabora l'input dell'utente, a volte definito front- *end*. In genere, il livello client richiede i dati da un server in base all'input e quindi formatta e visualizza il risultato. Vedere anche *livello intermedio*, *livello origine dati*, *applicazione distribuita*.  
  
 COM (Component Object Model)  
 Standard binario che consente agli oggetti di interagire in un ambiente di rete indipendentemente dalla lingua in cui sono stati sviluppati o dai computer che risiedono. Le tecnologie basate su COM includono controlli ActiveX, automazione e collegamento a oggetti e incorporamento (OLE). COM consente a un oggetto di esporre la funzionalità ad altri componenti e di ospitare le applicazioni. Definisce sia il modo in cui l'oggetto espone se stesso che la modalità di funzionamento di questa esposizione nei processi e nelle reti. COM definisce anche il ciclo di vita dell'oggetto.  
  
 Componente COM  
 File binario, ad esempio con estensione dll, ocx e alcuni file exe, che supporta lo standard COM per la fornitura di oggetti. Tale file contiene codice per uno o più class factory, classi COM, meccanismi di immissione del registro di sistema, caricamento del codice e così via.  
  
 operatore di confronto  
 Operatore che confronta due espressioni e restituisce un valore booleano.  
  
 Parametro dei criteri che può essere espresso come ">" (maggiore di), " \<" (less than), "=" (equal), "> =" (maggiore o uguale a), "<=" (minore o uguale a), "<>" (non uguale) o "like" (criteri di ricerca).  
  
 component  
 Oggetto che incapsula sia i dati che il codice e fornisce un set ben definito di servizi disponibili pubblicamente.  
  
 file composto  
 Implementazione dell'archiviazione strutturata COM per i file. Un file composto Archivia oggetti distinti in un singolo file strutturato costituito da due elementi principali: oggetti di archiviazione e oggetti flusso. Insieme, funzionano come un file system all'interno di un file.  
  
 Un numero di singoli file associati in un file fisico. È possibile accedere a ogni singolo file in un file composto come se si trattasse di un singolo file fisico.  
  
 constant  
 Valore numerico o stringa che non cambia. Le enumerazioni ADO denominate (costanti enumerate) possono essere usate nel codice anziché nei valori effettivi, ad esempio, **adUseClient** è una costante il cui valore è 3. (Const adUseClient = 3). Vedere anche *enumerazione*.  
  
 cursor  
 Elemento del database che controlla la navigazione dei record, l'aggiornamento dei dati e la visibilità delle modifiche apportate al database da altri utenti.  
  
## <a name="d"></a>D  
 data binding  
 Processo di associazione degli oggetti o dei controlli di un'applicazione a un'origine dati. Un controllo associato a un'origine dati viene chiamato *controllo con associazione a dati*.  
  
 Il contenuto di un controllo con associazione a dati è associato ai valori di un database. Un controllo griglia associato a un oggetto **Recordset** , ad esempio, può essere aggiornato quando vengono aggiornate le righe nel **Recordset** . Quando i nuovi valori vengono recuperati dal **Recordset**, i nuovi valori vengono visualizzati nella griglia.  
  
 provider di dati  
 Software che espone i dati a un'applicazione ADO direttamente o tramite un provider di servizi. Vedere anche provider di servizi.  
  
 Data Shaping  
 Una tecnica che utilizza una sintassi formalizzata (denominata linguaggio di **forma**) per definire un oggetto **Recordset** specializzato (denominato recordset con *forma*) che contiene non solo i dati, ma anche riferimenti ad altri oggetti **Recordset** e/o valori calcolati in base a tali oggetti **Recordset** .  
  
 livello origine dati  
 Livello logico di un sistema distribuito che rappresenta un computer che esegue un sistema DBMS, ad esempio un database SQL Server. Vedere anche *livello client*, *livello intermedio*, *applicazione distribuita*.  
  
 DCOM  
 Protocollo wire che consente ai componenti COM di comunicare direttamente tra loro attraverso una rete. Vedere anche *com*, *Component*.  
  
 DDL (Data Definition Language)  
 Tali istruzioni in SQL che definiscono, anziché modificare, i dati. Lo schema di un database viene creato o modificato con DDL. Ad esempio, **Create Table**, **create index**, **Grant** e **Revoke** sono istruzioni SQL DDL.  
  
 flusso predefinito  
 Flusso di testo o binario, rappresentato da un oggetto **flusso** , associato a oggetti **record** o **Recordset** quando si utilizzano determinati provider di OLE DB, ad esempio il provider Microsoft OLE DB per la pubblicazione Internet. Il flusso predefinito contiene in genere il contenuto di un file, ad esempio il codice HTML per la radice di un sito Web.  
  
 applicazione distribuita  
 Programma scritto in modo che l'elaborazione possa essere divisa tra più computer in una rete. In genere, un'applicazione distribuita è divisa in presentazione, logica di business e livelli di archivio dati o *livelli*. Vedere anche livello client, livello intermedio, livello origine dati.  
  
 Recordset disconnesso  
 Oggetto **Recordset** in una cache del client che non dispone più di una connessione in tempo reale al server. Se per qualche motivo è necessario accedere di nuovo all'origine dati originale, ad esempio l'aggiornamento dei dati, è necessario ristabilire la connessione. Tuttavia, è ancora possibile accedere alle raccolte, alle proprietà e ai metodi di un **Recordset** disconnesso.  
  
 DML (Data Manipulation Language)  
 Queste istruzioni in SQL che manipolano, anziché definire, i dati. I valori di un database vengono selezionati e modificati con DML. Ad esempio, **Insert**, **Update**, **Delete** e **Select** sono istruzioni SQL DML.  
  
 provider di origine del documento  
 Classe speciale di provider che gestiscono cartelle e documenti. Quando un documento è rappresentato da un oggetto **record** o una cartella di documenti viene rappresentata da un oggetto **Recordset** , il provider di origine del documento popola gli oggetti con un set univoco di campi che descrivono le caratteristiche del documento, invece del documento effettivo. Vedere anche record di risorse.  
  
 DSN (nome origine dati)  
 Raccolta di informazioni utilizzate per connettere l'applicazione a un database ODBC specifico. Gestione driver ODBC utilizza queste informazioni per creare una connessione al database. Un DSN può essere archiviato in un file (DSN di file) o nel registro di sistema di Windows (un DSN del computer).  
  
 proprietà dinamica  
 Proprietà specifica di un provider di dati o del servizio Cursor. La raccolta **Properties** di un oggetto viene popolata automaticamente con questi oggetti ("in modo dinamico"). Un oggetto non dispone di proprietà dinamiche fino a quando non viene connesso a un'origine dati tramite un provider di dati specifico. Vedere anche provider di dati, cursore.  
  
## <a name="e"></a>E  
 Enumerazione  
 Elenco di costanti denominate. I valori enumerati non devono essere univoci. Tuttavia, il nome di ogni valore deve essere univoco all'interno dell'ambito in cui è definita l'enumerazione. In ADO, le enumerazioni vengono utilizzate per il parametro numerico e i valori restituiti, per aggiungere significato al codice ADO e per schermare lo sviluppatore dai valori numerici (che potrebbero variare da una versione all'altra). Per aprire un **Recordset** statico, ad esempio, usare il valore enumerato **adOpenStatic** : `Recordset.Open ,,adOpenStatic`  
  
 Definito anche come *costante enumerata*. Vedere anche *Constant*.  
  
 evento  
 Azione riconosciuta da un oggetto, per la quale è possibile scrivere codice per rispondere. Gli eventi possono essere generati dall'esecuzione del comando, dal completamento delle transazioni, dall'esplorazione del recordset e dagli aggiornamenti dei dati, tra le altre azioni. Vedere anche *gestore eventi*.  
  
 gestore dell'evento  
 Un gestore eventi è il codice eseguito quando si verifica un evento. Vedere anche l'evento.  
  
## <a name="h"></a>H  
 gestore  
 Routine che gestisce una condizione o un'operazione comune e relativamente semplice, ad esempio il ripristino degli errori o la gestione dei dati.  
  
 Recordset gerarchico  
 **Recordset** che contiene un altro **Recordset**. Vedere anche data shaping, capitolo.  
  
 Per ulteriori informazioni, vedere [accesso alle righe in un recordset gerarchico](../guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
 gerarchia  
 In generale, una gerarchia è una struttura classificata con livelli di primo livello e subordinati. In ADO i **Recordset** gerarchici vengono utilizzati per rappresentare la relazione padre-figlio tra un record e un capitolo. Inoltre, in ADO, è possibile utilizzare gli oggetti **record** e **flusso** per accedere a strutture ad albero gerarchiche quali una cartella e i documenti. ADO MD include inoltre oggetti della **gerarchia** per rappresentare una relazione tra i livelli di una dimensione in un cubo OLAP. Vedere anche recordset gerarchici, relazione padre-figlio, capitolo, albero.  
  
## <a name="i-l"></a>I-L  
 ISAPI (Internet Server Application Programming Interface)  
 Un set di funzioni per i server Internet, ad esempio un server Windows NT®/Windows 2000 che esegue Microsoft® Internet Information Services (IIS).  
  
 Chiave  
 Una o più colonne di una tabella che identificano in modo univoco una riga; utilizzato spesso per indicizzare una tabella.  
  
## <a name="m"></a>M  
 marshaling  
 Il processo di creazione di pacchetti, invio e annullamento del packaging dei parametri del metodo di interfaccia tra i limiti del thread o del processo.  
  
 livello intermedio  
 Livello logico in un sistema distribuito tra un'interfaccia utente o un client Web e il database. Si tratta in genere del punto in cui viene creata un'istanza degli oggetti business. Il livello intermedio è una raccolta di regole e funzioni di business che generano e operano alla ricezione di informazioni. A tale scopo, le regole di business, che possono essere modificate di frequente, vengono incapsulate in componenti fisicamente separati dalla logica dell'applicazione stessa. Noto anche come *livello server applicazioni*. Vedere anche applicazione distribuita, livello client, livello origine dati.  
  
 MIME (estensione Internet mail multifunzione)  
 Protocollo Internet sviluppato originariamente per consentire lo scambio di messaggi di posta elettronica con contenuti avanzati in ambienti di rete, computer e posta elettronica eterogenei. In pratica, MIME è stato inoltre adottato ed esteso da applicazioni non di posta elettronica.  
  
 MIME è uno standard che consente la pubblicazione e la lettura di dati binari su Internet. L'intestazione di un file con dati binari contiene il tipo MIME dei dati; Ciò informa i programmi client, ad esempio i Web browser e i pacchetti di posta, che dovranno gestire i dati in modo diverso rispetto alla gestione del testo diretto. Ad esempio, l'intestazione di un documento Web contenente un grafico JPEG contiene il tipo MIME specifico del formato di file JPEG. Questo consente a un browser di visualizzare il file con il Visualizzatore JPEG, se presente.  
  
## <a name="n-o"></a>N-O  
 node  
 Elemento in una struttura ad albero gerarchica. Un nodo può essere la radice o l'elemento figlio di un altro nodo. Un nodo può essere anche l'elemento padre di più elementi figlio. Vedere anche gerarchia, albero, radice, figlio, padre.  
  
 variabile oggetto  
 Variabile che contiene un riferimento a un oggetto. Ad esempio, `objCustomObject` è una variabile che punta a un oggetto di tipo OggettoPersonalizzato:`Set objCustomObject = CreateObject(adodb.Recordset)`  
  
 ODBC (Open Database Connectivity)  
 Interfaccia del linguaggio di programmazione standard utilizzata per la connessione a un'ampia gamma di origini dati. Questa operazione viene in genere accessibile tramite il pannello di controllo, in cui è possibile assegnare i nomi delle origini dati (DSN) per utilizzare driver ODBC specifici.  
  
 OLE DB  
 Set di interfacce che espongono dati da diverse origini usando COM. OLE DB interfacce consentono alle applicazioni di accedere in modo uniforme ai dati archiviati in origini dati diverse. Queste interfacce supportano la quantità di funzionalità DBMS appropriate per l'origine dati, consentendo la condivisione dei dati. Vedere anche COM.  
  
 blocco ottimistico  
 Tipo di blocco in cui la pagina di dati contenente uno o più record, incluso il record da modificare, non è disponibile per altri utenti solo quando il record viene aggiornato dal metodo di **aggiornamento** , ma è disponibile prima e dopo la chiamata a **Update**.  
  
 Il blocco ottimistico viene utilizzato quando l'oggetto **Recordset** viene aperto con la proprietà o il parametro **LockType** impostato su **adLockOptimistic** o **adLockBatchOptimistic**. Vedere anche blocco pessimistico.  
  
 valore ordinale  
 Posizione numerica di un elemento all'interno di un ordine. In una raccolta ADO, il valore ordinale del primo elemento è zero (0). L'elemento successivo è uno (1) e così via.  
  
## <a name="p"></a>P  
 comando con parametri  
 Una query o un comando che consente di impostare i valori dei parametri prima dell'esecuzione del comando. È possibile, ad esempio, parametrizzare una stringa SQL incorporando i marcatori di parametro nella stringa SQL (designata dal carattere "?"). L'applicazione specifica quindi i valori per ogni parametro ed esegue il comando.  
  
 padre  
 Lato di controllo di una relazione gerarchica. In una struttura gerarchica, un elemento padre ha uno o più nodi figlio direttamente sotto di esso nella gerarchia. Vedere anche elemento padre-alias, relazione padre-figlio, figlio.  
  
 parent-alias  
 Alias che fa riferimento all'elemento padre. Vedere anche alias, padre.  
  
 relazione padre-figlio  
 Relazione in una struttura gerarchica in cui l'elemento padre è un livello superiore e associato direttamente a uno o più elementi figlio. Un elemento figlio è un livello inferiore e deve avere un solo padre. Vedere anche Parent, Child.  
  
 blocco pessimistico  
 Tipo di blocco in cui la pagina che contiene uno o più record, incluso il record da modificare, non è disponibile ad altri utenti per assicurarsi che venga eseguito un aggiornamento. Il comportamento di blocco pessimistico è definito dal provider OLE DB. In genere, i record vengono bloccati alla modifica e rimangono non disponibili fino al completamento del metodo di **aggiornamento** .  
  
 Il blocco pessimistico è abilitato quando l'oggetto **Recordset** viene aperto con la proprietà o il parametro **LockType** impostato su **adLockPessimistic**. Vedere anche blocco ottimistico.  
  
 pool  
 Ottimizzazione delle prestazioni basata sull'utilizzo di raccolte di risorse preallocate, ad esempio oggetti o connessioni di database. È più efficiente creare una risorsa esistente dal pool piuttosto che creare una nuova risorsa.  
  
 ProgID (identificatore a livello di codice)  
 Nome univoco mappato al registro di sistema di Windows da un'applicazione COM. Il ProgID per una connessione ADO è "ADODB. Connessione ". Vedere anche CLSID, COM.  
  
 proxy  
 Oggetto specifico dell'interfaccia che fornisce il marshalling dei parametri e la comunicazione necessari affinché un client chiami un oggetto applicazione in esecuzione in un ambiente di esecuzione diverso, ad esempio in un thread diverso o in un altro processo. Il proxy si trova con il client e comunica con uno stub corrispondente che si trova con l'oggetto applicazione chiamato. Vedere anche stub.  
  
## <a name="r"></a>R  
 URL relativo  
 URL parzialmente qualificato che specifica una risorsa in Internet o una rete Intranet il cui percorso è relativo a un punto di partenza specificato da un URL assoluto o un oggetto di connessione ADO equivalente. In effetti, gli URL assoluto e relativo concatenati costituisce un URL completo. Vedere anche URL e URL assoluto.  
  
 origine dati remota  
 Origine dati esistente in un altro computer, anziché nel sistema locale (in cui viene eseguita l'applicazione client).  
  
 record di risorse  
 Record da un provider di origine del documento che contiene i campi per la definizione e la descrizione di una cartella o di un documento. Il documento non è contenuto nel record di risorse, ma in genere è possibile accedervi dal flusso predefinito o da un campo nel record di risorse contenente un URL. Vedere anche provider di origine del documento, flusso predefinito, URL.  
  
 set di righe  
 Set di righe di un'origine dati con lo stesso schema di campo. Un set di righe può rappresentare tutti o alcuni campi di una tabella. Un set di righe può anche rappresentare una tabella virtuale, creata da una query o un join di due o più tabelle. In ADO i set di righe sono rappresentati da oggetti **Recordset** .  
  
## <a name="s"></a>S  
 Ambito  
 Intervallo di riferimento per un oggetto o una variabile oppure un intervallo di record in una vista o in una tabella. È ad esempio possibile fare riferimento alle variabili locali solo all'interno della routine in cui sono state definite. Le variabili pubbliche sono accessibili da qualsiasi punto dell'applicazione. Gli oggetti, ad esempio il database corrente, si trovano nell'ambito se si trovano nel percorso di ricerca definito. Gli intervalli di record possono essere specificati con una clausola SCOPE in molti comandi.  
  
 provider di servizi  
 Software che incapsula un servizio tramite la produzione e l'utilizzo di dati, potenziando le funzionalità nelle applicazioni ADO. Si tratta di un provider che non espone direttamente i dati, bensì fornisce un servizio, ad esempio l'elaborazione delle query. Il provider di servizi può elaborare i dati forniti da un provider di dati. Vedere anche provider di dati.  
  
 Recordset con forma  
 **Recordset** le cui colonne sono state definite in modo specifico per contenere non solo i dati, ma anche riferimenti (detti capitoli) ad altri oggetti **Recordset** e/o valori calcolati basati su altri oggetti **Recordset** .  
  
 pari livello  
 Due o più nodi in una struttura gerarchica che si trovano sullo stesso livello della gerarchia. Il nodo radice in una gerarchia non ha elementi di pari livello.  
  
 stored procedure  
 Raccolta precompilata di codice, ad esempio istruzioni SQL e istruzioni facoltative per il controllo di flusso archiviate con un nome ed elaborate come unità. Le stored procedure vengono archiviate in un database. possono essere eseguite con una sola chiamata da un'applicazione e consentono variabili dichiarate dall'utente, esecuzione condizionale e altre potenti funzionalità di programmazione.  
  
 stub  
 Oggetto specifico dell'interfaccia che fornisce il marshalling dei parametri e la comunicazione necessari per un oggetto applicazione per ricevere chiamate da un client in esecuzione in un ambiente di esecuzione diverso, ad esempio in un thread diverso o in un altro processo. Lo stub si trova con l'oggetto applicazione e comunica con un proxy corrispondente che si trova con il client che lo chiama. Vedere anche proxy.  
  
 nodo secondario  
 Vedere figlio.  
  
 operazione sincrona  
 Operazione avviata dal codice che viene completata prima che venga avviata l'operazione successiva. Vedere anche operazione asincrona.  
  
## <a name="t-z"></a>T-Z  
 Albero  
 Struttura che rappresenta una relazione gerarchica tra gli elementi (nodi). È presente un nodo nel livello superiore di un albero (radice). Sotto la radice possono essere presenti più elementi figlio. Ogni figlio può a sua volta essere l'elemento padre di altri elementi figlio, diramando così come un albero. Una cartella contenente documenti e altre cartelle è un esempio tipico di struttura ad albero. Vedere anche gerarchia, nodo, radice, figlio, padre.  
  
 Server Web  
 Computer che fornisce servizi Web e pagine a utenti Intranet e Internet.