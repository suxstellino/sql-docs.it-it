---
description: Preparare SQL Server per CDC
title: Preparare SQL Server per CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- prepSqlSrv
ms.assetid: 20b51dbf-a545-4234-87ae-4228268a0fb2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9a98ff226c4e2e861b60d04cb50e8b201035c16a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "88431083"
---
# <a name="prepare-sql-server-for-cdc"></a>Preparare SQL Server per CDC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Il servizio Oracle CDC richiede che tutte le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione contengano il database MSXDBCDC. Per creare questo database utilizzare l'azione Prepare SQL Server in CDC Service Configuration Console. Verrà creato uno script speciale eseguito per creare le tabelle, le stored procedure e altri artefatti obbligatori per il database. Questa attività viene eseguita una volta sola per ogni istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione.  
  
 Per ulteriori informazioni sul database MSXDBCDC, vedere Database MSXDBCDC.  
  
 In CDC Service Configuration Console, fare clic su **Prepare SQL Server**(Prepara SQL Server). Se questa opzione non è disponibile, verificare che nel riquadro sinistro della console sia selezionata l'opzione **Local CDC Services** (Servizi CDC locali).  
  
## <a name="options"></a>Opzioni  
  
### <a name="server-name"></a>Server Name  
 Digitare il nome del server in cui si trova [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="authentication"></a>Authentication  
 Selezionare uno degli elementi seguenti:  
  
-   **Autenticazione di Windows**  
  
-   **Autenticazione di SQL Server**: se si seleziona questa opzione, è necessario digitare **Nome utente** e **Password** per l'utente nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si esegue la connessione.  
  
 Per preparare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per Oracle CDC, l'account di accesso deve disporre di autorizzazioni di scrittura per il database MSXDBCDC. Immettere le credenziali per un account di accesso che dispone di autorizzazioni per il database MSXDBCDC, ad esempio un membro del ruolo `sysasmin` .  
  
### <a name="options"></a>Opzioni  
 Fare clic sulla freccia per visualizzare le opzioni disponibili da configurare. È possibile scegliere di non modificare il valore predefinito per queste opzioni. Sono disponibili le opzioni seguenti:  
  
-   **Connection Timeout** (Timeout connessione): Digitare il tempo, in secondi, di attesa del servizio CDC per Oracle per la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prima che si verifichi un errore di timeout. Il valore predefinito è **15**.  
  
-   **Timeout connessione** (Timeout esecuzione): Digitare il tempo, in secondi, di attesa del servizio Windows Oracle CDC per l'esecuzione di un comando prima che si verifichi un errore di timeout. Il valore predefinito è **30**.  
  
-   **Encrypt Connection**: selezionare **Encrypt Connection** per la comunicazione tra il servizio Oracle CDC e l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione tramite una connessione crittografata.  
  
-   **Advanced**: digitare eventuali proprietà di connessione aggiuntive, se necessario.  
  
### <a name="view-script"></a>View Script  
 Fare clic su **Visualizza script** per visualizzare una versione di sola lettura dello script di impostazione. Se necessario, un amministratore di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può copiare questo script nella console di gestione di SQL Server per modificarlo. Per altre informazioni sulla preparazione di SQL Server per lo script, vedere [Preparare SQL Server per lo script di visualizzazione di Oracle CDC](../../integration-services/change-data-capture/prepare-sql-server-for-oracle-cdc-view-script.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Procedura di utilizzo dei servizi CDC](../../integration-services/change-data-capture/how-to-work-with-cdc-services.md)   
 [Procedura di preparazione di SQL Server per CDC](../../integration-services/change-data-capture/how-to-prepare-sql-server-for-cdc.md)  
  
  
