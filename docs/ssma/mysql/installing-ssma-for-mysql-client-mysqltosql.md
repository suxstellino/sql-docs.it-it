---
title: Installazione di SSMA per il client MySQL (MySQLToSQL) | Microsoft Docs
description: Informazioni sui prerequisiti di installazione per il SQL Server Migration Assistant (SSMA) per il client MySQL e su come installare.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Installing client,Licensing
ms.assetid: ede3128c-370d-45a5-a815-3d94eecaea30
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e2eaa9daf9128b43ac061cad66e7e993882e43d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070136"
---
# <a name="installing-ssma-for-mysql-client-mysqltosql"></a>Installazione di SSMA per il client MySQL (MySQLToSQL)

Il client SSMA per MySQL è costituito dai file di programma che eseguono le attività seguenti:

- Connettersi a un database MySQL.  
- Connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .
- Convertire gli oggetti di database MySQL negli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] oggetti o.
- Caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .
- Eseguire la migrazione dei dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

Questo argomento illustra i prerequisiti di installazione e le istruzioni per l'installazione di SSMA per il client MySQL.

## <a name="prerequisites"></a>Prerequisiti

SSMA per MySQL è progettato per funzionare con MySQL 4,1 o versioni successive e tutte le edizioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 o successive, e [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

Prima di installare SSMA, verificare che il computer soddisfi i requisiti seguenti:

- Windows 7 o versioni successive o Windows Server 2008 o versioni successive.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o versione successiva.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] Versione 4.7.2 o successiva. È possibile ottenerlo dal [.NET Framework Developer Center](https://go.microsoft.com/fwlink/?LinkId=48882).
- Driver MySQL ODBC 5,1 e connettività ai database MySQL di cui si vuole eseguire la migrazione. È possibile installare MySQL dal sito web MySQL. Per informazioni sulla connettività, vedere la pagina relativa [alla connessione a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md).
- Accesso a e autorizzazioni sufficienti per il computer che ospita l'istanza di destinazione di in cui verrà eseguita la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] migrazione di dati e oggetti di database. Per ulteriori informazioni, vedere [connessione a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md).
- Nel caso di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] progetti, accedere a e a autorizzazioni sufficienti per l'istanza di in cui verrà eseguita la [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] migrazione di dati e oggetti di database. Per altre informazioni, vedere [connessione al database SQL di Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md).
- 4 GB di RAM consigliata.

## <a name="installing-ssma-for-mysql-client"></a>Installazione di SSMA per il client MySQL

SSMA è un download Web. Per scaricare la versione più recente, vedere la [pagina di download SQL Server Migration Assistant](https://aka.ms/ssmaformysql).

Per installare il client di SSMA:

1. Fare doppio clic su **SSMAforMySQL_ *n*. msi**, dove *n* è il numero di Build.
2. Nella pagina **Benvenuti** fare clic su **Avanti**.

   Se i prerequisiti non sono installati, verrà visualizzato un messaggio che indica che è necessario prima installare i componenti necessari. Assicurarsi di aver installato tutti i prerequisiti prima di eseguire di nuovo il programma di installazione.

3. Leggere il contratto di licenza End-User. Se si accettano le condizioni, selezionare Accetto **il contratto**, quindi fare clic su **Avanti**.
4. Nella pagina **Selezione tipo di installazione** fare clic su **tipico**.
5. Nella pagina **pronto per l'installazione** è possibile abilitare o disabilitare la telemetria e i controlli di aggiornamento automatici ogni volta che viene avviato lo strumento. Fare clic su **Installa** per avviare l'installazione.

> [!IMPORTANT]
> Disinstallare tutte le versioni precedenti di SSMA per MySQL prima di installare la nuova versione.

Il percorso di installazione predefinito è `C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL`.

## <a name="see-also"></a>Vedi anche

- [Migrazione di database MySQL a SQL Server-database SQL di Azure](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
