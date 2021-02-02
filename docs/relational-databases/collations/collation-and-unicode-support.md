---
description: Supporto Unicode e delle regole di confronto
title: Supporto Unicode e delle regole di confronto | Microsoft Docs
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- binary collations [SQL Server]
- expression-level collations [SQL Server]
- Windows collations [SQL Server]
- globalization [SQL Server], about globalization
- supplementary character support
- code pages [SQL Server], about code pages
- collations [SQL Server], about collations
- Unicode [SQL Server], collations
- database-level collations [SQL Server]
- reading order
- column-level collations
- locales [SQL Server], about locales
- locales [SQL Server]
- code pages [SQL Server]
- SQL Server collations
- UTF-8
- UTF-16
- UTF8
- UTF16
- UCS2
- server-level collations [SQL Server]
ms.assetid: 92d34f48-fa2b-47c5-89d3-a4c39b0f39eb
author: pmasl
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e2a5f205d7182d1ffedf07b885e7411920ca7b27
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251194"
---
# <a name="collation-and-unicode-support"></a>Supporto Unicode e delle regole di confronto
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Le regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] forniscono regole di ordinamento e proprietà di distinzione tra maiuscole e minuscole e tra caratteri accentati e non accentati per i dati. Le regole di confronto usate con dati di tipo carattere, quali **char** e **varchar**, definiscono la tabella codici e i caratteri corrispondenti che possono essere rappresentati per quel tipo di dati. 

Sia che si installi una nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si ripristini il backup di un database o si stabiliscano connessioni tra database server e client, è importante comprendere i requisiti delle impostazioni locali, l'ordinamento e la modalità di distinzione tra maiuscole e minuscole e tra caratteri accentati e non accentati dei dati da usare. Per visualizzare l'elenco delle regole di confronto disponibili nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md).    
    
Quando si selezionano le regole di confronto per un server, un database, una colonna o un'espressione, vengono assegnate determinate caratteristiche ai dati. Queste caratteristiche influiscono sui risultati di molte operazioni eseguite nel database. Ad esempio, quando viene costruita una query tramite `ORDER BY`, l'ordinamento del set di risultati può dipendere dalle regole di confronto applicate al database o specificate in una clausola `COLLATE` al livello di espressione della query.    
    
Per usare in modo ottimale il supporto delle regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si dovrebbe approfondire la conoscenza dei termini specificati in questo argomento e di come si correlano alle caratteristiche dei dati.    
    
##  <a name="collation-terms"></a><a name="Terms"></a> Termini delle regole di confronto    
    
