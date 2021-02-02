---
title: Creare un ruolo applicazione | Microsoft Docs
description: Creare un ruolo applicazione in SQL Server usando SQL Server Management Studio o Transact-SQL per limitare l'accesso a un database, a meno che non avvenga tramite un'applicazione.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.approle.general.f1
helpviewer_keywords:
- application roles [SQL Server], creating
ms.assetid: 6b8da1f5-3d8e-4f88-b111-b915788b06f1
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b9d77743864b3b5425379ec7b5c570629b470e04
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250750"
---
# <a name="create-an-application-role"></a>Creazione di un ruolo applicazione
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene descritto come creare un ruolo applicazione in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. I ruoli applicazione limitano l'accesso dell'utente a un database solo tramite applicazioni specifiche. Poiché i ruoli applicazione non dispongono di utenti, l'elenco **Membri ruolo** non viene visualizzato quando si seleziona **Ruolo applicazione** .  
  
> [!IMPORTANT]  
>  In fase di impostazione delle password per i ruoli applicazione viene eseguito il controllo dei requisiti di complessità delle password. Le applicazioni che richiamano i ruoli applicazione devono archiviare le relative password. Le password dei ruoli applicazione devono essere sempre archiviate in forma crittografata.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per creare un ruolo applicazione utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY APPLICATION ROLE nel database.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
##### <a name="to-create-an-application-role"></a>Per creare un ruolo applicazione  
  
1.  In Esplora oggetti espandere il database in cui si desidera creare un ruolo applicazione.  
  
2.  Espandere la cartella **Sicurezza** .  
  
3.  Espandere la cartella **Ruoli** .  
  
4.  Fare clic con il pulsante destro del mouse sulla cartella **Ruoli applicazione**, quindi scegliere **Nuovo ruolo applicazione...** .  
  
5.  Nella pagina **Generale** della finestra di dialogo **Ruolo applicazione - Nuovo** immettere il nome del nuovo ruolo applicazione nella casella **Nome ruolo**.  
  
6.  Nella casella **Schema predefinito** specificare lo schema che diventerà proprietario degli oggetti creati da questo ruolo immettendo i nomi degli oggetti. In alternativa, fare clic sui puntini di sospensione **(...)** per aprire la finestra di dialogo **Individua schema**.  
  
7.  Nella casella **Password** , immettere una password per il nuovo ruolo. Digitare di nuovo la password nella casella **Conferma password** .  
  
8.  In **Schemi di proprietà del ruolo**, selezionare o visualizzare gli schemi che saranno di proprietà di questo ruolo. Uno schema può essere di proprietà di un solo schema o ruolo.  
  
9. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

### <a name="additional-options"></a>Opzioni aggiuntive  
 La finestra di dialogo **Ruolo applicazione - Nuovo** offre anche opzioni in altre due pagine: **Entità a protezione diretta** e **Proprietà estese**.  
  
-   Nella pagina **Entità a protezione diretta** sono elencate tutte le possibili entità a protezione diretta e le autorizzazioni su quelle entità a protezione diretta che possono essere concesse all'account di accesso.  
  
-   La pagina **Proprietà estese** consente di aggiungere proprietà personalizzate a utenti di database.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-create-an-application-role"></a>Per creare un ruolo applicazione  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- Creates an application role called "weekly_receipts" that has the password "987Gbv876sPYY5m23" and "Sales" as its default schema.  
  
    CREATE APPLICATION ROLE weekly_receipts   
        WITH PASSWORD = '987G^bv876sPY)Y5m23'   
        , DEFAULT_SCHEMA = Sales;  
    GO  
    ```  
  
 Per altre informazioni, vedere [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-application-role-transact-sql.md).  
  
  
