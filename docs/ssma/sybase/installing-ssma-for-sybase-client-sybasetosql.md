---
title: Installazione di SSMA per il client SAP ASE (SybaseToSQL) | Microsoft Docs
description: Informazioni sui prerequisiti di installazione per SQL Server Migration Assistant (SSMA) per SAP Adaptive Server Enterprise (ASE) e su come installare.
ms.custom: ''
ms.date: 07/14/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: e770c2f2-52b9-4471-a207-0d35df41399c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d9b20774b0a32b0d9d007053bedd2674e79d4e12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015371"
---
# <a name="installing-ssma-for-sap-ase-client-sybasetosql"></a>Installazione di SSMA per il client SAP ASE (SybaseToSQL)

Il client SSMA è costituito dai file di programma usati per connettersi a un server di database di SAP Adaptive Server Enterprise (ASE) e da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , convertire gli oggetti di database dell'ambiente del servizio app [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] nella sintassi o, caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , quindi migrare i dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

In questo argomento vengono forniti i prerequisiti e le istruzioni per l'installazione di SSMA.

## <a name="prerequisites"></a>Prerequisiti

SSMA è progettato per funzionare con SAP ASE 11.9.2 o versioni successive e tutte le edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Prima di installare SSMA, verificare che il computer soddisfi i requisiti seguenti:

- Windows 7 o versioni successive o Windows Server 2008 o versioni successive.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o versione successiva.
- Il [!INCLUDE[msCoName](../../includes/msconame_md.md)] .NET Framework versione 4.7.2 o una versione successiva. È possibile ottenerlo dal [.NET Framework Developer Center](https://go.microsoft.com/fwlink/?LinkId=48882).
- Il provider OLE DB/ADO.Net/ODBC Sybase e la connettività al server di database SAP ASE che contiene i database di cui si desidera eseguire la migrazione. È possibile installare i provider dal supporto del prodotto SAP ASE. Per informazioni sulla connettività, vedere [connessione a Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).
- Accesso a e autorizzazioni sufficienti per il computer che ospita l'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] cui si eseguirà la migrazione di dati e oggetti di database. Per altre informazioni, vedere [connessione a SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sql-server-sybasetosql.md) / [connessione al database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md).
- 4 GB di RAM consigliata.

## <a name="installing-the-ssma-for-sybase-client"></a>Installazione di SSMA per il client Sybase

SSMA è un download Web. Per scaricare la versione più recente, vedere la [pagina di download SQL Server Migration Assistant](https://aka.ms/ssmaforsybase).

Per installare il client di SSMA:

1. Fare doppio clic su **SSMAforSybase_ *n*. msi**, dove *n* è il numero di Build.
2. Nella pagina Benvenuti fare clic su **Avanti**.

   Se i prerequisiti non sono installati, verrà visualizzato un messaggio che indica che è necessario prima installare i componenti necessari. Assicurarsi di aver installato tutti i prerequisiti, quindi eseguire di nuovo il programma di installazione.

3. Leggere il contratto di licenza End-User. Se si accettano le condizioni, selezionare Accetto **il contratto**, quindi fare clic su **Avanti**.
4. Nella pagina Selezione tipo di installazione fare clic su **tipico**.
5. Nella pagina **pronto per l'installazione** è possibile abilitare o disabilitare la telemetria e i controlli di aggiornamento automatici ogni volta che viene avviato lo strumento. Fare clic su **Installa** per avviare l'installazione.

> [!IMPORTANT]
> Disinstallare tutte le versioni precedenti di SSMA per Sybase prima di installare la nuova versione.

Il percorso di installazione predefinito è `C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase`.

Oltre ai file di programma SSMA, è necessario installare anche SSMA per Sybase Extension Pack in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni, vedere [installazione dei componenti SSMA in SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md).

## <a name="see-also"></a>Vedi anche

- [Installazione di componenti SSMA in SQL Server](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
- [Migrazione di database Sybase ASE a SQL Server-database SQL di Azure](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)