-   [Regole di confronto](#Collation_Defn) 
    - [Set di regole di confronto](#Collation_sets)
    - [Livelli delle regole di confronto](#Collation_levels)
-   [Impostazioni locali](#Locale_Defn)    
-   [Tabella codici](#Code_Page_Defn)    
-   [Ordinamento](#Sort_Order_Defn)    
    
###  <a name="collation"></a><a name="Collation_Defn"></a> Confronto    
Le regole di confronto specificano gli schemi di bit che rappresentano i diversi caratteri in un set di dati. Le regole di confronto determinano inoltre le regole in base alle quali i dati vengono ordinati e confrontati. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta l'archiviazione di oggetti con regole di confronto diverse in un singolo database. Per le colonne non Unicode, l'impostazione delle regole di confronto specifica la tabella codici dei dati e i caratteri che possono essere rappresentati. Per i dati spostati tra colonne non Unicode, è necessaria la conversione dalla tabella codici di origine a quella di destinazione.    
    
I risultati di un'istruzione[!INCLUDE[tsql](../../includes/tsql-md.md)] possono variare se l'istruzione viene eseguita nel contesto di database diversi che utilizzano impostazioni diverse per le regole di confronto. Se possibile, usare regole di confronto standardizzate per l'organizzazione. In questo modo, non è necessario specificare le regole di confronto in ogni carattere o espressione Unicode. Se è necessario usare oggetti con impostazioni diverse per tabelle codici e regole di confronto, codificare le query in modo da considerare la precedenza delle regole di confronto. Per altre informazioni, vedere [Precedenza delle regole di confronto (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md).    
    
Alle regole di confronto sono associate le opzioni seguenti: distinzione tra maiuscole e minuscole, distinzione tra caratteri accentati e non accentati, distinzione dei caratteri Kana, distinzione di larghezza e distinzione dei selettori di variazione. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduce un'opzione aggiuntiva per la codifica [UTF-8](https://www.wikipedia.org/wiki/UTF-8). 

È possibile specificare queste opzioni aggiungendole al nome delle regole di confronto. Ad esempio, le regole di confronto **Japanese_Bushu_Kakusu_100_CS_AS_KS_WS_UTF8** prevedono distinzione tra lettere maiuscole e minuscole, distinzione tra caratteri accentati e non accentati, distinzione dei caratteri Kana, distinzione di larghezza e codifica UTF-8. Sempre a titolo di esempio, le regole di confronto **Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS** prevedono le opzioni seguenti: nessuna distinzione tra lettere maiuscole e minuscole, nessuna distinzione tra caratteri accentati e non accentati, distinzione dei caratteri Kana, distinzione di larghezza, distinzione dei selettori di variazione e usano la codifica non Unicode. 

Il comportamento associato a queste opzioni è descritto nella tabella seguente:    
    
|Opzione|Descrizione|    
|------------|-----------------|    
|Distinzione maiuscole/minuscole (\_CS)|Opera una distinzione tra lettere maiuscole e minuscole. Se questa opzione è selezionata, le lettere minuscole precedono le versioni maiuscole corrispondenti nell'ordinamento. Se questa opzione non è selezionata, le regole di confronto non fanno distinzione tra maiuscole e minuscole. Ovvero, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera identiche le lettere maiuscole e minuscole ai fini dell'ordinamento. È possibile selezionare in modo esplicito l'esclusione della distinzione tra maiuscole e minuscole specificando \_CI.|   
|Distinzione caratteri accentati/non accentati (\_AS)|Opera una distinzione tra caratteri accentati e non accentati. Il carattere "a", ad esempio, non viene considerato uguale ad "ấ". Se questa opzione non è selezionata, le regole di confronto non fanno distinzione tra caratteri accentati e non accentati. Ovvero [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera identici i caratteri accentati e non accentati ai fini dell'ordinamento. È possibile selezionare in modo esplicito l'esclusione della distinzione tra caratteri accentati e non accentati specificando \_AI.|    
|Distinzione Kana (\_KS)|Opera una distinzione tra i due tipi di caratteri Kana giapponesi, ovvero Hiragana e Katakana. Se questa opzione non è selezionata, le regole di confronto non fanno distinzione tra caratteri Kana. Ovvero [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera uguali i caratteri Hiragana e Katakana ai fini dell'ordinamento. Omettere questa opzione è il solo metodo per specificare di non effettuare la distinzione Kana.|   
|Distinzione larghezza (\_WS)|Opera una distinzione tra caratteri a larghezza intera e caratteri a metà larghezza. Se questa opzione non è selezionata, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera identiche le rappresentazioni con caratteri a larghezza intera e a metà larghezza dello stesso carattere ai fini dell'ordinamento. Omettere questa opzione è il solo metodo per specificare di non effettuare la distinzione larghezza.|  
|Distinzione dei selettori di variazione (\_VSS)|Opera una distinzione tra vari selettori di variazione di simboli ideografici nelle regole di confronto giapponesi **Japanese_Bushu_Kakusu_140** e **Japanese_XJIS_140** introdotte in [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]. Una sequenza di variazione è costituita da un carattere di base e da un selettore di variazione aggiuntivo. Se questa opzione \_VSS non è selezionata, le regole di confronto non fanno distinzione tra selettori di variazione e il selettore di variazione non viene considerato nel confronto. Ciò significa che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera identici, ai fini dell'ordinamento, i caratteri basati sullo stesso carattere di base con selettori di variazione diversi. Per altre informazioni, vedere il [database delle variazioni dei simboli ideografici Unicode](https://www.unicode.org/reports/tr37/).<br/><br/> Le regole di confronto con distinzione tra selettori di variazione (\_VSS) non sono supportate negli indici di ricerca full-text. Questi indici supportano solo le opzioni Distinzione caratteri accentati/non accentati (\_AS), Distinzione Kana (\_KS) e Distinzione larghezza (\_WS). I motori CLR e XML di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supportano i selettori di variazione (\_VSS).|      
|Binario (\_BIN)<sup>1</sup>|Ordina e confronta i dati nelle tabelle di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in base agli schemi di bit definiti per ogni carattere. L'ordinamento binario prevede la distinzione tra lettere maiuscole e minuscole e tra caratteri accentati e non accentati e rappresenta inoltre il tipo di ordinamento più rapido. Per altre informazioni, vedere la sezione [Regole di confronto binarie](#Binary-collations) in questo articolo.|      
|Punto di codice binario (\_BIN2)<sup>1</sup> | Ordina e confronta i dati nelle tabelle di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in base ai punti di codice Unicode per i dati Unicode. Per i dati non Unicode, il punto di codice binario usa confronti identici a quelli per gli ordinamenti binari.<br/><br/> Il vantaggio di usare un ordinamento con punto di codice binario è rappresentato dal fatto che non è necessario alcun riordinamento dei dati nelle applicazioni che confrontano i dati ordinati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Di conseguenza, l'ordinamento con punto di codice binario consente di semplificare lo sviluppo delle applicazioni e di ottenere un possibile miglioramento delle prestazioni. Per altre informazioni, vedere la sezione [Regole di confronto binarie](#Binary-collations) in questo articolo.|
|UTF-8 (\_UTF8)|Abilita l'archiviazione dei dati con codifica UTF-8 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se questa opzione non è selezionata, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa il formato di codifica non Unicode predefinito per i tipi di dati applicabili. Per altre informazioni, vedere la sezione [Supporto di UTF-8](#utf8) in questo articolo.| 

<sup>1</sup> Se si seleziona Binario o Punto di codice binario, le opzioni Distinzione maiuscole/minuscole (\_CS), Distinzione caratteri accentati/non accentati (\_AS), Distinzione Kana (\_KS) e Distinzione larghezza (\_\WS) non sono disponibili.      

#### <a name="examples-of-collation-options"></a>Esempi di opzioni per le regole di confronto
Le regole di confronto sono costituite da una serie di suffissi che definiscono la distinzione tra lettere maiuscole e minuscole, tra caratteri accentati e non accentati, della larghezza o dei caratteri Kana. Gli esempi seguenti descrivono i tipi di ordinamento delle varie combinazioni di suffissi.

|Suffisso delle regole di confronto di Windows|Descrizione dell'ordinamento|
|------------|-----------------| 
|\_BIN<sup>1</sup>|Ordinamento binario|
|\_BIN2<sup>1, 2</sup>|Ordinamento con punto di codice binario|
|\_CI_AI<sup>2</sup>|Senza distinzione tra lettere maiuscole e minuscole, senza distinzione tra caratteri accentati e non accentati, senza distinzione dei caratteri Kana, senza distinzione di larghezza|
|\_CI_AI_KS<sup>2</sup>|Senza distinzione tra lettere maiuscole e minuscole, senza distinzione tra caratteri accentati e non accentati, con distinzione dei caratteri Kana, senza distinzione di larghezza|
|\_CI_AI_KS_WS<sup>2</sup>|Senza distinzione tra lettere maiuscole e minuscole, senza distinzione tra caratteri accentati e non accentati, con distinzione dei caratteri Kana, con distinzione di larghezza|
|\_CI_AI_WS<sup>2</sup>|Senza distinzione tra lettere maiuscole e minuscole, senza distinzione tra caratteri accentati e non accentati, senza distinzione dei caratteri Kana, con distinzione di larghezza|
|\_CI_AS<sup>2</sup>|Senza distinzione tra lettere maiuscole e minuscole, con distinzione tra caratteri accentati e non accentati, senza distinzione dei caratteri Kana, senza distinzione di larghezza|
|\_CI_AS_KS<sup>2</sup>|Senza distinzione tra lettere maiuscole e minuscole, con distinzione tra caratteri accentati e non accentati, con distinzione dei caratteri Kana, senza distinzione di larghezza|
|\_CI_AS_KS_WS<sup>2</sup>|Senza distinzione tra lettere maiuscole e minuscole, con distinzione tra caratteri accentati e non accentati, con distinzione dei caratteri Kana, con distinzione di larghezza|
|\_CI_AS_WS<sup>2</sup>|Senza distinzione tra lettere maiuscole e minuscole, con distinzione tra caratteri accentati e non accentati, senza distinzione dei caratteri Kana, con distinzione di larghezza|
|\_CS_AI<sup>2</sup>|Con distinzione tra lettere maiuscole e minuscole, senza distinzione tra caratteri accentati e non accentati, senza distinzione dei caratteri Kana, senza distinzione di larghezza|
|\_CS_AI_KS<sup>2</sup>|Con distinzione tra lettere maiuscole e minuscole, senza distinzione tra caratteri accentati e non accentati, con distinzione dei caratteri Kana, senza distinzione di larghezza|
|\_CS_AI_KS_WS<sup>2</sup>|Con distinzione tra lettere maiuscole e minuscole, senza distinzione tra caratteri accentati e non accentati, con distinzione dei caratteri Kana, con distinzione di larghezza|
|\_CS_AI_WS<sup>2</sup>|Con distinzione tra lettere maiuscole e minuscole, senza distinzione tra caratteri accentati e non accentati, senza distinzione dei caratteri Kana, con distinzione di larghezza|
|\_CS_AS<sup>2</sup>|Con distinzione tra lettere maiuscole e minuscole, con distinzione tra caratteri accentati e non accentati, senza distinzione dei caratteri Kana, senza distinzione di larghezza|
|\_CS_AS_KS<sup>2</sup>|Con distinzione tra lettere maiuscole e minuscole, con distinzione tra caratteri accentati e non accentati, con distinzione dei caratteri Kana, senza distinzione di larghezza|
|\_CS_AS_KS_WS<sup>2</sup>|Con distinzione tra lettere maiuscole e minuscole, con distinzione tra caratteri accentati e non accentati, con distinzione dei caratteri Kana, con distinzione di larghezza|
|\_CS_AS_WS<sup>2</sup>|Con distinzione tra lettere maiuscole e minuscole, con distinzione tra caratteri accentati e non accentati, senza distinzione dei caratteri Kana, con distinzione di larghezza|

<sup>1</sup> Se si seleziona Binario o Punto di codice binario, le opzioni Distinzione maiuscole/minuscole (\_CS), Distinzione caratteri accentati/non accentati (\_AS), Distinzione Kana (\_KS) e Distinzione larghezza (\_WS) non sono disponibili.    

<sup>2</sup> L'aggiunta dell'opzione UTF-8 (\_UTF8) consente la codifica dei dati Unicode con UTF-8. Per altre informazioni, vedere la sezione [Supporto di UTF-8](#utf8) in questo articolo. 

### <a name="collation-sets"></a><a name="Collation_sets"></a> Set di regole di confronto

In[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono supportati i seguenti set di regole di confronto:    

-  [Regole di confronto di Windows](#Windows-collations)
-  [Regole di confronto binarie](#Binary-collations)
-  [Regole di confronto di SQL Server](#SQL-collations)
    
#### <a name="windows-collations"></a><a name="Windows-collations"></a> Regole di confronto di Windows    
Le regole di confronto di Windows definiscono regole per l'archiviazione dei dati di tipo carattere basate sulle impostazioni locali del sistema Windows associate. Per le regole di confronto di Windows, è possibile implementare un confronto dei dati non Unicode mediante lo stesso algoritmo dei dati Unicode. Le regole di confronto di base di Windows specificano l'alfabeto o la lingua da usare quando viene applicato l'ordinamento del dizionario. Le regole specificano anche la tabella codici usata per l'archiviazione di dati di tipo carattere non Unicode. Sia l'ordinamento Unicode che quello non Unicode sono compatibili con i confronti di stringhe di una versione specifica di Windows. In questo modo viene garantita la coerenza tra i tipi di dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e gli sviluppatori possono ordinare le stringhe all'interno delle applicazioni mediante le stesse regole usate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Windows_collation_name (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md).    
    
#### <a name="binary-collations"></a><a name="Binary-collations"></a> Regole di confronto binarie    
Le regole di confronto binarie ordinano i dati in base alla sequenza di valori codificati definiti dalle impostazioni locali e dal tipo di dati. Supportano la distinzione tra maiuscole e minuscole. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le regole di confronto binarie definiscono le impostazioni locali e la tabella codici ANSI usata. In questo modo viene applicato un ordinamento binario. Grazie alla loro semplicità, le regole di confronto binarie consentono di migliorare le prestazioni dell'applicazione. Per i tipi di dati non Unicode, il confronto dei dati è basato sui punti di codice definiti nella tabella codici ANSI. Per i tipi di dati Unicode, il confronto dei dati è basato sui punti di codice Unicode. Per le regole di confronto binarie nei tipi di dati Unicode, le impostazioni locali non vengono considerate ai fini dell'ordinamento dei dati. Ad esempio, l'uso di **Latin_1_General_BIN** e **Japanese_BIN** su dati Unicode restituisce risultati di ordinamento identici. Per altre informazioni, vedere [Windows_collation_name (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md).   
    
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ci sono due tipi di regole di confronto binarie:

-  Le regole di confronto **BIN** precedenti, che eseguono un confronto tra punti di codice incompleto per i dati Unicode. Tali regole di confronto binarie legacy eseguono il confronto del primo carattere come WCHAR e quindi un confronto byte per byte. Nelle regole di confronto BIN solo il primo carattere viene ordinato in base al punto di codice e i restanti caratteri vengono ordinati in base ai relativi valori di byte.

-  Le regole di confronto **BIN2** più recenti, che implementano un confronto tra punti di codice puro. Nelle regole di confronto BIN2 tutti i caratteri vengono ordinati in base ai relativi punti di codice. Poiché la piattaforma Intel ha un'architettura little endian, i caratteri di codice Unicode vengono sempre archiviati con i byte scambiati.     
    
#### <a name="sql-server-collations"></a><a name="SQL-collations"></a> Regole di confronto di SQL Server    
Le regole di confronto[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQL_\*) garantiscono la compatibilità di ordinamento con le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le regole di ordinamento del dizionario per i dati non Unicode sono incompatibili con qualsiasi routine di ordinamento fornita dai sistemi operativi Windows. Tuttavia l'ordinamento dei dati Unicode è compatibile con una versione specifica di regole di ordinamento di Windows. Dato che le regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] differiscono per i dati non Unicode e i dati Unicode, confrontando gli stessi dati vengono visualizzati risultati diversi, a seconda del tipo di dati sottostante. Per altre informazioni, vedere [Nome delle regole di confronto di SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md). 

Durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'impostazione di installazione predefinita per le regole di confronto è determinata dalle impostazioni locali del sistema operativo. È possibile modificare le regole di confronto a livello di server durante l'installazione oppure modificando le impostazioni locali del sistema operativo prima dell'installazione. Per motivi di compatibilità con le versioni precedenti, le regole di confronto predefinite sono impostate sulla versione disponibile meno recente associata a impostazioni locali specifiche. Non sempre, quindi, si tratta delle regole di confronto consigliate. Per sfruttare tutti i vantaggi delle funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], modificare le impostazioni di installazione predefinite per usare le regole di confronto di Windows. Ad esempio, per le impostazioni locali del sistema operativo "Inglese (Stati Uniti)" (tabella codici 1252), le regole di confronto predefinite durante l'installazione sono impostate su **SQL_Latin1_General_CP1_CI_AS** ed è possibile modificare l'impostazione scegliendo la controparte di Windows più simile, **Latin1_General_100_CI_AS_SC**.
    
> [!NOTE]    
> Quando si aggiorna un'istanza in lingua inglese di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile specificare le regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQL_\*) per assicurare la compatibilità con le istanze esistenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Poiché le regole di confronto predefinite per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono definite durante la configurazione, è importante specificare attentamente le impostazioni delle regole di confronto quando le condizioni riportate di seguito sono vere:    
>     
> -   Il codice dell'applicazione dipende dal comportamento di regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedenti.    
> -   È necessario archiviare dati di tipo carattere in più lingue.    
    
### <a name="collation-levels"></a><a name="Collation_levels"></a> Livelli delle regole di confronto
L'impostazione delle regole di confronto è supportata ai seguenti livelli di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:    

-  [Regole di confronto a livello di server](#Server-level-collations)
-  [Regole di confronto a livello di database](#Database-level-collations)
-  [Regole di confronto a livello di colonna](#Column-level-collations)
-  [Regole di confronto a livello di espressione](#Expression-level-collations)

#### <a name="server-level-collations"></a><a name="Server-level-collations"></a> Regole di confronto a livello di server   
Le regole di confronto del server predefinite vengono determinate durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e diventano le regole predefinite per i database di sistema e per tutti i database utente. 

La tabella seguente illustra le designazioni delle regole di confronto predefinite determinate dalle impostazioni locali del sistema operativo, inclusi i relativi identificatori delle impostazioni locali (LCID) di Windows e SQL:

|Impostazioni locali di Windows|Identificatore delle impostazioni locali (LCID) di Windows|LCID SQL|Regole di confronto predefinite|
|---------------|---------|---------|---------------|
|Afrikaans (Sud Africa)|0x0436|0x0409|Latin1_General_CI_AS|
|Albanese (Albania)|0x041c|0x041c|Albanian_CI_AS|
|Alsaziano (Francia)|0x0484|0x0409|Latin1_General_CI_AS|
|Amarico (Etiopia)|0x045e|0x0409|Latin1_General_CI_AS|
|Arabo (Algeria)|0x1401|0x0401|Arabic_CI_AS|
|Arabo (Bahrein)|0x3c01|0x0401|Arabic_CI_AS|
|Arabo (Egitto)|0x0c01|0x0401|Arabic_CI_AS|
|Arabo (Iraq)|0x0801|0x0401|Arabic_CI_AS|
|Arabo (Giordania)|0x2c01|0x0401|Arabic_CI_AS|
|Arabo (Kuwait)|0x3401|0x0401|Arabic_CI_AS|
|Arabo (Libano)|0x3001|0x0401|Arabic_CI_AS|
|Arabo (Libia)|0x1001|0x0401|Arabic_CI_AS|
|Arabo (Marocco)|0x1801|0x0401|Arabic_CI_AS|
|Arabo (Oman)|0x2001|0x0401|Arabic_CI_AS|
|Arabo (Qatar)|0x4001|0x0401|Arabic_CI_AS|
|Arabo (Arabia Saudita)|0x0401|0x0401|Arabic_CI_AS|
|Arabo (Siria)|0x2801|0x0401|Arabic_CI_AS|
|Arabo (Tunisia)|0x1c01|0x0401|Arabic_CI_AS|
|Arabo (Emirati Arabi Uniti)|0x3801|0x0401|Arabic_CI_AS|
|Arabo (Yemen)|0x2401|0x0401|Arabic_CI_AS|
|Armeno (Armenia)|0x042b|0x0419|Latin1_General_CI_AS|
|Assamese (India)|0x044d|0x044d|Non disponibile a livello di server|
|Azerbaigiano (Azerbaigian, alfabeto cirillico)|0x082c|0x082c|Deprecato, non disponibile a livello di server|
|Azerbaigiano (Azerbaigian, alfabeto latino)|0x042c|0x042c|Deprecato, non disponibile a livello di server|
|Baschiro (Russia)|0x046d|0x046d|Latin1_General_CI_AI|
|Basco (Basco)|0x042d|0x0409|Latin1_General_CI_AS|
|Bielorusso (Bielorussia)|0x0423|0x0419|Cyrillic_General_CI_AS|
|Bengalese (Bangladesh)|0x0845|0x0445|Non disponibile a livello di server|
|Bengalese (India)|0x0445|0x0439|Non disponibile a livello di server|
|Bosniaco (Bosnia ed Erzegovina, alfabeto cirillico)|0x201a|0x201a|Latin1_General_CI_AI|
|Bosniaco (Bosnia ed Erzegovina, alfabeto latino)|0x141a|0x141a|Latin1_General_CI_AI|
|Bretone (Francia)|0x047e|0x047e|Latin1_General_CI_AI|
|Bulgaro (Bulgaria)|0x0402|0x0419|Cyrillic_General_CI_AS|
|Catalano (Catalogna)|0x0403|0x0409|Latin1_General_CI_AS|
|Cinese (Hong Kong - R.A.S., Repubblica popolare cinese)|0x0c04|0x0404|Chinese_Taiwan_Stroke_CI_AS|
|Cinese (RAS di Macao)|0x1404|0x1404|Latin1_General_CI_AI|
|Cinese (Macao)|0x21404|0x21404|Latin1_General_CI_AI|
|Cinese (RPC)|0x0804|0x0804|Chinese_PRC_CI_AS|
|Cinese (RPC)|0x20804|0x20804|Chinese_PRC_Stroke_CI_AS|
|Cinese (Singapore)|0x1004|0x0804|Chinese_PRC_CI_AS|
|Cinese (Singapore)|0x21004|0x20804|Chinese_PRC_Stroke_CI_AS|
|Cinese (Taiwan)|0x30404|0x30404|Chinese_Taiwan_Bopomofo_CI_AS|
|Cinese (Taiwan)|0x0404|0x0404|Chinese_Taiwan_Stroke_CI_AS|
|Corso (Francia)|0x0483|0x0483|Latin1_General_CI_AI|
|Croato (Bosnia ed Erzegovina, alfabeto latino)|0x101a|0x041a|Croatian_CI_AS|
|Croato (Croazia)|0x041a|0x041a|Croatian_CI_AS|
|Ceco (Repubblica Ceca)|0x0405|0x0405|Czech_CI_AS|
|Danese (Danimarca)|0x0406|0x0406|Danish_Norwegian_CI_AS|
|Dari (Afghanistan)|0x048c|0x048c|Latin1_General_CI_AI|
|Divehi (Maldive)|0x0465|0x0465|Non disponibile a livello di server|
|Olandese (Belgio)|0x0813|0x0409|Latin1_General_CI_AS|
|Olandese (Paesi Bassi)|0x0413|0x0409|Latin1_General_CI_AS|
|Inglese (Australia)|0x0c09|0x0409|Latin1_General_CI_AS|
|Inglese (Belize)|0x2809|0x0409|Latin1_General_CI_AS|
|Inglese (Canada)|0x1009|0x0409|Latin1_General_CI_AS|
|Inglese (Caraibi)|0x2409|0x0409|Latin1_General_CI_AS|
|Inglese (India)|0x4009|0x0409|Latin1_General_CI_AS|
|Inglese (Irlanda)|0x1809|0x0409|Latin1_General_CI_AS|
|Inglese (Giamaica)|0x2009|0x0409|Latin1_General_CI_AS|
|Inglese (Malesia)|0x4409|0x0409|Latin1_General_CI_AS|
|Inglese (Nuova Zelanda)|0x1409|0x0409|Latin1_General_CI_AS|
|Inglese (Filippine)|0x3409|0x0409|Latin1_General_CI_AS|
|Inglese (Singapore)|0x4809|0x0409|Latin1_General_CI_AS|
|Inglese (Sud Africa)|0x1c09|0x0409|Latin1_General_CI_AS|
|Inglese (Trinidad e Tobago)|0x2c09|0x0409|Latin1_General_CI_AS|
|Inglese (Regno Unito)|0x0809|0x0409|Latin1_General_CI_AS|
|Inglese (Stati Uniti)|0x0409|0x0409|SQL_Latin1_General_CP1_CI_AS|
|Inglese (Zimbabwe)|0x3009|0x0409|Latin1_General_CI_AS|
|Estone (Estonia)|0x0425|0x0425|Estonian_CI_AS|
|Faroese (Fær Øer)|0x0438|0x0409|Latin1_General_CI_AS|
|Filippino (Filippine)|0x0464|0x0409|Latin1_General_CI_AS|
|Finlandese (Finlandia)|0x040b|0x040b|Finnish_Swedish_CI_AS|
|Francese (Belgio)|0x080c|0x040c|French_CI_AS|
|Francese (Canada)|0x0c0c|0x040c|French_CI_AS|
|Francese (Francia)|0x040c|0x040c|French_CI_AS|
|Francese (Lussemburgo)|0x140c|0x040c|French_CI_AS|
|Francese (Monaco)|0x180c|0x040c|French_CI_AS|
|Francese (Svizzera)|0x100c|0x040c|French_CI_AS|
|Frisone (Paesi Bassi)|0x0462|0x0462|Latin1_General_CI_AI|
|Galiziano|0x0456|0x0409|Latin1_General_CI_AS|
|Georgiano (Georgia)|0x10437|0x10437|Georgian_Modern_Sort_CI_AS|
|Georgiano (Georgia)|0x0437|0x0419|Latin1_General_CI_AS|
|Tedesco - ordinamento alfabetico telefonico (DIN)|0x10407|0x10407|German_PhoneBook_CI_AS|
|Tedesco (Austria)|0x0c07|0x0409|Latin1_General_CI_AS|
|Tedesco (Germania)|0x0407|0x0409|Latin1_General_CI_AS|
|Tedesco (Liechtenstein)|0x1407|0x0409|Latin1_General_CI_AS|
|Tedesco (Lussemburgo)|0x1007|0x0409|Latin1_General_CI_AS|
|Tedesco (Svizzera)|0x0807|0x0409|Latin1_General_CI_AS|
|Greco (Grecia)|0x0408|0x0408|Greek_CI_AS|
|Groenlandese (Groenlandia)|0x046f|0x0406|Danish_Norwegian_CI_AS|
|Gujarati (India)|0x0447|0x0439|Non disponibile a livello di server|
|Hausa (Nigeria, alfabeto latino)|0x0468|0x0409|Latin1_General_CI_AS|
|Ebraico (Israele)|0x040d|0x040d|Hebrew_CI_AS|
|Hindi (India)|0x0439|0x0439|Non disponibile a livello di server|
|Ungherese (Ungheria)|0x040e|0x040e|Hungarian_CI_AS|
|Ungherese (ordinamento tecnico)|0x1040e|0x1040e|Hungarian_Technical_CI_AS|
|Islandese (Islanda)|0x040f|0x040f|Icelandic_CI_AS|
|Igbo (Nigeria)|0x0470|0x0409|Latin1_General_CI_AS|
|Indonesiano (Indonesia)|0x0421|0x0409|Latin1_General_CI_AS|
|Inuktitut (Canada, alfabeto latino)|0x085d|0x0409|Latin1_General_CI_AS|
|Inuktitut (alfabeto sillabico) Canada|0x045d|0x045d|Latin1_General_CI_AI|
|Irlandese (Irlanda)|0x083c|0x0409|Latin1_General_CI_AS|
|Italiano (Italia)|0x0410|0x0409|Latin1_General_CI_AS|
|Italiano (Svizzera)|0x0810|0x0409|Latin1_General_CI_AS|
|Giapponese (Giappone XJIS)|0x0411|0x0411|Japanese_CI_AS|
|Giapponese (Giappone)|0x040411|0x40411|Latin1_General_CI_AI|
|Kannada (India)|0x044b|0x0439|Non disponibile a livello di server|
|Kazako (Kazakhstan)|0x043f|0x043f|Kazakh_90_CI_AS|
|Khmer (Cambogia)|0x0453|0x0453|Non disponibile a livello di server|
|Quiché (Guatemala)|0x0486|0x0c0a|Modern_Spanish_CI_AS|
|Kinyarwanda (Ruanda)|0x0487|0x0409|Latin1_General_CI_AS|
|Konkani (India)|0x0457|0x0439|Non disponibile a livello di server|
|Coreano (ordinamento dizionario coreano)|0x0412|0x0412|Korean_Wansung_CI_AS|
|Kirghiso (Kirghizistan)|0x0440|0x0419|Cyrillic_General_CI_AS|
|Lao (Repubblica popolare democratica del Laos)|0x0454|0x0454|Non disponibile a livello di server|
|Lettone (Lettonia)|0x0426|0x0426|Latvian_CI_AS|
|Lituano (Lituania)|0x0427|0x0427|Lithuanian_CI_AS|
|Basso sorabo (Germania)|0x082e|0x0409|Latin1_General_CI_AS|
|Lussemburghese (Lussemburgo)|0x046e|0x0409|Latin1_General_CI_AS|
|Macedone (Macedonia del Nord, Ex Rep. Jugoslava di Macedonia)|0x042f|0x042f|Macedonian_FYROM_90_CI_AS|
|Malese (Brunei Darussalam)|0x083e|0x0409|Latin1_General_CI_AS|
|Malese (Malaysia)|0x043e|0x0409|Latin1_General_CI_AS|
|Malayalam (India)|0x044c|0x0439|Non disponibile a livello di server|
|Maltese (Malta)|0x043a|0x043a|Latin1_General_CI_AI|
|Maori (Nuova Zelanda)|0x0481|0x0481|Latin1_General_CI_AI|
|Mapudungun (Cile)|0x047a|0x047a|Latin1_General_CI_AI|
|Marathi (India)|0x044e|0x0439|Non disponibile a livello di server|
|Mohawk (Canada)|0x047c|0x047c|Latin1_General_CI_AI|
|Mongolo (Mongolia)|0x0450|0x0419|Cyrillic_General_CI_AS|
|Mongolo (RPC)|0x0850|0x0419|Cyrillic_General_CI_AS|
|Nepalese (Nepal)|0x0461|0x0461|Non disponibile a livello di server|
|Norvegese (Bokmål, Norvegia)|0x0414|0x0414|Latin1_General_CI_AI|
|Norvegese (Nynorsk, Norvegia)|0x0814|0x0414|Latin1_General_CI_AI|
|Occitano (Francia)|0x0482|0x040c|French_CI_AS|
|Odia (India)|0x0448|0x0439|Non disponibile a livello di server|
|Pashto (Afghanistan)|0x0463|0x0463|Non disponibile a livello di server|
|Persiano (Iran)|0x0429|0x0429|Latin1_General_CI_AI|
|Polacco (Polonia)|0x0415|0x0415|Polish_CI_AS|
|Portoghese (Brasile)|0x0416|0x0409|Latin1_General_CI_AS|
|Portoghese (Portogallo)|0x0816|0x0409|Latin1_General_CI_AS|
|Punjabi (India)|0x0446|0x0439|Non disponibile a livello di server|
|Quechua (Bolivia)|0x046b|0x0409|Latin1_General_CI_AS|
|Quechua (Ecuador)|0x086b|0x0409|Latin1_General_CI_AS|
|Quechua (Perù)|0x0c6b|0x0409|Latin1_General_CI_AS|
|Romeno (Romania)|0x0418|0x0418|Romanian_CI_AS|
|Romancio (Svizzera)|0x0417|0x0417|Latin1_General_CI_AI|
|Russo (Russia)|0x0419|0x0419|Cyrillic_General_CI_AS|
|Sacha (Russia)|0x0485|0x0485|Latin1_General_CI_AI|
|Sami (Inari, Finlandia)|0x243b|0x083b|Latin1_General_CI_AI|
|Sami (Lule, Norvegia)|0x103b|0x043b|Latin1_General_CI_AI|
|Sami (Lule, Svezia)|0x143b|0x083b|Latin1_General_CI_AI|
|Sami (settentrionale, Finlandia)|0x0c3b|0x083b|Latin1_General_CI_AI|
|Sami (settentrionale, Norvegia)|0x043b|0x043b|Latin1_General_CI_AI|
|Sami (settentrionale, Svezia)|0x083b|0x083b|Latin1_General_CI_AI|
|Sami (Skolt, Finlandia)|0x203b|0x083b|Latin1_General_CI_AI|
|Sami (meridionale, Norvegia)|0x183b|0x043b|Latin1_General_CI_AI|
|Sami (meridionale, Svezia)|0x1c3b|0x083b|Latin1_General_CI_AI|
|Sanscrito (India)|0x044f|0x0439|Non disponibile a livello di server|
|Serbo (Bosnia ed Erzegovina, alfabeto cirillico)|0x1c1a|0x0c1a|Latin1_General_CI_AI|
|Serbo (Bosnia ed Erzegovina, alfabeto latino)|0x181a|0x081a|Latin1_General_CI_AI|
|Serbo (Serbia, alfabeto cirillico)|0x0c1a|0x0c1a|Latin1_General_CI_AI|
|Serbo (Serbia, alfabeto latino)|0x081a|0x081a|Latin1_General_CI_AI|
|Sotho del nord/Sotho settentrionale (Sudafrica)|0x046c|0x0409|Latin1_General_CI_AS|
|SeTswana/Tswana (Sudafrica)|0x0432|0x0409|Latin1_General_CI_AS|
|Singalese (Sri Lanka)|0x045b|0x0439|Non disponibile a livello di server|
|Slovacco (Slovacchia)|0x041b|0x041b|Slovak_CI_AS|
|Sloveno (Slovenia)|0x0424|0x0424|Slovenian_CI_AS|
|Spagnolo (Argentina)|0x2c0a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Bolivia)|0x400a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Cile)|0x340a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Colombia)|0x240a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Costa Rica)|0x140a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Repubblica Dominicana)|0x1c0a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Ecuador)|0x300a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (El Salvador)|0x440a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Guatemala)|0x100a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Honduras)|0x480a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Messico)|0x080a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Nicaragua)|0x4c0a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Panama)|0x180a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Paraguay)|0x3c0a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Perù)|0x280a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Porto Rico)|0x500a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Spagna)|0x0c0a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Spagna, ordinamento tradizionale)|0x040a|0x040a|Traditional_Spanish_CI_AS|
|Spagnolo (Stati Uniti)|0x540a|0x0409|Latin1_General_CI_AS|
|Spagnolo (Uruguay)|0x380a|0x0c0a|Modern_Spanish_CI_AS|
|Spagnolo (Venezuela)|0x200a|0x0c0a|Modern_Spanish_CI_AS|
|Swahili (Kenya)|0x0441|0x0409|Latin1_General_CI_AS|
|Svedese (Finlandia)|0x081d|0x040b|Finnish_Swedish_CI_AS|
|Svedese (Svezia)|0x041d|0x040b|Finnish_Swedish_CI_AS|
|Siriano (Siria)|0x045a|0x045a|Non disponibile a livello di server|
|Tagico (Tajikistan)|0x0428|0x0419|Cyrillic_General_CI_AS|
|Tamazight (Algeria, alfabeto latino)|0x085f|0x085f|Latin1_General_CI_AI|
|Tamil (India)|0x0449|0x0439|Non disponibile a livello di server|
|Tartaro (Russia)|0x0444|0x0444|Cyrillic_General_CI_AS|
|Telugu (India)|0x044a|0x0439|Non disponibile a livello di server|
|Thai (Thailandia)|0x041e|0x041e|Thai_CI_AS|
|Tibetano (RPC)|0x0451|0x0451|Non disponibile a livello di server|
|Turco (Turchia)|0x041f|0x041f|Turkish_CI_AS|
|Turcomanno (Turkmenistan)|0x0442|0x0442|Latin1_General_CI_AI|
|Uiguro (RPC)|0x0480|0x0480|Latin1_General_CI_AI|
|Ucraino (Ucraina)|0x0422|0x0422|Ukrainian_CI_AS|
|Alto sorabo (Germania)|0x042e|0x042e|Latin1_General_CI_AI|
|Urdu (Pakistan)|0x0420|0x0420|Latin1_General_CI_AI|
|Uzbeco (Uzbekistan, alfabeto cirillico)|0x0843|0x0419|Cyrillic_General_CI_AS|
|Uzbeco (Uzbekistan, alfabeto latino)|0x0443|0x0443|Uzbek_Latin_90_CI_AS|
|Vietnamita (Vietnam)|0x042a|0x042a|Vietnamese_CI_AS|
|Gallese (Regno Unito)|0x0452|0x0452|Latin1_General_CI_AI|
|Wolof (Senegal)|0x0488|0x040c|French_CI_AS|
|Xhosa/isiXhosa (Sudafrica)|0x0434|0x0409|Latin1_General_CI_AS|
|Yi (RPC)|0x0478|0x0409|Latin1_General_CI_AS|
|Yoruba (Nigeria)|0x046a|0x0409|Latin1_General_CI_AS|
|Zulu/isiZulu (Sudafrica)|0x0435|0x0409|Latin1_General_CI_AS|

Una volta che le regole di confronto sono state assegnate al server, è possibile modificarle solo esportando tutti i dati e gli oggetti di database, ricostruendo il database *master* e importando tutti i dati e gli oggetti di database. Anziché modificare le regole di confronto predefinite di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile specificare quelle desiderate quando si creano nuovi database o colonne di database.    

Per eseguire una query sulle regole di confronto del server per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], usare la funzione `SERVERPROPERTY`:

