---
title: Sintassi della stringa di connessione
description: Informazioni sulla sintassi delle stringhe di connessione nel provider di dati Microsoft SqlClient per SQL Server. La sintassi per ogni provider è documentata nella relativa proprietà ConnectionString.
ms.date: 11/13/2020
ms.assetid: 0977aeee-04d1-4cce-bbed-750c77fce06e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6daf0dc719b284f753c6f089709ed6f0f51dfe2a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596404"
---
# <a name="connection-string-syntax"></a>Sintassi della stringa di connessione

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

<xref:Microsoft.Data.SqlClient> dispone di un oggetto `Connection` che eredita da <xref:System.Data.Common.DbConnection> e da una proprietà <xref:System.Data.Common.DbConnection.ConnectionString%2A> specifica del provider. La sintassi specifica della stringa di connessione per il provider SqlClient è documentata nella relativa proprietà `ConnectionString`. Per altre informazioni sulla sintassi della stringa di connessione, vedere <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>.

## <a name="connection-string-builders"></a>Generatori di stringhe di connessione

 Nel provider di dati Microsoft SqlClient per SQL Server è stato introdotto il generatore di stringhe di connessione riportato di seguito.

- <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>

I generatori di stringhe di connessione consentono di costruire stringhe di connessione sintatticamente valide in fase di esecuzione, pertanto non è necessario concatenare manualmente i valori delle stringhe di connessione nel codice. Per altre informazioni, vedere [Compilatori di stringhe di connessione](connection-string-builders.md).

## <a name="windows-authentication"></a>Autenticazione di Windows

Si consiglia di usare l'autenticazione di Windows (definita anche *sicurezza integrata*) per connettersi alle origini dati che la supportano. Nella tabella seguente viene illustrata la sintassi di autenticazione di Windows utilizzata con il provider di dati Microsoft SqlClient per SQL Server.

|Provider|Sintassi|  
|--------------|------------|  
|`SqlClient`|`Integrated Security=true;`<br /><br /> `-- or --`<br /><br /> `Integrated Security=SSPI;`|  

## <a name="sqlclient-connection-strings"></a>Stringhe di connessione SqlClient

La sintassi per una stringa di connessione <xref:Microsoft.Data.SqlClient.SqlConnection> è documentata nella proprietà <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>. È possibile usare la proprietà <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> per ottenere o impostare una stringa di connessione per un database di SQL Server. Le parole chiave della stringa di connessione sono inoltre mappate alle proprietà in <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>.

> [!IMPORTANT]
> L'impostazione predefinita per la parola chiave `Persist Security Info` è `false`. Impostandola su `true` o `yes`, è possibile ottenere informazioni sensibili, compresi l'ID utente e la password, dalla connessione dopo che questa è stata aperta. Mantenere `Persist Security Info` impostata su `false` per assicurarsi che le origini non attendibili non abbiano accesso a informazioni sensibili sulle stringhe di connessione.

### <a name="windows-authentication-with-sqlclient"></a>Autenticazione di Windows con SqlClient

Con tutti i tipi di sintassi seguenti viene usata l'autenticazione di Windows per stabilire la connessione al database **AdventureWorks** su un server locale.

```csharp  
"Persist Security Info=False;Integrated Security=true;  
    Initial Catalog=AdventureWorks;Server=MSSQL1"  
"Persist Security Info=False;Integrated Security=SSPI;  
    database=AdventureWorks;server=(local)"  
"Persist Security Info=False;Trusted_Connection=True;  
    database=AdventureWorks;server=(local)"  
```  

### <a name="sql-server-authentication-with-sqlclient"></a>Autenticazione di SQL Server con SqlClient

L'autenticazione di Windows è il sistema consigliato per le connessioni a SQL Server. Se tuttavia è necessario usare l'autenticazione di SQL Server, usare la sintassi seguente per specificare un nome utente e una password. In questo esempio per rappresentare un nome utente e una password validi vengono usati asterischi.

```csharp  
"Persist Security Info=False;User ID=*****;Password=*****;Initial Catalog=AdventureWorks;Server=MySqlServer"  
```  

Quando ci si connette al database SQL di Azure o ad Azure Synapse Analytics e si specifica un account di accesso nel formato `user@servername`, assicurarsi che il valore `servername` nell'account di accesso corrisponda al valore specificato per `Server=`.

> [!NOTE]
> L'autenticazione di Windows ha la precedenza sugli account di accesso di SQL Server. Se si specifica sia Integrated Security=true sia un nome utente e una password, questi ultimi saranno ignorati e sarà usata l'autenticazione di Windows.

### <a name="connect-to-a-named-instance-of-sql-server"></a>Connettersi a un'istanza denominata di SQL Server

Per connettersi a un'istanza denominata di SQL Server, usare la sintassi *nome server\nome istanza*.

