---
description: ALTER PROCEDURE (Transact-SQL)
title: ALTER PROCEDURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_PROCEDURE_TSQL
- ALTER_PROC_TSQL
- ALTER PROC
- ALTER PROCEDURE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PROCEDURE statement
- stored procedure modifications [SQL Server]
- modifying stored procedures
- stored procedures [SQL Server], modifying
ms.assetid: ed9b2f76-11ec-498d-a95e-75b490a75733
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 10491ba478d5aec85f6e213756ebe00e9904b59d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746421"
---
# <a name="alter-procedure-transact-sql"></a>ALTER PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Viene modificata una procedura creata in precedenza tramite l'istruzione CREATE PROCEDURE in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name [ ; number ]   
    [ { @parameter [ type_schema_name. ] data_type }   
        [ VARYING ] [ = default ] [ OUT | OUTPUT ] [READONLY]  
    ] [ ,...n ]   
[ WITH <procedure_option> [ ,...n ] ]  
[ FOR REPLICATION ]   
AS { [ BEGIN ] sql_statement [;] [ ...n ] [ END ] }  
[;]  
  
<procedure_option> ::=   
    [ ENCRYPTION ]  
    [ RECOMPILE ]  
    [ EXECUTE AS Clause ]  
```  
  
```syntaxsql  
-- Syntax for SQL Server CLR Stored Procedure  
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name [ ; number ]   
    [ { @parameter [ type_schema_name. ] data_type }   
        [ = default ] [ OUT | OUTPUT ] [READONLY]  
    ] [ ,...n ]   
[ WITH EXECUTE AS Clause ]  
AS { EXTERNAL NAME assembly_name.class_name.method_name }  
[;]  
```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name  
    [ { @parameterdata_type } [= ] ] [ ,...n ]  
AS { [ BEGIN ] sql_statement [ ; ] [ ,...n ] [ END ] }  
[;]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *schema_name*  
 Nome dello schema a cui appartiene la procedura.  
  
 *procedure_name*  
 Nome della procedura da modificare. I nomi delle procedure devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md).  
  
 **;** *number*  
 Integer facoltativo esistente usato per raggruppare procedure con lo stesso nome in modo da poter rimuoverle tramite un'unica istruzione DROP PROCEDURE.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 **@** *parameter*  
 Parametro della procedura. È possibile specificare un massimo di 2.100 parametri.  
  
 [ _type\_schema\_name_ **.** ] _data\_type_  
 Tipo di dati del parametro e schema a cui appartiene.  
  
 Per informazioni sulle restrizioni dei tipi di dati, vedere [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
 VARYING  
 Specifica il set di risultati supportato come parametro di output. Questo parametro viene creato in modo dinamico dalla stored procedure e il relativo contenuto può variare. Viene usato solo con parametri di cursore. Questa opzione non è valida per le procedure CLR.  
  
 *default*  
 Valore predefinito del parametro.  
  
 OUT | OUTPUT  
 Indica che si tratta di un parametro restituito.  
  
 READONLY  
 Indica che il parametro non può essere aggiornato o modificato all'interno del corpo della procedura. Se si tratta di un tipo di parametro con valori di tabella, è necessario specificare la parola chiave READONLY.  
  
 RECOMPILE  
 Indica che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] non memorizza nella cache un piano per la procedura e che la procedura viene ricompilata in fase di esecuzione.  
  
 ENCRYPTION  
 **Si applica a**: SQL Server ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
 Indica che il testo originale dell'istruzione ALTER PROCEDURE verrà convertito dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] in un formato offuscato. L'output dell'offuscamento non è visibile direttamente nelle viste del catalogo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Gli utenti che non hanno accesso a tabelle di sistema o file del database non possono recuperare il testo offuscato. Il testo è tuttavia disponibile per gli utenti con privilegi che consentono l'accesso alle tabelle di sistema attraverso la [porta DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) o l'accesso diretto a file del database. Gli utenti in grado di collegare un debugger al processo del server possono inoltre recuperare la procedura originale dalla memoria in fase di esecuzione. Per altre informazioni sull'accesso ai metadati di sistema, vedere [Configurazione della visibilità dei metadati](../../relational-databases/security/metadata-visibility-configuration.md).  
  
 Le procedure create con questa opzione non possono essere pubblicate durante la replica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Questa opzione non può essere specificata per stored procedure CLR (Common Language Runtime).  
  
> [!NOTE]  
>  Durante un aggiornamento [!INCLUDE[ssDE](../../includes/ssde-md.md)] usa i commenti offuscati archiviati in **sys.sql_modules** per ricreare procedure.  
  
 EXECUTE AS  
 Specifica il contesto di sicurezza in cui deve essere eseguita la stored procedure dopo l'accesso.  
  
 Per altre informazioni, vedere [Clausola EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
 FOR REPLICATION  

  
 Specifica che le stored procedure create per la replica non possono essere eseguite nel Sottoscrittore. Una stored procedure creata con l'opzione FOR REPLICATION viene usata come filtro di stored procedure ed eseguita solo durante la replica. Se viene specificata l'opzione FOR REPLICATION, non è possibile dichiarare alcun parametro. Questa opzione non è valida per le procedure CLR. L'opzione RECOMPILE viene ignorata per le procedure create con l'opzione FOR REPLICATION.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 { [ BEGIN ] *sql_statement* [;] [ ...*n* ] [ END ] }  
 Una o più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che includono il corpo della procedura. Per racchiudere le istruzioni è possibile usare le parole chiave facoltative BEGIN ed END. Per altre informazioni, vedere le sezioni Procedure consigliate, Osservazioni generali e Limitazioni e restrizioni in [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
 EXTERNAL NAME _assembly\_name_ **.** _class\_name_ **.** _method\_name_  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica il metodo di un assembly [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] affinché una stored procedure CLR vi faccia riferimento. *class_name* deve essere un identificatore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido e deve esistere come classe nell'assembly. Se alla classe è stato assegnato un nome completo con lo spazio dei nomi le cui parti sono separate da un punto (**.**), il nome della classe deve essere delimitato tramite parentesi quadre (**[]**) o virgolette (**""**). Il metodo specificato deve essere un metodo statico della classe.  
  
 Per impostazione predefinita, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può eseguire il codice CLR. È possibile creare, modificare ed eliminare gli oggetti di database che fanno riferimento a moduli CLR; tuttavia non è possibile eseguire questi riferimenti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] finché non viene abilitata l'opzione [clr enabled option](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Per abilitare questa opzione, usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
> [!NOTE]  
>  Le procedure CLR non sono supportate in un database indipendente.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] in stored procedure CLR e viceversa.  
  
 ALTER PROCEDURE non modifica le autorizzazioni e non ha effetto sulle stored procedure o sui trigger dipendenti. Le impostazioni della sessione corrente per QUOTED_IDENTIFIER e ANSI_NULLS, tuttavia, vengono incluse nella stored procedure quando viene modificata. Se queste impostazioni sono diverse rispetto a quelle applicate quando la stored procedure è stata creata, il funzionamento della stored procedure potrebbe cambiare.  
  
 Se una definizione di procedura precedente è stata creata tramite l'opzione WITH ENCRYPTION o WITH RECOMPILE, tale opzione viene abilitata solo se è inclusa nell'istruzione ALTER PROCEDURE.  
  
 Per altre informazioni, sulle stored procedure, vedere [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione **ALTER** per la procedura o l'appartenenza al ruolo predefinito del database **db_ddladmin**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente si crea la stored procedure `uspVendorAllInfo`, che restituisce i nomi di tutti i fornitori di [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)], i prodotti da essi forniti nonché le informazioni relative alla posizione creditizia e alla disponibilità. Questa procedura viene quindi modificata in modo da restituire un set di risultati diverso.  
  
```sql
IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
    DROP PROCEDURE Purchasing.uspVendorAllInfo;  