```sql
SELECT CONVERT(varchar, SERVERPROPERTY('collation'));
```

Per eseguire una query sul server per tutte le regole di confronto disponibili, usare la funzione predefinita `fn_helpcollations()` seguente:

```sql
SELECT * FROM sys.fn_helpcollations();
```
    
#### <a name="database-level-collations"></a><a name="Database-level-collations"></a> Regole di confronto a livello di database    
Quando si crea o si modifica un database, è possibile usare la clausola `COLLATE` dell'istruzione `CREATE DATABASE` o `ALTER DATABASE` per specificare le regole di confronto predefinite del database. Se non viene specificata alcuna regola di confronto, al database vengono assegnate le regole di confronto del server.    
    
Non è possibile modificare le regole di confronto dei database di sistema a meno che non vengano modificate per il server.
    
Le regole di confronto del database vengono usate per tutti i metadati nel database e rappresentano l'impostazione predefinita per tutte le colonne stringa, gli oggetti temporanei, i nomi di variabili e qualsiasi altra stringa usata nel database. Quando le regole di confronto di un database utente vengono modificate, è possibile che si verifichino conflitti tra le regole di confronto quando vengono eseguite query nelle tabelle temporanee di accesso al database. Le tabelle temporanee sono sempre archiviate nel database di sistema *tempdb*, che usa le regole di confronto per l'istanza. L'esecuzione di query di confronto dei dati di tipo carattere tra database utente e *tempdb* potrebbe non riuscire se le regole di confronto causano un conflitto nella valutazione dei dati di tipo carattere. È possibile risolvere il problema specificando la clausola `COLLATE` nella query. Per altre informazioni, vedere [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md).    

