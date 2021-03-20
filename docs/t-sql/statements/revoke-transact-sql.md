---
description: REVOKE (Transact-SQL)
title: REVOKE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REVOKE_TSQL
- REVOKE
dev_langs:
- TSQL
helpviewer_keywords:
- schema-level securables [SQL Server]
- REVOKE statement, Transact-SQL syntax
- removing permissions
- server-level securables [SQL Server]
- deleting permissions
- revoking permissions [SQL Server]
- REVOKE statement
- denying permissions [SQL Server], removing
- database-level securables [SQL Server]
- granting permissions [SQL Server], removing
- permissions [SQL Server], revoking
- dropping permissions
ms.assetid: 9d31d3e7-0883-45cd-bf0e-f0361bbb0956
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 40b950c34fa6cc506c0362e0fca8a531e5ce7423
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740201"
---
# <a name="revoke-transact-sql"></a>REVOKE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Rimuove un'autorizzazione precedentemente assegnata o negata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for REVOKE  
REVOKE [ GRANT OPTION FOR ]  
      {   
        [ ALL [ PRIVILEGES ] ]  
        |  
                permission [ ( column [ ,...n ] ) ] [ ,...n ]  
      }  
      [ ON [ class :: ] securable ]   
      { TO | FROM } principal [ ,...n ]   
      [ CASCADE] [ AS principal ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
REVOKE   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    [ FROM | TO ] principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
<permission> ::=  
{ see the tables below }  
  
<class_type> ::=  
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
 GRANT OPTION FOR  
 Indica che verrà revocata la capacità di concedere l'autorizzazione specificata. Argomento obbligatorio in caso di utilizzo dell'argomento CASCADE.  
  
> [!IMPORTANT]  
>  Se l'autorizzazione specificata è stata concessa all'entità senza l'opzione GRANT, l'autorizzazione stessa verrà revocata.  
  
 ALL  
**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive
  
 Questa opzione non revoca tutte le possibili autorizzazioni. La revoca di autorizzazioni tramite l'argomento ALL equivale alla revoca delle autorizzazioni seguenti.  
  
-   Se l'entità a protezione diretta è un database, ALL corrisponde a BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE e CREATE VIEW.  
  
-   Se l'entità a protezione diretta è una funzione scalare, ALL corrisponde a EXECUTE e REFERENCES.  
  
-   Se l'entità a protezione diretta è una funzione con valori di tabella, ALL corrisponde a DELETE, INSERT, REFERENCES, SELECT e UPDATE.  
  
-   Se l'entità a protezione diretta è una stored procedure, ALL corrisponde a EXECUTE.  
  
-   Se l'entità a protezione diretta è una tabella, ALL corrisponde a DELETE, INSERT, REFERENCES, SELECT e UPDATE.  
  
-   Se l'entità a protezione diretta è una vista, ALL corrisponde a DELETE, INSERT, REFERENCES, SELECT e UPDATE.  
  
> [!NOTE]  
>  La sintassi REVOKE ALL è deprecata. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Revocare invece autorizzazioni specifiche.  
  
 PRIVILEGES  
 Opzione inclusa per compatibilità con lo standard ISO. Non modifica il funzionamento di ALL.  
  
 *permission*  
 Nome di un'autorizzazione. I mapping validi delle autorizzazioni alle entità a protezione diretta vengono descritti negli argomenti riportati nella sezione [Sintassi specifica delle entità a protezione diretta](#securable) di seguito in questo argomento.  
  
 *column*  
 Specifica il nome di una colonna in una tabella alla quale vengono revocate le autorizzazioni. È necessario utilizzare le parentesi.  
  
 *class*  
 Specifica la classe dell'entità a protezione diretta alla quale viene revocata l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
 *securable*  
 Specifica l'entità a protezione diretta a cui viene revocata l'autorizzazione.  
  
 TO | FROM *principal*  
 Nome di un'entità. Le entità a cui è possibile revocare le autorizzazioni per un'entità a protezione diretta variano in base all'entità a protezione diretta specifica. Per altre informazioni sulle combinazioni valide, vedere gli argomenti elencati nella sezione [Sintassi specifica delle entità a protezione diretta](#securable) di seguito in questo argomento.  
  
 CASCADE  
 Indica che l'autorizzazione revocata viene revocata anche ad altre entità alle quali è stata concessa da questa entità. Se si utilizza l'argomento CASCADE, è inoltre necessario includere l'argomento GRANT OPTION FOR.  
  
> [!CAUTION]  
>  La revoca propagata di un'autorizzazione concessa con WITH GRANT OPTION comporterà la revoca sia delle autorizzazioni GRANT che delle autorizzazioni DENY per tale autorizzazione.  
  
 AS *principal*  
 Usare la clausola AS principal per indicare che si sta revocando un'autorizzazione che è stata concessa da un'altra entità. Si supponga ad esempio che l'utente Mary sia principal_id 12 e l'utente Raul sia principal_id 15. Sia Mary sia Raul concedono all'utente Steven la stessa autorizzazione. La tabella sys.database_permissions indica l'autorizzazione due volte con valori grantor_principal_id diversi. Mary può revocare l'autorizzazione usando la clausola `AS RAUL` per rimuovere la concessione dell'autorizzazione di Raul.
 
L'uso di AS in questa istruzione non implica la possibilità di rappresentare un altro utente.  
  
## <a name="remarks"></a>Osservazioni  
 La sintassi completa dell'istruzione REVOKE è complessa. Il diagramma della sintassi precedente è stato semplificato per evidenziarne la struttura. La sintassi completa per la revoca di autorizzazioni per entità a protezione diretta specifiche è descritta negli argomenti della sezione [Sintassi specifica delle entità a protezione diretta](#securable) di seguito in questo argomento.  
  
 È possibile utilizzare l'istruzione REVOKE per rimuovere le autorizzazioni concesse e l'istruzione DENY per negare un'autorizzazione specifica a un'entità anche in caso di esecuzione di un'istruzione GRANT.  
  
 La concessione di un'autorizzazione rimuove l'istruzione DENY o REVOKE di tale autorizzazione per l'entità a protezione diretta specificata. Se la stessa autorizzazione viene negata a un livello superiore e in tale livello è inclusa l'entità a protezione diretta, l'istruzione DENY ha la precedenza. Tuttavia, la revoca dell'autorizzazione concessa a un ambito superiore non ha la precedenza.  
  
> [!CAUTION]  
>  Un'istruzione DENY a livello di tabella non ha la precedenza rispetto a un'istruzione GRANT a livello di colonna. Questa incoerenza nella gerarchia delle autorizzazioni è stata mantenuta per compatibilità con le versioni precedenti. Verrà rimosso in una versione futura.  
  
 La stored procedure di sistema sp_helprotect restituisce le autorizzazioni per un'entità a protezione diretta a livello di database.  
  
 L'istruzione REVOKE avrà esito negativo se l'argomento CASCADE viene omesso in caso di revoca a un'entità di un'autorizzazione concessa tramite GRANT OPTION.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le entità con l'autorizzazione CONTROL per un'entità a protezione diretta possono revocare l'autorizzazione per quella entità. I proprietari degli oggetti possono revocare autorizzazioni per gli oggetti di cui sono proprietari.  
  
 Gli utenti che dispongono dell'autorizzazione CONTROL SERVER, ad esempio i membri del ruolo predefinito del server sysadmin, possono revocare qualsiasi autorizzazione per qualsiasi entità a protezione diretta nel server. Gli utenti che dispongono dell'autorizzazione CONTROL in un database, ad esempio i membri del ruolo predefinito del database db_owner, possono revocare qualsiasi autorizzazione per qualsiasi entità a protezione diretta nel database. Gli utenti che dispongono dell'autorizzazione CONTROL in uno schema possono revocare qualsiasi autorizzazione per qualsiasi oggetto all'interno dello schema.  
  
##  <a name="securable-specific-syntax"></a><a name="securable"></a> Sintassi specifica delle entità a protezione diretta  
 Nella tabella seguente vengono elencate le entità a protezione diretta e gli argomenti in cui viene descritta la relativa sintassi specifica.  
  
|Entità a protezione diretta|Argomento|  
|---------------|-----------|  
|Ruolo applicazione|[REVOKE - autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|Assembly|[REVOKE - Autorizzazioni per assembly &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-assembly-permissions-transact-sql.md)|  
|Chiave asimmetrica|[REVOKE - Autorizzazioni per chiavi asimmetriche &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-asymmetric-key-permissions-transact-sql.md)|  
|Gruppo di disponibilità|[REVOKE - autorizzazioni del gruppo di disponibilità &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-availability-group-permissions-transact-sql.md)|  
|Certificato|[REVOKE - Autorizzazioni per certificati &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-certificate-permissions-transact-sql.md)|  
|Contratto|[REVOKE - autorizzazioni per Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Database|[REVOKE - Autorizzazioni per database &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-database-permissions-transact-sql.md)|  
|Endpoint|[REVOKE - autorizzazioni per endpoint &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-endpoint-permissions-transact-sql.md)|  
|Credenziali con ambito database|[REVOKE - Credenziali con ambito database (Transact-SQL)](../../t-sql/statements/revoke-database-scoped-credential-transact-sql.md)|  
|Catalogo full-text|[REVOKE - Autorizzazioni per il catalogo full-text &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-full-text-permissions-transact-sql.md)|  
|Elenco di parole non significative full-text|[REVOKE - Autorizzazioni per il catalogo full-text &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-full-text-permissions-transact-sql.md)|  
|Funzione|[REVOKE - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Login|[Autorizzazioni per entità server REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-principal-permissions-transact-sql.md)|  
|Tipo di messaggio|[REVOKE - autorizzazioni per Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Oggetto|[REVOKE - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Coda|[REVOKE - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Associazione al servizio remoto|[REVOKE - autorizzazioni per Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Ruolo|[REVOKE - autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|Route|[REVOKE - autorizzazioni per Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|SCHEMA|[REVOKE - Autorizzazioni per schemi &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-schema-permissions-transact-sql.md)|  
|Elenco delle proprietà di ricerca|[REVOKE - Autorizzazioni per l'elenco delle proprietà di ricerca &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-search-property-list-permissions-transact-sql.md)|  
|Server|[REVOKE - autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md)|  
|Servizio|[REVOKE - autorizzazioni per Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Stored Procedure|[REVOKE - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Chiave simmetrica|[REVOKE - autorizzazioni per chiavi simmetriche &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-symmetric-key-permissions-transact-sql.md)|  
|Sinonimo|[REVOKE - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Oggetti di sistema|[REVOKE - autorizzazioni per oggetti di sistema &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-system-object-permissions-transact-sql.md)|  
|Tabella|[REVOKE - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Type|[REVOKE - Autorizzazioni per tipi &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-type-permissions-transact-sql.md)|  
|Utente|[REVOKE - autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|Visualizzazione|[REVOKE - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Raccolta di XML Schema|[REVOKE - autorizzazioni per raccolte di XML Schema &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-xml-schema-collection-permissions-transact-sql.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Gerarchia delle autorizzazioni &#40;Motore di database&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)  
  
  
