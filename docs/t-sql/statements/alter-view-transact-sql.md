---
description: ALTER VIEW (Transact-SQL)
title: ALTER VIEW (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/25/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_VIEW_TSQL
- ALTER VIEW
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], modifying
- views [SQL Server], modifying
- modifying views
- ALTER VIEW statement
ms.assetid: 03eba220-13e2-49e3-bd9d-ea9df84dc28c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ece59fa96d7fc3f81df663456f2ba276f96708d0
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575343"
---
# <a name="alter-view-transact-sql"></a>ALTER VIEW (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifica una vista creata in precedenza. È possibile utilizzare questa istruzione anche per le viste indicizzate. ALTER VIEW non influisce sulle stored procedure o i trigger dipendenti e non comporta modifiche delle autorizzazioni.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER VIEW [ schema_name . ] view_name [ ( column [ ,...n ] ) ]   
[ WITH <view_attribute> [ ,...n ] ]   
AS select_statement   
[ WITH CHECK OPTION ] [ ; ]  
  
<view_attribute> ::=   
{   
    [ ENCRYPTION ]  
    [ SCHEMABINDING ]  
    [ VIEW_METADATA ]       
}   
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER VIEW [ schema_name . ] view_name [  ( column_name [ ,...n ] ) ]   
AS <select_statement>   
[;]  
``` 
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *schema_name*  
 Nome dello schema a cui appartiene la vista.  
  
 *view_name*  
 Vista da modificare.  
  
 *column*  
 Nome o elenco di nomi delimitato da virgole delle colonne che si desidera includere nella vista specificata.  
  
> [!IMPORTANT]  
>  Le autorizzazioni di colonna vengono mantenute solo se il nome delle colonne rimane uguale prima e dopo l'esecuzione dell'istruzione ALTER VIEW.  
  
> [!NOTE]  
>  Nelle colonne di una vista, le autorizzazioni assegnate per un nome di colonna rimangono valide tra istruzioni CREATE VIEW e ALTER VIEW, indipendentemente dall'origine dei dati sottostanti. Se ad esempio si concedono autorizzazioni per la colonna **SalesOrderID** con un'istruzione CREATE VIEW, è possibile usare un'istruzione ALTER VIEW per rinominare la colonna **SalesOrderID**, ad esempio come **OrderRef**, e mantenere comunque le autorizzazioni associate alla vista che usa la colonna **SalesOrderID**.  
  
 ENCRYPTION  
 **SI APPLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Crittografa le voci nella tabella [sys.syscomments](../../relational-databases/system-compatibility-views/sys-syscomments-transact-sql.md) contenenti il testo dell'istruzione ALTER VIEW. Se si utilizza WITH ENCRYPTION, la vista non viene pubblicata nell'ambito della replica di SQL Server.  
  
 SCHEMABINDING  
 Associa la vista allo schema della tabella o delle tabelle sottostanti. Se si specifica SCHEMABINDING non è possibile apportare modifiche alle tabelle di base, che influiscono sulla definizione della vista. In questi casi, è necessario modificare o eliminare la definizione della vista per rimuovere le dipendenze dalla tabella da modificare. Quando si usa SCHEMABINDING, l'argomento _select\_statement_ deve includere i nomi composti da due parti (_schema_ **.** _object_) delle tabelle, delle visualizzazioni o delle funzioni definite dall'utente a cui viene fatto riferimento. Tutti gli oggetti a cui viene fatto riferimento devono essere presenti nello stesso database.  
  
 Le viste o tabelle che fanno parte di una vista creata con la clausola SCHEMABINDING non possono essere eliminate, a meno che tale vista non venga eliminata o modificata in modo che non sia più associata a uno schema. In caso contrario, nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene generato un errore. Inoltre, le istruzioni ALTER TABLE eseguite su tabelle che fanno parte di viste associate a schema hanno esito negativo se modificano la definizione della vista.  
  
 VIEW_METADATA  
 Specifica che l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dovrà restituire alle API DB-Library, ODBC e OLE DB le informazioni sui metadati relativi alla vista anziché alla tabella o alle tabelle di base quando vengono richiesti metadati in modalità browse per una query che fa riferimento alla vista. I metadati in modalità browse sono metadati aggiuntivi restituiti dall'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] alle API DB-Library, ODBC e OLE DB sul lato client. Questi metadati consentono alle API sul lato client di implementare cursori sul lato client aggiornabili. I metadati in modalità browse includono informazioni sulla tabella di base a cui appartengono le colonne del set di risultati.  
  
 Per le viste create con VIEW_METADATA, i metadati in modalità browse restituiscono il nome della vista anziché i nomi delle tabelle di base nella descrizione delle colonne della vista nel set di risultati.  
  
 Quando si crea una vista con l'opzione VIEW_METADATA, tutte le relative colonne, tranne una colonna **timestamp**, risultano aggiornabili se la vista include trigger INSERT o UPDATE INSTEAD OF. Per altre informazioni, vedere la sezione Osservazioni in [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 AS  
 Azioni che la vista deve eseguire.  
  
 *select_statement*  
 Istruzione SELECT che definisce la vista.  
  
 WITH CHECK OPTION  
 Forza il rispetto del set di criteri specificato in *select_statement* per tutte le istruzioni di modifica dei dati eseguite sulla vista.  
  
## <a name="remarks"></a>Commenti  
 Per altre informazioni su ALTER VIEW, vedere la sezione Osservazioni in [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
> [!NOTE]  
>  Se la definizione precedente della vista è stata creata con WITH ENCRYPTION o CHECK OPTION, queste opzioni vengono abilitate solo se sono incluse nell'istruzione ALTER VIEW.  
  
 Se si modifica una vista in uso con ALTER VIEW, nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene accettato un blocco di schema esclusivo sulla vista. Quando viene concesso il blocco e non esistono utenti attivi della vista, [!INCLUDE[ssDE](../../includes/ssde-md.md)] elimina tutte le copie della vista dalla cache delle procedure. I piani esistenti che fanno riferimento alla vista rimangono nella cache, ma vengono ricompilati per chiamate successive.  
  
 È possibile utilizzare ALTER VIEW per viste indicizzate. Tuttavia, in questo caso vengono eliminati tutti gli indici nella vista, senza eccezioni.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire ALTER VIEW, è richiesta come minimo l'autorizzazione ALTER per OBJECT.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creata una vista contenente tutti i dipendenti e le relative date di assunzione, denominata `EmployeeHireDate`. Vengono concesse autorizzazioni alla vista, ma è richiesta una modifica dei requisiti per selezionare i dipendenti con date di assunzione precedenti alla data specificata. La vista viene quindi sostituita tramite l'istruzione `ALTER VIEW`.  
  
```sql 
USE AdventureWorks2012 ;  
GO  
CREATE VIEW HumanResources.EmployeeHireDate  
AS  
SELECT p.FirstName, p.LastName, e.HireDate  
FROM HumanResources.Employee AS e JOIN Person.Person AS  p  
ON e.BusinessEntityID = p.BusinessEntityID ;  
GO  
```  
  
 È necessario modificare la vista per includere solo i dipendenti assunti prima del `2002`. Se non si utilizza ALTER VIEW, ma si elimina e ricrea la vista, sarà necessario specificare nuovamente l'istruzione GRANT utilizzata in precedenza e qualsiasi altra istruzione correlata alle autorizzazioni per questa vista.  
  
```sql  
ALTER VIEW HumanResources.EmployeeHireDate  
AS  
SELECT p.FirstName, p.LastName, e.HireDate  
FROM HumanResources.Employee AS e JOIN Person.Person AS p  
ON e.BusinessEntityID = p.BusinessEntityID  
WHERE HireDate < CONVERT(DATETIME,'20020101',101) ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md)   
 [Creazione di una stored procedure](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Apportare modifiche allo schema nei database di pubblicazione](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
