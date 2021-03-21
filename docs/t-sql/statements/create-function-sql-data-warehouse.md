---
description: CREATE FUNCTION (Azure Synapse Analytics)
title: CREATE FUNCTION (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 8cad1b2c-5ea0-4001-9060-2f6832ccd057
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 9fbbd79e7697924cc194100e83ab153e9511c86a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744481"
---
# <a name="create-function-azure-synapse-analytics"></a>CREATE FUNCTION (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Crea una funzione definita dall'utente in [!INCLUDE[ssSDW](../../includes/ssazuresynapse_md.md)] e nel [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Una funzione definita dall'utente è una routine [!INCLUDE[tsql](../../includes/tsql-md.md)] che accetta parametri, esegue un'azione, ad esempio un calcolo complesso, e restituisce il risultato di tale azione sotto forma di valore. In [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] il valore restituito deve essere un valore scalare (singolo). In [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] CREATE FUNCTION può restituire una tabella usando la sintassi per le funzioni inline con valori di tabella (anteprima) oppure può restituire un singolo valore usando la sintassi per le funzioni scalari. Utilizzare questa istruzione per creare una routine riutilizzabile che può essere utilizzata in queste modalità:  
  
-   Nelle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], ad esempio SELECT.  
  
-   Nelle applicazioni che chiamano la funzione.  
  
-   Nella definizione di un'altra funzione definita dall'utente.  
  
-   Per definire un vincolo CHECK su una colonna.  
  
-   Per sostituire una stored procedure.  
  
-   Usare una funzione inline come predicato di filtro per un criterio di sicurezza  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Transact-SQL Scalar Function Syntax  (in Azure Synapse Analytics and Parallel Data Warehouse)
CREATE FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN scalar_expression  
    END  
[ ; ]  
  
