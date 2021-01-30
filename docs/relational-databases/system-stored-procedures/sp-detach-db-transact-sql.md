---
description: sp_detach_db (Transact-SQL)
title: sp_detach_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/30/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_detach_db
- sp_detach_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_detach_db
- detaching databases [SQL Server]
ms.assetid: abcb1407-ff78-4c76-b02e-509c86574462
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1d4b484cd6d440b14b7639b513fba7e53f615443
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200755"
---
# <a name="sp_detach_db-transact-sql"></a>sp_detach_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Scollega un database attualmente non in uso da un'istanza del server e, facoltativamente, esegue UPDATE STATISTICS su tutte le tabelle prima dello scollegamento.  
  
> [!IMPORTANT]  
>  Per poter scollegare un database replicato, è necessario che non sia pubblicato. Per ulteriori informazioni, vedere la sezione "Osservazioni" di seguito in questo argomento.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_detach_db [ @dbname= ] 'database_name'   
    [ , [ @skipchecks= ] 'skipchecks' ]   
    [ , [ @keepfulltextindexfile = ] 'KeepFulltextIndexFile' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbname = ] 'database_name'` Nome del database da scollegare. *database_name* è un valore **sysname** e il valore predefinito è null.  
  
`[ @skipchecks = ] 'skipchecks'` Specifica se ignorare o eseguire UPDATE STATISTIC. *skipchecks* è un valore **nvarchar (10)** e il valore predefinito è null. Per ignorare UPDATE STATISTICs, specificare **true**. Per eseguire in modo esplicito UPDATE STATISTICs, specificare **false**.  
  
 Per impostazione predefinita, l'istruzione UPDATE STATISTICS viene eseguita per aggiornare le informazioni sui dati nelle tabelle e negli indici. L'esecuzione di UPDATE STATISTICS risulta utile per i database che devono essere spostati su supporti di sola lettura.  
  
`[ @keepfulltextindexfile = ] 'KeepFulltextIndexFile'` Specifica che il file di indice full-text associato al database che si desidera scollegare non verrà eliminato durante l'operazione di scollegamento del database. *Keepfulltextindexfile* è di **tipo nvarchar (10)** e il valore predefinito è **true**. Se *keepfulltextindexfile* è **false**, vengono eliminati tutti i file di indice full-text associati al database e i metadati dell'indice full-text, a meno che il database non sia di sola lettura. Se è NULL o **true**, vengono conservati i metadati correlati a full-text.  
  
> [!IMPORTANT]
>  Il parametro **\@ keepfulltextindexfile** verrà rimosso in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Non utilizzare questo parametro in un nuovo progetto di sviluppo e modificare non appena possibile le applicazioni in cui viene attualmente utilizzato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Quando un database è scollegato, tutti i suoi metadati vengono eliminati. Se il database è il database predefinito di tutti gli account di accesso, **Master** diventa il database predefinito.  
  
> [!NOTE]  
>  Per informazioni su come visualizzare il database predefinito di tutti gli account di accesso, vedere [sp_helplogins &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md). Se si dispone delle autorizzazioni necessarie, è possibile utilizzare [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) per assegnare un nuovo database predefinito a un account di accesso.  
  
## <a name="restrictions"></a>Restrizioni  
 Non è possibile scollegare un database se una delle seguenti condizioni è vera:  
  
-   Il database è attualmente in uso. Per ulteriori informazioni, vedere la sezione "Come ottenere l'accesso esclusivo" di seguito in questo argomento.  
  
-   Se è replicato, il database viene pubblicato.  
  
     Prima di poter scollegare il database, è necessario disabilitare la pubblicazione eseguendo [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
  
    > [!NOTE]  
    >  Se non è possibile usare **sp_replicationdboption**, rimuovere la replica eseguendo [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
-   Uno snapshot del database esiste nel database.  
  
     Prima di scollegare il database, è necessari eliminare tutti i relativi snapshot. Per altre informazioni, vedere [Eliminare uno snapshot del database &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)o a un'istanza diversa.  
  
    > [!NOTE]  
    >  Non è possibile scollegare o collegare uno snapshot del database.  
  
-   È in corso il mirroring del database.  
  
     Non è possibile scollegare il database finché non viene terminata la sessione di mirroring del database. Per altre informazioni, vedere [Rimozione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
-   Il database è sospetto.  
  
     Per scollegare un database sospetto è prima necessario attivare la modalità di emergenza. Per ulteriori informazioni sull'attivazione della modalità di emergenza per un database, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
-   Il database è un database di sistema.  
  
## <a name="obtaining-exclusive-access"></a>Come ottenere l'accesso esclusivo  
 Per scollegare un database, è necessario l'accesso esclusivo al database. Se il database che si desidera scollegare è in uso, per scollegarlo è necessario impostare la modalità SINGLE_USER, in modo da ottenere l'accesso esclusivo.

 Prima di impostare il database in modalità SINGLE_USER, verificare che l'opzione AUTO_UPDATE_STATISTICS_ASYNC sia impostata su OFF. Se l'opzione è impostata su ON, tramite il thread in background utilizzato per aggiornare le statistiche viene stabilita una connessione con il database che non sarà quindi accessibile in modalità utente singolo. Per altre informazioni, vedere [impostare un database in modalità utente singolo](../databases/set-a-database-to-single-user-mode.md).

 L'istruzione seguente, ad esempio, `ALTER DATABASE` consente di ottenere l'accesso esclusivo al [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] database dopo la disconnessione di tutti gli utenti correnti dal database.  
  
```  
USE master;  
ALTER DATABASE AdventureWorks2012  
SET SINGLE_USER;  
GO  
```  
  
> [!NOTE]  
>  Per forzare gli utenti correnti dal database immediatamente o entro un numero specificato di secondi, utilizzare anche l'opzione ROLLBACK: ALTER DATABASE *database_name* SET SINGLE_USER WITH ROLLBACK *rollback_option*. Per altre informazioni, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
## <a name="reattaching-a-database"></a>Ricollegamento di un database  
 I file scollegati non vengono eliminati e possono essere ricollegati tramite CREATE DATABASE (con l'opzione FOR ATTACH o FOR ATTACH_REBUILD_LOG). È possibile spostare e quindi collegare tali file in un altro server.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o al ruolo **db_owner** del database.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente il database viene scollegato [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] con *skipchecks* impostato su true.  
  
```  
EXEC sp_detach_db 'AdventureWorks2012', 'true';  
```  
  
 Nell'esempio seguente viene scollegato il database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] e vengono mantenuti i file di indice full-text e i metadati dell'indice full-text. Per impostazione predefinita, questo comando esegue UPDATE STATISTICS.  
  
```  
exec sp_detach_db @dbname='AdventureWorks2012'  
    , @keepfulltextindexfile='true';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Collegamento e scollegamento di un database &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [Scollegamento di un database](../../relational-databases/databases/detach-a-database.md)  
  