```csharp  
"Data Source=MySqlServer\MSSQL1;"  
```  

È anche possibile impostare la proprietà <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A> di `SqlConnectionStringBuilder` sul nome dell'istanza quando si compila una stringa di connessione. La proprietà <xref:Microsoft.Data.SqlClient.SqlConnection.DataSource%2A> di un oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> è di sola lettura.

### <a name="type-system-version-changes"></a>Modifiche della parola chiave Type System Version

La parola chiave `Type System Version` in <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> specifica la rappresentazione lato client dei tipi di SQL Server. Per altre informazioni sulla parola chiave <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>, vedere `Type System Version`.

## <a name="connect-and-attach-to-sql-server-express-user-instances"></a>Connettersi e collegarsi alle istanze utente di SQL Server Express

Le istanze utente sono una funzionalità di SQL Server Express. consentono a un utente che usa un account di Windows locale con privilegi minimi di collegarsi ed eseguire un database SQL Server senza la necessità di privilegi amministrativi. Un'istanza utente viene eseguita con le credenziali di Windows dell'utente, non come un servizio.

Per altre informazioni sull'utilizzo delle istanze utente, vedere [Istanze utente di SQL Server Express](./sql/sql-server-express-user-instances.md).

## <a name="use-trustservercertificate"></a>Usare TrustServerCertificate

La parola chiave `TrustServerCertificate` è valida solo quando ci si connette a un'istanza di SQL Server con un certificato valido. Quando `TrustServerCertificate` è impostato su `true`, l'utente richiede che nel livello trasporto venga utilizzato TLS/SSL per crittografare il canale e ignorare il passaggio alla catena di certificati per convalidare l'attendibilità.

```csharp  
"TrustServerCertificate=true;"
```  

> [!NOTE]
> Se `TrustServerCertificate` è impostata su `true` e la crittografia è attivata, il livello di crittografia specificato nel server verrà usato anche se `Encrypt` è impostato su `false` nella stringa di connessione. In caso contrario, la connessione non riuscirà.

### <a name="enable-encryption"></a>Abilitare la crittografia

Per attivare la crittografia quando non è stato eseguito il provisioning di un certificato nel server, è necessario che le opzioni **Forza crittografia protocollo** e **Considera attendibile certificato server** siano impostate in Gestione configurazione SQL Server. In questo caso, la crittografia utilizzerà un certificato server autofirmato senza convalida se nel server non è stato eseguito il provisioning di alcun certificato verificabile.

Le impostazioni delle applicazioni non possono ridurre il livello di sicurezza configurato in SQL Server, ma facoltativamente possono potenziarlo. Un'applicazione può richiedere la crittografia impostando le parole chiave `TrustServerCertificate` e `Encrypt` su `true`, garantendo che la crittografia venga applicata anche quando non è stato eseguito il provisioning di un certificato server e l'opzione **Forza crittografia protocollo** non è stata configurata per il client. Tuttavia, se `TrustServerCertificate` non è attivata nella configurazione client, è comunque necessario il provisioning di un certificato server.

La tabella seguente descrive tutti i casi.

|Impostazione client Forza crittografia protocollo|Impostazione client Considera attendibile certificato server|Attributo/stringa di connessione Encrypt/Use Encryption for Data|Attributo/stringa di connessione Trust Server Certificate|Risultato|  
|----------------------------------------------|---------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------|------------|  
|No|N/D|No (impostazione predefinita)|Ignorato|Nessuna crittografia.|  
|No|N/D|Sì|No (impostazione predefinita)|La crittografia viene applicata solo se è disponibile un certificato server verificabile; in caso contrario, il tentativo di connessione non riesce.|  
|No|N/D|Sì|Sì|La crittografia viene sempre applicata, ma può essere utilizzato un certificato server auto-firmato.|  
|Sì|No|Ignorato|Ignorato|La crittografia viene applicata solo se è disponibile un certificato server verificabile; in caso contrario, il tentativo di connessione non riesce.|  
|Sì|Sì|No (impostazione predefinita)|Ignorato|La crittografia viene sempre applicata, ma può essere utilizzato un certificato server auto-firmato.|  
|Sì|Sì|Sì|No (impostazione predefinita)|La crittografia viene applicata solo se è disponibile un certificato server verificabile; in caso contrario, il tentativo di connessione non riesce.|  
|Sì|Sì|Sì|Sì|La crittografia viene sempre applicata, ma può essere utilizzato un certificato server auto-firmato.|  

Per altre informazioni, vedere [Uso della crittografia senza convalida](../../relational-databases/native-client/features/using-encryption-without-validation.md).

## <a name="see-also"></a>Vedere anche

- [Stringhe di connessione](connection-strings.md)
- [Connessione a un'origine dati](connecting-to-data-source.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)