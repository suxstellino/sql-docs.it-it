---
title: Preparazione dei database di Access per la migrazione (AccessToSQL) | Microsoft Docs
description: Informazioni su come determinare i database di Access da migrare in SQL Server o nel database SQL di Azure e assicurarsi che tali database siano pronti per la migrazione.
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, versions
- Access databases, when to migrate
- Access databases, workgroup security
- backing up databases
- documenting databases
- files, preparing
- migrating databases, versions
- migrating databases, when to migrate
- versions of Access
- workgroup security
ms.assetid: 9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 7c52b13a2a71afc005f19d1733e8dff435d2a582
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066586"
---
# <a name="preparing-access-databases-for-migration-accesstosql"></a>Preparazione dei database di Access per la migrazione (AccessToSQL)
Prima di eseguire la migrazione dei database di Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario determinare i database di cui eseguire la migrazione e assicurarsi che tali database siano pronti per la migrazione.  
  
## <a name="determining-when-to-migrate-to-sql-server"></a>Determinazione del momento della migrazione a SQL Server  
Il motore di database Jet, usato come motore di database per l'accesso, è una soluzione flessibile e facile da usare per la gestione dei dati. Tuttavia, poiché i database diventano più grandi e più cruciali, molti utenti ritengono che siano necessari maggiori prestazioni, sicurezza o disponibilità. Per le applicazioni che richiedono una piattaforma dati più affidabile, provare a trasferire i database sottostanti per tali applicazioni a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni su come decidere quando eseguire la migrazione, vedere la pagina relativa alle [informazioni sulla migrazione](https://go.microsoft.com/fwlink/?LinkId=68571) nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sito Web.  
  
Dopo aver eseguito la migrazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dei database a, è possibile continuare a utilizzare l'accesso utilizzando le tabelle collegate oppure è possibile eseguire manualmente la migrazione delle applicazioni al [!INCLUDE[msCoName](../../includes/msconame_md.md)] codice basato su .NET Framework che interagisce direttamente con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="determining-which-databases-to-migrate"></a>Determinazione dei database di cui eseguire la migrazione  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) per l'accesso può individuare i database di Access. È quindi possibile esportare i metadati relativi a tali database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni su come esportare ed eseguire query sui metadati, vedere [esportazione di un inventario di accesso](exporting-an-access-inventory-accesstosql.md).  

   > [!NOTE]
   > Non tutte le funzionalità di accesso e le impostazioni sono supportate da oppure possono essere facilmente convertite in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Prima di iniziare la migrazione dei database, vedere [funzionalità di accesso non compatibili](incompatible-access-features-accesstosql.md).
  
## <a name="preparing-for-migration"></a>Preparazione alla migrazione  
Usare le linee guida seguenti per preparare i database di Access per la migrazione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="upgrading-older-access-databases"></a>Aggiornamento dei database di Access precedenti  
SSMA per Access supporta l'accesso 97 e versioni successive. Se si dispone di database di versioni precedenti di Access, aprire e salvare i database in Access 97 o versione successiva.  
  
### <a name="removing-workgroup-protection"></a>Rimozione della protezione del gruppo di lavoro  
SSMA non è in grado di migrare i database che utilizzano la protezione del gruppo Per rimuovere la protezione del gruppo di lavoro da un database di Access, seguire questa procedura:  
  
1.  Copiare il file di database di Access in un altro percorso.  
  
2.  Aprire il database copiato.  
  
3.  Scegliere **sicurezza** dal menu **strumenti** e quindi selezionare **autorizzazioni utente e gruppo**.  
  
4.  Selezionare l'opzione **utenti** , selezionare l'utente **amministratore** e quindi assicurarsi che sia selezionata l'autorizzazione **Amministrazione** .  
  
5.  Selezionare l'opzione **gruppi** , selezionare il gruppo **utenti** , quindi verificare che sia selezionata l'autorizzazione **Amministrazione** .  
  
6.  Fare clic su **OK**, quindi scegliere **Esci** dal menu **file** .  
  
È ora possibile usare SSMA per eseguire la migrazione del database copiato. Una volta caricato lo schema in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile proteggere manualmente il database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="backing-up-databases"></a>Backup dei database  
Prima di eseguire la migrazione dei database di Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario eseguire il backup di entrambi i database di Access di cui verrà eseguita la migrazione, nonché i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database in cui si eseguirà la migrazione di oggetti e dati di Access.  
  
Per eseguire il backup di un database di Access, scegliere **Utilità database** dal menu **strumenti** , quindi selezionare **backup database**.  
  
Per informazioni sul backup dei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database, vedere "backup e ripristino di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] " nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentazione online di.  
  
### <a name="documenting-databases"></a>Documentazione di database  
Potrebbe inoltre essere necessario documentare le proprietà, ad esempio elenchi di oggetti di database, dimensioni dei file e autorizzazioni, dei database di Access. Per generare questa documentazione in Access, scegliere **analizza** dal menu **strumenti** , quindi fare clic su **documentato**.  
  
## <a name="see-also"></a>Vedi anche  
[Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Collegamento di applicazioni di accesso a SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)
