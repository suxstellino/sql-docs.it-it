---
title: Servizio Windows ReportServer (MSSQLServer) 107 | Microsoft Docs
description: "Questa pagina di riferimento degli errori contiene informazioni sull'ID evento 107: Il servizio Windows ReportServer (SQL Server) non si connette al database del server di report."
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
helpviewer_keywords:
- MSSQLServer 107 error
ms.assetid: 52b5704b-27f9-400a-a821-d8fa0786afe4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: aebf8df47e1cda77dcbbce89bd1e74a439082281
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045078"
---
# <a name="report-server-windows-service-mssqlserver-107"></a>Servizio Windows ReportServer (MSSQLServer) 107
    
## <a name="details"></a>Dettagli  
  
|Category|Valore|  
|-|-|  
|Nome prodotto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID evento|107|  
|Origine evento|Servizio Windows ReportServer|  
|Componente|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|  
|Testo del messaggio|Il servizio Windows ReportServer (MSSQLSERVER) non è in grado di connettersi al database del server di report.|  
  
## <a name="explanation"></a>Spiegazione  
 Il servizio del server di report di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in grado di connettersi al database del server di report. Questo errore si verifica durante un riavvio del servizio se non è possibile stabilire una connessione al database del server di report. Di seguito vengono riportate le condizioni in presenza delle quali si verifica l'errore:  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] non è in esecuzione quando il servizio del server di report viene avviato.  
  
-   Non è possibile eseguire la connessione al servizio [!INCLUDE[ssDE](../../includes/ssde-md.md)] perché le connessioni remote o il protocollo TCP/IP non è abilitato.  
  
-   Il database del server di report non è configurato in modo corretto.  
  
-   L'account di servizio non è configurato correttamente o l'account non dispone più di autorizzazioni sul database del server di report. Questa situazione può verificarsi se non si utilizza lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per configurare l'account o il database del server di report.  
  
## <a name="user-action"></a>Azione dell'utente  
 Avviare il servizio [!INCLUDE[ssDE](../../includes/ssde-md.md)] se non è in esecuzione e verificare che le connessioni remote siano abilitate per il protocollo TCP/IP.  
  
 Utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per configurare il database del server di report e l'account di servizio.  
  
## <a name="internal-only"></a>Solo interno  
  
## <a name="see-also"></a>Vedere anche  

 [Configurare l'account del servizio del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [Gestione configurazione del server di report &#40;modalità nativa&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   

 [Avviare e arrestare il servizio del server di report](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)  
  
  
