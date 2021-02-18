---
title: utilità tablediff
description: Usare l'utilità tablediff per confrontare i dati in due tabelle per rilevarne l'eventuale non convergenza e risolvere i problemi relativi alla non convergenza in una topologia di replica.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- comparing data
- tablediff utility
- tables [SQL Server replication]
- table comparisons [SQL Server]
- command prompt utilities [SQL Server], tablediff
- troubleshooting [SQL Server replication], non-convergence
- non-convergence [SQL Server]
ms.assetid: 3c3cb865-7a4d-4d66-98f2-5935e28929fc
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 9054ebafe95fdfc1ab07e868f1d8e9233aee1795
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335934"
---
# <a name="tablediff-utility"></a>utilità tablediff
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  L'utilità **tablediff** viene usata per confrontare i dati in due tabelle per rilevarne l'eventuale non convergenza e risulta particolarmente utile per la risoluzione dei problemi relativi alla non convergenza in una topologia di replica. Questa utilità può essere utilizzata dal prompt dei comandi oppure in un file batch per eseguire le attività seguenti:  
  
-   Confronto riga per riga tra una tabella di origine in un'istanza di [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] che funge da server di pubblicazione per la replica e una tabella di destinazione in una o più istanze di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] che fungono da sottoscrittori della replica.  
  
-   Esegue un confronto rapido mediante il confronto solo dei conteggi delle righe e degli schemi.  
  
-   Confronti a livello di colonna.  
  
-   Generazione di uno script [!INCLUDE[tsql](../includes/tsql-md.md)] per correggere le discrepanze nel server di destinazione e rendere convergenti le tabelle di origine e di destinazione.  
  
