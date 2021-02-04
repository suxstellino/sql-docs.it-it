---
description: Formattare gli indirizzi di cercapersone per gli avvisi
title: Formattare gli indirizzi di cercapersone per gli avvisi
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- pager addresses [SQL Server]
- SQL Server Agent, alerts
- formats [SQL Server], pager addresses
- pager notifications [SQL Server]
- addresses [SQL Server]
- alerts [SQL Server], pager addresses
ms.assetid: a9797d01-1050-442c-9038-ed4bfee1e76a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 32f019897d12ff2cc8620302a48734fc1fbde3a6
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251072"
---
# <a name="format-pager-addresses-for-alerts"></a>Formattare gli indirizzi di cercapersone per gli avvisi
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento illustra la procedura di formattazione degli indirizzi dei cercapersone per gli avvisi di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Per impostazione predefinita, i membri del ruolo predefinito del server **sysadmin** possono visualizzare le informazioni su un avviso. Gli altri utenti devono appartenere al ruolo predefinito del database **SQLAgentOperatorRole** nel database **msdb** .  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-format-pager-addresses"></a>Per formattare gli indirizzi di cercapersone  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server che contiene l'avviso da inviare a un cercapersone.  
  
2.  Fare clic con il pulsante destro del mouse su **SQL Server Agent** e scegliere **Proprietà**  
  
3.  In **Selezione pagina** selezionare **Sistema avvisi**.  
  
4.  Nelle caselle **Riga A:** e **Riga Cc:** del campo **Formato indirizzo per messaggi di posta elettronica tramite cercapersone** digitare il prefisso o il suffisso dell'indirizzo di cercapersone. L'indirizzo effettivo del cercapersone dell'operatore viene inserito al momento dell'invio di una notifica.  
  
5.  Immettere il prefisso o il suffisso per la riga dell'oggetto nella casella **Oggetto** .  
  
6.  Selezionare **Includi corpo del messaggio nel messaggio di notifica** per includere l'intero messaggio di posta elettronica nel messaggio inviato al cercapersone (invece del solo oggetto).  
  
7.  Al termine, fare clic su **OK**.  
