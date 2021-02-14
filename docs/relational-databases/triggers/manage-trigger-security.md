---
description: Gestione della sicurezza dei trigger
title: Gestire la sicurezza dei trigger | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- triggers [SQL Server], security
ms.assetid: e94720a8-a3a2-4364-b0a3-bbe86e3ce4d5
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 931ba1487b8212c6c088b044df5e0b9fa30b5ef1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347590"
---
# <a name="manage-trigger-security"></a>Gestione della sicurezza dei trigger

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Per impostazione predefinita, i trigger DML e DDL vengono eseguiti nel contesto dell'utente che li chiama. Il chiamante di un trigger è l'utente che esegue l'istruzione che causa l'esecuzione del trigger. Se ad esempio l'utente **Mary** esegue un'istruzione DELETE che causa l'esecuzione del trigger DML **DML_trigMary** , il codice all'interno di **DML_trigMary** viene eseguito nel contesto dei privilegi utente relativi a **Mary**. Il funzionamento predefinito può essere sfruttato dagli utenti che desiderano introdurre malware nel database o nell'istanza del server. Ad esempio, il trigger DDL seguente viene creato dall'utente **JohnDoe**:  

```sql
CREATE TRIGGER DDL_trigJohnDoe
ON DATABASE
FOR ALTER_TABLE
AS
SET NOCOUNT ON;

BEGIN TRY
  EXEC(N'
    USE [master];
    GRANT CONTROL SERVER TO [JohnDoe];
');
END TRY
BEGIN CATCH
  DECLARE @DoNothing INT;
END CATCH;
GO
```

Il trigger indica che non appena un utente autorizzato a eseguire un'istruzione `GRANT CONTROL SERVER`, ad esempio un membro del ruolo predefinito del server **sysadmin**, esegue un'istruzione `ALTER TABLE`, all'utente **JohnDoe** viene concessa l'autorizzazione `CONTROL SERVER`. In altre parole, anche se **JohnDoe** non può concedere l'autorizzazione `CONTROL SERVER` a se stesso, ha abilitato il codice del trigger che gli concede tale autorizzazione per l'esecuzione con privilegi con escalation. I trigger DML e DDL sono esposti a questo tipo di minaccia per la sicurezza.  
  
## <a name="trigger-security-best-practices"></a>Procedure consigliate per la sicurezza dei trigger  
 Per impedire l'esecuzione del codice del trigger con privilegi alzati di livello, è possibile adottare le misure seguenti:  
  
::: moniker range=">=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"

-   Individuare i trigger DML e DDL esistenti nel database e nell'istanza del server eseguendo query sulle viste del catalogo [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) e [sys.server_triggers](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md) . La query seguente restituisce tutti i trigger DML e DDL a livello di database nel database corrente e tutti i trigger DDL a livello di server nell'istanza del server:  
  
    ```sql
    SELECT type, name, parent_class_desc FROM sys.triggers
    UNION ALL
    SELECT type, name, parent_class_desc FROM sys.server_triggers;
    ```  

   > [!NOTE]
   > **sys.triggers** è disponibile solo per [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] a meno che non si usi [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)].

::: moniker-end

::: moniker range="=azuresqldb-current"

-   Individuare i trigger DML e DDL esistenti nel database eseguendo query sulla vista del catalogo [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md). La query seguente restituisce tutti i trigger DML e DDL a livello di database nel database corrente:  
  
    ```sql
    SELECT type, name, parent_class_desc FROM sys.triggers;
    ```  
  
::: moniker-end

-   Per disabilitare i trigger che possono compromettere l'integrità del database o del server se vengono eseguiti con privilegi alzati di livello, usare [DISABLE TRIGGER](../../t-sql/statements/disable-trigger-transact-sql.md) . L'istruzione seguente disabilita tutti i trigger DDL a livello di database nel database corrente:  
  
    ```sql
    DISABLE TRIGGER ALL ON DATABASE;
    ```  
  
     L'istruzione seguente disabilita tutti i trigger DDL a livello di server nell'istanza del server:  
  
    ```sql
    DISABLE TRIGGER ALL ON ALL SERVER;
    ```  
  
     L'istruzione seguente disabilita tutti i trigger DML nel database corrente:  
  
    ```sql
    DECLARE @schema_name sysname, @trigger_name sysname, @object_name sysname;
    DECLARE @sql nvarchar(max);
    DECLARE trig_cur CURSOR FORWARD_ONLY READ_ONLY FOR
        SELECT SCHEMA_NAME(schema_id) AS schema_name,
            name AS trigger_name,
            OBJECT_NAME(parent_object_id) AS object_name
        FROM sys.objects WHERE type IN ('TR', 'TA');

    OPEN trig_cur;
    FETCH NEXT FROM trig_cur INTO @schema_name, @trigger_name, @object_name;
  
    WHILE @@FETCH_STATUS = 0
    BEGIN
        SELECT @sql = N'DISABLE TRIGGER ' + QUOTENAME(@schema_name) + N'.'
            + QUOTENAME(@trigger_name)
            + N' ON ' + QUOTENAME(@schema_name) + N'.'
            + QUOTENAME(@object_name) + N'; ';
        EXEC (@sql);
        FETCH NEXT FROM trig_cur INTO @schema_name, @trigger_name, @object_name;
    END;
    GO

    -- Verify triggers are disabled. Should return an empty result set.
    SELECT * FROM sys.triggers WHERE is_disabled = 0;
    GO

    CLOSE trig_cur;
    DEALLOCATE trig_cur;
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Trigger DML](../../relational-databases/triggers/dml-triggers.md)   
 [Trigger DDL](../../relational-databases/triggers/ddl-triggers.md)  
 [Trigger LOGON](../../relational-databases/triggers/logon-triggers.md)  
  