> [!NOTE]
> Non è possibile modificare le regole di confronto dopo che il database è stato creato in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

È possibile modificare le regole di confronto di un database utente usando un'istruzione `ALTER DATABASE` simile alla seguente:

```sql
ALTER DATABASE myDB COLLATE Greek_CS_AI;
```

> [!IMPORTANT]
> Le modifiche apportate alle regole di confronto a livello di database non influiscono sulle regole di confronto a livello di colonna o di espressione.

È possibile recuperare le regole di confronto correnti di un database usando un'istruzione simile alla seguente:

```sql
SELECT CONVERT (VARCHAR(50), DATABASEPROPERTYEX('database_name','collation'));
```

#### <a name="column-level-collations"></a><a name="Column-level-collations"></a> Regole di confronto a livello di colonna    
Quando una tabella viene creata o modificata, è possibile specificare regole di confronto per ciascuna colonna di stringhe di caratteri usando la clausola `COLLATE`. Se non si specificano regole di confronto, alla colonna verranno assegnate le regole di confronto predefinite del database.    

È possibile modificare le regole di confronto di una colonna usando un'istruzione `ALTER TABLE` simile alla seguente:

```sql
ALTER TABLE myTable ALTER COLUMN mycol NVARCHAR(10) COLLATE Greek_CS_AI;
```
    
