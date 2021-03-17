---
description: 'Autorizzazioni: GRANT, DENY, REVOKE (Azure Synapse Analytics, Parallel Data Warehouse)'
title: Autorizzazioni GRANT-DENY-REVOKE
titleSuffix: Azure Synapse Analytics
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5a3b7424-408e-4cb0-8957-667ebf4596fc
author: VanMSFT
ms.author: vanto
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: c1dd1d663122327e5a9aeefc673909605fa5f116
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575010"
---
# <a name="permissions-grant-deny-revoke-azure-synapse-analytics-parallel-data-warehouse"></a>Autorizzazioni: GRANT, DENY, REVOKE (Azure Synapse Analytics, Parallel Data Warehouse)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Usare le istruzioni **GRANT** e **DENY** di [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] per concedere o negare un'autorizzazione, ad esempio **UPDATE**, in un'entità a protezione diretta (ad esempio un database, una tabella, una visualizzazione, ecc.) per un'entità di sicurezza (un account di accesso, un utente database o un ruolo del database). Usare **REVOKE** per rimuovere l'assegnazione o la revoca di un'autorizzazione.  
  
 Le autorizzazioni a livello server vengono applicate agli account di accesso. Le autorizzazioni a livello di database vengono applicate agli utenti database e ai ruoli del database.  
  
 Per visualizzare le autorizzazioni concesse e negate, eseguire una query nelle visualizzazioni sys.server_permissions e sys.database_permissions. Le autorizzazioni che non sono concesse o negate in modo esplicito per un'entità di sicurezza possono essere ereditate attraverso l'appartenenza a un ruolo con autorizzazioni. Le autorizzazioni dei ruoli predefiniti del database non possono essere modificate e non sono incluse nelle visualizzazioni sys.server_permissions e sys.database_permissions.  
  
-   **GRANT** concede in modo esplicito una o più autorizzazioni.  
  
-   **DENY** nega in modo esplicito una o più autorizzazioni per un'entità.  
  
-   **REVOKE** rimuove le autorizzazioni **GRANT** o **DENY** esistenti.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Azure Synapse Analytics and Parallel Data Warehouse  
GRANT   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    TO principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
[;]  
  
DENY   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    TO principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
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
  
## <a name="arguments"></a>Argomenti  
 \<permission>[ **,** ...*n* ]  
 Una o più autorizzazioni da concedere, negare o revocare.  
  
 ON [ \<class_type> :: ] *securable* La clausola **ON** descrive il parametro dell'entità a protezione diretta in cui concedere, negare o revocare le autorizzazioni.  
  
 \<class_type> Tipo di classe dell'entità a protezione diretta. Può essere **LOGIN**, **DATABASE**, **OBJECT**, **SCHEMA**, **ROLE** o **USER**. È possibile concedere le autorizzazioni anche a **SERVER**_class\_type_, ma **SERVER** non è specificato per tali autorizzazioni. **DATABASE** non è specificato quando l'autorizzazione include la parola **DATABASE**, ad esempio **ALTER ANY DATABASE**. Se *class_type* non è specificato e il tipo di autorizzazione non è limitato al server o alla classe del database, viene usata la classe **OBJECT**.  
  
 *securable*  
 Nome di account di accesso, database, tabella, visualizzazione, schema, procedura, ruolo o utente in cui concedere, negare o revocare le autorizzazioni. Il nome dell'oggetto può essere specificato con le regole di denominazione in tre parti descritte in [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
 TO *principal* [ **,** ...*n* ]  
 Una o più entità di sicurezza a cui vengono concesse, negate o revocate le autorizzazioni. L'entità di sicurezza corrisponde al nome di un account di accesso, di un utente database o di un ruolo del database.  
  
 FROM *principal* [ **,** ...*n* ]  
 Una o più entità di sicurezza a cui revocare le autorizzazioni.  L'entità di sicurezza corrisponde al nome di un account di accesso, di un utente database o di un ruolo del database. **FROM** può essere usato solo con un'istruzione **REVOKE**. **TO** può essere usato con **GRANT**, **DENY** o **REVOKE**.  
  
 WITH GRANT OPTION  
 Indica che l'utente autorizzato potrà inoltre concedere l'autorizzazione specificata ad altre entità.  
  
 CASCADE  
 Indica che l'autorizzazione viene negata o revocata all'entità specificata e a tutte le entità a cui l'entità di sicurezza ha concesso l'autorizzazione. Obbligatorio quando l'entità ha l'autorizzazione con **GRANT OPTION**.  
  
 GRANT OPTION FOR  
 Indica che verrà revocata la capacità di concedere l'autorizzazione specificata. Obbligatorio quando viene usato l'argomento **CASCADE**.  
  
