---
description: Rinominare viste
title: Rinominare viste | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- views [SQL Server], renaming
- renaming views
ms.assetid: 5eed0488-81d2-40e8-8fdf-b0a640a591d0
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c9dee892596871d51572ce4891a340b46d9e2d8f
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99249560"
---
# <a name="rename-views"></a>Rinominare viste
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]
  È possibile rinominare una vista in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!WARNING]  
>  Se si rinomina una vista, è possibile che smettano di funzionare il codice e le applicazioni che dipendono da essa, incluse altre viste, query, stored procedure, funzioni definite dall'utente e applicazioni client. Tali errori inoltre tendono a propagarsi a cascata.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Prerequisiti](#Prerequisites)  
  
     [Sicurezza](#Security)  
  
-   **Per rinominare una vista usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Follow Up:**  [After renaming a view](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
 Ottenere un elenco di tutte le dipendenze dalla vista. È necessario modificare qualsiasi oggetto, script o applicazione che fa riferimento alla vista per riflettere il nuovo nome di quest'ultima. Per altre informazioni, vedere [Get Information About a View](../../relational-databases/views/get-information-about-a-view.md). Si consiglia di eliminare la vista e di ricrearla con un nuovo nome anziché rinominarla. Se si ricrea la vista, si aggiornano le informazioni sulle dipendenze per gli oggetti a cui viene fatto riferimento nella vista.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Sono richieste l'autorizzazione ALTER per SCHEMA o CONTROL per OBJECT e l'autorizzazione CREATE VIEW per il database.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-rename-a-view"></a>Per rinominare una vista  
  
1.  In **Esplora oggetti** espandere il database contenente la vista da rinominare, quindi espandere la cartella **Vista** .  
  
2.  Fare clic con il pulsante destro del mouse sulla vista da rinominare e scegliere **Rinomina**.  
  
3.  Immettere il nuovo nome della vista.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 **Per rinominare una vista**  
  
 Nonostante sia possibile usare **sp_rename** per modificare il nome della vista, si consiglia di eliminare quella esistente e di ricrearla con il nuovo nome.  
  
 Per altre informazioni, vedere [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md) e [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md).  
  
##  <a name="follow-up-after-renaming-a-view"></a><a name="FollowUp"></a> Completamento: dopo che una vista è stata rinominata  
 Assicurarsi che tutti gli oggetti, gli script e le applicazioni che fanno riferimento al nome precedente della vista usino ora il nuovo nome.  
  
  
