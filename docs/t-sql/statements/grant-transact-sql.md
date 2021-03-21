---
description: GRANT (Transact-SQL)
title: GRANT (Transact-SQL)
ms.custom: ''
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GRANT_TSQL
- GRANT
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], GRANT statement
- schema-level securables [SQL Server]
- GRANT statement
- cross-database permissions
- GRANT statement, about GRANT statement
- server-level securables [SQL Server]
- database-level securables [SQL Server]
- permissions [SQL Server], granting
ms.assetid: a760c16a-4d2d-43f2-be81-ae9315f38185
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: efdce09afb69773f068735356c83b88d5f61508d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740081"
---
# <a name="grant-transact-sql"></a>GRANT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Concede a un'entità autorizzazioni per un'entità a protezione diretta.  Il formato da usare è GRANT \<some permission> ON \<some object> TO \<some user, login, or group>. Per una descrizione generale delle autorizzazioni, vedere [Autorizzazioni &#40;Motore di database&#41;](../../relational-databases/security/permissions-database-engine.md).  
  
 ![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un articolo") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for GRANT  
GRANT { ALL [ PRIVILEGES ] }  
      | permission [ ( column [ ,...n ] ) ] [ ,...n ]  
      [ ON [ class :: ] securable ] TO principal [ ,...n ]   
      [ WITH GRANT OPTION ] [ AS principal ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
GRANT
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]
    TO principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
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

ALL  
Questa opzione è deprecata ed è stata mantenuta solo a scopo di compatibilità con le versioni precedenti. Non concede tutte le possibili autorizzazioni. L'impostazione di ALL equivale a concedere le autorizzazioni seguenti:

  - Se l'entità a protezione diretta è un database, ALL corrisponde a BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE e CREATE VIEW.  
  
  - Se l'entità a protezione diretta è una funzione scalare, ALL corrisponde a EXECUTE e REFERENCES.  
  
  - Se l'entità a protezione diretta è una funzione con valori di tabella, ALL corrisponde a DELETE, INSERT, REFERENCES, SELECT e UPDATE.  
  
  - Se l'entità a protezione diretta è una stored procedure, ALL corrisponde a EXECUTE.  
  
  - Se l'entità a protezione diretta è una tabella, ALL corrisponde a DELETE, INSERT, REFERENCES, SELECT e UPDATE.  
  
  - Se l'entità a protezione diretta è una vista, ALL corrisponde a DELETE, INSERT, REFERENCES, SELECT e UPDATE.  

PRIVILEGES  
Opzione inclusa per compatibilità con lo standard ISO. Non modifica il funzionamento di ALL.  
  
*permission*  
Nome di un'autorizzazione. I mapping validi di autorizzazioni ed entità a protezione diretta sono descritti negli argomenti correlati elencati di seguito.  
  
*column*  
Specifica il nome di una colonna in una tabella per la quale vengono concesse autorizzazioni. È necessario utilizzare le parentesi ().  
  
*class*  
Specifica la classe dell'entità a protezione diretta per la quale viene concessa l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
*securable*  
Specifica l'entità a protezione diretta a cui viene concessa l'autorizzazione.  
  
TO *principal*  
Nome di un'entità. Le entità a cui è possibile concedere le autorizzazioni per un'entità a protezione diretta variano in base all'entità a protezione diretta specifica. Per altre informazioni sulle combinazioni valide, vedere gli argomenti secondari elencati di seguito.  
  
GRANT OPTION  
Indica che l'utente autorizzato potrà inoltre concedere l'autorizzazione specificata ad altre entità.  
  
AS *principal*  
Usare la clausola AS principal per indicare che l'entità registrata come l'utente che concede l'autorizzazione deve essere un'entità diversa dalla persona che esegue l'istruzione. Si supponga ad esempio che l'utente Mary sia principal_id 12 e l'utente Raul sia principal 15. Mary esegue `GRANT SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;` Ora la tabella sys.database_permissions indica che il grantor_prinicpal_id è 15 (Raul) anche se l'istruzione è stata effettivamente eseguita dall'utente 13 (Mary).  

L'uso della clausola AS in genere non è consigliabile, a meno che non sia necessario definire in modo esplicito la catena di autorizzazione. Per altre informazioni, vedere la sezione **Riepilogo delle informazioni sull'algoritmo di controllo delle autorizzazioni** di [Autorizzazioni (Motore di database)](../../relational-databases/security/permissions-database-engine.md).

L'uso di AS in questa istruzione non implica la possibilità di rappresentare un altro utente.

## <a name="remarks"></a>Osservazioni

La sintassi completa dell'istruzione GRANT è complessa. Il diagramma della sintassi precedente è stato semplificato per evidenziarne la struttura. La sintassi completa per la concessioni di autorizzazioni per entità a protezione diretta specifiche viene descritta negli articoli riportati di seguito.  

È possibile utilizzare l'istruzione REVOKE per rimuovere le autorizzazioni concesse e l'istruzione DENY per negare un'autorizzazione specifica a un'entità anche in caso di esecuzione di un'istruzione GRANT.  

La concessione di un'autorizzazione rimuove l'istruzione DENY o REVOKE di tale autorizzazione per l'entità a protezione diretta specificata. Se la stessa autorizzazione viene negata a un livello superiore e in tale livello è inclusa l'entità a protezione diretta, l'istruzione DENY ha la precedenza. Tuttavia, la revoca dell'autorizzazione concessa a un ambito superiore non ha la precedenza.  

Le autorizzazioni a livello di database vengono concesse nell'ambito del database specificato. Se un utente deve ottenere autorizzazioni per gli oggetti di un database diverso da quello corrente, è necessario creare un account utente nel secondo database o consentire all'account utente di accedere a tale database oltre che al database corrente.  

> [!CAUTION]  
> Un'istruzione DENY a livello di tabella non ha la precedenza rispetto a un'istruzione GRANT a livello di colonna. Questa incongruenza nella gerarchia di autorizzazioni è stata mantenuta per garantire la compatibilità con le versioni precedenti Verrà rimosso in una versione futura.  

La stored procedure di sistema sp_helprotect visualizza le autorizzazioni per un'entità a protezione diretta a livello di database.  

## <a name="with-grant-option"></a>WITH GRANT OPTION

**GRANT** ... **WITH GRANT OPTION** specifica che l'entità di sicurezza che riceve l'autorizzazione ha la possibilità di concedere l'autorizzazione specificata ad altri account di sicurezza. Se l'entità che riceve l'autorizzazione è un ruolo o un gruppo di Windows, è necessario usare la clausola **AS** quando l'autorizzazione per gli oggetti deve essere concessa ulteriormente a utenti che non sono membri del gruppo o del ruolo. Poiché un'istruzione **GRANT** può essere eseguita solo da un utente, anziché da un gruppo o da un ruolo, è necessario che un membro specifico del gruppo o del ruolo usi la clausola **AS** per chiamare in modo esplicito l'appartenenza a un gruppo o a un ruolo per la concessione dell'autorizzazione. Nell'esempio seguente viene illustrato l'uso di **WITH GRANT OPTION** quando viene concesso a un ruolo o a un gruppo di Windows.  
  
```sql
-- Execute the following as a database owner  
GRANT EXECUTE ON TestProc TO TesterRole WITH GRANT OPTION;  
EXEC sp_addrolemember TesterRole, User1;  
-- Execute the following as User1  
-- The following fails because User1 does not have the permission as the User1  
GRANT EXECUTE ON TestMe TO User2;  
-- The following succeeds because User1 invokes the TesterRole membership  
GRANT EXECUTE ON TestMe TO User2 AS TesterRole;  
```

## <a name="chart-of-sql-server-permissions"></a>Grafico delle autorizzazioni di SQL Server

Per un'anteprima di grandi dimensioni di tutte le autorizzazioni del [!INCLUDE[ssDE](../../includes/ssde-md.md)] in formato pdf, vedere [https://aka.ms/sql-permissions-poster](https://aka.ms/sql-permissions-poster).  

## <a name="permissions"></a>Autorizzazioni

L'utente che concede le autorizzazioni (o l'entità specificata con l'opzione AS) deve disporre della relativa autorizzazione con GRANT OPTION oppure di un'autorizzazione di livello superiore che include l'autorizzazione che viene concessa. Se si utilizza l'opzione AS, sono previsti requisiti aggiuntivi. Per altre informazioni dettagliate, vedere l'articolo relativo alle entità a protezione diretta.

I proprietari degli oggetti possono concedere autorizzazioni per gli oggetti di cui sono proprietari. Le entità con l'autorizzazione CONTROL per un'entità a sicurezza diretta possono concedere l'autorizzazione per quella entità.

Gli utenti che dispongono dell'autorizzazione CONTROL SERVER, ad esempio i membri del ruolo predefinito del server sysadmin, possono concedere qualsiasi autorizzazione per qualsiasi entità a protezione diretta nel server. Gli utenti che dispongono dell'autorizzazione CONTROL in un database, ad esempio i membri del ruolo predefinito del database db_owner, possono concedere qualsiasi autorizzazione per qualsiasi entità a protezione diretta nel database. Gli utenti che dispongono dell'autorizzazione CONTROL in uno schema, possono concedere qualsiasi autorizzazione per qualsiasi oggetto all'interno dello schema.

## <a name="examples"></a>Esempi

Nella tabella seguente vengono elencate le entità a protezione diretta e gli articoli in cui viene descritta la relativa sintassi specifica.  

| Entità a protezione diretta | Sintassi GRANT|
| ---------| ------ |
|Ruolo applicazione|[GRANT - Autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Assembly|[GRANT - Autorizzazioni per assembly &#40;Transact-SQL&#41;](../../t-sql/statements/grant-assembly-permissions-transact-sql.md)|  
|Chiave asimmetrica|[GRANT - Autorizzazioni per chiavi asimmetriche &#40;Transact-SQL&#41;](../../t-sql/statements/grant-asymmetric-key-permissions-transact-sql.md)|  
|Gruppo di disponibilità|[GRANT - Autorizzazioni del gruppo di disponibilità &#40;Transact-SQL&#41;](../../t-sql/statements/grant-availability-group-permissions-transact-sql.md)|  
|Certificato|[GRANT - Autorizzazioni per certificati &#40;Transact-SQL&#41;](../../t-sql/statements/grant-certificate-permissions-transact-sql.md)|  
|Contratto|[GRANT - Autorizzazioni di Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Database|[GRANT - Autorizzazioni per database &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md)|
|Credenziali con ambito database|[GRANT - Credenziali con ambito database (Transact-SQL)](../../t-sql/statements/grant-database-scoped-credential-transact-sql.md)|  
|Endpoint|[GRANT - Autorizzazioni per endpoint &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)|  
|Catalogo full-text|[GRANT - Autorizzazioni per il catalogo full-text &#40;Transact-SQL&#41;](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)|  
|Elenco di parole non significative full-text|[GRANT - Autorizzazioni per il catalogo full-text &#40;Transact-SQL&#41;](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)|  
|Funzione|[GRANT - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Login|[GRANT - Autorizzazioni per entità server &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)|  
|Tipo di messaggio|[GRANT - Autorizzazioni di Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Oggetto|[GRANT - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Coda|[GRANT - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Associazione al servizio remoto|[GRANT - Autorizzazioni di Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Ruolo|[GRANT - Autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Route|[GRANT - Autorizzazioni di Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|SCHEMA|[GRANT - Autorizzazioni per schemi &#40;Transact-SQL&#41;](../../t-sql/statements/grant-schema-permissions-transact-sql.md)|  
|Elenco delle proprietà di ricerca|[GRANT - Autorizzazioni per l'elenco delle proprietà di ricerca &#40;Transact-SQL&#41;](../../t-sql/statements/grant-search-property-list-permissions-transact-sql.md)|  
|Server|[GRANT - autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md)|  
|Service|[GRANT - Autorizzazioni di Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Stored Procedure|[GRANT - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Chiave simmetrica|[GRANT - Autorizzazioni per chiavi asimmetriche &#40;Transact-SQL&#41;](../../t-sql/statements/grant-symmetric-key-permissions-transact-sql.md)|  
|Sinonimo|[GRANT - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Oggetti di sistema|[GRANT - autorizzazioni per oggetti di sistema &#40;Transact-SQL&#41;](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)|  
|Tabella|[GRANT - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Type|[GRANT - Autorizzazioni per tipi &#40;Transact-SQL&#41;](../../t-sql/statements/grant-type-permissions-transact-sql.md)|  
|Utente|[GRANT - Autorizzazioni per entità di database &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Visualizzazione|[GRANT - Autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Raccolta di XML Schema|[GRANT - Autorizzazioni per raccolte di XML Schema &#40;Transact-SQL&#41;](../../t-sql/statements/grant-xml-schema-collection-permissions-transact-sql.md)|  

## <a name="see-also"></a>Vedere anche

- [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)
- [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)
- [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)
- [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)
- [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)
- [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)
- [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)
- [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)