> [!IMPORTANT]  
>  Se l'autorizzazione specificata è stata concessa all'entità senza l'opzione **GRANT**, l'autorizzazione stessa verrà revocata.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per concedere un'autorizzazione, l'utente che concede le autorizzazioni deve disporre dell'autorizzazione con **WITH GRANT OPTION** oppure di un'autorizzazione di livello superiore che include l'autorizzazione che viene concessa.  I proprietari degli oggetti possono concedere autorizzazioni per gli oggetti di cui sono proprietari. Le entità con l'autorizzazione **CONTROL** per un'entità a sicurezza diretta possono concedere l'autorizzazione per l'entità.  I membri dei ruoli del database predefiniti **db_owner** e **db_securityadmin** possono concedere qualsiasi autorizzazione nel database.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 La negazione o la revoca delle autorizzazioni a un'entità di sicurezza non ha effetto sulle richieste che hanno passato l'autorizzazione e sono attualmente in esecuzione. Per limitare immediatamente l'accesso, è necessario annullare le richieste attive o terminare le sessioni correnti.  
  
> [!NOTE]  
>  La maggior parte dei ruoli predefiniti del server non è disponibile in questa versione. In alternativa, usare i ruoli del database definiti dall'utente. Non è possibile aggiungere gli account di accesso al ruolo predefinito del server **sysadmin**. La concessione dell'autorizzazione **CONTROL SERVER** è simile all'appartenenza al ruolo predefinito del server **sysadmin**.  
  
 Alcune istruzioni richiedono più autorizzazioni. Ad esempio, per creare una tabella sono necessarie le autorizzazioni **CREATE TABLE** nel database e l'autorizzazione **ALTER SCHEMA** per la tabella che conterrà la tabella.  
  
 PDW esegue a volte le stored procedure per distribuire le azioni utente ai nodi di calcolo. Pertanto, non è possibile negare l'autorizzazione di esecuzione per un intero database. (Ad esempio `DENY EXECUTE ON DATABASE::<name> TO <user>;` avrà esito negativo). Per risolvere il problema, negare l'autorizzazione di esecuzione per gli schemi utente oppure per oggetti specifici (procedure).  
  
### <a name="implicit-and-explicit-permissions"></a>Autorizzazioni implicite ed esplicite  
 Un'*autorizzazione esplicita* è un'autorizzazione **GRANT** o **DENY** concessa a un'entità di sicurezza tramite un'istruzione **GRANT** o **DENY**.  
  
 Un'*autorizzazione implicita* è un'autorizzazione **GRANT** o **DENY** che un'entità di sicurezza (account di accesso, utente o ruolo del database) ha ereditato da un altro ruolo del database.  
  
 Un'autorizzazione implicita può anche essere ereditata da un'autorizzazione di copertura o da un'autorizzazione padre. Ad esempio, l'autorizzazione **UPDATE** in una tabella può essere ereditata tramite un'autorizzazione **UPDATE** nello schema che contiene la tabella o un'autorizzazione **CONTROL** nella tabella.  
  
