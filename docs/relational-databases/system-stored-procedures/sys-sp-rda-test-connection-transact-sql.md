---
title: sys.sp_rda_test_connection (Transact-SQL) | Microsoft Docs
description: Informazioni su come usare sys.sp_rda_test_connection per testare la connessione da SQL Server al server Azure remoto e segnala problemi che possono impedire la migrazione dei dati.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_test_connection
- sys.sp_rda_test_connection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_test_connection stored procedure
ms.assetid: e2ba050c-d7e3-4f33-8281-c9b525b4edb4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e584604a0229e3e2e0c213b70d8fd4bca0321c2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211720"
---
# <a name="syssp_rda_test_connection-transact-sql"></a>sys.sp_rda_test_connection (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Verifica la connessione da SQL Server al server Azure remoto e segnala problemi che possono impedire la migrazione dei dati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
EXECUTE sys.sp_rda_test_connection  
   @database_name = N'db_name',   
   @server_address = N'azure_server_fully_qualified_address',  
   @azure_username = N'azure_username',   
   @azure_password = N'azure_password',  
   @credential_name = N'credential_name'  
  
```  
  
## <a name="arguments"></a>Argomenti  
 @database_name = N'*db_name*'  
 Nome del database di SQL Server abilitato per l'estensione. Questo parametro è facoltativo.  
  
 @server_address = N'*azure_server_fully_qualified_address*'  
 Indirizzo completo del server Azure.  
  
-   Se si specifica un valore per **\@ database_name**, ma il database specificato non è abilitato per l'estensione, è necessario specificare un valore per **\@ server_address**.  
  
-   Se si specifica un valore per **\@ database_name** e il database specificato è abilitato per l'estensione, non è necessario specificare un valore per **\@ server_address**. Se si specifica un valore per **\@ server_address**, il stored procedure lo ignora e usa il server di Azure esistente già associato al database abilitato per l'estensione.  
  
 @azure_username = N'*azure_username*  
 Nome utente del server Azure remoto.  
  
 @azure_password = N'*azure_password*'  
 Password per il server Azure remoto.  
  
 @credential_name = N'*credential_name*'  
 Anziché fornire un nome utente e una password, è possibile specificare il nome di una credenziale archiviata nel database abilitato per l'estensione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 In caso di **esito positivo**, sp_rda_test_connection restituisce l'errore 14855 (STRETCH_MAJOR, STRETCH_CONNECTION_TEST_PROC_SUCCEEDED) con EX_INFO di gravità e un codice restituito con esito positivo.  
  
 In caso di **errore**, sp_rda_test_connection restituisce l'errore 14856 (STRETCH_MAJOR, STRETCH_CONNECTION_TEST_PROC_FAILED) con EX_USER di gravità e un codice di errore restituito.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|link_state|INT|Uno dei valori seguenti, che corrispondono ai valori per **link_state_desc**.<br /><br /> -0<br />-1<br />-2<br />-3<br />-4|  
|link_state_desc|varchar(32)|Uno dei valori seguenti, che corrispondono ai valori precedenti per **link_state**.<br /><br /> -INTEGRo<br />     Il tra SQL Server e il server Azure remoto è integro.<br />-ERROR_AZURE_FIREWALL<br />     Il firewall di Azure impedisce il collegamento tra SQL Server e il server Azure remoto.<br />-ERROR_NO_CONNECTION<br />     SQL Server non è in grado di effettuare una connessione al server Azure remoto.<br />-ERROR_AUTH_FAILURE<br />     Un errore di autenticazione impedisce il collegamento tra SQL Server e il server Azure remoto.<br />-ERRORE<br />     Un errore che non è un problema di autenticazione, un problema di connettività o un problema del firewall impedisce il collegamento tra SQL Server e il server Azure remoto.|  
|error_number|INT|Numero dell'errore. Se non si verifica alcun errore, il campo è NULL.|  
|error_message|nvarchar(1024)|Messaggio di errore. Se non si verifica alcun errore, il campo è NULL.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede autorizzazioni db_owner.  
  
## <a name="examples"></a>Esempio  
  
### <a name="check-the-connection-from-sql-server-to-the-remote-azure-server"></a>Controllare la connessione dal SQL Server al server Azure remoto  
  
```sql  
EXECUTE sys.sp_rda_test_connection @database_name = N'<Stretch-enabled database>'  
GO  
  
```  
  
 I risultati mostrano che SQL Server non è in grado di connettersi al server Azure remoto.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|2|ERROR_NO_CONNECTION|*\<connection-related error number>*|*\<connection-related error message>*|  
  
### <a name="check-the-azure-firewall"></a>Controllare il firewall di Azure  
  
```sql  
USE <Stretch-enabled database>  
GO  
EXECUTE sys.sp_rda_test_connection  
GO  
  
```  
  
 I risultati mostrano che il firewall di Azure impedisce il collegamento tra SQL Server e il server Azure remoto.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|1|ERROR_AZURE_FIREWALL|*\<firewall-related error number>*|*\<firewall-related error message>*|  
  
### <a name="check-authentication-credentials"></a>Verifica credenziali di autenticazione  
  
```sql  
USE <Stretch-enabled database>  
GO  
EXECUTE sys.sp_rda_test_connection  
GO  
  
```  
  
 I risultati mostrano che un errore di autenticazione impedisce il collegamento tra SQL Server e il server Azure remoto.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|3|ERROR_AUTH_FAILURE|*\<authentication-related error number>*|*\<authentication-related error message>*|  
  
### <a name="check-the-status-of-the-remote-azure-server"></a>Verificare lo stato del server Azure remoto  
  
```sql  
USE <SQL Server database>  
GO  
EXECUTE sys.sp_rda_test_connection   
    @server_address = N'<server name>.database.windows.net',   
    @azure_username = N'<user name>',   
    @azure_password = N'<password>'  
GO  
  
```  
  
 I risultati mostrano che la connessione è integra e che è possibile abilitare Stretch Database per il database specificato.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|0|HEALTHY|NULL|NULL|  
  
  
