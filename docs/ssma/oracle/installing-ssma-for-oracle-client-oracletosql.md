---
title: Installazione di SSMA per il client Oracle (OracleToSQL) | Microsoft Docs
description: Informazioni sui prerequisiti di installazione per il SQL Server Migration Assistant (SSMA) per il client Oracle e su come installare.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d5d4903d-e296-4bbf-8780-63674c4d62d5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 5bed1589c0852acc2d782028f6b9cc253301babe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073492"
---
# <a name="installing-ssma-for-oracle-client-oracletosql"></a>Installazione di SSMA per il client Oracle (OracleToSQL)

Il client SSMA è costituito dai file di programma che eseguono le attività seguenti:  
  
- Connettersi a un database Oracle.
- Connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Convertire gli oggetti di database Oracle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Syntax.
- Caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
- Eseguire la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

In questo argomento vengono forniti i prerequisiti e le istruzioni per l'installazione di SSMA.

## <a name="prerequisites"></a>Prerequisiti

SSMA è progettato per funzionare con Oracle 9 o versioni successive e tutte le edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Prima di installare SSMA, verificare che il computer soddisfi i requisiti seguenti:

- Windows 7 o versioni successive o Windows Server 2008 o versioni successive.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o versione successiva.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] Versione 4.7.2 o successiva. È possibile ottenerlo dal [.NET Framework Developer Center](https://go.microsoft.com/fwlink/?LinkId=48882).
- Connettività ai database Oracle di cui si desidera eseguire la migrazione.
- Accesso a e autorizzazioni sufficienti per il computer che ospita l'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] cui si eseguirà la migrazione di dati e oggetti di database. Per ulteriori informazioni, vedere [connessione a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md).
- 4 GB di RAM consigliata.  
  
## <a name="installing-the-ssma-for-oracle-client"></a>Installazione di SSMA per il client Oracle

SSMA è un download Web. Per scaricare la versione più recente, vedere la [pagina di download SQL Server Migration Assistant](https://aka.ms/ssmafororacle).

Per installare il client di SSMA:

1. Fare doppio clic su **SSMAforOracle_ *n*. msi**, dove *n* è il numero di Build.
2. Nella pagina Benvenuti fare clic su **Avanti**.

   Se i prerequisiti non sono installati, verrà visualizzato un messaggio che indica che è necessario prima installare i componenti necessari. Assicurarsi di aver installato tutti i prerequisiti, quindi eseguire di nuovo il programma di installazione.  

3. Leggere il contratto di licenza End-User. Se si accettano le condizioni, selezionare Accetto **il contratto**, quindi fare clic su **Avanti**.
4. Nella pagina **Selezione tipo di installazione** fare clic su **tipico**.
5. Nella pagina **pronto per l'installazione** è possibile abilitare o disabilitare la telemetria e i controlli di aggiornamento automatici ogni volta che viene avviato lo strumento. Fare clic su **Installa** per avviare l'installazione.

> [!IMPORTANT]
> Prima di installare la nuova versione, disinstallare tutte le versioni precedenti di SSMA per Oracle.

Il percorso di installazione predefinito è `C:\Program Files\Microsoft SQL Server Migration Assistant for Oracle`.

Oltre ai file di programma SSMA, è necessario installare anche SSMA per Oracle Extension Pack in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni, vedere [installazione dei componenti di SSMA su SQL Server](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md).

## <a name="see-also"></a>Vedi anche

- [Installazione di componenti SSMA in SQL Server](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md)
- [Migrazione di database Oracle a SQL Server](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
