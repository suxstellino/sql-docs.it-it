---
title: Connessione al Motore di database tramite sqlcmd
description: 'Informazioni su come selezionare il protocollo usato da sqlcmd per comunicare con SQL Server. Scegliere tra: TCP/IP, Named Pipes e Shared Memory.'
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- sqlcmd utility, Database Engine connections
- Named Pipes [SQL Server], sqlcmd utility
- TCP/IP [SQL Server], client protocols
- network protocols [SQL Server], sqlcmd utility
- protocols [SQL Server], sqlcmd utility
- VIA
- client protocols [SQL Server]
ms.assetid: 74b0fb71-7f8e-4171-9431-d07528532524
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1b235dc6b3a332c0ec9e165e4498d152737e3d42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353919"
---
# <a name="sqlcmd---connect-to-the-database-engine"></a>sqlcmd - Connettersi al motore di database
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta le comunicazioni client con il protocollo di rete TCP/IP (predefinito) e il protocollo Named Pipes. Se il client si connette a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] nello stesso computer, è inoltre disponibile il protocollo Shared Memory. La selezione del protocollo può essere in genere eseguita in tre modi. Il protocollo usato dall'utilità **sqlcmd** viene determinato nel seguente ordine:  
  
-   **sqlcmd** usa il protocollo specificato nella stringa di connessione come illustrato di seguito.  
  
-   Se nella stringa di connessione non viene specificato alcun protocollo, **sqlcmd** usa il protocollo definito nell'alias con il quale viene stabilita la connessione. Per configurare **sqlcmd** per l'uso di un protocollo di rete specifico creando un alias, vedere [Creare o eliminare un alias server per l'uso da parte di un client &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md).  
  
-   Se il protocollo non viene specificato in un altro modo, **sqlcmd** usa il protocollo di rete determinato dall'ordine dei protocolli in Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Negli esempi seguenti vengono illustrate diverse modalità di connessione all'istanza predefinita di [!INCLUDE[ssDE](../../includes/ssde-md.md)] sulla porta 1433 e a istanze denominate di [!INCLUDE[ssDE](../../includes/ssde-md.md)] in attesa sulla porta 1691. In alcuni degli esempi viene utilizzato l'indirizzo IP dell'adattatore loopback (127.0.0.1). Eseguire una prova utilizzando l'indirizzo IP della scheda di interfaccia di rete del computer in uso.  
  
 Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)] specificando il nome dell'istanza:  
  
```  
sqlcmd -S ComputerA  
sqlcmd -S ComputerA\instanceB  
```  
  
 Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)] specificando l'indirizzo IP:  
  
```  
sqlcmd -S 127.0.0.1  
sqlcmd -S 127.0.0.1\instanceB  
```  
  
 Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)] specificando il numero della porta TCP\IP:  
  
```  
sqlcmd -S ComputerA,1433  
sqlcmd -S ComputerA,1691  
sqlcmd -S 127.0.0.1,1433  
sqlcmd -S 127.0.0.1,1691  
```  
  
### <a name="to-connect-using-tcpip"></a>Per connettersi utilizzando il protocollo TCP/IP  
  
-   Connettersi utilizzando la sintassi generale seguente:  
  
    ```  
    sqlcmd -S tcp:<computer name>,<port number>  
    ```  
  
-   Connettersi all'istanza predefinita:  
  
    ```  
    sqlcmd -S tcp:ComputerA,1433  
    sqlcmd -S tcp:127.0.0.1,1433  
    ```  
  
-   Connettersi a un'istanza denominata:  
  
    ```  
    sqlcmd -S tcp:ComputerA,1691  
    sqlcmd -S tcp:127.0.0.1,1691  
    ```  
  
### <a name="to-connect-using-named-pipes"></a>Per connettersi utilizzando il protocollo Named Pipes  
  
-   Stabilire la connessione utilizzando la sintassi generale seguente:  
  
    ```  
    sqlcmd -S np:\\<computer name>\<pipe name>  
    ```  
  
-   Connettersi all'istanza predefinita:  
  
    ```  
    sqlcmd -S np:\\ComputerA\pipe\sql\query  
    sqlcmd -S np:\\127.0.0.1\pipe\sql\query  
    ```  
  
-   Connettersi a un'istanza denominata:  
  
    ```  
    sqlcmd -S np:\\ComputerA\pipe\MSSQL$<instancename>\sql\query  
    sqlcmd -S np:\\127.0.0.1\pipe\MSSQL$<instancename>\sql\query  
    ```  
  
### <a name="to-connect-using-shared-memory-a-local-procedure-call-from-a-client-on-the-server"></a>Per connettersi utilizzando il protocollo Shared Memory (chiamata a una procedura locale) da un client nel server  
  
-   Stabilire la connessione utilizzando la sintassi generale seguente:  
  
    ```  
    sqlcmd -S lpc:<computer name>  
    ```  
  
-   Connettersi all'istanza predefinita:  
  
    ```  
    sqlcmd -S lpc:ComputerA  
    ```  
  
-   Connettersi a un'istanza denominata:  
  
    ```  
    sqlcmd -S lpc:ComputerA\<instancename>  
    ```  
  
  
