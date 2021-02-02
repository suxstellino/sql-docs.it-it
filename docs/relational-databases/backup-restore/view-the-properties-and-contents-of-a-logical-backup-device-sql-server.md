---
title: Visualizzare il contenuto di un dispositivo di backup logico
description: Informazioni su come visualizzare le proprietà e il contenuto di un dispositivo di backup logico in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- displaying backup content
- viewing backup content
- database backups [SQL Server], viewing content
- backing up databases [SQL Server], viewing content
- backing up databases [SQL Server], properties
- displaying backup properties
- backup devices [SQL Server], viewing information
- viewing backup properties
- database backups [SQL Server], properties
ms.assetid: 3a309074-e816-454d-b6c3-fcfdde0cbf74
author: cawrites
ms.author: chadam
ms.openlocfilehash: 059b327db0726f21a7dcc2e7d78d471b88097292
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251190"
---
# <a name="view-the-properties-and-contents-of-a-logical-backup-device-sql-server"></a>Visualizzazione delle proprietà e del contenuto di un dispositivo di backup logico (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento viene illustrato come visualizzare le proprietà e il contenuto di un dispositivo di backup logico in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per visualizzare le proprietà e il contenuto di un dispositivo di backup logico utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 Per informazioni sulla sicurezza, vedere [RESTORE LABELONLY & #40; Transact-SQL & #41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, per ottenere informazioni su un set o un dispositivo di backup è necessario disporre dell'autorizzazione CREATE DATABASE. Per altre informazioni, vedere [GRANT - autorizzazioni per database &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-view-the-properties-and-contents-of-a-logical-backup-device"></a>Per visualizzare le proprietà e il contenuto di un dispositivo di backup logico  
  
1.  Dopo aver stabilito la connessione all'istanza appropriata del [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], in Esplora oggetti fare clic sul nome del server per espanderne l'albero.  
  
2.  Espandere **Oggetti server** e quindi **Dispositivi di backup**.  
  
3.  Fare clic sul dispositivo e quindi fare clic con il pulsante destro del mouse su **Proprietà** per visualizzare la finestra di dialogo **Dispositivo di backup** .  
  
4.  Nella pagina **Generale** sono indicati il nome del dispositivo e la destinazione, costituita da un dispositivo nastro o da un percorso di file.  
  
5.  Nel riquadro **Selezione pagina** fare clic su **Contenuto supporti**.  
  
6.  Nel riquadro di destra verranno visualizzati i pannelli delle proprietà seguenti:  
  
    -   **Supporti**  
  
         Informazioni sulla sequenza dei supporti, ovvero numero di sequenza dei supporti, numero di sequenza del gruppo e identificatore mirror, se presente, e data e ora di creazione dei supporti.  
  
    -   **Set di supporti**  
  
         Informazioni sul set di supporti: nome e, se disponibile, descrizione del set di supporti e numero di gruppi nel set.  
  
7.  Nella griglia **Set di backup** sono visualizzate le informazioni sui contenuti del set di supporti.  
  
> [!NOTE]  
>  Per altre informazioni, vedere [Pagina Contenuto supporti](../../relational-databases/backup-restore/backup-device-media-contents-page.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-view-the-properties-and-contents-of-a-logical-backup-device"></a>Per visualizzare le proprietà e il contenuto di un dispositivo di backup logico  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Utilizzare l'istruzione [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) . In questo esempio vengono restituite informazioni sul dispositivo di backup logico `AdvWrks2008R2Backup` .  
  
```sql  
USE AdventureWorks2012 ;  
RESTORE LABELONLY  
   FROM AdvWrks2008R2Backup ;  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)  
  
  
