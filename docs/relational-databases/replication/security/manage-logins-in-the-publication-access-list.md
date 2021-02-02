---
description: Gestione degli account nell'elenco di accesso alla pubblicazione
title: Gestire gli account nell'elenco di accesso alla pubblicazione | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- logins [SQL Server replication], publication access list
- publications [SQL Server replication], publication access lists
- publication access list (PAL)
- PAL (publication access list)
- logins [SQL Server replication], managing
ms.assetid: fceb216b-0b18-4e3b-8ae0-13e35920dcbc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 526d46149bbafccf8592a20b8a592012e3d6b6aa
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250650"
---
# <a name="manage-logins-in-the-publication-access-list"></a>Gestione degli account nell'elenco di accesso alla pubblicazione
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento si illustra come gestire gli account di accesso nell'elenco di accesso alla pubblicazione in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. L'accesso a una pubblicazione viene controllato tramite l'elenco di accesso alla pubblicazione. Accessi e gruppi possono essere aggiunti e rimossi dell'elenco di accesso alla pubblicazione.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Prerequisiti](#Prerequisites)  
  
-   **Per gestire gli account di accesso nell'elenco di accesso alla pubblicazione, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   Prima di aggiungere l'account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] all'elenco di accesso alla pubblicazione, è necessario associarlo a un utente di database nel database di pubblicazione.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 L'elenco di accesso alla pubblicazione nella pagina **Elenco di accesso alla pubblicazione** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** consente di gestire gli account di accesso. Per altre informazioni sull'accesso a questa finestra di dialogo, vedere [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-manage-logins-in-the-pal"></a>Per gestire gli account nell'elenco di accesso alla pubblicazione  
  
1.  Nella pagina **Elenco di accesso alla pubblicazione** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** usare i pulsanti **Aggiungi**, **Rimuovi**  e **Rimuovi tutto** per aggiungere e rimuovere gruppi e account di accesso dall'elenco di accesso alla pubblicazione. Non rimuovere **distributor_admin** dall'elenco di accesso alla pubblicazione. Questo account è usato dalla replica.  
  
    > [!NOTE]  
    >  Se si usano un server di distribuzione remoto, gli account nell'elenco di accesso alla pubblicazione devono essere disponibili sia nel server di pubblicazione che nel server di distribuzione. L'account deve essere un account di dominio o un account locale definito in entrambi i server. Le password associate a entrambi gli account di accesso devono essere identiche.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-view-groups-and-logins-that-belong-to-the-pal"></a>Per visualizzare gruppi e account di accesso che appartengono all'elenco di accesso alla pubblicazione  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_help_publication_access](../../../relational-databases/system-stored-procedures/sp-help-publication-access-transact-sql.md). Per `@publication`, specificare il nome della pubblicazione. Verranno visualizzate informazioni sui gruppi e gli account di accesso presenti nell'elenco di accesso alla pubblicazione.  
  
#### <a name="to-add-groups-and-logins-to-the-pal"></a>Per aggiungere gruppi e account di accesso all'elenco di accesso alla pubblicazione  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_grant_publication_access](../../../relational-databases/system-stored-procedures/sp-grant-publication-access-transact-sql.md). Per `@publication`, specificare il nome della pubblicazione e per `@login` il nome dell'account di accesso o del gruppo da aggiungere.  
  
#### <a name="to-remove-groups-and-logins-from-the-pal"></a>Per rimuovere gruppi e account di accesso dall'elenco di accesso alla pubblicazione  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_revoke_publication_access](../../../relational-databases/system-stored-procedures/sp-revoke-publication-access-transact-sql.md). Per `@publication`, specificare il nome della pubblicazione e per `@login` il nome dell'account di accesso o del gruppo da rimuovere.  
  
## <a name="see-also"></a>Vedere anche  
 [Gestione degli account nell'elenco di accesso alla pubblicazione](../../../relational-databases/replication/security/manage-logins-in-the-publication-access-list.md)   
 [Modello di sicurezza dell'agente di replica](../../../relational-databases/replication/security/replication-agent-security-model.md)   
 [Proteggere una topologia di replica](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [Proteggere il server di pubblicazione](../../../relational-databases/replication/security/secure-the-publisher.md)  
  
  
