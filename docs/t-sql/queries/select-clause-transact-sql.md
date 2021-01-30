---
description: SELECT Clause (Transact-SQL)
title: SELECT Clause (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SELECT Clause
- SELECT_Clause_TSQL
- DISTINCT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- parentheses [SQL Server]
- identity columns [SQL Server], SELECT clause
- SELECT clause
- $IDENTITY keyword
- user-defined types [SQL Server], invoking methods and properties
- SELECT statement [SQL Server], processing orders
- clauses [SQL Server], SELECT
- $ROWGUID keyword
- queries [SQL Server], results
ms.assetid: 2616d800-4853-4cf1-af77-d32d68d8c2ef
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2a633f4cfb8a0c9e5158b24c89035c5e97588a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207598"
---
# <a name="select-clause-transact-sql"></a>SELECT Clause (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Specifica le colonne che devono essere restituite dalla query.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SELECT [ ALL | DISTINCT ]  
[ TOP ( expression ) [ PERCENT ] [ WITH TIES ] ]   
<select_list>   
<select_list> ::=   
    {   
      *   
      | { table_name | view_name | table_alias }.*   
      | {  
          [ { table_name | view_name | table_alias }. ]  
               { column_name | $IDENTITY | $ROWGUID }   
          | udt_column_name [ { . | :: } { { property_name | field_name }   
            | method_name ( argument [ ,...n] ) } ]  
          | expression  
          [ [ AS ] column_alias ]   
         }  
      | column_alias = expression   
    } [ ,...n ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **ALL**  
 Specifica che nel set di risultati possono essere visualizzate righe duplicate. Il valore predefinito è ALL.  
  
 DISTINCT  
 Specifica che nel set di risultati devono essere visualizzate solo righe univoche. I valori Null vengono considerati valori uguali.  
  
 TOP (*expression* ) [ PERCENT ] [ WITH TIES ]  
 Indica che dal set di risultati della query verrà restituito solamente un primo set specificato o una prima percentuale di righe specificata. Il valore di *expression* può essere specificato come numero o come percentuale di righe.  
  
 L'uso di TOP *expression* senza parentesi nelle istruzioni SELECT è supportato per compatibilità con le versioni precedenti, ma non è consigliato. Per altre informazioni, vedere [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
\< select_list > Colonne da selezionare per il set di risultati. Tale elenco è una serie di espressioni separate da virgola Il numero massimo di espressioni che è possibile specificare nell'elenco selezionato è 4096.  
  
 \*  
 Specifica che devono essere restituite tutte le colonne di tutte le tabelle e viste elencate nella clausola FROM. Le colonne vengono restituite in base alla tabella o vista, a seconda di quanto specificato nella clausola FROM, nello stesso ordine in cui sono visualizzate nella tabella o vista.  
  
 *table_name* | *view_name* | *table* _ *alias*.*  
 Limita l'ambito dell'argomento \* alla tabella o vista specificata.  
  
 *column_name*  
 Nome della colonna da restituire. Qualificare l'argomento *column_name* in modo da impedire riferimenti ambigui, come nel caso di due tabelle della clausola FROM che includono colonne con nomi duplicati. Ad esempio, entrambe le tabelle SalesOrderHeader e SalesOrderDetail nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] hanno una colonna ModifiedDate. Se le due tabelle sono unite in join in una query, la data modificata delle voci SalesOrderDetail può essere specificata nell'elenco di selezione come SalesOrderDetail.ModifiedDate.  
  
 *expression*  
 Costante, funzione o qualsiasi combinazione di nomi di colonna, costanti e funzioni collegati da uno o più operatori oppure da una sottoquery.  
  
 $IDENTITY  
 Restituisce la colonna Identity. Per altre informazioni, vedere [IDENTITY &#40;Property&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md), [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) e [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 Se più tabelle della clausola FROM includono una colonna con la proprietà IDENTITY, è necessario qualificare la funzione $IDENTITY con il nome della tabella, ad esempio T1.$IDENTITY.  
  
 $ROWGUID  
 Restituisce la colonna GUID della riga.  
  
 Se più tabelle della clausola FROM includono la proprietà ROWGUIDCOL, è necessario qualificare $ROWGUID con il nome della tabella, ad esempio T1.$ROWGUID.  
  
 *udt_column_name*  
 Nome di colonna CLR (Common Language Runtime) definito dall'utente da restituire.  
  
> [!NOTE]  
>  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] restituisce i valori dei tipi definiti dall'utente in una rappresentazione binaria. Per restituire i valori dei tipi definiti dall'utente in formato XML o stringa, usare [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) o [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 { . | :: }  
 Specifica un metodo, una proprietà o un campo CLR definito dall'utente. Usare . per metodi, proprietà o campi (non statici) di un'istanza. Utilizzare :: per metodi, proprietà o campi statici. Per richiamare un metodo, una proprietà o un campo di un tipo CLR definito dall'utente, è necessario disporre dell'autorizzazione EXECUTE per il tipo.  
  
 *property_name*  
 Proprietà pubblica di *udt_column_name*.  
  
 *field_name*  
 Membro dati pubblico di *udt_column_name*.  
  
 *method_name*  
 Metodo pubblico di *udt_column_name* che accetta uno o più argomenti. *method_name* non può essere un metodo mutatore.  
  
 Nell'esempio seguente vengono selezionati i valori per la colonna `Location`, definita come tipo `point`, dalla tabella `Cities`, richiamando un metodo del tipo `Distance`:  
  
```sql
CREATE TABLE dbo.Cities (  
     Name VARCHAR(20),  
     State VARCHAR(20),  
     Location POINT);  
GO  
DECLARE @p POINT (32, 23), @distance FLOAT;  
GO  
SELECT Location.Distance (@p)  
FROM Cities;  
```  
  
 *column_ alias*  
 Nome alternativo per la colonna specificata nel set di risultati della query. Per una colonna denominata quantity, ad esempio, è possibile specificare come alias Quantity, Quantity to Date o Qty.  
  
 Gli alias vengono utilizzati inoltre come nomi dei risultati di espressioni, ad esempio:  
  
 ```sql
 USE AdventureWorks2012;  
 GO  
 SELECT AVG(UnitPrice) AS [Average Price]  
 FROM Sales.SalesOrderDetail;
 ```  
  
 È possibile usare *column_alias* in una clausola ORDER BY, ma non in una clausola WHERE, GROUP BY o HAVING. Se l'espressione della query fa parte di un'istruzione DECLARE CURSOR, non è possibile usare l'argomento *column_alias* nella clausola FOR UPDATE.  
  
## <a name="remarks"></a>Osservazioni  
 La lunghezza dei dati restituiti per colonne di tipo **text** o **ntext** incluse nell'elenco di selezione viene impostata sul valore più basso tra i seguenti: dimensioni reali della colonna **text**, impostazione predefinita della sessione TEXTSIZE e limite dell'applicazione specificato a livello di codice. Per modificare la lunghezza del testo restituito per una sessione, utilizzare l'istruzione SET. Per impostazione predefinita il limite della lunghezza dei dati di testo restituiti con un'istruzione SELECT è pari a 4.000 byte.  
  
 In [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] viene generata l'eccezione 511 e viene eseguito il rollback dell'istruzione in fase di esecuzione se si verifica una delle condizioni seguenti:  
  
-   L'istruzione SELECT produce una riga di risultati o una tabella di lavoro intermedia di dimensioni superiori a 8.060 byte.  
  
-   L'istruzione DELETE, INSERT o UPDATE tenta di eseguire un'azione in una riga di dimensioni superiori a 8.060 byte.  
  
 Se non si assegna un nome a una colonna creata con un'istruzione SELECT INTO o CREATE VIEW, viene generato un errore.  
  
## <a name="see-also"></a>Vedere anche  
 [Esempi di istruzioni SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
  
  
