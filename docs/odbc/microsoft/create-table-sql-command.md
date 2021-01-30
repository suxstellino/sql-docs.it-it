---
description: CREATE TABLE (comando SQL)
title: Comando CREATE TABLE-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CREATE TABLE [ODBC]
ms.assetid: be2143ba-fc16-42c9-84f7-8985cd924860
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0fb1e91c03f7a1a1fee98ce887be991370450999
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183201"
---
# <a name="create-table---sql-command"></a>CREATE TABLE (comando SQL)
Crea una tabella con i campi specificati.  
  
 Il driver ODBC Visual FoxPro supporta la sintassi nativa del linguaggio Visual FoxPro per questo comando. Per informazioni specifiche del driver, vedere **Note sul driver**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
CREATE TABLE | DBF TableName1 [NAME LongTableName] [FREE]  
   (FieldName1FieldType [(nFieldWidth [, nPrecision])]  
      [NULL | NOT NULL]   
      [CHECK lExpression1 [ERROR cMessageText1]]  
      [DEFAULT eExpression1]  
      [PRIMARY KEY | UNIQUE]  
      [REFERENCES TableName2 [TAG TagName1]]  
      [NOCPTRANS]  
   [, FieldName2 ...]  
      [, PRIMARY KEY eExpression2 TAG TagName2  
      |, UNIQUE eExpression3 TAG TagName3]  
      [, FOREIGN KEY eExpression4 TAG TagName4 [NODUP]  
            REFERENCES TableName3 [TAG TagName5]]  
      [, CHECK lExpression2 [ERROR cMessageText2]])  
| FROM ARRAY ArrayName  
```  
  
## <a name="arguments"></a>Argomenti  
 CREATE TABLE &#124; DBF *TableName1*  
 Specifica il nome della tabella da creare. Le opzioni TABLE e DBF sono identiche.  
  
 NOME *LongTableName*  
 Specifica un nome lungo per la tabella. Un nome di tabella lungo può essere specificato solo quando un database è aperto, perché i nomi di tabella lunghi vengono archiviati nei database di.  
  
 I nomi lunghi possono contenere fino a 128 caratteri e possono essere utilizzati al posto di nomi di file brevi nel database.  
  
 FREE  
 Specifica che la tabella non verrà aggiunta a un database aperto. FREE non è obbligatorio se un database non è aperto.  
  
 *(FieldName1 FieldType* [( *NFieldWidth* [, *nPrecision*])]  
 Specifica rispettivamente il nome del campo, il tipo di campo, la larghezza del campo e la precisione del campo (numero di posizioni decimali).  
  
 *FieldType* è una singola lettera che indica il [tipo di dati](../../odbc/microsoft/visual-foxpro-field-data-types.md)del campo. Per alcuni tipi di dati del campo è necessario specificare *nFieldWidth* o *nPrecision* o entrambi.  
  
 *nFieldWidth* e *nPrecision* vengono ignorati per i tipi D, G, I, L, M, P, T e Y. il valore predefinito di *nPrecision* è zero (nessuna posizione decimale) se *nPrecision* non è incluso per i tipi B, F o N.  
  
 NULL  
 Consente valori null nel campo.  
  
 NOT NULL  
 Impedisce i valori null nel campo.  
  
 Se si omette NULL e NOT NULL, l'impostazione corrente di SET NULL determina se nel campo sono consentiti valori null. Tuttavia, se si omette NULL e NOT NULL e si include la clausola PRIMARY KEY o UNIQUE, l'impostazione corrente di SET NULL viene ignorata e il campo predefinito è NOT NULL.  
  
 CONTROLLARE *lExpression1*  
 Specifica una regola di convalida per il campo. *lExpression1* può essere una funzione definita dall'utente. Ogni volta che viene aggiunto un record vuoto, viene controllata la regola di convalida. Se la regola di convalida non consente un valore di campo vuoto in un record accodato, viene generato un errore.  
  
 ERRORE *cMessageText1*  
 Specifica il messaggio di errore visualizzato in Visual FoxPro quando la regola di campo genera un errore. Il messaggio viene visualizzato solo quando i dati vengono modificati all'interno di una finestra di esplorazione o di modifica.  
  
 *EEXPRESSION1* predefinito  
 Specifica un valore predefinito per il campo. Il tipo di dati di *eExpression1* deve corrispondere al tipo di dati del campo.  
  
 PRIMARY KEY  
 Crea un indice primario per il campo. Il tag di indice primario ha lo stesso nome del campo.  
  
 UNIQUE  
 Crea un indice candidato per il campo. Il tag index candidato ha lo stesso nome del campo.  
  
> [!NOTE]  
>  Gli indici candidati (creati includendo l'opzione UNIQUE in CREATE TABLE o ALTER TABLE-SQL) non sono gli stessi degli indici creati con l'opzione UNIQUE del comando INDEX. Un indice creato con l'opzione UNIQUE del comando INDEX consente la duplicazione delle chiavi di indice. gli indici candidati non consentono chiavi di indice duplicate. Per ulteriori informazioni sull'opzione UNIVOCa, vedere [index](../../odbc/microsoft/index-command.md) .  
  
 I valori null e i record duplicati non sono consentiti in un campo usato per un indice primario o candidato. Tuttavia, Visual FoxPro non genererà un errore se si crea un indice primario o candidato per un campo che supporta valori null. Visual FoxPro genererà un errore se si tenta di immettere un valore null o duplicato in un campo usato per un indice primario o candidato.  
  
 REFERENCEs *TableName2*[tag *TagName1*]  
 Specifica la tabella padre a cui viene stabilita una relazione permanente. Se si omette il TAG *TagName1*, la relazione viene stabilita usando la chiave di indice primaria della tabella padre. Se la tabella padre non dispone di un indice primario, Visual FoxPro genera un errore.  
  
 Includere il TAG *TagName1* per stabilire una relazione basata su un tag di indice esistente per la tabella padre. I nomi dei tag di indice possono contenere un massimo di 10 caratteri.  
  
 La tabella padre non può essere una tabella gratuita.  
  
 NOCPTRANS  
 Impedisce la conversione in una tabella codici diversa per i campi di tipo carattere e memo. Se la tabella viene convertita in un'altra tabella codici, i campi per cui è stato specificato NOCPTRANS non vengono convertiti. NOCPTRANS può essere specificato solo per i campi di tipo carattere e memo.  
  
 Nell'esempio seguente viene creata una tabella denominata MyTable contenente due campi di tipo carattere e due campi di tipo Memo. Il secondo campo carattere, CHAR2, e il secondo campo Memo, Memo2, includono NOCPTRANS per impedire la conversione.  
  
```  
CREATE TABLE mytable (char1 C(10), char2 C(10) NOCPTRANS,;  
   memo1 M, memo2 M NOCPTRANS)  