#### <a name="expression-level-collations"></a><a name="Expression-level-collations"></a> Regole di confronto a livello di espressione    
Le regole di confronto a livello di espressione vengono impostate al momento dell'esecuzione di un'istruzione e interessano la modalità di restituzione di un set di risultati. In questo modo i risultati dell'ordinamento di `ORDER BY` possono essere specifici delle impostazioni locali. Per implementare regole di confronto a livello di espressione, usare una clausola `COLLATE` simile alla seguente:    
    
```sql    
SELECT name FROM customer ORDER BY name COLLATE Latin1_General_CS_AI;    
```    
    
###  <a name="locale"></a><a name="Locale_Defn"></a> Impostazioni locali    
Le impostazioni locali rappresentano un set di informazioni associate a un paese o a una lingua. Queste informazioni possono includere il nome e l'identificatore della lingua parlata, l'alfabeto usato per la scrittura della lingua e le convenzioni culturali. Le regole di confronto possono essere associate a uno o più set di impostazioni locali. Per altre informazioni, vedere [ID delle impostazioni locali assegnati da Microsoft](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c).    
    
###  <a name="code-page"></a><a name="Code_Page_Defn"></a> Tabella codici    
Una tabella codici è un set ordinato di caratteri di uno script specifico nel quale a ogni carattere viene associato un indice numerico o un valore punto di codice. Per tabella codici di Windows si intende in genere un *set di caratteri* o *charset*. Queste tabelle vengono usate per supportare i set di caratteri e i layout di tastiera impiegati per le diverse impostazioni locali di Windows.     
 
###  <a name="sort-order"></a><a name="Sort_Order_Defn"></a> Ordinamento    
L'ordinamento specifica il modo in cui vengono ordinati i valori dei dati. L'ordine influisce sui risultati del confronto dei dati stessi. I dati vengono ordinati tramite regole di confronto e possono essere ottimizzati tramite indici.    
    
##  <a name="unicode-support"></a><a name="Unicode_Defn"></a> Supporto Unicode    
Unicode è uno standard per il mapping dei punti di codice ai caratteri. Dato che è progettato per supportare tutti i caratteri di tutte le lingue del mondo, non sono necessarie tabelle codici diverse per gestire set di caratteri diversi.

### <a name="unicode-basics"></a>Nozioni fondamentali su Unicode
L'archiviazione di dati in lingue diverse all'interno di un singolo database presenta difficoltà di gestione se si usano solo tabelle codici e dati di tipo carattere. È anche difficile trovare una singola tabella codici per il database che contenga tutti i caratteri necessari specifici della lingua. È inoltre difficile garantire che i caratteri speciali letti o aggiornati da una varietà di client che eseguono tabelle codici diverse vengano convertiti correttamente. È consigliabile che i database che supportano client internazionali usino sempre tipi di dati Unicode invece di tipi di dati non Unicode.

Si consideri, ad esempio, un database di clienti dell'America del Nord per cui è necessario gestire tre lingue principali:

-  Nomi e indirizzi in spagnolo per il Messico
-  Nomi e indirizzi in francese per il Quebec
-  Nomi e indirizzi in inglese per il resto del Canada e gli Stati Uniti

