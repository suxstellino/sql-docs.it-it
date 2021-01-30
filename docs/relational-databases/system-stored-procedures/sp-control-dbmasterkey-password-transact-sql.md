---
description: sp_control_dbmasterkey_password (Transact-SQL)
title: sp_control_dbmasterkey_password (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_control_dbmasterkey_password
- sp_control_dbmasterkey_password_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_control_dbmasterkey_password
ms.assetid: 63979a87-42a2-446e-8e43-30481faaf3ca
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 06de3460a55eecba6c1525576caec85d6baa905f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190037"
---
# <a name="sp_control_dbmasterkey_password-transact-sql"></a>sp_control_dbmasterkey_password (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiunge o rimuove una credenziale contenente la password necessaria per aprire una chiave master del database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_control_dbmasterkey_password @db_name = 'database_name,  
     @password = 'master_key_password' , @action = { 'add' | 'drop' }  
```  
  
## <a name="arguments"></a>Argomenti  
 @db_name= N'*database_name*'  
 Specifica il nome del database associato alla credenziale. Non è possibile specificare un database di sistema. *database_name* è di **tipo nvarchar**.  
  
 @password= N'*password*'  
 Specifica la password della chiave master. la *password* è di **tipo nvarchar**.  
  
 @action= N'Add '  
 Specifica che una credenziale per il database specificato verrà aggiunta all'archivio delle credenziali. La credenziale conterrà la password della chiave master del database. Il valore passato a @action è **nvarchar**.  
  
 @action= N'drop '  
 Specifica che una credenziale per il database specificato verrà rimossa dall'archivio delle credenziali. Il valore passato a @action è **nvarchar**.  
  
## <a name="remarks"></a>Commenti  
 Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve utilizzare una chiave master del database per decrittografare o crittografare una chiave, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue un tentativo di decrittografia della chiave master del database con la chiave master del servizio dell'istanza. Se il tentativo non riesce, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue una ricerca nell'archivio delle credenziali per individuare le credenziali di chiave master con lo stesso GUID del database per cui è necessaria la chiave master. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si tenta quindi di decrittografare la chiave master del database con ogni credenziale corrispondente fino a quando non si riesce a completare l'operazione o non sono disponibili altre credenziali da provare.  
  
> [!CAUTION]  
>  Non creare una credenziale per la chiave master di un database che non deve essere accessibile per l'account sa e altre entità server con privilegi elevati. È possibile configurare un database in modo che la sua gerarchia di chiavi non possa essere decrittografata dalla chiave master del servizio. Questa opzione è supportata come difesa in profondità per i database contenenti informazioni crittografate che non devono essere accessibili per l'account sa o altre entità server con privilegi elevati. La creazione di una credenziale per la chiave master di un database con queste esigenze annulla questa difesa in profondità, consentendo all'account sa e ad altre entità server con privilegi elevati di decrittografare il database.  
  
 Le credenziali create utilizzando sp_control_dbmasterkey_password sono visibili nella vista del catalogo [sys.master_key_passwords](../../relational-databases/system-catalog-views/sys-master-key-passwords-transact-sql.md) . I nomi delle credenziali create per le chiavi master dei database hanno il formato seguente: `##DBMKEY_<database_family_guid>_<random_password_guid>##`. La password viene archiviata come segreto della credenziale. Per ogni password aggiunta all'archivio delle credenziali esiste una riga in sys.credentials.  
  
 Non è possibile utilizzare sp_control_dbmasterkey_password per creare una credenziale per i database di sistema seguenti: master, model, msdb o tempdb.  
  
 sp_control_dbmasterkey_password non verifica che la password consenta effettivamente di aprire la chiave master del database specificato.  
  
 Se si specifica una password già archiviata in una credenziale per il database specificato, l'esecuzione di sp_control_dbmasterkey_password avrà esito negativo.  
  
> [!NOTE]  
>  Due database disponibili in istanze diverse del server possono condividere lo stesso GUID. In questo caso, per i database verranno utilizzati gli stessi record di chiave master nell'archivio delle credenziali.  
  
 I parametri passati a sp_control_dbmasterkey_password non sono visibili nelle tracce.  
  
> [!NOTE]  
>  Quando si utilizza la credenziale aggiunta tramite sp_control_dbmasterkey_password per aprire la chiave del database master, quest'ultima viene nuovamente crittografata dalla chiave master del servizio. Se il database si trova in modalità sola lettura, non sarà possibile eseguire nuovamente la crittografia e la chiave master del database rimarrà non crittografata. Per accedere successivamente alla chiave master del database, è necessario utilizzare l'istruzione OPEN MASTER KEY e una password. Per evitare di utilizzare una password, creare le credenziali prima di applicare al database la modalità sola lettura.  
  
 **Potenziale problema di compatibilità con le versioni precedenti:** Attualmente, il stored procedure non controlla se esiste una chiave master. Questo è consentito solo per la compatibilità con le versioni precedenti, ma viene visualizzato un avviso. Questo comportamento è deprecato. In una versione futura la chiave master deve esistere e la password usata nella stored procedure **sp_control_dbmasterkey_password** deve avere la stessa password di una delle password usate per crittografare la chiave master del database.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-credential-for-the-adventureworks2012-master-key"></a>R. Creazione di una credenziale per la chiave master di AdventureWorks2012  
 Nell'esempio seguente viene creata una credenziale per la chiave master del database `AdventureWorks2012` e la password della chiave master viene salvata come segreto nella credenziale. Poiché tutti i parametri passati a `sp_control_dbmasterkey_password` devono essere di tipo di dati **nvarchar**, le stringhe di testo vengono convertite con l'operatore di Cast `N` .  
  
```  
EXEC sp_control_dbmasterkey_password @db_name = N'AdventureWorks2012',   
    @password = N'sdfjlkj#mM00sdfdsf98093258jJlfdk4', @action = N'add';  
GO  
```  
  
### <a name="b-dropping-a-credential-for-a-database-master-key"></a>B. Rimozione di una credenziale per una chiave master del database  
 Nell'esempio seguente viene rimossa la credenziale creata nell'esempio A. Si noti che tutti i parametri sono obbligatori, inclusa la password.  
  
```  
EXEC sp_control_dbmasterkey_password @db_name = N'AdventureWorks2012',   
    @password = N'sdfjlkj#mM00sdfdsf98093258jJlfdk4', @action = N'drop';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Impostare un database mirror crittografato](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)   
 [Credenziali &#40;motore di database&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)  
  
  
