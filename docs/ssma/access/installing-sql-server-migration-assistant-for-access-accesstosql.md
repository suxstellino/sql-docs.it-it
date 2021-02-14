---
title: Installazione di SQL Server Migration Assistant per Access (AccessToSQL) | Microsoft Docs
description: Informazioni sui prerequisiti di installazione per SQL Server Migration Assistant (SSMA) per l'accesso e su come installare, concedere in licenza, aggiornare e disinstallare.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- installing SSMA
- instructions, installation
- instructions, upgrade
- licensing SSMA
- prerequisites for installing SSMA
- procedure, installation
- procedure, licensing
- procedure, upgrading
- removing SSMA
- Setup
- uninstalling SSMA
- upgrading SSMA
ms.assetid: dd50eebd-75df-4e0d-8c4d-88b511aae4c7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3e34f70a41816dcf9a6009f8b38feb016f409a3a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078962"
---
# <a name="installing-sql-server-migration-assistant-for-access-accesstosql"></a>Installazione di SQL Server Migration Assistant per Access (AccessToSQL)

[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Migration Assistant (SSMA) per l'accesso viene installato tramite una procedura guidata basata su Windows Installer. In questo argomento vengono fornite informazioni sui prerequisiti di installazione, un collegamento alla versione più recente di SSMA e istruzioni per l'installazione, la gestione delle licenze, la disinstallazione e l'aggiornamento di SSMA.

## <a name="prerequisites"></a>Prerequisiti

Prima di installare SSMA, verificare che il sistema soddisfi i requisiti seguenti:

- Windows 7 o versione successiva o Windows Server 2008 o una versione successiva.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o versione successiva.
- Il [!INCLUDE[msCoName](../../includes/msconame_md.md)] .NET Framework versione 4.7.2 o una versione successiva. Il .NET Framework è disponibile in [Microsoft .net Guida](/dotnet/framework/).
- Accesso a e autorizzazioni sufficienti per il computer che ospita l'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] a cui si eseguirà la migrazione di dati e oggetti di database.
- Provider Microsoft Data Access Object (DAO) versione 12,0 o 14,0. È possibile installare il provider DAO da Microsoft Office prodotto 2010/2007 o scaricarlo dal sito Web Microsoft.
- 4 GB di RAM (scelta consigliata).

## <a name="installing-ssma"></a>Installazione di SSMA

SSMA è un download Web. Per scaricare la versione più recente, vedere la [pagina di download SQL Server Migration Assistant](https://aka.ms/ssmaforaccess).

> [!IMPORTANT]
> Disinstallare tutte le versioni precedenti di SSMA per l'accesso prima di installare la nuova versione.

Per installare SSMA:
  
1. Fare doppio clic su **SSMAforAccess_ *n*. msi**, dove *n* è il numero di Build.
2. Nella pagina Benvenuti fare clic su **Avanti**.

   Se i prerequisiti non sono installati, viene visualizzato un messaggio che indica che è necessario prima installare i componenti necessari. Assicurarsi di aver installato tutti i prerequisiti, quindi eseguire di nuovo il programma di installazione.

3. Leggi il contratto di licenza End-User; Se si accettano le condizioni, selezionare Accetto **il contratto**, quindi fare clic su **Avanti**.
4. Nella pagina **Selezione tipo di installazione** fare clic su **tipico**.
5. Nella pagina **pronto per l'installazione** è possibile abilitare o disabilitare la telemetria e i controlli di aggiornamento automatici ogni volta che viene avviato lo strumento. Fare clic su **Installa** per avviare l'installazione.
  
Il percorso di installazione predefinito è `C:\Program Files\Microsoft SQL Server Migration Assistant for Access`.

## <a name="uninstalling-ssma-for-access"></a>Disinstallazione di SSMA per l'accesso

Disinstallare SSMA utilizzando **Installazione applicazioni** nel pannello di controllo. Tenere presente che la disinstallazione del programma non elimina i file di progetto o i file di log di SSMA.

Per disinstallare SSMA:

1. Fare clic su **Start**, su **Pannello di controllo** e infine su **Installazione applicazioni**.
2. Selezionare **Microsoft SQL Server Migration Assistant per l'accesso**, quindi fare clic su **Rimuovi**.

## <a name="upgrading-to-a-later-version"></a>Aggiornamento a una versione successiva

Se si vuole eseguire l'aggiornamento a una versione successiva di SSMA per l'accesso, è necessario prima disinstallare SSMA per l'accesso e quindi installare la versione più recente. Per completare questo processo, seguire le istruzioni riportate nella sezione Disinstallazione di SSMA per l'accesso.

Se si apre un progetto creato in una versione precedente di SSMA per l'accesso, SSMA potrebbe chiedere se si vuole convertire il progetto nella versione più recente. Fare clic su **Sì** per usare il progetto nella versione più recente di SSMA.

## <a name="see-also"></a>Vedi anche

- [Preparazione dei database di Access per la migrazione](preparing-access-databases-for-migration-accesstosql.md)
- [Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
- [Collegamento di applicazioni di accesso a SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)