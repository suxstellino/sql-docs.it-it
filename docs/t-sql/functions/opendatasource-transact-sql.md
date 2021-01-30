---
description: OPENDATASOURCE (Transact-SQL)
title: OPENDATASOURCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENDATASOURCE
- OPENDATASOURCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data sources [SQL Server]
- OPENDATASOURCE function
- remote data access [SQL Server], OPENDATASOURCE
- ad hoc distributed queries
- OLE DB data sources [SQL Server]
- ad hoc connection information
ms.assetid: 5510b846-9cde-4687-8798-be9a273aad31
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 57b832a82329e3eb38d7d2d332d0eaa9c210c8be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189835"
---
# <a name="opendatasource-transact-sql"></a>OPENDATASOURCE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce informazioni sulla connessione ad hoc all'interno di un nome di oggetto in quattro parti, senza utilizzare il nome di un server collegato.  

 ![Icona di collegamento](../../database-engine/configure-windows/media/topic-link.gif "icona di collegamento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
OPENDATASOURCE ( 'provider_name', 'init_string' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 '*provider_name*'  
 Nome registrato come valore PROGID del provider OLE DB utilizzato per l'accesso all'origine dati. *provider_name* è un tipo di dati **char** e non prevede alcun valore predefinito.  

 > [!IMPORTANT]
 > Il provider Microsoft OLE DB per SQL Server (SQLOLEDB) e il provider OLE DB SQL Server Native Client (SQLNCLI) precedenti rimangono deprecati e non è consigliabile usarli per nuovi progetti di sviluppo. Usare invece il nuovo [Microsoft OLE DB Driver per SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL) che verrà aggiornato con le funzionalità server più recenti.
 
 '*init_string*'  
 Stringa di connessione passata all'interfaccia IDataInitialize del provider di destinazione. La sintassi della stringa del provider si basa su coppie chiave/valore separate da punti e virgola, ad esempio: **'** _chiave1_=_valore_ **;** _chiave2_=_valore_ **'** .  
  
 Per informazioni sulle coppie parola chiave/valore specifiche supportate nel provider, vedere [!INCLUDE[msCoName](../../includes/msconame-md.md)] Data Access SDK. In questa documentazione è definita la sintassi di base. Nella tabella seguente sono elencate le parole chiave di più frequente utilizzo nell'argomento *init_string*.  
  
|Parola chiave|Proprietà OLE DB|Valori validi e descrizione|  
|-------------|---------------------|----------------------------------|  
|origine dati|DBPROP_INIT_DATASOURCE|Nome dell'origine dei dati a cui connettersi. Viene interpretato in modo diverso nei vari provider. Per il provider OLE DB per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, indica il nome del server. Per il provider OLE DB di Jet indica il percorso completo del file mdb o xls.|  
|Location|DBPROP_INIT_LOCATION|Posizione del database a cui connettersi.|  
|Extended Properties|DBPROP_INIT_PROVIDERSTRING|Stringa di connessione specifica del provider.|  
|Connect timeout|DBPROP_INIT_TIMEOUT|Valore di timeout trascorso il quale il tentativo di connessione viene considerato non riuscito.|  
|ID utente|DBPROP_AUTH_USERID|ID utente da utilizzare per la connessione.|  
|Password|DBPROP_AUTH_PASSWORD|Password da utilizzare per la connessione.|  
|Catalogo|DBPROP_INIT_CATALOG|Nome del catalogo iniziale o predefinito nella connessione all'origine dei dati.|  
|Sicurezza integrata|DBPROP_AUTH_INTEGRATED|SSPI per specificare l'autenticazione di Windows.|  
  
## <a name="remarks"></a>Commenti  
`OPENROWSET` eredita sempre le regole di confronto dell'istanza, indipendentemente dalle regole di confronto impostate per le colonne.

È possibile usare `OPENDATASOURCE` per accedere ai dati remoti da origini dati OLE DB solo se l'opzione del Registro di sistema DisallowAdhocAccess è impostata esplicitamente su 0 per il provider specificato e l'opzione di configurazione avanzata Ad Hoc Distributed Queries è abilitata. Quando queste opzioni non vengono impostate, il comportamento predefinito non consente l'accesso ad hoc.  
  
La funzione `OPENDATASOURCE` può essere usata nella stessa posizione della sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] in cui è consentito specificare un nome di server collegato. È pertanto possibile usare `OPENDATASOURCE` come prima parte di un nome composto da quattro parti che fa riferimento a un nome di tabella o vista in un'istruzione SELECT, INSERT, UPDATE o DELETE oppure a una stored procedure remota in un'istruzione EXECUTE. Durante l'esecuzione di stored procedure remote la funzione `OPENDATASOURCE` deve fare riferimento a un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La funzione non accetta variabili come argomenti.  
  
In modo analogo alla funzione `OPENROWSET`, è consigliabile usare la funzione `OPENDATASOURCE` solo per fare riferimento a origini dei dati OLE DB a cui si accede raramente. Per le origini dei dati a cui si accede con maggiore frequenza definire un server collegato. Sia OPENDATASOURCE che OPENROWSET non offrono tutte le funzionalità delle definizioni di server collegati, quali la gestione della sicurezza e la possibilità di eseguire query per ottenere informazioni sui cataloghi. A ogni chiamata della funzione OPENDATASOURCE è necessario fornire tutte le informazioni di connessione, comprese le password.  
  
> [!IMPORTANT]  
> L'autenticazione di Windows offre una sicurezza decisamente maggiore rispetto all'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando possibile, utilizzare l'autenticazione di Windows. `OPENDATASOURCE` non deve essere usata con password esplicite nella stringa di connessione.  
  
I requisiti relativi alla connessione per ogni provider sono analoghi a quelli per i parametri utilizzati durante la creazione di server collegati. I dettagli per molti provider di utilizzo comune sono disponibili nell'articolo [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
Qualsiasi chiamata a `OPENDATASOURCE`, `OPENQUERY` r `OPENROWSET` nella clausola `FROM` viene valutata separatamente e indipendentemente da qualsiasi altra chiamata a queste funzioni usate come destinazione dell'aggiornamento, anche se alle due chiamate vengono forniti argomenti identici. In particolare, le condizioni di filtro o join applicate al risultato di una di tali chiamate non hanno effetto sui risultati dell'altra.  
  
## <a name="permissions"></a>Autorizzazioni  
 Qualsiasi utente può eseguire OPENDATASOURCE. Le autorizzazioni utilizzate per connettersi al server remoto sono determinate dalla stringa di connessione.  
  
## <a name="examples"></a>Esempi  

### <a name="a-using-opendatasource-with-select-and-the-sql-server-ole-db-driver"></a>R. Uso di OPENDATASOURCE con SELECT e OLE DB Driver per SQL Server  
 L'esempio seguente usa il provider [Microsoft OLE DB Driver per SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) per accedere alla tabella `HumanResources.Department` nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] nel server remoto `Seattle1`. Viene usata un'istruzione `SELECT` per definire il set di righe restituito. La stringa del provider contiene le parole chiave `Server` e `Trusted_Connection`. Queste parole chiave sono riconosciute da OLE DB Driver per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT GroupName, Name, DepartmentID  
FROM OPENDATASOURCE('MSOLEDBSQL', 'Server=Seattle1;Database=AdventureWorks2016;TrustServerCertificate=Yes;Trusted_Connection=Yes;').HumanResources.Department  
ORDER BY GroupName, Name;  
``` 

### <a name="b-using-opendatasource-with-select-and-the-sql-server-ole-db-provider"></a>B. Uso di OPENDATASOURCE con SELECT e il provider OLE DB per SQL Server  
Nell'esempio seguente viene creata una connessione ad hoc all'istanza `Payroll` di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel server `London` e viene eseguita una query sulla tabella `AdventureWorks2012.HumanResources.Employee`. 

> [!NOTE] 
> L'uso di SQLNCLI reindirizzerà [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alla versione più recente del provider OLE DB per SQL Server Native Client. Il provider OLE DB deve essere registrato nel Registro di sistema con il valore PROGID specificato. 

> [!IMPORTANT]  
> Il provider OLE DB per SQL Server Native Client (SQLNCLI) rimane deprecato e non è consigliabile usarlo per nuovi progetti di sviluppo. Usare invece il nuovo [Microsoft OLE DB Driver per SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL) che verrà aggiornato con le funzionalità server più recenti.
  
```sql  
SELECT *  
FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source=London\Payroll;Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Employee;  
```  

### <a name="c-using-the-microsoft-ole-db-provider-for-jet"></a>C. Utilizzo del provider Microsoft OLE DB per Jet   
 Nell'esempio seguente viene creata una connessione ad hoc a un foglio di calcolo di Excel in formato 1997 - 2003.  
  
```sql  
SELECT * FROM OPENDATASOURCE('Microsoft.Jet.OLEDB.4.0',  
    'Data Source=C:\DataFolder\Documents\TestExcel.xls;Extended Properties=EXCEL 5.0')...[Sheet1$] ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)  
  