<function_option>::=   
{  
    [ SCHEMABINDING ]  
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
}  
```

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax (Preview in Azure Synapse Analytics only)
CREATE FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] parameter_data_type
    [ = default ] }
    [ ,...n ]
  ]
)
RETURNS TABLE
    [ WITH SCHEMABINDING ]
    [ AS ]
    RETURN [ ( ] select_stmt [ ) ]
[ ; ]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argomenti  
 *schema_name*  
 Nome dello schema a cui appartiene la funzione definita dall'utente.  
  
 *function_name*  
 Nome della funzione definita dall'utente. I nomi di funzione devono essere conformi alle regole per gli identificatori ed essere univoci all'interno del database e rispetto al relativo schema.  
  
> [!NOTE]  
>  È necessario apporre le parentesi dopo il nome della funzione anche se non viene specificato alcun parametro.  
  
 @*parameter_name*  
 Parametro della funzione definita dall'utente. È possibile dichiarare uno o più parametri.  
  
 Una funzione può avere al massimo 2.100 parametri. Il valore di ciascun parametro dichiarato deve essere specificato dall'utente quando viene eseguita la funzione, a meno che non venga definito un valore predefinito per tale parametro.  
  
 Specificare un nome di parametro utilizzando come primo carattere il simbolo di chiocciola (@). I nomi di parametro devono essere conformi alle regole per gli identificatori. I parametri sono locali rispetto alla funzione. È pertanto possibile utilizzare gli stessi nomi di parametro in altre funzioni. I parametri possono rappresentare solo costanti, non nomi di tabella, di colonna o di altri oggetti di database.  
  
> [!NOTE]  
>  ANSI_WARNINGS non viene applicata quando vengono passati parametri a una stored procedure, una funzione definita dall'utente oppure in caso di dichiarazione e impostazione delle variabili in un'istruzione batch. Se, ad esempio, la variabile viene definita come **char(3)** e quindi impostata su un valore maggiore di tre caratteri, i dati verranno troncati alla dimensione definita e l'istruzione INSERT o UPDATE avrà esito positivo.  
  
 *parameter_data_type*  
 Tipo di dati del parametro. Per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], sono consentiti tutti i tipi di dati scalari supportati in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Il tipo di dati timestamp (rowversion) non è supportato.  
  
 [ =*default* ]  
 Valore predefinito del parametro. Se viene definito un valore *default*, è possibile eseguire la funzione senza specificare un valore per il parametro corrispondente a tale valore.  
  
 Se a un parametro della funzione è associato un valore predefinito, alla chiamata della funzione è necessario specificare la parola chiave DEFAULT per recuperare il valore predefinito. Questo comportamento risulta diverso dall'utilizzo di parametri con valore predefinito nelle stored procedure in cui l'omissione del parametro implica l'utilizzo del valore predefinito.  
  
 *return_data_type*  
 Valore restituito di una funzione scalare definita dall'utente. Per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], sono consentiti tutti i tipi di dati scalari supportati in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Il tipo di dati timestamp (rowversion) non è supportato. I tipi non scalari cursore e tabella non sono consentiti.  
  
 *function_body*  
 Serie di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  L'argomento function_body non può contenere un'istruzione SELECT e non può fare riferimento a dati del database.  L'argomento function_body non può fare riferimento a tabelle o viste. Il corpo della funzione può chiamare altre funzioni deterministiche, ma non è possibile chiamare funzioni non deterministiche. 
  
 Nelle funzioni scalari *function_body* corrisponde a una serie di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che in combinazione restituiscono un valore scalare.  
  
 *scalar_expression*  
 Specifica il valore scalare restituito dalla funzione scalare.  

 *select_stmt* **SI APPLICA A**: Azure Synapse Analytics  
 Istruzione SELECT singola che definisce il valore restituito di una funzione inline con valori di tabella (anteprima).

 TABLE **SI APPLICA A**: Azure Synapse Analytics  
 Specifica che il valore restituito della funzione con valori di tabella è una tabella. Alle funzioni con valori di tabella è possibile passare solo costanti e @*local_variables*.

 Nelle funzioni inline con valori di tabella il valore restituito di TABLE viene definito tramite una singola istruzione SELECT. Alle funzioni inline non sono associate variabili restituite.
  
 **\<function_option>::=** 
  
 Specifica che la funzione includerà una o più delle opzioni seguenti.  
  
 SCHEMABINDING  
 Specifica che la funzione è associata agli oggetti di database a cui fa riferimento. Quando la clausola SCHEMABINDING viene specificata, non è possibile apportare agli oggetti di base modifiche che hanno effetto sulla definizione della funzione. È necessario prima modificare o eliminare la definizione della funzione per rimuovere le dipendenze dall'oggetto da modificare.  
  
 L'associazione della funzione agli oggetti cui fa riferimento viene rimossa solo quando viene eseguita una delle azioni seguenti:  
  
-   La funzione viene eliminata.  
  
-   La funzione viene modificata tramite l'istruzione ALTER senza specificare l'opzione SCHEMABINDING.  
  
 Una funzione può essere associata a uno schema solo se vengono soddisfatte le condizioni seguenti:  
  
-   Le funzioni definite dall'utente a cui la funzione fa riferimento sono anch'esse associate a uno schema.  
  
-   Alle funzioni e alle altre funzioni definite dall'utente a cui fa riferimento la funzione viene fatto riferimento tramite un nome composto da una o due parti.  
  
-   Alle funzioni predefinite e alle altre funzioni definite dall'utente nello stesso database è possibile fare riferimento solo all'interno del corpo di funzioni definite dall'utente.  
  
-   L'utente che ha eseguito l'istruzione CREATE FUNCTION dispone dell'autorizzazione REFERENCES per gli oggetti di database a cui la funzione fa riferimento.  
  
 Per rimuovere SCHEMABINDING usare ALTER  
  
 RETURNS NULL ON NULL INPUT | **CALLED ON NULL INPUT**  
 Specifica l'attributo **OnNULLCall** di una funzione a valori scalari. Se omesso, viene utilizzata l'opzione CALLED ON NULL INPUT per impostazione predefinita. Ciò significa che viene eseguito il corpo della funzione anche se come argomento viene passato NULL.  
  
## <a name="best-practices"></a>Procedure consigliate  
 Se una funzione definita dall'utente non viene creata tramite la clausola SCHEMABINDING, le modifiche apportate agli oggetti sottostanti possono influire sulla definizione della funzione e produrre risultati imprevisti quando viene richiamata. È consigliabile implementare uno dei metodi seguenti per assicurarsi che la funzione non diventi obsoleta in seguito a modifiche degli oggetti sottostanti:  
  
-   Specificare la clausola WITH SCHEMABINDING quando si crea la funzione. In questo modo, gli oggetti a cui si fa riferimento nella definizione della funzione possono essere modificati solo se viene modificata anche la funzione.  
  
## <a name="interoperability"></a>Interoperabilità  
 In una funzione a valori scalari sono valide le istruzioni seguenti:  
  
-   Istruzioni di assegnazione.  
  
-   Istruzioni per il controllo di flusso, escluse le istruzioni TRY...CATCH.  
  
-   Istruzioni DECLARE che definiscono le variabili dati locali.  

In una funzione inline con valori di tabella (anteprima) è consentita una sola istruzione SELECT.
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Non è possibile utilizzare funzioni definite dall'utente per eseguire azioni che modificano lo stato del database.  
  
 È possibile nidificare le funzioni definite dall'utente, ovvero una funzione definita dall'utente ne può richiamare un'altra. Il livello di nidificazione aumenta all'avvio della funzione richiamata e diminuisce al termine dell'esecuzione della funzione. Le funzioni definite dall'utente possono essere nidificate fino a un massimo di 32 livelli. Se viene superato il livello massimo di nidificazioni, l'intera sequenza di funzioni chiamanti ha esito negativo.   
  
## <a name="metadata"></a>Metadati  
 Nella sezione seguente vengono elencate le viste del catalogo di sistema usate per restituire i metadati sulle funzioni definite dall'utente.  
  
 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md): Visualizza la definizione delle funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] definite dall'utente. Ad esempio:  
  
```sql  
SELECT definition, type   
FROM sys.sql_modules AS m  
JOIN sys.objects AS o   
    ON m.object_id = o.object_id   
    AND type = ('FN');  