Se si usano solo tabelle codici e colonne di tipo carattere, è necessario assicurarsi che il database sia installato con una tabella codici in grado di gestire i caratteri delle tre lingue. È anche necessario garantire che i caratteri di una qualsiasi delle lingue vengano convertiti correttamente quando sono letti dai client che eseguono una tabella codici di un'altra lingua.
   
> [!NOTE]
> Le tabelle codici usate da un client vengono determinate in base alle impostazioni del sistema operativo. Per impostare le tabelle codici del client nel sistema operativo Windows usare l'opzione **Impostazioni internazionali** del Pannello di controllo.    

È difficile selezionare una tabella codici per i dati di tipo carattere in grado di supportare tutti i caratteri necessari per gli utenti di tutto il mondo. Il modo più semplice per gestire dati di tipo carattere in database internazionali consiste nell'usare sempre un tipo di dati che supporta Unicode. 

### <a name="unicode-data-types"></a>Tipi di dati Unicode
Se vengono archiviati dati di tipo carattere che riflettono più lingue in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive), usare tipi di dati Unicode (**nchar**, **nvarchar** e **ntext**) anziché tipi di dati non Unicode (**char**, **varchar** e **text**). 

> [!NOTE]
> Per i tipi di dati Unicode, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] può rappresentare un massimo di 65.535 caratteri con UCS-2 o l'intera gamma Unicode (1.114.111 caratteri), se vengono usati caratteri supplementari. Per altre informazioni sull'abilitazione dei caratteri supplementari, vedere [Caratteri supplementari](#Supplementary_Characters).

In alternativa, a partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], se si usano regole di confronto abilitate per UTF-8 (\_UTF8) i tipi di dati in precedenza non Unicode (**char** e **varchar**) diventano tipi di dati Unicode che usano la codifica UTF-8. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] non modifica il comportamento dei tipi di dati Unicode esistenti in precedenza (**nchar**, **nvarchar** e **ntext**), che continuano a usare la codifica UCS-2 o UTF-16. Per altre informazioni, vedere [Differenze nell'archiviazione tra UTF-8 e UTF-16](#storage_differences).

### <a name="unicode-considerations"></a>Considerazioni su Unicode
Ai tipi di dati non Unicode sono associate numerose limitazioni, perché nei computer non Unicode è disponibile una sola tabella codici. Usando Unicode è possibile ottenere prestazioni migliori, in quanto richiede un minor numero di conversioni tramite la tabella codici. Le regole di confronto Unicode devono essere selezionate singolarmente a livello di database, di colonna o di espressione perché non sono supportate a livello di server.    

Quando si spostano dati da un server a un client, le regole di confronto del server potrebbero non essere riconosciute dai driver client meno recenti. Questa situazione può verificarsi quando si spostano dati da un server Unicode a un client non Unicode. La migliore opzione potrebbe consistere nell'aggiornare il sistema operativo client affinché vengano aggiornate le regole di confronto del sistema sottostanti. Se nel client è installato il software client del database, è possibile applicare un aggiornamento dei servizi a tale software.    
    
> [!TIP]
> È anche possibile tentare di usare regole di confronto diverse per i dati nel server. Scegliere regole di confronto con mapping a una tabella codici nel client.    
>
> Per usare le regole di confronto UTF-16 disponibili in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive) e migliorare la ricerca e l'ordinamento di alcuni caratteri Unicode (solo regole di confronto Windows), è possibile selezionare una delle regole di confronto dei caratteri supplementari (\_SC) o una delle regole di confronto versione 140.    
 
Per usare le regole di confronto UTF-8 disponibili in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e migliorare la ricerca e l'ordinamento di alcuni caratteri Unicode (solo regole di confronto Windows), è necessario selezionare regole di confronto abilitate per la codifica UTF-8 (\_UTF8).
 
-   Il flag UTF8 può essere applicato a:    
    -   Regole di confronto linguistiche che supportano già caratteri supplementari (\_SC) o riconoscono la distinzione tra selettori di variazione (\_VSS)
    -   Regole di confronto BIN2<sup>1</sup>
    
-   Il flag UTF8 non può essere applicato a:    
    -   Regole di confronto linguistiche che non supportano caratteri supplementari (\_SC) o non riconoscono la distinzione tra selettori di variazione (\_VSS)
    -   Regole di confronto binarie BIN o BIN2<sup>2</sup>
    -   Regole di confronto di SQL\_*  
    