-   Registrazione dei risultati in un file di output oppure in una tabella nel database di destinazione.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
tablediff   
[ -? ] |   
{  
        -sourceserver source_server_name[\instance_name]  
        -sourcedatabase source_database  
        -sourcetable source_table_name   
    [ -sourceschema source_schema_name ]  
    [ -sourcepassword source_password ]  
    [ -sourceuser source_login ]  
    [ -sourcelocked ]  
        -destinationserver destination_server_name[\instance_name]  
        -destinationdatabase subscription_database   
        -destinationtable destination_table   
    [ -destinationschema destination_schema_name ]  
    [ -destinationpassword destination_password ]  
    [ -destinationuser destination_login ]  
    [ -destinationlocked ]  
    [ -b large_object_bytes ]   
    [ -bf number_of_statements ]   
    [ -c ]   
    [ -dt ]   
    [ -et table_name ]   
    [ -f [ file_name ] ]   
    [ -o output_file_name ]   
    [ -q ]   
    [ -rc number_of_retries ]   
    [ -ri retry_interval ]   
    [ -strict ]  
    [ -t connection_timeouts ]   
}  
```  
  
## <a name="arguments"></a>Argomenti  
 [ **-?** ]  
 Restituisce l'elenco dei parametri supportati.  
  
 **-sourceserver** _source_server_name_[ **\\** _instance\_name_]  
 Nome del server di origine. Specificare *source_server_name* per l'istanza predefinita di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Specificare _source_server_name_ **\\** _instance_name_ per un'istanza denominata di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 **-sourcedatabase** _source_database_  
 Nome del database di origine.  
  
 **-sourcetable** _source_table_name_  
 Nome della tabella di origine sottoposta al controllo.  
  
 **-sourceschema** _source_schema_name_  
 Proprietario dello schema della tabella di origine. Per impostazione predefinita, dbo viene considerato il proprietario della tabella.  
  
 **-sourcepassword** _source_password_  
 Password di accesso usata per connettersi al server di origine mediante l'autenticazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Se possibile, specificare le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è consigliabile proteggere il file per evitare accessi non autorizzati.  
  
 **-sourceuser** _source_login_  
 Account di accesso usato per connettersi al server di origine mediante l'autenticazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Se non si specifica il parametro *source_login* , durante la connessione al server di origine viene usata l'autenticazione di Windows. [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]  
  
 **-sourcelocked**  
 La tabella di origine viene bloccata durante il confronto mediante gli hint di tabella TABLOCK e HOLDLOCK.  
  
 **-destinationserver** _destination_server_name_[ **\\** _instance_name_]  
 Nome del server di destinazione. Specificare *destination_server_name* per l'istanza predefinita di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Specificare _destination_server_name_ **\\** _instance_name_ per un'istanza denominata di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 **-destinationdatabase** _subscription_database_  
 Nome del database di destinazione.  
  
 **-destinationtable** _destination_table_  
 Nome della tabella di destinazione.  
  
 **-destinationschema** _destination_schema_name_  
 Proprietario dello schema della tabella di destinazione. Per impostazione predefinita, dbo viene considerato il proprietario della tabella.  
  
 **-destinationpassword** _destination_password_  
 Password di accesso usata per connettersi al server di destinazione mediante l'autenticazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Se possibile, specificare le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è consigliabile proteggere il file per evitare accessi non autorizzati.  
  
 **-destinationuser** _destination_login_  
 Account di accesso usato per connettersi al server di destinazione mediante l'autenticazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Se non si specifica il parametro *destination_login* , durante la connessione al server viene usata l'autenticazione di Windows. [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]  
  
 **-destinationlocked**  
 La tabella di destinazione viene bloccata durante il confronto mediante gli hint di tabella TABLOCK e HOLDLOCK.  
  
 **-b** _large_object_bytes_  
 Numero di byte da confrontare per le colonne dei tipi di dati di grandi dimensioni, ovvero: **text**, **ntext**, **image**, **varchar(max)** , **nvarchar(max)** e **varbinary(max)** . L'impostazione predefinita di *large_object_bytes* corrisponde alle dimensioni della colonna. I dati che superano il valore di *large_object_bytes* non verranno confrontati.  
  
 **-bf**  _number_of_statements_  
 Numero di istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] da scrivere nel file script [!INCLUDE[tsql](../includes/tsql-md.md)] corrente quando si usa l'opzione **-f** . Se il numero di istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] supera il valore di *number_of_statements*, viene creato un nuovo file script [!INCLUDE[tsql](../includes/tsql-md.md)] .  
  
 **-c**  
 Esegue il confronto per individuare eventuali differenze a livello di colonna.  
  
 **-dt**  
 Elimina la tabella dei risultati specificata da *table_name* se la tabella esiste già.  
  
 **-et** _table_name_  
 Specifica il nome della tabella dei risultati da creare. Se questa tabella esiste già, è necessario usare l'opzione **-DT** . In caso contrario, l'operazione ha esito negativo.  
  
 **-f** [ *file_name* ]  
 Genera uno script [!INCLUDE[tsql](../includes/tsql-md.md)] per ripristinare la convergenza tra la tabella nel server di destinazione e quella nel server di origine. È possibile specificare facoltativamente un nome e un percorso per il file script [!INCLUDE[tsql](../includes/tsql-md.md)] generato. Se *file_name* viene omesso, il file script [!INCLUDE[tsql](../includes/tsql-md.md)] verrà generato nella directory in cui si esegue l'utilità.  
  
 **-o** _output_file_name_  
 Nome e percorso completi del file di output.  
  
 **-q**  
 Esegue un confronto rapido mediante il confronto solo dei conteggi delle righe e degli schemi.  
  
 **-rc** _number_of_retries_  
 Numero di tentativi di esecuzione di un'operazione non riuscita compiuti dall'utilità.  
  
 **-ri**  _retry_interval_  
 Intervallo espresso in secondi tra i vari tentativi.  
  
 **-strict**  
 Gli schemi di origine e di destinazione vengono confrontati rigorosamente.  
  
 **-t** _connection_timeouts_  
 Imposta il periodo di timeout della connessione, espresso in secondi, per le connessioni al server di origine e al server di destinazione.  
  
## <a name="return-value"></a>Valore restituito  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**0**|Operazione completata|  
|**1**|Errore critico|  
|**2**|Differenze tra tabelle|  
  
## <a name="remarks"></a>Osservazioni  
 L'utilità **tablediff** non può essere usata con server non [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 Le tabelle contenenti colonne con il tipo di dati **sql_variant** non sono supportate.  
  
 Per impostazione predefinita, l'utilità **tablediff** supporta i mapping dei tipi di dati tra colonne di origine e di destinazione elencati di seguito.  
  
|Tipo di dati di origine|Tipo di dati di destinazione|  
|----------------------|---------------------------|  
|**tinyint**|**smallint**, **int** o **bigint**|  
|**smallint**|**int** o **bigint**|  
|**int**|**bigint**|  
|**timestamp**|**varbinary**|  
|**ntext**|**text**|  
|**nvarchar(max)**|**ntext**|  
|**varbinary(max)**|**image**|  
|**text**|**ntext**|  
|**ntext**|**nvarchar(max)**|  
|**image**|**varbinary(max)**|  
  
 Usare l'opzione **-strict** per disabilitare questi mapping ed eseguire una convalida di tipo strict.  
  
 La tabella di origine utilizzata per il confronto deve contenere almeno una chiave primaria, un'identità o una colonna ROWGUID. Se si usa l'opzione **-strict** , anche la tabella di destinazione deve contenere una chiave primaria, un'identità o una colonna ROWGUID.  
  
 Lo script [!INCLUDE[tsql](../includes/tsql-md.md)] generato per rendere convergente la tabella di destinazione non include i tipi di dati seguenti:  
  
-   **ntext**  
  
-   **nvarchar(max)**  
  
-   **varbinary(max)**  
  
-   **timestamp**  
  
-   **xml**  
  
-   **text**  
  
-   **ntext**  
  
-   **image**  
  
## <a name="permissions"></a>Autorizzazioni  
 Per confrontare tabelle, è necessario disporre delle autorizzazioni SELECT ALL sugli oggetti tabella da confrontare.  
  
 Per usare l'opzione **-et** , è necessario essere membro del ruolo predefinito del database db_owner oppure disporre almeno dell'autorizzazione CREATE TABLE nel database di sottoscrizione e dell'autorizzazione ALTER per lo schema del proprietario della destinazione nel server di destinazione.  
  
 Per usare l'opzione **-dt** , è necessario essere membro del ruolo predefinito del database db_owner oppure disporre almeno dell'autorizzazione ALTER per lo schema del proprietario della destinazione nel server di destinazione.  
  
 Per usare l'opzione **-o** oppure **-f** , è necessario avere autorizzazioni di scrittura per il percorso della directory di file specificato.  
  
## <a name="see-also"></a>Vedere anche  
 [Confronto di tabelle replicate al fine di individuare le differenze &#40;programmazione della replica&#41;](../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md)  
  
  
