---
description: DENY (Transact-SQL)
title: DENY (Transact-SQL)
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DENY
- DENY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- schema-level securables [SQL Server]
- security [SQL Server], denying access
- DENY statement
- permissions [SQL Server], denying
- server-level securables [SQL Server]
- inherited security settings [SQL Server]
- denying permissions [SQL Server], DENY statement
- principal security [SQL Server]
- database-level securables [SQL Server]
- denying permissions [SQL Server]
ms.assetid: c32d1e01-9ee9-4665-a516-fcfece58078e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0c07bd840b819a6bb25624c14584089fdcfc2ac8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159357"
---
# <a name="deny-transact-sql"></a>DENY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Nega un'autorizzazione a un'entità. Impedisce a tale entità di ereditare l'autorizzazione tramite l'appartenenza al ruolo o al gruppo. DENY ha la precedenza su tutte le autorizzazioni, ma non si applica ai proprietari di oggetti o ai membri del ruolo predefinito del server sysadmin.
  **Nota sulla sicurezza**: non è possibile negare le autorizzazioni ai membri del ruolo predefinito del server sysadmin e ai proprietari di oggetti.
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for DENY  
DENY   { ALL [ PRIVILEGES ] } 
     | <permission>  [ ( column [ ,...n ] ) ] [ ,...n ]  
    [ ON [ <class> :: ] securable ] 
    TO principal [ ,...n ]   
    [ CASCADE] [ AS principal ]  
[;]

<permission> ::=  
{ see the tables below }  
  
<class> ::=  
{ see the tables below }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DENY   
    <permission> [ ,...n ]  
    [ ON [ <class_> :: ] securable ]   
    TO principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
<permission> ::=  
{ see the tables below }  
  
<class> ::=  
{  
      LOGIN  
    | DATABASE  
    | OBJECT  
    | ROLE  
    | SCHEMA  
    | USER  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 ALL  
 Questa opzione non nega tutte le autorizzazioni possibili. Negare ALL equivale a negare le autorizzazioni seguenti.  
  
-   Se l'entità a protezione diretta è un database, ALL corrisponde a BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE e CREATE VIEW.  
  
-   Se l'entità a protezione diretta è una funzione scalare, ALL corrisponde a EXECUTE e REFERENCES.  
  
-   Se l'entità a protezione diretta è una funzione con valori di tabella, ALL corrisponde a DELETE, INSERT, REFERENCES, SELECT e UPDATE.  
  
-   Se l'entità a protezione diretta è una stored procedure, ALL corrisponde a EXECUTE.  
  
-   Se l'entità a protezione diretta è una tabella, ALL corrisponde a DELETE, INSERT, REFERENCES, SELECT e UPDATE.  
  
-   Se l'entità a protezione diretta è una vista, ALL corrisponde a DELETE, INSERT, REFERENCES, SELECT e UPDATE.  
  
> [!NOTE]  
>  La sintassi DENY ALL è deprecata. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]Negare invece autorizzazioni specifiche.  
  
 PRIVILEGES  
 Opzione inclusa per compatibilità con lo standard ISO. Non modifica il funzionamento di ALL.  
  
 *permission*  
 Nome di un'autorizzazione. I mapping validi tra le autorizzazioni e le entità a protezione diretta sono descritti negli argomenti correlati elencati di seguito.  
  
 *column*  
 Specifica il nome di una colonna in una tabella per la quale vengono negate le autorizzazioni. È necessario utilizzare le parentesi ().  
  
 *class*  
 Specifica la classe dell'entità a protezione diretta per la quale viene negata l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
 *securable*  
 Specifica l'entità a protezione diretta per cui viene negata l'autorizzazione.  
  
 TO *principal*  
 Nome di un'entità. Le entità alle quali possono essere negate le autorizzazioni per un'entità a protezione diretta variano in base all'entità a protezione diretta. Per le combinazioni valide, vedere gli argomenti relativi alle entità a protezione diretta elencati di seguito.  
  
 CASCADE  
 Indica che l'autorizzazione viene negata all'entità specificata e a tutte le entità alle quali l'entità ha concesso l'autorizzazione. Obbligatorio quando l'entità dispone dell'autorizzazione con GRANT OPTION.  
  
 AS *principal*  
 Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di negare l'autorizzazione.