<sup>1</sup> A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 2.3. In [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 3.0 le regole di confronto **UTF8_BIN2** sono state sostituite con **Latin1_General_100_BIN2_UTF8**.        
<sup>2</sup> Fino a [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 2.3.    
    
Per valutare i problemi relativi all'utilizzo di tipi di dati Unicode o non Unicode, è necessario eseguire il test dello scenario per verificare le differenze di prestazioni nell'ambiente specifico. È consigliabile standardizzare le regole di confronto usate nei sistemi presenti nell'ambito dell'organizzazione e distribuire server e client Unicode laddove possibile.    
    
In molti casi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interagisce con altri server o client e l'organizzazione può usare più standard di accesso ai dati tra applicazioni e istanze del server. I client[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono principalmente di due tipi:    
    
-   **Client Unicode** che usano OLE DB e ODBC (Open Database Connectivity) versione 3.7 o successive.    
-   **Client non Unicode** che usano DB-Library e ODBC versione 3.6 o precedenti.    
    
Nella tabella seguente sono riportate informazioni sull'uso di dati multilingue con diverse combinazioni di server Unicode e non Unicode:    
    
|Server|Client|Vantaggi o limitazioni|    
|------------|------------|-----------------------------|    
|Unicode|Unicode|Visto che i dati Unicode vengono usati in tutto il sistema, questo scenario offre prestazioni ottimali e la migliore protezione da eventuali danni ai dati recuperati. Si applica ad ADO (ActiveX Data Objects), OLE DB e ODBC versione 3.7 o successive.|    
|Unicode|Non Unicode|In questo scenario, in particolare con le connessioni tra un server che esegue un sistema operativo più recente e un client in cui è in esecuzione una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un sistema operativo meno recente, possono riscontrarsi limitazioni o errori quando si spostano i dati in un computer client. Per convertire i dati si tenta il mapping tra i dati Unicode presenti nel server e una tabella codici corrispondente nel client non Unicode.|    
|Non Unicode|Unicode|Non si tratta di una configurazione ideale per l'uso di dati multilingue. Non è possibile scrivere dati Unicode nel server non Unicode e potrebbero verificarsi problemi all'invio dei dati ai server che non rientrano nella tabella codici del server.|    
|Non Unicode|Non Unicode|Si tratta di uno scenario che presenta numerose limitazioni per i dati multilingue. È possibile usare solo una tabella codici.|    
    
##  <a name="supplementary-characters"></a><a name="Supplementary_Characters"></a> Caratteri supplementari    
L'Unicode Consortium assegna a ogni carattere un punto di codice univoco, che corrisponde a un valore nell'intervallo 000000-10FFFF. I caratteri usati più di frequente hanno valori dei punti di codice nell'intervallo 000000-00FFFF (65.535 caratteri), compresi in una parola a 8 bit o a 16 bit in memoria e su disco. Questo intervallo viene in genere designato come Basic Multilingual Plane (BMP). 

L'Unicode Consortium ha tuttavia stabilito 16 "piani" aggiuntivi di caratteri, ognuno con le stesse dimensioni di BMP. Questa definizione offre a Unicode la possibilità di rappresentare 1.114.112 caratteri (ovvero, 2<sup>16</sup> * 17 caratteri) nell'intervallo di punti di codice 000000-10FFFF. I caratteri con valori di punti di codice maggiori di 00FFFF richiedono da due a quattro parole a 8 bit consecutive (UTF-8) o due parole a 16 bit consecutive (UTF-16). Questi caratteri posizionati oltre BMP sono denominati *caratteri supplementari* e le parole a 8 bit o a 16 bit consecutive aggiuntive sono denominate *coppie di surrogati*. Per altre informazioni su caratteri supplementari, surrogati e coppie di surrogati, vedere lo [standard Unicode](http://www.unicode.org/standard/standard.html).    

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce tipi di dati come **nchar** e **nvarchar** per archiviare i dati Unicode nell'intervallo BMP (000000-00FFFF), che viene codificato da [!INCLUDE[ssde_md](../../includes/ssde_md.md)] tramite UCS-2. 

In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] è stata introdotta una nuova famiglia di regole di confronto per caratteri supplementari (\_SC) utilizzabili con i tipi di dati **nchar**, **nvarchar** e **sql_variant** per rappresentare l'intera gamma di caratteri Unicode (000000-10FFFF). Ad esempio: **Latin1_General_100_CI_AS_SC** o, se si usano regole di confronto per il giapponese, **Japanese_Bushu_Kakusu_100_CI_AS_SC**. 
 
[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] estende il supporto dei caratteri supplementari ai tipi di dati **char** e **varchar** con le nuove regole di confronto abilitate per UTF-8 ([\_UTF8](#utf8)). Questi tipi di dati sono inoltre in grado di rappresentare l'intera gamma di caratteri Unicode.   

> [!NOTE]
> A partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], tutte le nuove regole di confronto \_140 supportano automaticamente i caratteri supplementari.

Se si utilizzano caratteri supplementari:    
    
-   I caratteri supplementari possono essere usati in operazioni di ordinamento e confronto solo con le versioni delle regole di confronto 90 o successive.    
-   Tutte le regole di confronto versione 100 supportano l'ordinamento linguistico con caratteri supplementari.    
-   L'uso dei caratteri supplementari in metadati, ad esempio in nomi di oggetti di database, non è supportato.    
-   Il flag SC può essere applicato a:    
    -   Regole di confronto versione 90    
    -   Regole di confronto versione 100    
    
-   Il flag SC non può essere applicato a:    
    -   Regole di confronto di Windows senza versione, versione 80    
    -   Regole di confronto binarie BIN o BIN2    
    -   Regole di confronto di SQL \*    
    -   Regole di confronto versione 140 (non devono necessariamente avere il flag SC perché supportano già i caratteri supplementari)    
    
Nella tabella seguente viene confrontato il comportamento di alcune funzioni per i valori stringa e di alcuni operatori di stringa quando vengono usati caratteri supplementari con e senza regole di confronto che supportano caratteri complementari:    
    
|Funzione di stringa o operatore|Con regole di confronto che supportano caratteri complementari|Senza regole di confronto che supportano caratteri complementari|    
|---------------------------------|--------------------------|-----------------------------|    
|[CHARINDEX](../../t-sql/functions/charindex-transact-sql.md)<br /><br /> [LEN](../../t-sql/functions/len-transact-sql.md)<br /><br /> [PATINDEX](../../t-sql/functions/patindex-transact-sql.md)|La coppia di surrogati UTF-16 viene conteggiata come singolo punto di codice.|La coppia di surrogati UTF-16 viene conteggiata come due punti di codice.|    
|[LEFT](../../t-sql/functions/left-transact-sql.md)<br /><br /> [REPLACE](../../t-sql/functions/replace-transact-sql.md)<br /><br /> [REVERSE](../../t-sql/functions/reverse-transact-sql.md)<br /><br /> [RIGHT](../../t-sql/functions/right-transact-sql.md)<br /><br /> [SUBSTRING](../../t-sql/functions/substring-transact-sql.md)<br /><br /> [STUFF](../../t-sql/functions/stuff-transact-sql.md)|Queste funzioni considerano ogni coppia di surrogati un singolo punto di codice e funzionano come previsto.|Queste funzioni potrebbero dividere qualsiasi coppia di surrogati e provocare risultati imprevisti.|    
|[NCHAR](../../t-sql/functions/nchar-transact-sql.md)|Restituisce il carattere corrispondente al valore del punto di codice Unicode specificato nell'intervallo 0-0x10FFFF. Se il valore specificato è incluso nell'intervallo 0-0xFFFF, viene restituito un carattere. Per valori superiori, viene restituito il surrogato corrispondente.|Un valore maggiore di 0xFFFF restituisce NULL anziché il surrogato corrispondente.|    
|[UNICODE](../../t-sql/functions/unicode-transact-sql.md)|Restituisce un punto di codice UTF-16 nell'intervallo 0-0x10FFFF.|Restituisce un punto di codice UCS-2 nell'intervallo 0-0xFFFF.|    
|[Carattere jolly per corrispondenze di singoli caratteri](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)<br /><br /> [Carattere jolly per la mancata corrispondenza dei caratteri](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)|Sono supportati caratteri supplementari per tutte le operazioni con caratteri jolly.|Non sono supportati caratteri supplementari per queste operazioni con caratteri jolly. Sono supportati altri operatori jolly.|    
    
## <a name="gb18030-support"></a><a name="GB18030"></a> Supporto GB18030    
GB18030 è uno standard separato usato nella Repubblica popolare cinese per la codifica dei caratteri cinesi. In GB18030, i caratteri possono essere di 1, 2 o 4 byte di lunghezza. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] offre supporto per i caratteri con codifica GB18030 consentendone il riconoscimento al momento dell'ingresso nel server da un'applicazione client e la conversione e archiviazione come caratteri Unicode a livello nativo. Dopo essere stati archiviati nel server, questi caratteri vengono trattati come caratteri Unicode in tutte le operazioni successive. 

È possibile usare una qualsiasi regola di confronto cinese, preferibilmente la versione 100 più recente. Tutte le regole di confronto di livello \_100 supportano l'ordinamento linguistico con caratteri GB18030. Se nei dati sono inclusi caratteri supplementari (coppie di surrogati), è possibile usare le regole di confronto SC disponibili in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] per migliorare la ricerca e l'ordinamento.    

> [!NOTE]
> Assicurarsi che gli strumenti client, come [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], usino il tipo di carattere Dengxian per visualizzare correttamente le stringhe contenenti caratteri con codifica GB18030.
    
## <a name="complex-script-support"></a><a name="Complex_script"></a> Supporto di lingue con alfabeti non latini    
In[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può essere supportata l'immissione, l'archiviazione, la modifica e la visualizzazione di lingue con alfabeti non latini. Le lingue con alfabeti non latini includono i siti seguenti:    
    
-   Lingue che presentano una combinazione di testo scritto sia da destra verso sinistra sia da sinistra verso destra, ad esempio una combinazione di testo in arabo e inglese.    
-   Lingue i cui caratteri assumono una forma diversa a seconda della posizione oppure combinati con altri caratteri, ad esempio arabi, indiani e thai.    
-   Lingue, come il thai, che richiedono dizionari interni per il riconoscimento delle parole in quanto non vi sono interruzioni tra loro.    
    
Le applicazioni di database che interagiscono con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devono usare controlli che supportano le lingue con alfabeti non latini. I form standard di Windows creati in codice gestito sono abilitati per le lingue con alfabeti non latini.    

## <a name="japanese-collations-added-in--sssql17-md"></a><a name="Japanese_Collations"></a> Regole di confronto per il giapponese aggiunte in [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]
 
A partire da [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)], sono supportate nuove famiglie di regole di confronto per il giapponese, con permutazioni di varie opzioni (\_CS, \_AS, \_KS, \_WS e \_VSS). 

Per elencare queste regole di confronto, è possibile eseguire una query nel [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]:      

```sql 
SELECT Name, Description FROM fn_helpcollations()  
WHERE Name LIKE 'Japanese_Bushu_Kakusu_140%' OR Name LIKE 'Japanese_XJIS_140%'
``` 

Tutte le nuove regole di confronto supportano in modo predefinito i caratteri supplementari. Per nessuna nuova regola di confronto **\_140** esiste o è necessario il flag SC.

Queste regole di confronto sono supportate negli indici del [!INCLUDE[ssde_md](../../includes/ssde_md.md)], nelle tabelle ottimizzate per la memoria, negli indici columnstore e nei moduli compilati in modo nativo.

<a name="ctp23"></a>

## <a name="utf-8-support"></a><a name="utf8"></a> Supporto di UTF-8
In [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] viene introdotto il supporto completo per la codifica dei caratteri di grande diffusione UTF-8 come codifica di importazione o esportazione e come regola di confronto di livello database o colonna per i dati di tipo stringa. La codifica UTF-8 è consentita nei tipi di dati **char** e **varchar** ed è abilitata quando si crea o si modifica la regola di confronto di un oggetto convertendola in una regola di confronto che ha il suffisso *UTF8*. Un esempio è la modifica di **LATIN1_GENERAL_100_CI_AS_SC** in **LATIN1_GENERAL_100_CI_AS_SC_UTF8**. 

UTF-8 è disponibile solo per le regole di confronto di Windows che supportano i caratteri supplementari. Questa funzionalità è stata introdotta in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. I tipi di dati **nchar** e **nvarchar** consentono solo la codifica UCS-2 o UTF-16 e rimangono invariati.

### <a name="storage-differences-between-utf-8-and-utf-16"></a><a name="storage_differences"></a> Differenze nell'archiviazione tra UTF-8 e UTF-16
L'Unicode Consortium assegna a ogni carattere un punto di codice univoco, che corrisponde a un valore nell'intervallo 000000-10FFFF. Con [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], sono disponibili entrambe le codifiche UTF-8 e UTF-16 per rappresentare l'intera gamma:    
-  Con la codifica UTF-8, i caratteri nell'intervallo ASCII (000000-00007F) richiedono 1 byte, i punti di codice nell'intervallo 000080-0007FF richiedono 2 byte, i punti di codice nell'intervallo 000800-00FFFF richiedono 3 byte e i punti di codice nell'intervallo 0010000-0010FFFF richiedono 4 byte. 
-  Con la codifica UTF-16, i punti di codice nell'intervallo 000000-00FFFF richiedono 2 byte e i punti di codice nell'intervallo 0010000-0010FFFF richiedono 4 byte. 

Nella tabella seguente sono elencati i byte per l'archiviazione della codifica per ogni intervallo di caratteri e tipo di codifica:

|Intervallo di codici (esadecimale)|Intervallo di codici (decimale)|Byte per l'archiviazione <sup>1</sup> con UTF-8|Byte per l'archiviazione <sup>1</sup> con UTF-16|    
|---------------------------------|---------------------------------|--------------------------|-----------------------------|   
|000000–00007F|0–127|1|2|
|000080–00009F<br />0000A0–0003FF<br />000400–0007FF|128–159<br />160–1.023<br />1\.024–2.047|2|2|
|000800–003FFF<br />004000–00FFFF|2\.048–16.383<br />16.384–65.535|3|2|
|010000–03FFFF<sup>2</sup><br /><br />040000–10FFFF<sup>2</sup>|65,536–262,143<sup>2</sup><br /><br />262.144–1.114.111<sup>2</sup>|4|4|

<sup>1</sup> I *byte per l'archiviazione* si riferiscono alla lunghezza in byte con codifica, non alle dimensioni di archiviazione su disco del tipo di dati. Per altre informazioni sulle dimensioni di archiviazione su disco, vedere [nchar e nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) e [char e varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md).

<sup>2</sup> Intervallo di punti di codice per [caratteri supplementari](#Supplementary_Characters).

> [!TIP]   
> Si pensa comunemente che in [CHAR(*n*) e VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md) o in [NCHAR(*n*) e NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) l'elemento *n* definisca il numero di caratteri. Questo è dovuto al fatto che nell'esempio di una colonna CHAR(10) è possibile archiviare 10 caratteri ASCII nell'intervallo 0-127 usando regole di confronto come **Latin1_General_100_CI_AI**, perché ogni carattere in questo intervallo usa solo 1 byte.
>    
> Tuttavia, in [CHAR(*n*) e VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md) l'elemento *n* definisce le dimensioni della stringa in *byte* (0-8.000), mentre in [NCHAR(*n*) e NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) l'elemento *n* definisce le dimensioni della stringa in *coppie di byte* (0-4.000). *n* non definisce mai il numero di caratteri che è possibile archiviare,

Come visto in precedenza, a seconda del set di caratteri in uso, la scelta della codifica Unicode e del tipo di dati appropriati potrebbe offrire importanti risparmi di risorse di archiviazione o al contrario incrementare il footprint della memoria. Ad esempio se si usa una regola di confronto in caratteri latini con supporto UTF-8, come **Latin1_General_100_CI_AI_SC_UTF8**, una colonna `CHAR(10)` archivia 10 byte e può contenere 10 caratteri ASCII nell'intervallo 0-127. Ma può contenere solo 5 caratteri nell'intervallo 128-2047 e solo 3 caratteri nell'intervallo 2048-65535. Tuttavia, dato che una colonna `NCHAR(10)` archivia 10 coppie di byte (20 byte), può ospitare 10 caratteri nell'intervallo 0-65535.  

Prima di scegliere se usare la codifica UTF-8 o UTF-16 per un database o una colonna, considerare la distribuzione dei dati di tipo stringa che verranno archiviati:
-  Se sono principalmente nell'intervallo ASCII 0-127 (ad esempio, per la lingua inglese), ogni carattere richiede 1 byte con UTF-8 e 2 byte con UTF-16. L'uso di UTF-8 offre vantaggi in termini di archiviazione. Se si cambia il tipo di dati di una colonna esistente con caratteri ASCII nell'intervallo 0-127 da `NCHAR(10)` a `CHAR(10)` usando una regola di confronto con supporto UTF-8, i requisiti di archiviazione vengono ridotti del 50%. Questa riduzione deriva dal fatto che `NCHAR(10)` richiede 20 byte per l'archiviazione, in confronto a `CHAR(10)` che richiede solo 10 byte per la rappresentazione della stessa stringa Unicode.    
-  Oltre l'intervallo ASCII, quasi tutti gli script basati sull'alfabeto latino, greco, cirillico, copto, armeno, ebraico, arabo, siriaco, tāna e n'ko richiedono 2 byte per carattere sia in UTF-8 che in UTF-16. In questi casi non vi sono differenze significative a livello di archiviazione per i tipi di dati simili (ad esempio, tra l'uso di **char** o **nchar**).
-  Se si tratta principalmente di alfabeti dell'Asia orientale (ad esempio, coreano, cinese e giapponese), ogni carattere richiede 3 byte con UTF-8 e 2 byte con UTF-16. L'uso di UTF-16 offre vantaggi in termini di archiviazione. 
-  I caratteri nell'intervallo 010000-10FFFF richiedono 4 byte sia in UTF-8 che in UTF-16. In questi casi non vi sono differenze di archiviazione per i tipi di dati simili (ad esempio, tra l'uso di **char** o **nchar**).

Per altre considerazioni, vedere [Scrittura di istruzioni Transact-SQL internazionali](../../relational-databases/collations/write-international-transact-sql-statements.md).

### <a name="converting-to-utf-8"></a><a name="converting"></a> Conversione a UTF-8
Dato che in [CHAR (*n*) e VARCHAR (*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md) o in [NCHAR (*n*) e NVARCHAR (*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) il segnaposto *n* definisce le dimensioni di archiviazione in byte e non il numero di caratteri che è possibile archiviare, è importante determinare le dimensioni del tipo di dati in cui è necessario eseguire la conversione, per evitare il troncamento dei dati. 

Si consideri ad esempio una colonna definita come **NVARCHAR(100)** che archivia 180 byte di caratteri giapponesi. In questo esempio i dati della colonna sono attualmente codificati con UCS-2 o UTF-16, che usa 2 byte per carattere. La conversione del tipo di colonna in **VARCHAR(200)** non è sufficiente per impedire il troncamento dei dati, perché il nuovo tipo di dati può archiviare solo 200 byte, ma i caratteri giapponesi richiedono 3 byte quando sono codificati in UTF-8. Pertanto la colonna deve essere definita come **VARCHAR(270)** per evitare la perdita di dati a causa del troncamento.

Di conseguenza è necessario conoscere in anticipo le dimensioni in byte previste per la definizione della colonna, prima di convertire i dati esistenti in UTF-8 e modificare di conseguenza le nuove dimensioni del tipo di dati. Vedere lo script [!INCLUDE[tsql](../../includes/tsql-md.md)] o il notebook SQL in [Data Samples GitHub](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/unicode), che usa la funzione [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) e l'istruzione [COLLATE](../../t-sql/statements/collations.md) per determinare i requisiti di lunghezza dati corretti per le operazioni di conversione UTF-8 in un database esistente.

Per modificare le regole di confronto e il tipo di dati delle colonne in una tabella esistente, usare uno dei metodi descritti in [Impostare o modificare le regole di confronto delle colonne](../../relational-databases/collations/set-or-change-the-column-collation.md).

Per modificare le regole di confronto del database consentendo a nuovi oggetti di ereditarle per impostazione predefinita o per modificare le regole di confronto del server consentendo ai nuovi database di ereditare le regole di confronto di sistema per impostazione predefinita, vedere la sezione [Attività correlate](#Related_Tasks) di questo articolo. 

##  <a name="related-tasks"></a><a name="Related_Tasks"></a> Related tasks    
    
|Attività|Argomento|    
|----------|-----------|    
|Viene descritto come impostare o modificare le regole di confronto dell'istanza di SQL Server. Si noti che la modifica delle regole di confronto del server non comporta la modifica delle regole di confronto dei database esistenti.|[Impostazione o modifica di regole di confronto del server](../../relational-databases/collations/set-or-change-the-server-collation.md)|    
|Viene descritto come impostare o modificare le regole di confronto di un database utente. Si noti che la modifica delle regole di confronto di un database non comporta la modifica delle regole di confronto delle colonne di tabella esistenti.|[Impostare o modificare le regole di confronto del database](../../relational-databases/collations/set-or-change-the-database-collation.md)|    
|Viene descritto come impostare o modificare le regole di confronto di una colonna nel database|[Impostare o modificare le regole di confronto delle colonne](../../relational-databases/collations/set-or-change-the-column-collation.md)|    
|Viene descritto come restituire informazioni sulle regole di confronto a livello di server, di database o di colonna|[Visualizzazione di informazioni sulle regole di confronto](../../relational-databases/collations/view-collation-information.md)|    
|Viene descritto come scrivere istruzioni Transact-SQL più portabili da un linguaggio a un altro o in grado di supportare più linguaggi con maggiore facilità|[Scrittura di istruzioni Transact-SQL internazionali](../../relational-databases/collations/write-international-transact-sql-statements.md)|    
|Viene descritto come modificare la lingua dei messaggi di errore e le preferenze di uso e visualizzazione dei dati di tipo data, ora e valuta|[Impostazione di una lingua di sessione](../../relational-databases/collations/set-a-session-language.md)|    
    
##  <a name="related-content"></a><a name="Related_Content"></a> Related content    
Per altre informazioni, vedere i seguenti contenuti correlati:
* [SQL Server Best Practices Collation Change](https://go.microsoft.com/fwlink/?LinkId=113891)  
* [Usare il formato carattere Unicode per importare o esportare dati (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
* [Scrittura di istruzioni Transact-SQL internazionali](../../relational-databases/collations/write-international-transact-sql-statements.md)  
* [Procedure consigliate di SQL Server: migrazione a Unicode](https://go.microsoft.com/fwlink/?LinkId=113890) (non più aggiornato)   
* [Unicode Consortium](https://go.microsoft.com/fwlink/?LinkId=48619)  
* [Standard Unicode](http://www.unicode.org/standard/standard.html)  
* [Supporto UTF-8 in OLE DB Driver for SQL Server](../../connect/oledb/features/utf-8-support-in-oledb-driver-for-sql-server.md)  
* [Nome delle regole di confronto di SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)  
* [Windows_collation_name (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md)  
* [Introduzione al supporto di UTF-8 per SQL Server](https://techcommunity.microsoft.com/t5/SQL-Server/Introducing-UTF-8-support-for-SQL-Server/ba-p/734928)      
* [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md)      
* [Precedenza delle regole di confronto](../../t-sql/statements/collation-precedence-transact-sql.md)    
    
## <a name="see-also"></a>Vedere anche    
[Regole di confronto dei database indipendenti](../../relational-databases/databases/contained-database-collations.md)     
[Scelta di una lingua durante la creazione di un indice full-text](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md)     
[sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)       
[Set di caratteri a byte singolo e multibyte](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)