### <a name="ownership-chaining"></a>Concatenamento della proprietà  
 Quando più oggetti di database accedono l'uno all'altro in sequenza, questa sequenza è nota come *catena*. Sebbene queste catene non esistono in modo indipendente, quando in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono attraversati i collegamenti contenuti in una catena, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valuta le autorizzazioni per gli oggetti che compongono la catena diversamente da quanto farebbe se accedesse agli oggetti separatamente. Il concatenamento della proprietà ha implicazioni importanti per la gestione della sicurezza. Per altre informazioni sulle catene di proprietà, vedere [Catene di proprietà](https://msdn.microsoft.com/library/ms188676\(v=sql11\).aspx) e [Esercitazione: Catene di proprietà e cambio di contesto](../../relational-databases/tutorial-ownership-chains-and-context-switching.md).  
  
## <a name="permission-list"></a>Elenco delle autorizzazioni  
  
### <a name="server-level-permissions"></a>Autorizzazioni a livello di server  
 Le autorizzazioni a livello di server possono essere concesse, negate e revocate dagli account di accesso.  
  
 **Autorizzazioni valide per i server**  
  
-   CONTROL SERVER  
  
-   ADMINISTER BULK OPERATIONS  
  
-   ALTER ANY CONNECTION  
  
-   ALTER ANY DATABASE  
  
-   CREATE ANY DATABASE  
  
-   ALTER ANY EXTERNAL DATA SOURCE  
  
-   ALTER ANY EXTERNAL FILE FORMAT  
  
-   ALTER ANY LOGIN  
  
-   ALTER SERVER STATE  
  
-   CONNECT SQL  
  
-   VIEW ANY DEFINITION  
  
-   VIEW ANY DATABASE  
  
-   VIEW SERVER STATE  
  
 **Autorizzazioni valide per gli account di accesso**  
  
-   CONTROL ON LOGIN  
  
-   ALTER ON LOGIN  
  
-   IMPERSONATE ON LOGIN  
  
-   VIEW DEFINITION  
  
### <a name="database-level-permissions"></a>Autorizzazioni a livello di database  
 Le autorizzazioni a livello di database possono essere concesse, negate e revocate dagli utenti database e dai ruoli del database definiti dall'utente.  
  
 **Autorizzazioni valide per tutte le classi di database**  
  
-   CONTROL  
  
-   ALTER  
  
-   VIEW DEFINITION  
  
 **Autorizzazioni valide per tutte le classi di database esclusi gli utenti**  
  
-   TAKE OWNERSHIP  
  
 **Autorizzazioni valide solo per i database**  
  
-   ALTER ANY DATABASE  
  
-   ALTER ON DATABASE  
  
-   ALTER ANY DATASPACE  
  
-   ALTER ANY ROLE  
  
-   ALTER ANY SCHEMA  
  
-   ALTER ANY USER  
  
-   BACKUP DATABASE  
  
-   CONNECT ON DATABASE  
  
-   CREATE PROCEDURE  
  
-   CREATE ROLE  
  
-   CREATE SCHEMA  
  
-   CREA TABELLA  
  
-   CREATE VIEW  
  
-   SHOWPLAN  
  
 **Autorizzazioni valide solo per gli utenti**  
  
-   IMPERSONATE  
  
 **Autorizzazioni valide per i database, gli schemi e gli oggetti**  
  
-   ALTER  
  
-   DELETE  
  
-   EXECUTE  
  
-   INSERT  
  
-   SELECT  
  
-   UPDATE  
  
-   REFERENCES  
  
 Per una definizione di ogni tipo di autorizzazione, vedere [Autorizzazioni (motore di database)](../../relational-databases/security/permissions-database-engine.md).  
  
### <a name="chart-of-permissions"></a>Grafico delle autorizzazioni  
 Nel poster seguente sono rappresentate graficamente tutte le autorizzazioni. Il poster consente di visualizzare chiaramente la gerarchia nidificata delle autorizzazioni. Ad esempio, l'autorizzazione **ALTER ON LOGIN** può essere concessa singolarmente ma è inclusa anche nel caso in cui venga concessa l'autorizzazione **CONTROL** per l'account di accesso specifico oppure nel caso in cui venga concessa a un account di accesso l'autorizzazione **ALTER ANY LOGIN**.  
  
 ![Poster delle autorizzazioni di sicurezza APS](../../t-sql/statements/media/aps-security-perms-poster.png "Poster delle autorizzazioni di sicurezza APS")  
  
  
## <a name="default-permissions"></a>Autorizzazioni predefinite  
 L'elenco seguente descrive le autorizzazioni predefinite:  
  
-   Quando un account di accesso viene creato usando l'istruzione **CREATE LOGIN**, il nuovo account riceve l'autorizzazione **CONNECT SQL**.  
  
-   Tutti gli account di accesso sono membri del ruolo del server **public** e non possono essere rimossi da **public**.  
  
-   Quando un utente database viene creato usando l'autorizzazione **CREATE USER**, l'utente riceve l'autorizzazione **CONNECT** nel database.  
  
-   Tutte le entità di sicurezza, incluso il ruolo **public**, non hanno alcuna autorizzazione esplicita o implicita per impostazione predefinita.  
  
-   Quando un account di accesso o un utente diventa il proprietario di un database o un oggetto, l'account o l'utente ha sempre tutte le autorizzazioni per il database o l'oggetto. Le autorizzazioni di proprietà non possono essere modificate e non sono visibili come autorizzazioni esplicite. Le istruzioni **GRANT**, **DENY** e **REVOKE** non hanno effetto sui proprietari.  
  
-   L'account di accesso **sa** ha tutte le autorizzazioni nell'appliance. Analogamente alle autorizzazioni di proprietà, le autorizzazioni **sa** non possono essere modificate e non sono visibili come autorizzazioni esplicite. Le istruzioni **GRANT**, **DENY** e **REVOKE** non hanno effetto sull'account di accesso **sa**. L'account di accesso **sa** non può essere rinominato.  
  
-   L'istruzione **USE** non richiede autorizzazioni. Tutte le entità di sicurezza possono eseguire l'istruzione **USE** in qualsiasi database.  
  
##  <a name="examples-sssdw-and-sspdw"></a><a name="Examples"></a> Esempi: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-granting-a-server-level-permission-to-a-login"></a>R. Concessione di un'autorizzazione a livello di server a un account di accesso  
 Le due istruzioni seguenti concedono un'autorizzazione a livello di server a un account di accesso.  
  
```sql  
GRANT CONTROL SERVER TO [Ted];  
```  
  
```sql  
GRANT ALTER ANY DATABASE TO Mary;  
```  
  
### <a name="b-granting-a-server-level-permission-to-a-login"></a>B. Concessione di un'autorizzazione a livello di server a un account di accesso  
 L'esempio seguente concede un'autorizzazione a livello di server in un account di accesso a un'entità di sicurezza del server (un altro account di accesso).  
  
```sql  
GRANT  VIEW DEFINITION ON LOGIN::Ted TO Mary;  
```  
  
### <a name="c-granting-a-database-level-permission-to-a-user"></a>C. Concessione di un'autorizzazione a livello di database a un utente  
 L'esempio seguente concede un'autorizzazione a livello di database in un utente a un'entità di sicurezza del database (un altro utente).  
  
```sql  
GRANT VIEW DEFINITION ON USER::[Ted] TO Mary;  
```  
  
### <a name="d-granting-denying-and-revoking-a-schema-permission"></a>D. Concessione, negazione e revoca di un'autorizzazione dello schema  
 L'istruzione **GRANT** seguente concede a Yuen la possibilità di selezionare dati da qualsiasi tabella o visualizzazione nello schema dbo.  
  
```sql  
GRANT SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
 L'istruzione **DENY** seguente impedisce a Yuen di selezionare dati da qualsiasi tabella o visualizzazione nello schema dbo. Yuen non può leggere i dati anche se dispone dell'autorizzazione in modo diverso, ad esempio tramite l'appartenenza a un ruolo.  
  
```sql  
DENY SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
 L'istruzione **REVOKE** seguente rimuove l'autorizzazione **DENY**. Le autorizzazioni esplicite di Yuen sono ora neutre. Yuen può selezionare dati da qualsiasi tabella attraverso un'altra autorizzazione implicita, ad esempio l'appartenenza a un ruolo.  
  
```sql  
REVOKE SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
### <a name="e-demonstrating-the-optional-object-clause"></a>E. Dimostrazione della clausola OBJECT:: facoltativa  
 Poiché OBJECT è la classe predefinita di un'istruzione di autorizzazione, le due istruzioni seguenti corrispondono. La clausola **OBJECT::** è facoltativa.  
  
```sql  
GRANT UPDATE ON OBJECT::dbo.StatusTable TO [Ted];  
```  
  
```sql  
GRANT UPDATE ON dbo.StatusTable TO [Ted];  
```  
  