GO  
  
```  
  
 [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md): Visualizza le informazioni sui parametri definiti nelle funzioni definite dall'utente.  
  
 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md): Visualizza gli oggetti sottostanti a cui fa riferimento una funzione.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessario disporre dell'autorizzazione CREATE FUNCTION nel database e dell'autorizzazione ALTER per lo schema in cui la funzione è in fase di creazione.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-a-scalar-valued-user-defined-function-to-change-a-data-type"></a>R. Uso di una funzione definita dall'utente a valori scalari per modificare un tipo di dati  
 Questa semplice funzione accetta un tipo di dati **int** come input e restituisce un tipo di dati **decimal(10,2)** come output.  
  
```sql  
CREATE FUNCTION dbo.ConvertInput (@MyValueIn int)  
RETURNS decimal(10,2)  
AS  
BEGIN  
    DECLARE @MyValueOut int;  
    SET @MyValueOut= CAST( @MyValueIn AS decimal(10,2));  
    RETURN(@MyValueOut);  
END;  
GO  
  
SELECT dbo.ConvertInput(15) AS 'ConvertedValue';  
```  

## <a name="examples-sssdwfull"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]  

### <a name="a-creating-an-inline-table-valued-function-preview"></a>R. Creazione di una funzione inline con valori di tabella (anteprima)
 L'esempio seguente crea una funzione inline con valori di tabella per restituire alcune informazioni chiave nei moduli, applicando un filtro in base al parametro `objectType`. Include un valore predefinito per restituire tutti i moduli quando la funzione viene chiamata con il parametro DEFAULT. Questo esempio usa alcune delle viste del catalogo di sistema indicate in [Metadati](#metadata).

```sql
CREATE FUNCTION dbo.ModulesByType(@objectType CHAR(2) = '%%')
RETURNS TABLE
AS
RETURN
(
    SELECT 
        sm.object_id AS 'Object Id',
        o.create_date AS 'Date Created',
        OBJECT_NAME(sm.object_id) AS 'Name',
        o.type AS 'Type',
        o.type_desc AS 'Type Description', 
        sm.definition AS 'Module Description'
    FROM sys.sql_modules AS sm  
    JOIN sys.objects AS o ON sm.object_id = o.object_id
    WHERE o.type like '%' + @objectType + '%'
);
GO
```
La funzione può quindi essere chiamata per restituire tutti gli oggetti View (**V**) con:
```sql
select * from dbo.ModulesByType('V');
```

### <a name="b-combining-results-of-an-inline-table-valued-function-preview"></a>B. Combinazione dei risultati di una funzione inline con valori di tabella (anteprima)
 Questo semplice esempio usa la funzione inline con valori di tabella creata in precedenza per dimostrare in che modo è possibile combinare i risultati con altre tabelle usando Cross Apply. In questo caso vengono selezionate tutte le colonne di entrambi gli oggetti sys.object e i risultati di `ModulesByType` per tutte le righe corrispondenti nella colonna del *tipo*. Per altri dettagli sull'uso di apply, vedere la [clausola FROM con JOIN, APPLY, PIVOT](../../t-sql/queries/from-transact-sql.md).

```sql
SELECT * 
FROM sys.objects o
CROSS APPLY dbo.ModulesByType(o.type);
GO
```
  
## <a name="see-also"></a>Vedere anche  
 [ALTER FUNCTION (SQL Server PDW)](/previous-versions/sql/)   
 [DROP FUNCTION (SQL Server PDW)](/previous-versions/sql/)  
  
  