Usare la clausola AS principal per indicare che l'entità registrata come l'utente che nega l'autorizzazione deve essere un'entità diversa dalla persona che esegue l'istruzione. Si supponga ad esempio che l'utente Mary sia principal_id 12 e l'utente Raul sia principal 15. Mary esegue `DENY SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;`. Ora la tabella sys.database_permissions indicherà l'utente 15 (Raul) come grantor_principal_id dell'istruzione di negazione anche se l'istruzione è stata effettivamente eseguita dall'utente 13 (Mary).
  
L'uso di AS in questa istruzione non implica la possibilità di rappresentare un altro utente.  
  
## <a name="remarks"></a>Osservazioni  
 La sintassi completa dell'istruzione DENY è complessa. Il diagramma della sintassi precedente è stato semplificato per evidenziarne la struttura. La sintassi completa per negare le autorizzazioni su entità a protezione diretta specifiche è descritta negli argomenti elencati di seguito.  
  
 DENY ha esito negativo se l'argomento CASCADE viene omesso quando si nega un'autorizzazione a un'entità a cui è stata concessa tale autorizzazione con l'opzione GRANT OPTION specificata.  
  
 La stored procedure di sistema sp_helprotect visualizza le autorizzazioni per un'entità a protezione diretta a livello di database.  
  
> [!CAUTION]  
>  Un'istruzione DENY a livello di tabella non ha la precedenza rispetto a un'istruzione GRANT a livello di colonna. Questa incongruenza nella gerarchia di autorizzazioni è stata mantenuta per garantire la compatibilità con le versioni precedenti Verrà rimosso in una versione futura.  
  
> [!CAUTION]  
>  Negando l'autorizzazione CONTROL per un database viene negata implicitamente anche l'autorizzazione CONNECT per il database. Un'entità a cui viene negata l'autorizzazione CONTROL per un database non può connettersi a tale database.  
  
> [!CAUTION]  
>  Se si nega l'autorizzazione CONTROL SERVER, viene implicitamente negata anche l'autorizzazione CONNECT SQL per il server. Un'entità a cui viene negata l'autorizzazione CONTROL SERVER per un server non può connettersi a tale server.  
  
## <a name="permissions"></a>Autorizzazioni  
 Il chiamante (o l'entità specificata con l'opzione AS) deve disporre dell'autorizzazione CONTROL per un'entità a protezione diretta o di un'autorizzazione di livello superiore che implichi l'autorizzazione CONTROL per tale entità a protezione diretta. Se si utilizza l'opzione AS, l'entità specificata deve essere proprietaria dell'entità a protezione diretta per la quale viene negata un'autorizzazione.  
  
 Gli utenti che dispongono dell'autorizzazione CONTROL SERVER, ad esempio i membri del ruolo predefinito del server sysadmin, possono negare qualsiasi autorizzazione per qualsiasi entità a protezione diretta nel server. Gli utenti che dispongono dell'autorizzazione CONTROL per il database, ad esempio i membri del ruolo predefinito del database db_owner, possono negare qualsiasi autorizzazione per ogni entità a protezione diretta nel database. Gli utenti che dispongono dell'autorizzazione CONTROL per uno schema possono negare qualsiasi autorizzazione per qualsiasi oggetto nello schema. Se si utilizza la clausola AS, l'entità specificata deve essere proprietaria dell'entità a protezione diretta per cui vengono negate le autorizzazioni.  
  
