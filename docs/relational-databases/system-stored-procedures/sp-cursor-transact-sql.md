---
description: sp_cursor (Transact-SQL)
title: sp_cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_cursor_TSQL
- sp_cursor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor
ms.assetid: 41ade0ca-5f11-469d-bd4d-c8302ccd93b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 07dcf9bfc42705f2ad49e3ce476ffcd8ad70f052
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813275"
---
# <a name="sp_cursor-transact-sql"></a>sp_cursor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Richiede aggiornamenti posizionati. Con questa routine è possibile effettuare operazioni in una o più righe all'interno del buffer di recupero di un cursore. sp_cursor viene richiamato specificando ID = 1 in un pacchetto del flusso TDS (Tabular Data Stream).  
  
||  
|-|  
|**Si applica a**: SQL Server (da alla [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).|  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_cursor  cursor, optype, rownum, table  
    [ , value[...n]]]  
```  
  
## <a name="arguments"></a>Argomenti  
 *cursor*  
 Handle del cursore. *Cursor* è un parametro obbligatorio che richiede un valore di input **int** . *Cursor* è il valore dell' *handle* generato da SQL Server e restituito dalla routine sp_cursoropen.  
  
 *optype*  
 Parametro obbligatorio che definisce l'operazione che verrà effettuata dal cursore. *optype* richiede uno dei valori di input **int** seguenti.  
  
|Valore|Nome|Descrizione|  
|-----------|----------|-----------------|  
|0X0001|UPDATE|Consente di aggiornare una o più righe nel buffer di recupero.  Le righe specificate in *rownum* vengono nuovamente accessibili e aggiornate.|  
|0x0002|DELETE|Consente di eliminare una o più righe nel buffer di recupero. Le righe specificate in *rownum* vengono nuovamente accessibili ed eliminate.|  
|0X0004|INSERT|Inserisce dati senza compilare un'istruzione SQL **Insert** .|  
|0X0008|REFRESH|Consente di inserire nuovamente dati nel buffer dalle tabelle sottostanti e può essere usato per aggiornare la riga nel caso in cui un aggiornamento o un'eliminazione non riesca a causa del controllo della concorrenza ottimistica oppure dopo un'operazione UPDATE.|  
|0X10|LOCK|Determina l'acquisizione di un SQL Server U-Lock nella pagina contenente la riga specificata. Tale blocco è compatibile con i blocchi S ma non con i blocchi X o con altri blocchi U. Può essere usato per implementare la funzione di blocco a breve termine.|  
|0X20|SETPOSITION|Viene utilizzato solo quando il programma emette una SQL Server successiva istruzione DELETE o UPDATE posizionata.|  
|0X40|ABSOLUTE|Può essere usato solo insieme ad UPDATE o DELETE.  ABSOLUTE viene usato solo con i cursori KEYSET, viene ignorato per i cursori DYNAMIC e i cursori STATIC non possono essere aggiornati.<br /><br /> Nota: se si specifica ABSOLUTe in una riga del keyset che non è stata recuperata, l'operazione potrebbe non riuscire a eseguire il controllo della concorrenza e il risultato restituito non può essere garantito.|  
  
 *rownum*  
 Specifica le righe del buffer di recupero che verranno usate, aggiornate o eliminate mediante il cursore.  
  
> [!NOTE]  
>  Non influisce sul punto di partenza di qualsiasi operazione di recupero RELATIVE, NEXT o PREVIOUS, né su eliminazioni o aggiornamenti eseguiti utilizzando sp_cursor.  
  
 *rownum* è un parametro obbligatorio che richiede un valore di input **int** .  
  
 1  
 Indica la prima riga del buffer di recupero.  
  
 2  
 Indica la seconda riga del buffer di recupero.  
  
 3, 4, 5  
 Indica la terza riga e così via.  
  
 n  
 Indica l'ennesima riga del buffer di recupero.  
  
 0  
 Indica tutte le righe del buffer di recupero.  
  
> [!NOTE]  
>  È valido solo per l'utilizzo con i valori *optype* di aggiornamento, eliminazione, aggiornamento o blocco.  
  
 *tabella*  
 Nome della tabella che identifica la tabella a cui viene applicato *optype* quando la definizione del cursore include un join o nomi di colonna ambigui restituiti dal parametro del *valore* . Se non è definita una tabella specifica, l'impostazione predefinita è la prima tabella nella clausola FROM. *Table* è un parametro facoltativo che richiede un valore di input stringa. È possibile specificare la stringa come qualsiasi tipo di dati UNICODE o carattere. *Table* può essere un nome di tabella in più parti.  
  
 *value*  
 Usato per inserire o aggiornare valori. Il parametro della stringa di *valore* viene usato solo con i valori *optype* di aggiornamento e inserimento. È possibile specificare la stringa come qualsiasi tipo di dati UNICODE o carattere.  
  
> [!NOTE]  
>  I nomi dei parametri per *value* possono essere assegnati dall'utente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Quando si utilizza una RPC, un'operazione di eliminazione o aggiornamento posizionata con un numero di buffer 0 restituirà un messaggio completato con un *conteggio delle righe* pari a 0 (esito negativo) o 1 (esito positivo) per ogni riga del buffer di recupero.  
  
## <a name="remarks"></a>Commenti  
  
## <a name="optype-parameter"></a>Parametro optype  
 Fatta eccezione per le combinazioni di seposition con UPDATE, DELETE, REFRESH o LOCK; o ABSOLUTe con UPDATE o DELETE, i valori *optype* si escludono a vicenda.  
  
 La clausola SET del valore di aggiornamento viene costruita dal parametro *value* .  
  
 Un vantaggio dell'uso del valore INSERT *optype* è che è possibile evitare di convertire dati non di tipo carattere in formato carattere per gli inserimenti. I valori vengono specificati con le stesse modalità di UPDATE. Se non sono incluse colonne obbligatorie, l'operazione INSERT non riesce.  
  
-   Il valore SETPOSITION non influisce sul punto di partenza di qualsiasi operazione di recupero RELATIVE, NEXT o PREVIOUS, né su eliminazioni o aggiornamenti eseguiti utilizzando l'interfaccia di sp_cursor. Qualsiasi numero che non specifichi una riga nel buffer di recupero comporterà l'impostazione della posizione su 1 senza restituzione di errori. Una volta eseguita la funzione seposition, la posizione rimane attiva fino alla successiva operazione di sp_cursorfetch, operazione di **recupero** T-SQL o sp_cursor operazione di posizione tramite lo stesso cursore. Un'operazione sp_cursorfetch successiva imposterà la posizione del cursore sulla prima riga nel nuovo buffer di recupero, mentre le altre chiamate del cursore non influiranno sul valore della posizione. SETPOSITION può essere collegato da una clausola OR con REFRESH, UPDATE, DELETE o LOCK allo scopo di impostare il valore della posizione sull'ultima riga modificata.  
  
 Se una riga nel buffer di recupero non viene specificata tramite il parametro *rownum* , la posizione verrà impostata su 1, senza restituire alcun errore. Dopo l'impostazione, la posizione rimane effettiva fino a quando non verrà eseguita sullo stesso cursore la successiva operazione sp_cursorfetch, T-SQL o SETPOSITION di sp_cursor.  
  
 SETPOSITION può essere collegato da una clausola OR con REFRESH, UPDATE, DELETE o LOCK allo scopo di impostare la posizione del cursore sull'ultima riga modificata.  
  
## <a name="rownum-parameter"></a>Parametro rownum  
 Se specificato, il parametro *rownum* può essere interpretato come il numero di riga all'interno del keyset anziché il numero di riga all'interno del buffer di recupero. È responsabilità dell'utente garantire la gestione del controllo della concorrenza. Ciò significa che per i cursori SCROLL_LOCKS è necessario gestire indipendentemente un blocco nella riga specificata. Questa operazione può essere eseguita tramite una transazione. Per eseguire questa operazione con i cursori OPTIMISTIC, è necessario avere recuperato la riga precedentemente.  
  
## <a name="table-parameter"></a>Parametro table  
 Se il valore *optype* è Update o INSERT e viene inviata un'istruzione Update o INSERT completa come parametro *value* , il valore specificato per *Table* viene ignorato.  
  
> [!NOTE]  
>  È possibile modificare una sola tabella che partecipa a una vista. I nomi delle colonne dei parametri di *valore* devono riflettere i nomi delle colonne nella vista, ma il nome della tabella può essere quello della tabella di base sottostante. in questo caso sp_cursor sostituirà il nome della vista.  
  
## <a name="value-parameter"></a>Parametro value  
 Esistono due alternative alle regole per l'utilizzo di *value* come indicato in precedenza nella sezione arguments:  
  
1.  È possibile usare un nome ' \@ ' anteposto al nome della colonna nell'elenco di selezione per tutti i parametri del *valore* denominato. Un vantaggio di questa alternativa consiste nella possibilità che la conversione di dati non sia necessaria.  
  
2.  Utilizzare un parametro per inviare un'istruzione UPDATE o INSERT completa oppure utilizzare più parametri per inviare parti di un'istruzione UPDATE o INSERT che SQL Server verrà compilata in un'istruzione completa. È possibile trovare alcuni esempi nella sezione Esempi più avanti in questo argomento.  
  
## <a name="examples"></a>Esempio  
  
### <a name="alternative-value-parameter-uses"></a>Utilizzi alternativi del parametro value  
 Per UPDATE:  
  
 Quando viene usato un solo parametro, si potrebbe inviare un'istruzione UPDATE usando la sintassi seguente:  
  
 `[ [ UPDATE <table name> ] SET ] {<column name> = expression} [,...n]`  
  
> [!NOTE]  
>  Se \<table name> si specifica Update, qualsiasi valore specificato per il parametro *Table* verrà ignorato.  
  
 Quando vengono usati più parametri, il primo parametro deve essere una stringa nel formato seguente:  
  
 `[ SET ] <column name> = expression  [,...n]`  
  
 e i parametri successivi devono essere nel formato:  
  
 `<column name> = expression  [,...n]`  
  
 In questo caso, l'oggetto \<table name> nell'istruzione di aggiornamento costruita è quello specificato o impostato come valore predefinito dal parametro *Table* .  
  
 Per INSERT:  
  
 Quando viene usato un solo parametro, si potrebbe inviare un'istruzione INSERT usando la sintassi seguente:  
  
 `[ [ INSERT [INTO] <table name> ] VALUES ] ( <expression> [,...n] )`  
  
> [!NOTE]  
>  Se *\<table name>* viene specificato INSERT, qualsiasi valore specificato per il parametro *Table* verrà ignorato.  
  
 Quando vengono usati più parametri, il primo parametro deve essere una stringa nel formato seguente:  
  
 `[ VALUES ( ] <expression>  [,...n]`  
  
 e i parametri successivi devono essere nel formato:  
  
 `expression [,...n]`  
  
 ad eccezione dei casi in cui è stato specificato VALUES. In questo caso, l'ultima espressione deve essere seguita da una parentesi finale ")". In questo caso, l'oggetto *\<table name>* nell'istruzione di aggiornamento costruita è quello specificato o impostato come valore predefinito dal parametro *Table* .  
  
> [!NOTE]  
>  È possibile inviare un parametro come parametro denominato, ad esempio "`@VALUES`". In questo caso è possibile non usare altri parametri denominati.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_cursoropen &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
