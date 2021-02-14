---
description: Connetti a SQL Server (AccessToSQL)
title: Connetti a SQL Server (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ceb77a97-d6d5-4a92-90a6-342e97d12b54
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f2c98dedcb785f7425bde3f3186aec278bb762d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076282"
---
# <a name="connect-to-sql-server-accesstosql"></a>Connetti a SQL Server (AccessToSQL)
Utilizzare la finestra di dialogo **Connetti a SQL Server** per connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui si desidera eseguire la migrazione. Per accedere alla finestra di dialogo **Connetti a SQL Server** , scegliere **Connetti a SQL Server** dal menu **file** .  
  
## <a name="options"></a>Opzioni  
**Nome server**  
Consente di immettere o selezionare l'istanza di SQL Server a cui connettersi. Per impostazione predefinita, viene visualizzata l'istanza di connessa più di recente.  
  
-   Se ci si connette all'istanza predefinita nel computer locale, è possibile immettere **localhost** o un punto (**.**).  
  
-   Se ci si connette all'istanza predefinita in un altro computer, immettere il nome del computer.  
  
-   Se ci si connette a un'istanza denominata in un altro computer, immettere il nome del computer, una barra rovesciata e il nome dell'istanza, ad esempio *MyServer* \\ *istanza*.  
  
**Porta server**  
Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è configurata in modo da accettare le connessioni sulla porta predefinita (1433), immettere il numero di porta. In caso contrario, lasciare vuoto questo valore.  
  
**Database**  
Consente di specificare il database in cui eseguire la migrazione di oggetti e dati. Questa opzione non è disponibile quando si esegue la riconnessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**autenticazione**  
Selezionare il metodo di autenticazione utilizzato per la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per utilizzare l'account di Windows corrente, selezionare autenticazione di Windows. Per specificare un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso e una password, selezionare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione.  
  
**Nome utente**  
Se si utilizza l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione di, immettere l'account di accesso per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se si utilizza l'autenticazione di Windows, questa opzione non è disponibile.  
  
**Password**  
Se si utilizza l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticazione di, immettere la password per l'account di accesso in tale istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se si utilizza l'autenticazione di Windows, questa opzione non è disponibile.  
  
**Encrypt Connection**  
Se si desidera connettersi in modo sicuro a SQL Server, utilizzare la casella di controllo Crittografa connessione selezionando la casella di controllo **Crittografa connessione** .  
  
**TrustServerCertificate**  
Se si desidera utilizzare questa opzione, selezionare la casella di controllo **certificato server attendibile** .  
  
> [!NOTE]  
> Per abilitare il **certificato del server di attendibilità**, "Encrypt" deve essere impostato su **true**.  
  
