---
description: Esecuzione dello script SQL per Address Book
title: Esecuzione dello script SQL Address Book | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: 409b3f8b-0ced-4867-acbe-b245dcdf6702
author: rothja
ms.author: jroth
ms.openlocfilehash: 8e36c60e0c3e56ab0c159c362e98c2280e6c5426
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036391"
---
# <a name="running-the-address-book-sql-script"></a>Esecuzione dello script SQL per Address Book
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Per eseguire lo script SQL incluso (Sampleemp. SQL), è necessario utilizzare l'utilità della riga di comando ISQL/Query Analyzer o gestione SQL Server Enterprise:  
  
-   Crea un nuovo database, AddrBookDB, sul dispositivo predefinito.  
  
-   Si connette al database AddrBookDB.  
  
-   Crea una tabella Employee.  
  
-   Popola la tabella con dati di esempio.  
  
-   Esegue una semplice istruzione SELECT per verificare il popolamento della tabella di database.  
  
-   Imposta un account utente denominato "adcdemo" con la password "adcdemo".  
  
#### <a name="to-run-the-sampleempsql-script-in-microsoft-sql-server-65"></a>Per eseguire lo script Sampleemp. SQL in Microsoft SQL Server 6,5  
  
1.  Fare clic sul pulsante **Start**, scegliere **programmi**, quindi **Microsoft SQL Server 6,5**. Fare clic su **SQL Enterprise Manager**.  
  
2.  Scegliere **strumento query SQL** dal menu **strumenti** .  
  
3.  Fare clic su **carica script SQL** e passare a c:\Platform SDK\Samples\DataAccess\RDS\AddressBook.  
  
4.  Selezionare il file Sampleemp. SQL. Fare clic su **Apri**.  
  
5.  Fare clic sul pulsante **Esegui query** (la freccia verde sulla barra degli strumenti).  
  
6.  Al termine dell'esecuzione, chiudere le finestre **query** ed **Enterprise Manager** .  
  
#### <a name="to-run-the-sampleempsql-script-in-microsoft-sql-server-70"></a>Per eseguire lo script Sampleemp. SQL in Microsoft SQL Server 7,0  
  
1.  Fare clic sul pulsante **Start**, scegliere **programmi**, quindi **Microsoft SQL Server 7,0**. Fare clic su **Enterprise Manager**.  
  
2.  Assicurarsi che l'SQL Server che si desidera utilizzare sia selezionato nell'elenco dei server registrati in Enterprise Manager.  
  
3.  Dal menu **strumenti** fare clic su **SQL Server analizzatore query**.  
  
4.  Fare clic sul pulsante **carica script SQL** (aprire la cartella sulla barra degli strumenti) e passare a c:\Platform SDK\Samples\DataAccess\RDS\AddressBook.  
  
5.  Selezionare il file Sampleemp. SQL. Fare clic su **Apri**.  
  
6.  Fare clic sul pulsante **Esegui query** (la freccia verde sulla barra degli strumenti) o su **F5**.  
  
7.  Al termine dell'esecuzione, chiudere le finestre **query**, **Query Analyzer** e **Enterprise Manager** .  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione dell'applicazione di esempio Address Book](./running-the-address-book-sample-application.md)