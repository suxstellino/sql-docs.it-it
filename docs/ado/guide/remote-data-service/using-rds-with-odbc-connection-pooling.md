---
description: Uso di RDS con pool di connessioni ODBC
title: Utilizzo di RDS con il pool di connessioni ODBC | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connection pooling in RDS [ADO]
ms.assetid: e8b912c1-da5b-4e85-a000-1e6648a94237
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c353d7e0038f40eea574b895f2cd55ea3f47585
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032593"
---
# <a name="using-rds-with-odbc-connection-pooling"></a>Uso di RDS con pool di connessioni ODBC
Se si utilizza un'origine dati ODBC, è possibile utilizzare l'opzione pool di connessioni in Internet Information Services (IIS) per ottenere una gestione elevata delle prestazioni del carico client. Il pool di connessioni è un Resource Manager per le connessioni, mantenendo lo stato aperto sulle connessioni usate di frequente.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Per abilitare il pool di connessioni, vedere la documentazione Internet Information Services.  
  
 Si noti che l'abilitazione del pool di connessioni può sottoporre il server Web ad altre restrizioni, come indicato nella documentazione di Microsoft Internet Information Services.  
  
 Per assicurarsi che il pool di connessioni sia stabile e fornisca un miglioramento delle prestazioni aggiuntivo, è necessario configurare Microsoft SQL Server per l'utilizzo della libreria di rete socket TCP/IP.  
  
 A questo scopo, è necessario:  
  
-   Configurare il computer SQL Server per l'utilizzo dei socket TCP/IP.  
  
-   Configurare il server Web per l'utilizzo dei socket TCP/IP.  
  
## <a name="configuring-the-sql-server-computer-to-use-tcpip-sockets"></a>Configurazione del computer SQL Server per l'utilizzo di socket TCP/IP  
 Nel computer SQL Server eseguire il programma di installazione di SQL Server in modo che le interazioni con l'origine dati usino la libreria di rete socket TCP/IP.  
  
### <a name="to-specify-the-tcpip-socket-network-library-on-the-sql-server-computer"></a>Per specificare la libreria di rete socket TCP/IP nel computer SQL Server  
  
### <a name="in-microsoft-sql-server-65"></a>In Microsoft SQL Server 6,5:  
  
1.  Dal menu Start scegliere programmi, Microsoft SQL Server 6,5, quindi fare clic su installazione di SQL.  
  
2.  Fare clic due volte su continua.  
  
3.  Nella finestra di dialogo Microsoft SQL Server-Opzioni selezionare Cambia supporto di rete e quindi fare clic su continua.  
  
4.  Verificare che la casella di controllo TCP/IP Sockets sia selezionata e fare clic su OK.  
  
5.  Fare clic su continua per terminare e uscire dal programma di installazione.  
  
### <a name="in-microsoft-sql-server-70"></a>In Microsoft SQL Server 7,0:  
  
1.  Dal menu Start scegliere programmi, Microsoft SQL Server 7,0, quindi fare clic su utilità di rete server.  
  
2.  Nella scheda generale della finestra di dialogo fare clic su Aggiungi.  
  
3.  Nella finestra di dialogo Aggiungi configurazione libreria di rete fare clic su TCP/IP.  
  
4.  Nelle caselle numero di porta e indirizzo proxy immettere il numero di porta e l'indirizzo proxy forniti dall'amministratore di rete.  
  
5.  Fare clic su OK per terminare e uscire dal programma di installazione.  
  
## <a name="configuring-the-web-server-to-use-tcpip-sockets"></a>Configurazione del server Web per l'utilizzo di socket TCP/IP  
 Sono disponibili due opzioni per la configurazione del server Web per l'utilizzo dei socket TCP/IP. Le operazioni da eseguire variano a seconda che si acceda a tutti i server SQL dal server Web o che venga eseguito l'accesso solo a un SQL Server specifico dal server Web.  
  
 Se si accede a tutti i server SQL dal server Web, è necessario eseguire l'utilità di configurazione client di SQL Server nel computer server Web. Nei passaggi seguenti viene modificata la libreria di rete predefinita per tutte le connessioni SQL Server effettuate da questo server Web IIS per l'utilizzo della libreria di rete TCP/IP Sockets.  
  
### <a name="to-configure-the-web-server-all-sql-servers"></a>Per configurare il server Web (tutti i server SQL)  
  
### <a name="for-microsoft-sql-server-65"></a>Per Microsoft SQL Server 6,5:  
  
1.  Dal menu Start scegliere programmi, Microsoft SQL Server 6,5, quindi fare clic su utilità di configurazione client SQL.  
  
2.  Fare clic sulla scheda NET Library.  
  
3.  Nella casella rete predefinita selezionare socket TCP/IP.  
  
4.  Fare clic su fine per salvare le modifiche e uscire dall'utilità.  
  
### <a name="for-microsoft-sql-server-70"></a>Per Microsoft SQL Server 7,0:  
  
1.  Dal menu Start scegliere programmi, Microsoft SQL Server 7,0, quindi fare clic su utilità di rete client.  
  
2.  Fare clic sulla scheda General.  
  
3.  Nella casella libreria di rete predefinita fare clic su TCP/IP.  
  
4.  Fare clic su OK per salvare le modifiche e uscire dall'utilità.  
  
 Se si accede a un SQL Server specifico da un server Web, è necessario eseguire l'utilità di configurazione client SQL Server sul computer server Web. Per modificare la libreria di rete per una connessione di SQL Server specifica, configurare il software client SQL Server nel computer server Web come indicato di seguito.  
  
### <a name="to-configure-the-web-server-a-specific-sql-server"></a>Per configurare il server Web (uno specifico SQL Server)  
  
### <a name="for-microsoft-sql-server-65"></a>Per Microsoft SQL Server 6,5:  
  
1.  Dal menu Start scegliere programmi, Microsoft SQL Server 6,5, quindi fare clic su utilità di configurazione client SQL.  
  
2.  Fare clic sulla scheda Avanzate.  
  
3.  Nella casella Server digitare il nome del server a cui connettersi utilizzando i socket TCP/IP.  
  
4.  Nella casella nome DLL selezionare socket TCP/IP.  
  
5.  Fare clic su Aggiungi/modifica. Tutte le origini dati che puntano a questo server utilizzeranno ora i socket TCP/IP.  
  
6.  Fare clic su Done.  
  
### <a name="for-microsoft-sql-server-70"></a>Per Microsoft SQL Server 7,0:  
  
1.  Dal menu Start scegliere programmi, Microsoft SQL Server 7,0, quindi fare clic su utilità configurazione client.  
  
2.  Fare clic sulla scheda General.  
  
3.  Fare clic su Aggiungi.  
  
4.  Immettere l'alias del server nella casella alias server. Nella casella librerie di rete fare clic su TCP/IP. Nella casella nome computer immettere il nome del computer che ascolta i client TCP/IP Sockets. Nella casella numero porta immettere la porta su cui è in ascolto il SQL Server.  
  
5.  Fare clic su OK e quindi di nuovo su OK per uscire dall'utilità.  
  
## <a name="see-also"></a>Vedere anche  
 [Nozioni fondamentali su RDS](./rds-fundamentals.md)