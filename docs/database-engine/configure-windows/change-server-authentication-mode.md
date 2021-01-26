---
title: Modifica della modalità di autenticazione del server
description: Informazioni su come modificare la modalità di autenticazione del server in SQL Server. Per questa attività è possibile usare SQL Server Management Studio o Transact-SQL.
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- sa account
- authentication [SQL Server], changing modes
- server authentication mode [SQL Server]
- modifying server authentication mode
ms.assetid: 79babcf8-19fd-4495-b8eb-453dc575cac0
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.date: 02/18/2020
ms.openlocfilehash: a4cdc32bd0514be768ac8ed1723fc88840a7e930
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783418"
---
# <a name="change-server-authentication-mode"></a>Modificare la modalità di autenticazione del server

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo argomento viene descritto come modificare la modalità di autenticazione del server in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Durante l'installazione [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] è impostato su **Autenticazione di Windows** o **Autenticazione di SQL Server e di Windows**. Dopo l'installazione, è possibile modificare in qualsiasi momento la modalità di autenticazione.

Se si seleziona **Modalità di autenticazione di Windows** durante l'installazione, l'account di accesso sa viene disabilitato e il programma di installazione assegna una password. Se in seguito si modifica la modalità di autenticazione in **Autenticazione di SQL Server e di Windows**, l'account di accesso sa resterà disabilitato. Per usare l'account di accesso sa, usare l'istruzione ALTER LOGIN per abilitare l'account sa e assegnare una nuova password. È possibile connettersi al server tramite l'account sa solo se si utilizza l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="before-you-begin"></a>Prima di iniziare

L'account sa è un account noto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che viene spesso preso di mira da utenti malintenzionati. Non abilitare l'account sa a meno che l'applicazione non lo richieda. È importante usare una password complessa per l'accesso all'account sa.

## <a name="change-authentication-mode-with-ssms"></a>Modificare la modalità di autenticazione con SSMS

1. In Esplora oggetti di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fare clic con il pulsante destro del mouse sul server e quindi scegliere **Proprietà**.

2. Nella pagina **Sicurezza** selezionare la nuova modalità di autenticazione del server dall'elenco **Autenticazione server** e quindi fare clic su **OK**.

3. Nella finestra di dialogo [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fare clic su **OK** per confermare il riavvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

4. In Esplora oggetti fare clic con il pulsante destro del mouse sul server e quindi scegliere **Riavvia**. Se è in esecuzione, è necessario riavviare anche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.

## <a name="enable-sa-login"></a>Abilitare l'account di accesso sa

È possibile abilitare l'account di accesso **sa** con SSMS o T-SQL.

### <a name="use-ssms"></a>Usare SSMS

1. In Esplora oggetti espandere **Sicurezza** e quindi Account di accesso, fare clic con il pulsante destro del mouse su **sa** e infine scegliere **Proprietà**.

2. Nella pagina **Generale** potrebbe essere necessario creare e confermare una password per l'account di accesso **sa**.

3. Nella pagina **Stato** fare clic su **Abilitato** nella sezione **Account di accesso**, quindi scegliere **OK**.

### <a name="using-transact-sql"></a>Uso di Transact-SQL

Nell'esempio seguente viene abilitato l'account di accesso sa e viene impostata una nuova password. Sostituire `<enterStrongPasswordHere>` con una password complessa prima di eseguire l'esempio.

```sql  
ALTER LOGIN sa ENABLE ;  
GO  
ALTER LOGIN sa WITH PASSWORD = '<enterStrongPasswordHere>' ;  
GO  
```

## <a name="change-authentication-mode-t-sql"></a>Modificare la modalità di autenticazione (T-SQL)

Nell'esempio seguente l'autenticazione server viene modificata da modalità mista (Windows + SQL) a solo Windows.

> [!CAUTION]
> Nell'esempio seguente viene usata una stored procedure estesa per modificare il Registro di sistema del server. L'errata modifica del Registro di sistema può causare problemi gravi. Questi problemi potrebbero richiedere la reinstallazione del sistema operativo. Microsoft non è in grado di garantire che questi problemi possano essere risolti. La modifica del Registro di sistema è a rischio e pericolo dell'utente.

```sql
USE [master]
GO
EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', 
     N'Software\Microsoft\MSSQLServer\MSSQLServer',
     N'LoginMode', REG_DWORD, 1
GO
```

> [!Note]
> Le autorizzazioni necessarie per cambiare la modalità di autenticazione sono [sysadmin](../../relational-databases/security/authentication-access/server-level-roles.md#fixed-server-level-roles) o [Controllo server](../../relational-databases/security/permissions-database-engine.md)

## <a name="see-also"></a>Vedi anche

- [Password complesse](../../relational-databases/security/strong-passwords.md)
- [Considerazioni sulla sicurezza per un'installazione di SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)
- [Connettersi a SQL Server se gli amministratori di sistema sono bloccati](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)