## <a name="examples"></a>Esempi

 Nella tabella seguente vengono elencate le entità a protezione diretta e gli argomenti in cui viene descritta la relativa sintassi specifica.  
  
|Entità a protezione diretta|Sintassi|
|----------|------|
|Ruolo applicazione|[DENY - Autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Assembly|[DENY - Autorizzazioni per assembly &#40;Transact-SQL&#41;](../../t-sql/statements/deny-assembly-permissions-transact-sql.md)|  
|Chiave asimmetrica|[DENY - Autorizzazioni per chiavi asimmetriche &#40;Transact-SQL&#41;](../../t-sql/statements/deny-asymmetric-key-permissions-transact-sql.md)|  
|Gruppo di disponibilità|[DENY - Autorizzazioni del gruppo di disponibilità &#40;Transact-SQL&#41;](../../t-sql/statements/deny-availability-group-permissions-transact-sql.md)|  
|Certificato|[DENY - Autorizzazioni per certificati &#40;Transact-SQL&#41;](../../t-sql/statements/deny-certificate-permissions-transact-sql.md)|  
|Contratto|[DENY - Autorizzazioni di Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Database|[DENY - Autorizzazioni per database &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-permissions-transact-sql.md)|  
|Credenziali con ambito database|[DENY - Credenziali con ambito database (Transact-SQL)](../../t-sql/statements/deny-database-scoped-credential-transact-sql.md)|  
|Endpoint|[DENY - Autorizzazioni per endpoint &#40;Transact-SQL&#41;](../../t-sql/statements/deny-endpoint-permissions-transact-sql.md)|  
|Catalogo full-text|[DENY - Autorizzazioni per il catalogo full-text &#40;Transact-SQL&#41;](../../t-sql/statements/deny-full-text-permissions-transact-sql.md)|  
|Elenco di parole non significative full-text|[DENY - Autorizzazioni per il catalogo full-text &#40;Transact-SQL&#41;](../../t-sql/statements/deny-full-text-permissions-transact-sql.md)|  
|Funzione|[DENY - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Login|[DENY - Autorizzazioni per entità server &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-principal-permissions-transact-sql.md)|  
|Tipo di messaggio|[DENY - Autorizzazioni di Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Oggetto|[DENY - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Coda|[DENY - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Associazione al servizio remoto|[DENY - Autorizzazioni di Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Ruolo|[DENY - Autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Route|[DENY - Autorizzazioni di Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|SCHEMA|[DENY - Autorizzazioni per schemi &#40;Transact-SQL&#41;](../../t-sql/statements/deny-schema-permissions-transact-sql.md)|  
|Elenco delle proprietà di ricerca|[DENY - Autorizzazioni per l'elenco delle proprietà di ricerca &#40;Transact-SQL&#41;](../../t-sql/statements/deny-search-property-list-permissions-transact-sql.md)|  
|Server|[DENY - Autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-permissions-transact-sql.md)|  
|Service|[DENY - Autorizzazioni di Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Stored Procedure|[DENY - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Chiave simmetrica|[DENY - Autorizzazioni per chiavi simmetriche &#40;Transact-SQL&#41;](../../t-sql/statements/deny-symmetric-key-permissions-transact-sql.md)|  
|Sinonimo|[DENY - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Oggetti di sistema|[DENY - autorizzazioni per oggetti di sistema &#40;Transact-SQL&#41;](../../t-sql/statements/deny-system-object-permissions-transact-sql.md)|  
|Tabella|[DENY - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Type|[DENY - Autorizzazioni per tipi &#40;Transact-SQL&#41;](../../t-sql/statements/deny-type-permissions-transact-sql.md)|  
|Utente|[DENY - Autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Visualizzazione|[DENY - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Raccolta di XML Schema|[DENY - Autorizzazioni per raccolte di XML Schema &#40;Transact-SQL&#41;](../../t-sql/statements/deny-xml-schema-collection-permissions-transact-sql.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)  
  
  
