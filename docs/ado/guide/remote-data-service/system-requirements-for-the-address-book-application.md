---
description: Requisiti di sistema per l'applicazione Address Book
title: Requisiti di sistema per l'applicazione Rubrica | Microsoft Docs
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
ms.assetid: da385405-1c9a-478b-9bf6-fba70015324c
author: rothja
ms.author: jroth
ms.openlocfilehash: 0fa1eb879a536a2958184942c5d779ebb1c998b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036081"
---
# <a name="system-requirements-for-the-address-book-application"></a>Requisiti di sistema per l'applicazione Address Book
Per configurare l'applicazione di esempio Address Book, è necessario soddisfare i requisiti software e di database seguenti:  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="software-requirements"></a>Requisiti software  
 I requisiti software per il computer server per l'esecuzione di questa applicazione Web includono:  
  
-   Microsoft Windows NT® Server 4,0, con Service Pack 3 o versioni successive o Microsoft Windows® 2000 Server.  
  
-   Microsoft Internet Information Services (IIS) versione 3,0 o successiva, con Active Server pagine.  
  
 I requisiti software del computer client per l'esecuzione di questa applicazione Web includono:  
  
-   Microsoft Internet Explorer 4,0 o versione successiva.  
  
-   Workstation o server Microsoft Windows NT 4,0, Windows 2000 o Microsoft Windows 98.  
  
## <a name="database-requirements"></a>Requisiti del database  
 Per utilizzare questo esempio, è necessario disporre di:  
  
-   Un server di database operativo Microsoft® SQL Server versione 6,5 o successiva.  
  
-   Privilegi per creare il database e popolarlo con i dati di esempio.  
  
-   Verifica dei dati popolati tramite Enterprise Manager o le utilità ISQL (denominata Analizzatore query in SQL Server 7,0).  
  
 Se non si dispone dei privilegi, è possibile che l'amministratore del database debba configurare il sistema e concedere l'autorizzazione di accesso al database o configurare il database.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione dello script SQL Address Book](./running-the-address-book-sql-script.md)   
 [Oggetto DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md)   
 [Esecuzione dell'applicazione di esempio Address Book](./running-the-address-book-sample-application.md)