GO  
CREATE PROCEDURE Purchasing.uspVendorAllInfo  
WITH EXECUTE AS CALLER  
AS  
    SET NOCOUNT ON;  
    SELECT v.Name AS Vendor, p.Name AS 'Product name',   
      v.CreditRating AS 'Rating',   
      v.ActiveFlag AS Availability  
    FROM Purchasing.Vendor v   
    INNER JOIN Purchasing.ProductVendor pv  
      ON v.BusinessEntityID = pv.BusinessEntityID   
    INNER JOIN Production.Product p  
      ON pv.ProductID = p.ProductID   
    ORDER BY v.Name ASC;  
GO    
```  
  
 Nell'esempio seguente viene modificata la stored procedure `uspVendorAllInfo`. Viene rimossa la clausola EXECUTE AS CALLER e modificato il corpo della procedura in modo da restituire solo i fornitori del prodotto specificato. Le funzioni `LEFT` e `CASE` personalizzano l'aspetto del set di risultati.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER PROCEDURE Purchasing.uspVendorAllInfo  
    @Product VARCHAR(25)   
AS  
    SET NOCOUNT ON;  
    SELECT LEFT(v.Name, 25) AS Vendor, LEFT(p.Name, 25) AS 'Product name',   
    'Rating' = CASE v.CreditRating   
        WHEN 1 THEN 'Superior'  
        WHEN 2 THEN 'Excellent'  
        WHEN 3 THEN 'Above average'  
        WHEN 4 THEN 'Average'  
        WHEN 5 THEN 'Below average'  
        ELSE 'No rating'  
        END  
    , Availability = CASE v.ActiveFlag  
        WHEN 1 THEN 'Yes'  
        ELSE 'No'  
        END  
    FROM Purchasing.Vendor AS v   
    INNER JOIN Purchasing.ProductVendor AS pv  
      ON v.BusinessEntityID = pv.BusinessEntityID   
    INNER JOIN Production.Product AS p   
      ON pv.ProductID = p.ProductID   
    WHERE p.Name LIKE @Product  
    ORDER BY v.Name ASC;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```   
 Vendor               Product name  Rating    Availability  
-------------------- ------------- -------   ------------  
Proseware, Inc.      LL Crankarm   Average   No  
Vision Cycles, Inc.  LL Crankarm   Superior  Yes  
(2 row(s) affected)`  
```  

## <a name="see-also"></a>Vedere anche  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Stored procedure &#40;Motore di database&#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)   
 [sys.procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)  
  
  
