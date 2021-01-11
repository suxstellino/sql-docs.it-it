---
title: Connessione a un database SQL di Azure
description: Questo articolo illustra i problemi relativi all'uso di Microsoft JDBC Driver per SQL Server per connettersi a un database SQL di Azure.
ms.custom: ''
ms.date: 12/18/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 49645b1f-39b1-4757-bda1-c51ebc375c34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03768a309ac10fc16fd1a743660df6fe74b088e7
ms.sourcegitcommit: bc8474fa200ef0de7498dbb103bc76e3e3a4def4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/21/2020
ms.locfileid: "97709670"
---
# <a name="connecting-to-an-azure-sql-database"></a>Connessione a un database SQL di Azure

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

In questo articolo vengono illustrati i problemi dell'uso di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] per la connessione a un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]. Per altre informazioni sulla connessione a un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], vedere:  
  
- [Database SQL di Azure](/azure/sql-database/sql-database-technical-overview)  
  
- [Procedura: Connettersi ad Azure SQL tramite JDBC](/azure/sql-database/sql-database-connect-query-java)  

- [Connessione tramite autenticazione di Azure Active Directory](connecting-using-azure-active-directory-authentication.md)  
  
## <a name="details"></a>Dettagli

Per la connessione a un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] è necessario connettersi al database master per chiamare **SQLServerDatabaseMetaData.getCatalogs**.  
[!INCLUDE[ssAzure](../../includes/ssazure_md.md)] non supporta la restituzione dell'intero set di cataloghi da un database utente. **SQLServerDatabaseMetaData.getCatalogs** usa la vista sys.databases per ottenere i cataloghi. Vedere la discussione relativa alle autorizzazioni in [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) per comprendere il comportamento di **SQLServerDatabaseMetaData.getCatalogs** in un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)].  
  
## <a name="connections-dropped"></a>Connessioni eliminate

Durante la connessione a un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], è possibile che le connessioni inattive siano terminate da un componente di rete, ad esempio un firewall, dopo un periodo di inattività. In questo contesto esistono due tipi di inattività:  

- Inattività a livello TCP, in cui le connessioni possono essere eliminate da un certo numero di dispositivi di rete.  

- Inattività nel gateway Azure SQL, in cui potrebbero verificarsi messaggi TCP **keepalive** (rendendo la connessione non inattiva da una prospettiva TCP), ma in cui non è stata rilevata una query attiva da 30 minuti. In questo scenario il gateway determinerà che la connessione TDS è inattiva da 30 minuti e la terminerà.  
  
Per risolvere il secondo punto ed evitare che il gateway interrompa le connessioni inattive, è possibile:

* Usare i [criteri di connessione](/azure/azure-sql/database/connectivity-architecture#connection-policy) di **reindirizzamento** quando si configura l'origine dati di Azure SQL.

* Mantenere attive le connessioni con attività leggere. Questo metodo non è consigliabile e deve essere usato solo in mancanza di altre opzioni possibili.

Per risolvere il secondo punto ed evitare che un componente di rete elimini le connessioni inattive, è consigliabile configurare le impostazioni del Registro di sistema indicate di seguito, o le relative equivalenti non Windows, nel sistema operativo in cui viene caricato il driver:  
  
|Impostazione del Registro di sistema|Valore consigliato|  
|----------------------|-----------------------|  
|HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveTime|30000|  
|HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveInterval|1000|  
|HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\TcpMaxDataRetransmissions|10|  
  
Riavviare il computer per attivare le impostazioni del Registro di sistema.  

I valori KeepAliveTime e KeepAliveInterval sono espressi in millisecondi. Queste impostazioni avranno l'effetto di disconnettere una connessione che non risponde entro un periodo da 10 a 40 secondi. Dopo l'invio di un pacchetto keep-alive, se non si riceve risposta, l'invio verrà ritentato ogni secondo fino a 10 volte. Se in questo periodo di tempo non si riceve risposta, il socket sul lato client viene disconnesso. A seconda dell'ambiente, potrebbe essere necessario aumentare il valore di KeepAliveInterval per gestire le interferenze note, come le migrazioni di macchine virtuali, che potrebbero causare la mancata risposta da parte di un server per più di 10 secondi.

> [!NOTE]
> TcpMaxDataRetransmissions non è controllabile in Windows Vista o Windows 2008 e versioni successive.

Per eseguire questa configurazione durante l'esecuzione in Azure, creare un'attività di avvio per aggiungere le chiavi del Registro di sistema.  Ad esempio, aggiungere l'attività di avvio al file di definizione del servizio:  


```xml
<Startup>  
    <Task commandLine="AddKeepAlive.cmd" executionContext="elevated" taskType="simple">  
    </Task>  
</Startup>  
```

Successivamente, aggiungere il file AddKeepAlive.cmd al progetto in uso. Impostare l'impostazione "Copia nella directory di output" su Copia sempre. Lo script seguente è un esempio di file AddKeepAlive.cmd:  

```bat
if exist keepalive.txt goto done  
time /t > keepalive.txt  
REM Workaround for JDBC keep alive on Azure SQL  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveTime /t REG_DWORD /d 30000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveInterval /t REG_DWORD /d 1000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v TcpMaxDataRetransmissions /t REG_DWORD /d 10 >> keepalive.txt  
shutdown /r /t 1  
:done  
```

## <a name="appending-the-server-name-to-the-userid-in-the-connection-string"></a>Accodamento del nome del server all'ID utente nella stringa di connessione  

Prima della versione 4.0 di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], durante la connessione a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] veniva richiesto l'accodamento del nome del server all'ID utente nella stringa di connessione. Ad esempio: user@servername. A partire dalla versione 4.0 di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] l'accodamento di @servername all'ID utente nella stringa di connessione non è più necessario.  

## <a name="using-encryption-requires-setting-hostnameincertificate"></a>Impostazione di hostNameInCertificate obbligatoria per l'uso della crittografia

Nelle versioni di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] precedenti alla 7.2, quando ci si connette a un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] è necessario specificare **hostNameInCertificate** se si specifica **encrypt=true** (se il nome server nella stringa di connessione è *shortName*.*domainName*, impostare la proprietà **hostNameInCertificate** su \*.*domainName*). Questa proprietà è facoltativa a partire dalla versione 7.2 del driver.

Ad esempio:

```java
jdbc:sqlserver://abcd.int.mscds.com;databaseName=myDatabase;user=myName;password=myPassword;encrypt=true;hostNameInCertificate=*.int.mscds.com;
```

## <a name="see-also"></a>Vedere anche

[Connessione a SQL Server con il driver JDBC](connecting-to-sql-server-with-the-jdbc-driver.md)
