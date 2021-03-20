---
title: Credenziali (motore di database) | Microsoft Docs
description: Informazioni sulle credenziali in SQL Server. Descrizione delle informazioni di autenticazione necessarie per connettersi a una risorsa all'esterno di SQL Server.
ms.custom: ''
ms.date: 06/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- principals [SQL Server], credentials
- schemas [SQL Server], credentials
- permissions [SQL Server], credentials
- groups [SQL Server], credentials
- ALTER ANY CREDENTIAL permission
- security [SQL Server], credentials
- authentication [SQL Server], credentials
- users [SQL Server], credentials
- credentials [SQL Server], about credentials
- credentials [SQL Server]
ms.assetid: c8df6022-e0b4-46b8-9670-3f86938d3177
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 605b9373f21a7a1a0776f154851b03cad2caac1c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753271"
---
# <a name="credentials-database-engine"></a>Credenziali (Motore di database)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Una credenziale è un record contenente le informazioni di autenticazione (credenziali) necessarie per connettersi a una risorsa all'esterno di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Queste informazioni vengono utilizzate internamente da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La maggior parte delle credenziali contiene un nome utente e una password di Windows.  
  
 Le informazioni archiviate in una credenziale consentono a un utente che ha eseguito la connessione a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tramite l'autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] di accedere a risorse esterne all'istanza del server. Quando la risorsa esterna è Windows, l'utente viene autenticato come l'utente di Windows specificato nella credenziale. È possibile eseguire il mapping di una singola credenziale solo a un singolo account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. È inoltre possibile eseguire il mapping degli account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a una sola credenziale.  
  
 Per le credenziali archiviate nel database master che possono essere usate nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vedere [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md). Per le credenziali usate da un database specifico e trasferibili con il database, vedere [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-scoped-credential-transact-sql.md).  
  
 Le credenziali del sistema vengono create automaticamente e associate a endpoint specifici. I nomi delle credenziali di sistema iniziano con due segni di cancelletto (##).  
  
 Per altre informazioni sulle credenziali, vedere le viste del catalogo [sys.credentials](../../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md) e [sys.database_scoped_credentials](../../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md).  
  
## <a name="related-content"></a>Contenuto correlato  
 [Creare una credenziale](../../../relational-databases/security/authentication-access/create-a-credential.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)   
 [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-scoped-credential-transact-sql.md)  
 [Sicurezza di SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
  
