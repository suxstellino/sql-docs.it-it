---
description: SELECT (comando SQL)
title: Comando SELECT-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- select [ODBC]
ms.assetid: 2149c3ca-3a71-446d-8d53-3d056e2f301a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fc6ae552bc4e6e8bd681aa3d47ffadeaf81fd0a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99153519"
---
# <a name="select---sql-command"></a>SELECT (comando SQL)
Recupera i dati da una o più tabelle.  
  
 Il driver ODBC Visual FoxPro supporta la sintassi nativa del linguaggio Visual FoxPro per questo comando. Per informazioni specifiche del driver, vedere **Note sul driver**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
SELECT [ALL | DISTINCT]  
   [Alias.] Select_Item [AS Column_Name]  
   [, [Alias.] Select_Item [AS Column_Name] ...]   
FROM [DatabaseName!]Table [Local_Alias]  
   [, [DatabaseName!]Table [Local_Alias] ...]   
[WHERE JoinCondition [AND JoinCondition  
...]  
   [AND | OR FilterCondition [AND | OR FilterCondition ...]]]  
[GROUP BY GroupColumn [, GroupColumn ...]]  
[HAVING FilterCondition]  
[UNION [ALL] SELECTCommand]  
[ORDER BY Order_Item [ASC | DESC] [, Order_Item [ASC | DESC] ...]]  
```  
  
## <a name="arguments"></a>Argomenti  
  
> [!NOTE]  
>  Una *sottoquery*, a cui si fa riferimento negli argomenti seguenti, è un'SELECT all'interno di un'Select e deve essere racchiusa tra parentesi. È possibile avere fino a due sottoquery allo stesso livello (non annidato) nella clausola WHERE. Vedere la sezione degli argomenti. Le sottoquery possono contenere più condizioni di join.  
  
 [TUTTI &#124; DISTINTI]   [*Alias*.] *Select_Item* [As *column_name*] [, [*alias*.] *Select_Item* [come *column_name*]...]  
 La clausola SELECT specifica i campi, le costanti e le espressioni visualizzate nei risultati della query.  
  
 Per impostazione predefinita, tutti gli oggetti visualizzano tutte le righe nei risultati della query.  
  
 DISTINCT esclude i duplicati di qualsiasi riga dai risultati della query.  
  
> [!NOTE]  
>  È possibile utilizzare DISTINCT solo una volta per ogni clausola SELECT.  
  
 *Alias*. qualifica i nomi degli elementi corrispondenti. Ogni elemento specificato con *Select_Item* genera una colonna dei risultati della query. Se due o più elementi hanno lo stesso nome, includere l'alias della tabella e un punto prima del nome dell'elemento per impedire la duplicazione delle colonne.  
  
 *Select_Item* specifica un elemento da includere nei risultati della query. Un elemento può essere uno dei seguenti:  
  
-   Nome di un campo di una tabella nella clausola FROM.  
  
-   Costante che specifica che lo stesso valore costante deve essere visualizzato in ogni riga dei risultati della query.  
  
-   Espressione che può essere il nome di una funzione definita dall'utente.  
  
 **Funzioni definite dall'utente con SELECT**  
  
 Anche se l'uso di funzioni definite dall'utente nella clausola SELECT presenta vantaggi evidenti, è necessario considerare anche le restrizioni seguenti:  
  
-   La velocità delle operazioni eseguite con SELECT può essere limitata dalla velocità con cui vengono eseguite tali funzioni definite dall'utente. Le manipolazioni di volumi elevati che coinvolgono funzioni definite dall'utente possono essere eseguite in modo più efficiente tramite l'API e le funzioni definite dall'utente scritte in C o linguaggio assembly.  
  
-   L'unico modo affidabile per passare i valori alle funzioni definite dall'utente richiamate da SELECT è dall'elenco di argomenti passato alla funzione quando viene richiamato.  
  
-   Anche se si sperimenta e si individua una manipolazione apparentemente proibita che funziona correttamente in una determinata versione di FoxPro, non esiste alcuna garanzia che continuerà a funzionare nelle versioni successive.  
  
 Oltre a queste restrizioni, le funzioni definite dall'utente sono accettabili nella clausola SELECT. Tuttavia, tenere presente che l'uso di SELECT potrebbe rallentare le prestazioni.  
  
 Le funzioni di campo seguenti sono disponibili per l'uso con un elemento SELECT che rappresenta un campo o un'espressione che interessa un campo:  
  
-   AVG (*Select_Item*): calcola la media di una colonna di dati numerici.  
  
-   CONTEGGIO (*Select_Item*): conta il numero di elementi selezionati in una colonna. COUNT (*) conta il numero di righe nell'output della query.  
  
-   MIN (*Select_Item*): determina il valore più piccolo di *Select_Item* in una colonna.  
  
-   MAX (*Select_Item*): determina il valore più grande di *Select_Item* in una colonna.  
  
-   SUM (*Select_Item*): somma una colonna di dati numerici.  
  
 Non è possibile annidare le funzioni di campo.  
  
 COME *column_name*  
 Specifica l'intestazione per una colonna nell'output della query. Questa operazione è utile quando *Select_Item* è un'espressione o contiene una funzione Field e si desidera assegnare alla colonna un nome significativo. *Column_name* può essere un'espressione, ma non può contenere caratteri, ad esempio spazi, che non sono consentiti nei nomi dei campi tabella.  
  
 DA [*DatabaseName*]] *Table* [*Local_Alias*] [, [*DatabaseName*]] *Tabella* [*Local_Alias*]...]  
 Elenca le tabelle che contengono i dati recuperati dalla query. Se non è aperta alcuna tabella, Visual FoxPro Visualizza la finestra di dialogo **Apri** in cui è possibile specificare il percorso del file. Una volta aperta, la tabella resta aperta dopo il completamento della query.  
  
 *DatabaseName*! Specifica il nome di un database diverso da quello specificato con l'origine dati. Se il database non è specificato con l'origine dati, è necessario includere il nome del database che contiene la tabella. Includere il delimitatore punto esclamativo (!) dopo il nome del database e prima del nome della tabella.  
  
 *Local_Alias* specifica un nome temporaneo per la tabella denominata nella *tabella*. Se si specifica un alias locale, è necessario utilizzare l'alias locale anziché il nome della tabella nell'istruzione SELECT. L'alias locale non influisce sull'ambiente Visual FoxPro.  
  
 DOVE *joinCondition* [e *joinCondition* ...]    [E &#124; o *FilterCondition* [e &#124; o *FilterCondition* ...]]  
 Indica a Visual FoxPro di includere solo determinati record nei risultati della query. DOVE è necessario per recuperare i dati da più tabelle.  
  
 *JoinCondition* specifica i campi che collegano le tabelle nella clausola from. Se si include più di una tabella in una query, è necessario specificare una condizione di join per ogni tabella dopo la prima.  
  
> [!IMPORTANT]  
>  Quando si creano condizioni di join, tenere presenti le seguenti informazioni:  
  
-   Se si includono due tabelle in una query e non si specifica una condizione di join, ogni record della prima tabella viene unito a tutti i record della seconda tabella, purché vengano soddisfatte le condizioni del filtro. Una query di questo tipo può produrre risultati lunghi.  
  
-   Prestare attenzione quando si uniscono tabelle con campi vuoti perché Visual FoxPro corrisponde a campi vuoti. Se ad esempio si partecipa a CUSTOMER.ZIP e INVOICE.ZIP e se il cliente contiene 100 codici postali vuoti e la fattura contiene 400 codici postali vuoti, l'output della query contiene 40.000 record aggiuntivi risultanti dai campi vuoti. Utilizzare la funzione **Empty ()** per eliminare i record vuoti dall'output della query.  
  
-   Per connettere più condizioni di join, è necessario usare l'operatore AND. Ogni condizione di join ha il formato seguente:  
  
     *Confronto FieldName1 FieldName2*  
  
     *FieldName1* è il nome di un campo di una tabella, *FieldName2* è il nome di un campo di un'altra tabella e *Comparison* è uno degli operatori descritti nella tabella seguente.  
  
|Operatore|Confronto|  
|--------------|----------------|  
|=|Uguale a|  
|==|Esattamente uguale|  
|LIKE|LIKE SQL|  
|<>,! =, #|Diverso da|  
|>|Più di|  
|>=|Maggiore o uguale a|  
|<|Minore di|  
|<=|Minore o uguale a|  
  
 Quando si usa l'operatore = con stringhe, agisce in modo diverso, a seconda dell'impostazione di SET ANSI. Quando l'opzione SET ANSI è impostata su OFF, Visual FoxPro considera i confronti di stringhe in modo familiare agli utenti di Xbase. Quando l'impostazione ANSI è impostata su ON, Visual FoxPro segue gli standard ANSI per i confronti tra stringhe. Per ulteriori informazioni sul modo in cui Visual FoxPro esegue confronti tra stringhe, vedere [set ANSI](../../odbc/microsoft/set-ansi-command.md) e [set exact](../../odbc/microsoft/set-exact-command.md) .  
  
 *FilterCondition* specifica i criteri che i record devono soddisfare per essere inclusi nei risultati della query. È possibile includere tutte le condizioni di filtro in una query come desiderato, connetterle con l'operatore AND o OR. È inoltre possibile utilizzare l'operatore NOT per invertire il valore di un'espressione logica oppure è possibile utilizzare **Empty ()** per verificare la presenza di un campo vuoto. *FilterCondition* può assumere qualsiasi forma negli esempi seguenti:  
  
 **Esempio 1** *confronto FieldName1 FieldName2*  
  
 `customer.cust_id = orders.cust_id`  
  
 **Esempio 2** *espressione di confronto FieldName*  
  
 `payments.amount >= 1000`  
  
 **Esempio 3** *FieldName Comparison* All (*sottoquery*)  
  
 `company < ALL ;`  
  
 `(SELECT company FROM customer WHERE country = "USA")`  
  
 Quando la condizione di filtro include ALL, il campo deve soddisfare la condizione di confronto per tutti i valori generati dalla sottoquery prima che il relativo record venga incluso nei risultati della query.  
  
 **Esempio 4** *confronto fieldname* qualsiasi &#124; some (*sottoquery*)  
  
 `company < ANY ;`  
  
 `(SELECT company FROM customer WHERE country = "USA")`  
  
 Quando la condizione di filtro include uno o alcuni, il campo deve soddisfare la condizione di confronto per almeno uno dei valori generati dalla sottoquery.  
  
 Nell'esempio seguente viene verificato se i valori nel campo sono compresi in un intervallo di valori specificato:  
  
 **Esempio 5** *FieldName* [not] between *Start_Range* and *End_Range*  
  
 `customer.postalcode BETWEEN 90000 AND 99999`  
  
 Nell'esempio seguente viene verificato se almeno una riga soddisfa i criteri della sottoquery. Quando la condizione di filtro include EXISTs, la condizione di filtro restituisce true (. T.), a meno che la sottoquery non restituisca il set vuoto.  
  
 **Esempio 6** [not] Exists (*sottoquery*)  
  
 `EXISTS ;`  
  
 `(SELECT * FROM orders WHERE customer.postalcode =`  
  
 `orders.postalcode)`  
  
 **Esempio 7** *FieldName* [not] in *value_set*  
  
 `customer.postalcode NOT IN ("98052","98072","98034")`  
  
 Quando la condizione di filtro include IN, il campo deve contenere uno dei valori prima che il relativo record venga incluso nei risultati della query.  
  
 **Esempio 8** *FieldName* [not] in (*sottoquery*)  
  
 `customer.cust_id IN ;`  
  
 `(SELECT orders.cust_id FROM orders WHERE orders.city="Seattle")`  
  
 Qui il campo deve contenere uno dei valori restituiti dalla sottoquery prima che il relativo record venga incluso nei risultati della query.  
  
 **Esempio 9** *FieldName* [not] come *cExpression*  
  
 `customer.country NOT LIKE "USA"`  
  
 Questa condizione di filtro cerca ogni campo corrispondente a *cExpression*. È possibile utilizzare il segno di percentuale (%) e caratteri jolly di sottolineatura (_) come parte di *cExpression*. Il carattere di sottolineatura rappresenta un singolo carattere sconosciuto nella stringa.  
  
 GROUP BY *GroupColumn* [, *GroupColumn* ...]  
 Raggruppa le righe nella query in base ai valori in una o più colonne. *GroupColumn* può essere uno dei seguenti:  
  
-   Nome di un campo normale della tabella.  
  
-   Campo che include una funzione di campo SQL.  
  
-   Espressione numerica che indica il percorso della colonna nella tabella dei risultati. Il numero di colonna più a sinistra è 1.  
  
 CON *FilterCondition*  
 Specifica una condizione di filtro che i gruppi devono soddisfare per essere inclusi nei risultati della query. HAVING deve essere usato con GROUP BY e può includere il numero di condizioni di filtro desiderato, connesso dall'operatore AND o OR. È inoltre possibile utilizzare NOT per invertire il valore di un'espressione logica.  
  
 *FilterCondition* non può contenere una sottoquery.  
  
 Una clausola HAVING senza una clausola GROUP BY si comporta come una clausola WHERE. È possibile usare alias locali e funzioni di campo nella clausola HAVING. Usare una clausola WHERE per ottenere prestazioni più veloci se la clausola HAVING non contiene funzioni di campo.  
  
 [Unione [tutti] *SELECTCommand*]  
 Combina i risultati finali di un'unica selezione con i risultati finali di un'altra SELECT. Per impostazione predefinita, UNION controlla i risultati combinati ed Elimina le righe duplicate. Usare le parentesi per combinare più clausole UNION.  
  
 ALL impedisce a UNION di eliminare righe duplicate dai risultati combinati.  
  
 Le clausole UNION rispettano le regole seguenti:  
  
-   Non è possibile utilizzare UNION per combinare sottoquery.  
  
-   Entrambi i comandi SELECT devono avere lo stesso numero di colonne nell'output della query.  
  
-   Ogni colonna nei risultati della query di un oggetto SELECT deve avere lo stesso tipo di dati e lo stesso spessore della colonna corrispondente nell'altra SELECT.  
  
-   Solo l'istruzione SELECT finale può includere una clausola ORDER BY, che deve fare riferimento alle colonne di output per numero. Se è inclusa una clausola ORDER BY, questo influirà sul risultato completo.  
  
 È anche possibile usare la clausola UNION per simulare un outer join.  
  
 Quando si uniscono due tabelle in una query, nell'output vengono inclusi solo i record con valori corrispondenti nei campi di join. Se un record nella tabella padre non dispone di un record corrispondente nella tabella figlio, il record nella tabella padre non verrà incluso nell'output. Un outer join consente di includere tutti i record nella tabella padre nell'output, insieme ai record corrispondenti nella tabella figlio. Per creare un outer join in Visual FoxPro, è necessario usare un comando di selezione nidificato, come nell'esempio seguente:  
  
```  
SELECT customer.company, orders.order_id, orders.emp_id ;  
FROM customer, orders ;  
WHERE customer.cust_id = orders.cust_id ;  
UNION ;  
SELECT customer.company, 0, 0 ;  
FROM customer ;  
WHERE customer.cust_id NOT IN ;  
(SELECT orders.cust_id FROM orders)  
```  
  
> [!NOTE]  
>  Assicurarsi di includere lo spazio che precede immediatamente ogni punto e virgola. In caso contrario, verrà visualizzato un errore.  
  
 La sezione del comando prima che la clausola UNION selezioni i record da entrambe le tabelle con valori corrispondenti. Le società del cliente che non dispongono di fatture associate non sono incluse. La sezione del comando dopo la clausola UNION seleziona i record nella tabella Customer che non dispongono di record corrispondenti nella tabella Orders.  
  
 Per quanto riguarda la seconda sezione del comando, tenere presente quanto segue:  
  
-   L'istruzione SELECT racchiusa tra parentesi viene elaborata per prima. Questa istruzione crea una selezione di tutti i numeri dei clienti nella tabella Orders.  
  
-   La clausola WHERE trova tutti i numeri dei clienti nella tabella Customer che non sono presenti nella tabella Orders. Poiché nella prima sezione del comando sono state fornite tutte le aziende con un numero di cliente nella tabella Orders, tutte le aziende della tabella Customer sono ora incluse nei risultati della query.  
  
-   Poiché le strutture delle tabelle incluse in un'Unione devono essere identiche, nella seconda istruzione SELECT sono presenti due segnaposto per rappresentare *Orders.order_id* e *Orders.emp_id* dalla prima istruzione SELECT.  
  
    > [!NOTE]  
    >  I segnaposto devono essere dello stesso tipo dei campi che rappresentano. Se il campo è un tipo di data, il segnaposto deve essere {//}. Se il campo è un campo di tipo carattere, il segnaposto deve essere una stringa vuota ("").  
  
 ORDER BY *Order_Item* [ASC &#124; DESC] [, *Order_Item* [ASC &#124; DESC]...]  
 Ordina i risultati della query in base ai dati in una o più colonne. Ogni *Order_Item* deve corrispondere a una colonna nei risultati della query e può essere una delle seguenti:  
  
-   Un campo in una tabella FROM che è anche un elemento SELECT nella clausola SELECT principale (non in una sottoquery).  
  
-   Espressione numerica che indica il percorso della colonna nella tabella dei risultati. La colonna più a sinistra è il numero 1.  
  
 ASC specifica un ordine crescente per i risultati della query, in base all'elemento o agli elementi dell'ordine, ed è il valore predefinito per ORDER BY.  
  
 DESC specifica un ordine decrescente per i risultati della query.  
  
 I risultati della query non vengono ordinati se non si specifica un ordine con ORDER BY.  
  
## <a name="remarks"></a>Commenti  
 SELECT è un comando SQL incorporato in Visual FoxPro come qualsiasi altro comando Visual FoxPro. Quando si usa SELECT per rappresentare una query, Visual FoxPro interpreta la query e recupera i dati specificati dalle tabelle. È possibile creare una query SELECT dalla finestra del prompt dei comandi o da un programma Visual FoxPro, come per qualsiasi altro comando Visual FoxPro.  
  
> [!NOTE]  
>  SELECT non rispetta la condizione di filtro corrente specificata con il filtro SET.  
  
## <a name="driver-remarks"></a>Osservazioni del driver  
 Quando l'applicazione invia l'istruzione SQL ODBC SELECT all'origine dati, il driver ODBC Visual FoxPro converte il comando nel comando SELECT di Visual FoxPro senza conversione, a meno che il comando non contenga una sequenza di escape ODBC. Gli elementi racchiusi in una sequenza di escape ODBC vengono convertiti nella sintassi di Visual FoxPro. Per ulteriori informazioni sull'utilizzo delle sequenze di escape ODBC, vedere la pagina relativa alle [funzioni di data e ora](../../odbc/microsoft/time-and-date-functions-visual-foxpro-odbc-driver.md) e in *Microsoft ODBC Programmer ' s Reference*, vedere [sequenze di escape in ODBC](../../odbc/reference/develop-app/escape-sequences-in-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE TABLE-SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [INSERT-SQL](../../odbc/microsoft/insert-sql-command.md)   
 [IMPOSTA ANSI](../../odbc/microsoft/set-ansi-command.md)   
 [IMPOSTA ESATTA](../../odbc/microsoft/set-exact-command.md)