```  
  
 CHIAVE primaria *eExpression2* tag *TagName2*  
 Specifica un indice primario da creare. *eExpression2* specifica un campo o una combinazione di campi nella tabella. TAG *TagName2* specifica il nome del tag di indice primario creato. I nomi dei tag di indice possono contenere un massimo di 10 caratteri.  
  
 Poiché una tabella può avere un solo indice primario, non è possibile includere questa clausola se è già stato creato un indice primario per un campo. Visual FoxPro genera un errore se si include più di una clausola PRIMARY KEY in CREATE TABLE.  
  
 TAG *EEXPRESSION3* univoco *TagName3*  
 Crea un indice candidato. *eExpression3* specifica un campo o una combinazione di campi nella tabella. Tuttavia, se è stato creato un indice primario con una delle opzioni di chiave primaria, non è possibile includere il campo specificato per l'indice primario. TAG *TagName3* specifica un nome di tag per il tag di indice candidato creato. I nomi dei tag di indice possono contenere un massimo di 10 caratteri.  
  
 Una tabella può avere più indici candidati.  
  
 CHIAVE esterna *eExpression4* tag *TagName4*[NODUP]  
 Crea un indice esterno (non primario) e stabilisce una relazione con una tabella padre. *eExpression4* specifica l'espressione di chiave dell'indice esterno e *TagName4* specifica il nome del tag di chiave dell'indice esterno creato. I nomi dei tag di indice possono contenere un massimo di 10 caratteri. Includere NODUP per creare un indice esterno candidato.  
  
 È possibile creare più indici esterni per la tabella, ma le espressioni di indice esterno devono specificare campi diversi nella tabella.  
  
 REFERENCEs *TableName3*[tag *TagName5*]  
 Specifica la tabella padre a cui viene stabilita una relazione permanente. Includere il TAG *TagName5* per stabilire una relazione basata su un tag di indice per la tabella padre. I nomi dei tag di indice possono contenere un massimo di 10 caratteri. Per impostazione predefinita, se si omette TAG *TagName5,* la relazione viene stabilita usando la chiave di indice primaria della tabella padre.  
  
 CHECK *eExpression2*[Error *cMessageText2*]  
 Specifica la regola di convalida della tabella. ERRORE *cMessageText2* specifica il messaggio di errore visualizzato in Visual FoxPro quando viene eseguita la regola di convalida della tabella. Il messaggio viene visualizzato solo quando i dati vengono modificati all'interno di una finestra di esplorazione o di modifica.  
  
 DA ARRAY  arrayName  
 Specifica il nome di una matrice esistente il cui contenuto è il nome, il tipo, la precisione e la scala per ogni campo della tabella. Il contenuto della matrice può essere definito con la funzione **distanti ()**.  
  
## <a name="remarks"></a>Commenti  
 La nuova tabella viene aperta nell'area di lavoro disponibile più bassa ed è possibile accedervi tramite il relativo alias. La nuova tabella viene aperta esclusivamente, indipendentemente dall'impostazione corrente di SET EXCLUSIVe.  
  
 Se un database è aperto e non si include la clausola FREE, la nuova tabella verrà aggiunta al database. Non è possibile creare una nuova tabella con lo stesso nome di una tabella nel database.  
  
 Se un database è aperto, CREATE TABLE-SQL richiede l'utilizzo esclusivo del database. Per aprire un database per l'utilizzo esclusivo, includere esclusivo nel DATABASE OPEN.  
  
 Se un database non è aperto quando si crea la nuova tabella, incluse le clausole NAME, CHECK, DEFAULT, FOREIGN KEY, PRIMARY KEY o REFERENCEs, viene generato un errore.  
  
> [!NOTE]  
>  CREATE TABLE sintassi usa le virgole per separare alcune opzioni di CREATE TABLE. Inoltre, le clausole NULL, NOT NULL, CHECK, DEFAULT, PRIMARY KEY e UNIQUE devono essere inserite tra parentesi contenenti le definizioni delle colonne.  
  
## <a name="driver-remarks"></a>Osservazioni del driver  
 Quando l'applicazione invia l'istruzione SQL ODBC CREATE TABLE all'origine dati, il driver ODBC Visual FoxPro converte il comando nel comando TABLE di Visual FoxProCREATE utilizzando la sintassi illustrata nella tabella seguente.  
  
|Sintassi ODBC|Sintassi di Visual FoxPro|  
|-----------------|--------------------------|  
|CREATE TABLE *nome-tabella-base*<br /><br /> (*tipo di dati identificatore di colonna* )<br /><br /> [NOT NULL]<br /><br /> [,*tipo di dati identificatore di colonna*<br /><br /> [NOT NULL]...)|CREATE TABLE *TableName1* [nome *LongTableName*]<br /><br /> (*FieldName1* *FieldType*<br /><br /> [(*nFieldWidth* [, *nPrecision*])]<br /><br /> [NOT NULL])|  
  
 Quando si crea una tabella utilizzando il driver, il driver chiude la tabella immediatamente dopo la creazione per consentire l'accesso alla tabella da parte di altri utenti. Questo comportamento è diverso da Visual FoxPro, che lascia la tabella aperta esclusivamente al momento della creazione. Tuttavia, se viene eseguito un stored procedure sull'origine dati contenente un'istruzione CREATE TABLE, la tabella viene lasciata aperta.  
  
 Se l'origine dati è un database (file con estensione DBC), il driver ODBC Visual FoxPro crea una tabella denominata *LongTableName* con lo stesso nome della *tabella di base*.  
  
### <a name="using-data-definition-language-ddl"></a>Utilizzo di DDL (Data Definition Language)  
 Non è possibile includere DDL nelle posizioni seguenti:  
  
-   In un'istruzione SQL batch che richiede una transazione  
  
-   In modalità di commit manuale, dopo un'istruzione che ha richiesto una transazione, a meno che l'applicazione prima chiami **SQLTransact**.  
  
 Se, ad esempio, si desidera creare una tabella temporanea, è necessario creare la tabella prima di iniziare l'istruzione che richiede una transazione. Se si include l'istruzione CREATE TABLE in un'istruzione SQL batch che richiede una transazione, il driver restituisce un messaggio di errore.  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE-comando SQL](../../odbc/microsoft/alter-table-sql-command.md)   
 [Tipi di dati supportati (driver ODBC Visual FoxPro)](../../odbc/microsoft/supported-data-types-visual-foxpro-odbc-driver.md)   
 [Comando INSERT-SQL](../../odbc/microsoft/insert-sql-command.md)   
 [SELECT (comando SQL)](../../odbc/microsoft/select-sql-command.md)
