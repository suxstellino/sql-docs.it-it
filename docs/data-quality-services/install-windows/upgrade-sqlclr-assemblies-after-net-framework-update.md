---
title: Aggiornare gli assembly SQLCLR dopo l'aggiornamento di .NET Framework
description: Informazioni su come aggiornare gli assembly SQLCLR usati da SQL Server Data Quality Services (DQS) dopo l'aggiornamento di .NET Framework.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: b1a008cc-7e6b-4655-a869-bd429f986400
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 4b835c60da2d2848b33dd453605290a1fb056180
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765482"
---
# <a name="upgrade-sqlclr-assemblies-after-net-framework-update"></a>Aggiornare gli assembly SQLCLR dopo l'aggiornamento di .NET Framework

[!INCLUDE [SQL Server - Windows only ](../../includes/applies-to-version/sql-windows-only.md)]

  [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) è una raccolta di routine SQLCR (SQL Common Language Runtime) che fanno riferimento agli assembly Microsoft .NET Framework 4. Quando si installano aggiornamenti di .NET Framework nel computer che possono interessare un assembly .NET Framework di riferimento di questo tipo, tale operazione comporta una modifica nell'ID della versione del modulo (MVID, Module Version ID) dell'assembly nel Global Assembly Cache (GAC). Questa modifica determina una mancata corrispondenza tra i MVID dell'assembly a cui si fa riferimento nella GAC e l'assembly in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
 Se per l'aggiornamento di .NET Framework viene richiesto il riavvio del computer di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] , gli assembly SQLCLR interessati vengono aggiornati automaticamente per correggere il problema di mancata corrispondenza di MVID al riavvio di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] . Per gli aggiornamenti di .NET Framework per cui non è richiesto il riavvio del computer di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] , si verifica tuttavia un errore a causa della mancata corrispondenza nei MVID degli assembly quando si tenta di connettersi a [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] tramite [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]:  
  
```  
A new version of .NET was installed on this machine. In order to continue to work with DQS please run dqsinstaller.exe -upgradedlls.  
```  
  
 Per correggere questo problema, è necessario aggiornare gli assembly SQLCLR interessati in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . È possibile eseguire questa operazione eseguendo il file DQSInstaller.exe con il parametro della riga di comando **upgradedlls** per ignorare la ricreazione di database di DQS e aggiornare solo gli assembly interessati. In questo modo viene garantita l'integrità della Knowledge Base, dei progetti Data Quality e di qualsiasi altro dato di DQS.  
  
## <a name="prerequisites"></a>Prerequisiti  
  
-   È necessario aver eseguito l'accesso come membro del gruppo di amministratori nel computer di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .  
  
-   È necessario che l'account utente di Windows sia membro del ruolo predefinito del server sysadmin nell'istanza di SQL Server in cui è installato [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .  
  
### <a name="to-upgrade-sqlclr-assemblies"></a>Per aggiornare gli assembly SQLCLR  
  
1.  Avviare il prompt dei comandi.  
  
2.  Al prompt dei comandi impostare la directory sul percorso in cui è disponibile il file DQSInstaller.exe. Se è stata installata l'istanza predefinita di SQL Server, il file DQSinstaller.exe è disponibile in C:\Programmi\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn:  
  
    ```  
    cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn  
    ```  
  
3.  Al prompt dei comandi digitare il comando seguente e premere INVIO:  
  
    ```  
    dqsinstaller.exe -upgradedlls  
    ```  
  
4.  I passaggi rimanenti sono uguali ai passaggi 2-6 della sezione [Eseguire DQSInstaller.exe dalla schermata Start, dal menu Start o da Esplora risorse](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md#WindowsExplorer) in [Eseguire DQSInstaller.exe per completare l'installazione del server DQS](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Installare Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Aggiornare lo schema dei database DQS dopo l'installazione dell'aggiornamento di SQL Server](../../data-quality-services/install-windows/upgrade-dqs-databases-schema-after-installing-sql-server-update.md)  
  
  
