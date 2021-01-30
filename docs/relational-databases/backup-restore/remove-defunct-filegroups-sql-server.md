---
title: Rimuovere filegroup inattivi (SQL Server) | Microsoft Docs
description: Questo articolo illustra come rimuovere filegroup inattivi in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- piecemeal restores [SQL Server], defunct filegroups
- defunct filegroups
- restoring filegroups [SQL Server]
- deferred transactions
- filegroups [SQL Server], defunct
- unrestored filegroups
ms.assetid: 055f9c6a-5c18-4942-98e7-ec918f0ff975
author: cawrites
ms.author: chadam
ms.openlocfilehash: 88f08e8f660fc2887f38395e2737ee79b8bccefb
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077049"
---
# <a name="remove-defunct-filegroups-sql-server"></a>Rimozione di filegroup inattivi (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come rimuovere filegroup inattivi in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
-   [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per rimuovere filegroup inattivi utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Le informazioni in questo argomento sono rilevanti per database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che contengono più file o filegroup nonché, nel modello con registrazione minima, solo per i filegroup di sola lettura.  
  
-   Lo stato di tutti i file di un filegroup è defunct quando si rimuove un filegroup offline.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Se non sarà mai necessario ripristinare un filegroup non ripristinato, è possibile rendere il filegroup *inattivo* rimuovendolo dal database. Il filegroup inattivo non potrà mai essere ripristinato in questo database, ma i relativi metadati verranno mantenuti. Dopo che il filegroup è reso inattivo, è possibile riavviare il database. Il recupero renderà il database consistente rispetto ai filegroup ripristinati.  
  
     Ad esempio, rendere un filegroup inattivo è un'opzione per risolvere le transazioni posticipate causate da un filegroup offline che si desidera escludere dal database. Quando il filegroup in questione diventa offline, lo stato di transazione posticipata viene annullato. Per altre informazioni, vedere [Transazioni posticipate &#40;SQL Server&#41;](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per il database.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-remove-defunct-filegroups"></a>Per rimuovere filegroup inattivi  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] , quindi espandere questa istanza.  
  
2.  Espandere **Database**, fare clic con il pulsante destro del mouse sul database da cui eliminare il file e quindi scegliere **Proprietà**.  
  
3.  Selezionare la pagina **File** .  
  
4.  Nella griglia **File di database** selezionare i file da eliminare, fare clic su **Rimuovi** e quindi su **OK**.  
  
5.  Fare clic sulla pagina **Filegroup** .  
  
6.  Nella griglia **Righe** selezionare il filegroup da eliminare, fare clic su **Rimuovi** e quindi su **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-remove-defunct-filegroups"></a>Per rimuovere filegroup inattivi  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. **Nota:** in questo esempio si presuppone che i file e il filegroup siano già presenti. Per creare questi oggetti, vedere l'esempio B nell'argomento [Opzioni per file e filegroup ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md). Nel primo esempio vengono rimossi i file `test1dat3` e `test1dat4` dal filegroup inattivo tramite l'istruzione `ALTER DATABASE` con la clausola `REMOVE FILE`. Nel secondo esempio viene rimosso il filegroup `Test1FG1`inattivo tramite la clausola `REMOVE FILEGROUP` .  
  
```sql  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE test1dat3 ;  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE test1dat4 ;  
GO  
  
```  
  
```sql  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
REMOVE FILEGROUP Test1FG1 ;  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Opzioni per file e filegroup ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)   
 [Transazioni posticipate &#40;SQL Server&#41;](../../relational-databases/backup-restore/deferred-transactions-sql-server.md)   
 [Ripristini di file &#40;modello di recupero con registrazione completa&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [Ripristini di file &#40;modello di recupero con registrazione minima&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Ripristino online &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [Ripristino di pagine &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Ripristini a fasi &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  
