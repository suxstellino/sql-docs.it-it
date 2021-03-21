---
title: Concedere un'autorizzazione a un'entità| Microsoft Docs
description: Informazioni su come concedere l'autorizzazione a un'entità di sicurezza in SQL Server usando SQL Server Management Studio o Transact-SQL, incluse le procedure consigliate.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Grant permission to a principal
ms.assetid: 4107389d-05b6-4aa3-9fa8-95b40cdf05dc
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 78c6fc0078ff91190a0211a902239a5aa0cf1127
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750391"
---
# <a name="grant-a-permission-to-a-principal"></a>Concedere un'autorizzazione a un'entità
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  In questo argomento viene descritto come concedere un'autorizzazione a un'entità in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per concedere un'autorizzazione a un'entità utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 Le procedure consigliate riportate di seguito consentono di gestire più facilmente le autorizzazioni.  
  
-   Concedere autorizzazione a ruoli, anziché a singoli account di accesso o utenti. Quando un singolo utente viene sostituito da un altro, rimuovere l'utente che non è più incluso nel ruolo e aggiungere il nuovo utente al ruolo. Le numerose autorizzazioni eventualmente associate al ruolo saranno automaticamente disponibili per il nuovo utente. Se diverse persone in un'organizzazione richiedono le stesse autorizzazioni, l'aggiunta di ciascuna persona al ruolo comporterà la concessione automatica delle stesse autorizzazioni.  
  
-   Configurare entità a protezione diretta simili, quali tabelle, viste e procedure, come di proprietà di uno schema, quindi concedere le autorizzazioni allo schema. Ad esempio, lo schema del libro paga potrebbe possedere diverse tabelle, viste e stored procedure. Concedendo l'accesso allo schema, verranno concesse simultaneamente tutte le necessarie autorizzazioni per eseguire la funzione di retribuzione. Per ulteriori informazioni sulle entità a protezione diretta a cui è possibile concedere le autorizzazioni, vedere [Securables](../../../relational-databases/security/securables.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 L'utente che concede le autorizzazioni (o l'entità specificata con l'opzione AS) deve disporre della relativa autorizzazione con GRANT OPTION oppure di un'autorizzazione di livello superiore che include l'autorizzazione che viene concessa. I membri del ruolo predefinito del server **sysadmin** possono concedere qualsiasi autorizzazione.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-grant-permission-to-a-principal"></a>Per concedere un'autorizzazione a un'entità  
  
1.  In Esplora oggetti espandere il database contenente l'oggetto a cui si desiderano concedere autorizzazioni.  
  
    > [!NOTE]  
    >  In questi passaggi viene trattata specificatamente la concessione delle autorizzazioni a una stored procedure, ma è possibile utilizzare passaggi simili per aggiungere autorizzazioni a tabelle, viste, funzioni e assembly e altre entità a protezione diretta. Per altre informazioni, vedere [GRANT &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-transact-sql.md).  
  
2.  Espandere la cartella **Programmabilità** .  
  
3.  Espandere la cartella **Stored procedure** .  
  
4.  Fare clic con il pulsante destro del mouse su una stored procedure e selezionare **Proprietà**.  
  
5.  Nella finestra di dialogo **Proprietà stored procedure -** _nome\_stored\_procedure_ selezionare **Autorizzazioni** in Selezione pagina. Utilizzare questa pagina per aggiungere utenti o ruoli alla stored procedure e specificare le autorizzazioni degli utenti o dei ruoli.  
  
6.  Al termine, fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-grant-permission-to-a-principal"></a>Per concedere un'autorizzazione a un'entità  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- Grants EXECUTE permission on stored procedure HumanResources.uspUpdateEmployeeHireInfo to an application role called Recruiting11.   
    USE AdventureWorks2012;  
    GO  
    GRANT EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
        TO Recruiting11;  
    GO  
    ```  
  
 Per altre informazioni, vedere [GRANT &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-transact-sql.md) e [GRANT - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-object-permissions-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Entità &#40;motore di database&#41;](../../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
