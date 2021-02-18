---
title: Avvio dell'utilità sqlcmd
description: Informazioni su come avviare l'utilità sqlcmd, che consente di immettere istruzioni Transact-SQL, procedure di sistema e file di script, in modalità SQLCMD o in script e processi.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 00d57437-7a29-4da1-b639-ee990db055fb
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c7ce2ca9d2d11a88dbc2b567887b0061795b20ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340996"
---
# <a name="sqlcmd---start-the-utility"></a>sqlcmd - Avviare l'utilità
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
   L'[utilità sqlcmd](../../tools/sqlcmd-utility.md) consente di immettere istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], procedure di sistema e file script al prompt dei comandi, nell'editor di query in modalità SQLCMD, in un file di script Windows o in un passaggio di processo del sistema operativo (Cmd.exe) in un processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.
> [!NOTE]  
>  La modalità di autenticazione predefinita per **sqlcmd** è l'autenticazione di Windows. Per usare l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario specificare un nome utente e una password con le opzioni **-U** e **-P** .  
  
> [!NOTE]  
>  Per impostazione predefinita, [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] viene installato come istanza denominata **sqlexpress**.  
  
### <a name="start-the-sqlcmd-utility-and-connect-to-a-default-instance-of-sql-server"></a>Avviare l'utilità sqlcmd e connettersi a un'istanza predefinita di SQL Server  
  
1.  Fare clic sul menu **Start** e scegliere **Esegui**. Nella casella **Apri** digitare **cmd** e quindi fare clic su **OK** per aprire una finestra del prompt dei comandi. Se non ci si è mai connessi a questa istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] , potrebbe essere necessario configurare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modo che accetti le connessioni.  
  
2.  Al prompt dei comandi digitare **sqlcmd**.  
  
3.  Premere INVIO.  
  
     Verrà quindi stabilita una connessione trusted all'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in esecuzione nel computer in uso.  
  
     **1>** rappresenta il prompt di **sqlcmd** che indica il numero di riga. Il numero viene aumentato di un'unità ogni volta che si preme INVIO.  
  
4.  Per terminare la sessione di **sqlcmd** , digitare **EXIT** al prompt di **sqlcmd** .  
  
### <a name="start-the-sqlcmd-utility-and-connect-to-a-named-instance-of-sql-server"></a>Avviare l'utilità sqlcmd e connettersi a un'istanza denominata di SQL Server  
  
1.  Aprire una finestra del prompt dei comandi e digitare **sqlcmd -S**_server\nomeIstanza_. Sostituire *server\nomeIstanza* con il nome del computer e dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui ci si vuole connettere.  
  
2.  Premere INVIO.  
  
     Il prompt di **sqlcmd** (1>) indica che si è connessi all'istanza specificata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
    > [!NOTE]  
    >  Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] immesse vengono archiviate in un buffer. Vengono eseguite in batch quando viene rilevato il comando GO.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di file script Transact-SQL mediante sqlcmd](./sqlcmd-run-transact-sql-script-files.md)  
  
