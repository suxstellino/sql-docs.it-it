---
title: Autorizzazioni necessarie
description: Informazioni sulle autorizzazioni necessarie per varie attività in SQL Server Data Tools, ad esempio la creazione di un database, l'esecuzione di unit test e il confronto di schemi.
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: b27038c4-94ab-449c-90b7-29d87ce37a8b
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.openlocfilehash: 6e519a3786b45d87344f8f5f5355353fd5865d62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072455"
---
# <a name="required-permissions-for-sql-server-data-tools"></a>Autorizzazioni necessarie per SQL Server Data Tools

Prima di poter eseguire un'azione in un database in Visual Studio, è necessario accedere con un account provvisto di determinate autorizzazioni. Le autorizzazioni specifiche necessarie variano in base all'azione che si desidera eseguire. Nelle sezioni seguenti vengono descritte le azioni che è possibile eseguire e le autorizzazioni specifiche necessarie per eseguirle.  
  
-   [Autorizzazioni per creare o distribuire un database](#DatabaseCreationAndDeploymentPermissions)  
  
-   [Autorizzazioni per effettuare il refactoring di un database](#DatabaseRefactoringPermissions)  
  
-   [Autorizzazioni per eseguire unit test in un database di SQL Server](#DatabaseUnitTestingPermissions)  
  
-   [Autorizzazioni per generare dati](#DataGenerationPermissions)  
  
-   [Autorizzazioni per confrontare schemi e dati](#SchemaAndDataComparePermissions)  
  
-   [Autorizzazioni per eseguire l'editor Transact-SQL](#Transact-SQLEditorPermissions)  
  
-   [Autorizzazioni per progetti Common Language Runtime di SQL Server (CLR SQL)](#SQLCLRPermissions)  
  
## <a name="permissions-to-create-or-deploy-a-database"></a><a name="DatabaseCreationAndDeploymentPermissions"></a>Autorizzazioni per creare o distribuire un database  
Per creare o distribuire un database è necessario disporre delle autorizzazioni riportate di seguito.  
  
|Azioni|Autorizzazioni necessarie|  
|-|-|  
|Importare oggetti di database e relative impostazioni|È necessario avere la possibilità di connettersi al database di origine.<br /><br />Se il database di origine è basato su SQL Server 2005, è necessario avere anche l'autorizzazione **VIEW DEFINITION** per ogni oggetto.<br /><br />Anche se il database di origine è basato su SQL Server 2008 o versioni successive, è necessario avere l'autorizzazione **VIEW DEFINITION** per ogni oggetto. Per l'accesso è necessaria l'autorizzazione **VIEW SERVER STATE** per le chiavi di crittografia del database.|  
|Importare oggetti server e relative impostazioni|È necessario avere la possibilità di connettersi al database master nel server specificato.<br /><br />Se nel server è in esecuzione SQL Server 2005, è necessario avere l'autorizzazione **VIEW ANY DEFINITION** per il server.<br /><br />Anche se il database di origine è basato su SQL Server 2008 o versioni successive, è necessario avere l'autorizzazione **VIEW ANY DEFINITION** per il server. Per l'accesso è necessaria l'autorizzazione **VIEW SERVER STATE** per le chiavi di crittografia del database.|  
|Creare o aggiornare un progetto di database|Non è necessario disporre di alcuna autorizzazione per database per creare o modificare un progetto di database.|  
|Distribuire un nuovo database o distribuire con l'opzione **Ricrea sempre database** impostata|È necessario avere l'autorizzazione **CREATE DATABASE** oppure essere membro del ruolo **dbcreator** nel server di destinazione.<br /><br />Quando si crea un database, Visual Studio si connette al database modello e ne copia il contenuto. Per l'accesso iniziale, ad esempio *yourLogin*, usato per connettersi al database di destinazione, sono necessarie le autorizzazioni **db_creator** e **CONNECT SQL**. Questo accesso deve disporre di un mapping utente nel database modello. Se si hanno autorizzazioni **sysadmin**, è possibile creare il mapping eseguendo le istruzioni Transact\-SQL seguenti:<br /><br />`USE [model] CREATE USER yourUser FROM LOGIN yourLogin`<br /><br />L'utente, in questo esempio yourUser, deve avere le autorizzazioni **CONNECT** e **VIEW DEFINITION** per il database modello. Se si hanno autorizzazioni **sysadmin**, è possibile concederle eseguendo le istruzioni Transact\-SQL seguenti:<br /><br />`USE [model] GRANT CONNECT to yourUser GRANT VIEW DEFINITION TO yourUser`<br /><br />Se si distribuisce un database contenente vincoli senza nome e l'opzione **CheckNewContraints** è abilitata, come da impostazione predefinita, è necessario avere l'autorizzazione **db_owner** o **sysadmin** o la distribuzione non verrà eseguita. Questa regola è applicabile solo ai vincoli senza nome. Per altre informazioni sull'opzione **CheckNewConstraints**, vedere [Impostazioni del progetto di database](../ssdt/database-project-settings.md).|  
|Distribuire aggiornamenti in un database esistente|È necessario essere un utente di database valido. È anche necessario essere membro del ruolo **db_ddladmin**, proprietario dello schema oppure degli oggetti che si vogliono creare o modificare nel database di destinazione. Per utilizzare concetti più avanzati come account di accesso o server collegati negli script pre-distribuzione o post-distribuzione, sono necessarie autorizzazioni aggiuntive.<br /><br />**NOTA:** se si esegue la distribuzione al database master, è necessario avere anche l'autorizzazione **VIEW ANY DEFINITION** per il server in cui si distribuisce.|  
|Utilizzare un assembly con l'opzione EXTERNAL_ACCESS in un progetto di database|È necessario impostare la proprietà TRUSTWORTHY per il progetto di database. È necessario avere l'autorizzazione EXTERNAL ACCESS ASSEMBLY per l'account di accesso di SQL Server.|  
|Distribuire assembly in un database nuovo o esistente|È necessario essere membro del ruolo sysadmin nel server di distribuzione di destinazione.|  
  
Per ulteriori informazioni, vedere la documentazione online di SQL Server.  
  
## <a name="permissions-to-refactor-a-database"></a><a name="DatabaseRefactoringPermissions"></a>Autorizzazioni per eseguire il refactoring di un database  
Il *refactoring del database* viene eseguito solo all'interno del progetto di database. È necessario disporre delle autorizzazioni per utilizzare il progetto di database. Le autorizzazioni per un database di destinazione non sono necessarie finché non si distribuiscono le modifiche in tale database.  
  
## <a name="permissions-to-perform-unit-testing-on-a-sql-server-database"></a><a name="DatabaseUnitTestingPermissions"></a>Autorizzazioni per eseguire unit test in un database di SQL Server  
Per eseguire unit test in un database, è necessario disporre delle autorizzazioni seguenti.  
  
|Azioni|Autorizzazioni necessarie|  
|-|-|   
|Eseguire un'azione di test|È necessario utilizzare la connessione al database del contesto di esecuzione. Per altre informazioni, vedere [Panoramica delle stringhe di connessione e delle autorizzazioni](../ssdt/overview-of-connection-strings-and-permissions.md).|  
|Eseguire un'azione di pre-test o post-test|È necessario utilizzare la connessione al database del contesto autorizzato. Questa connessione ha più autorizzazioni rispetto alla connessione del contesto di esecuzione.|  
|Eseguire script TestInitialize e TestCleanup|È necessario utilizzare la connessione al database del contesto autorizzato.|  
|Distribuire le modifiche apportate al database prima di eseguire i test|È necessario utilizzare la connessione al database del contesto autorizzato. Per altre informazioni, vedere [Procedura: Configurare l'esecuzione di unit test di SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).|  
|Generare dati prima di eseguire i test|È necessario utilizzare la connessione al database del contesto autorizzato. Per altre informazioni, vedere [Procedura: Configurare l'esecuzione di unit test di SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).|  
  
## <a name="permissions-to-generate-data"></a><a name="DataGenerationPermissions"></a>Autorizzazioni per generare dati  
Per generare dati di test tramite il generatore di dati, è necessario avere le autorizzazioni **INSERT** e **SELECT** per gli oggetti del database di destinazione. Se si eliminano dei dati prima di generarne altri, è necessario avere anche le autorizzazioni **DELETE** per gli oggetti del database di destinazione. Per reimpostare la colonna **IDENTITY** di una tabella, è necessario essere proprietari della tabella oppure membri del ruolo db_owner o db_ddladmin.  
  
## <a name="permissions-to-compare-schemas-and-data"></a><a name="SchemaAndDataComparePermissions"></a>Autorizzazioni per confrontare schemi e dati  
Per confrontare schemi o dati, è necessario disporre delle autorizzazioni seguenti.  
  
|Azioni|Autorizzazioni necessarie|  
|-|-|   
|Confrontare gli schemi di due database|È necessario avere le autorizzazioni per importare oggetti e impostazioni dai database come descritto nella sezione [Autorizzazioni per creare o distribuire un database](#DatabaseCreationAndDeploymentPermissions).|  
|Confrontare gli schemi di un database e di un progetto di database|È necessario avere le autorizzazioni per importare oggetti e impostazioni dal database come descritto nella sezione [Autorizzazioni per creare o distribuire un database](#DatabaseCreationAndDeploymentPermissions). Il progetto di database deve inoltre essere aperto in Visual Studio.|  
|Scrivere aggiornamenti in un database di destinazione|È necessario avere le autorizzazioni per distribuire gli aggiornamenti al database di destinazione come descritto nella sezione [Autorizzazioni per creare o distribuire un database](#DatabaseCreationAndDeploymentPermissions).|  
|Confrontare i dati di due database|Oltre alle autorizzazioni necessarie per confrontare gli schemi di due database, è necessaria anche l'autorizzazione **SELECT** per tutte le tabelle che si vuole confrontare e l'autorizzazione **VIEW DATABASE STATE**.|  
  
Per ulteriori informazioni, vedere la documentazione online di SQL Server.  
  
## <a name="permissions-to-run-the-transact-sql-editor"></a><a name="Transact-SQLEditorPermissions"></a>Autorizzazioni per eseguire l'editor Transact\-SQL  
Le azioni possibili all'interno dell'editor Transact\-SQL sono determinate dal contesto di esecuzione nel database di destinazione.  
  
## <a name="permissions-for-sql-server-common-language-run-time-projects"></a><a name="SQLCLRPermissions"></a>Autorizzazioni per progetti Common Language Runtime di SQL Server  
Nella tabella seguente sono elencate le autorizzazioni necessarie per effettuare la distribuzione o eseguire il debug di progetti CLR:  
  
|Azioni|Autorizzazioni necessarie|  
|-----------|------------------------|  
|Eseguire la distribuzione (iniziale o incrementale) di un assembly del set di autorizzazioni sicuro|db_DDLAdmin: questa autorizzazione concede autorizzazioni CREATE e ALTER per assembly e tipi di oggetti distribuiti<br /><br />VIEW DEFINITION a livello di database: obbligatorio per la distribuzione<br /><br />CONNECT a livello di database: consente di stabilire la connessione al database|  
|Distribuire un assembly del set di autorizzazioni external_access|db_DDLAdmin: questa autorizzazione concede autorizzazioni CREATE e ALTER per assembly e tipi di oggetti distribuiti<br /><br />VIEW DEFINITION a livello di database: obbligatorio per la distribuzione<br /><br />CONNECT a livello di database: consente di stabilire la connessione al database<br /><br />È inoltre necessario disporre di quanto segue:<br /><br />Opzione di database TRUSTWORTHY impostata su ON<br /><br />L'accesso utilizzato per la distribuzione deve disporre dell'autorizzazione External Access Assembly per il server|  
|Distribuire un assembly del set di autorizzazioni non sicuro|db_DDLAdmin: questa autorizzazione concede autorizzazioni CREATE e ALTER per assembly e tipi di oggetti distribuiti<br /><br />VIEW DEFINITION a livello di database: obbligatorio per la distribuzione<br /><br />CONNECT a livello di database: consente di stabilire la connessione al database<br /><br />È inoltre necessario disporre di quanto segue:<br /><br />Opzione di database TRUSTWORTHY impostata su ON<br /><br />L'accesso utilizzato per la distribuzione deve disporre dell'autorizzazione Unsafe Assembly per il server|  
|Eseguire il debug remoto di un assembly CLR SQL|È necessario disporre dell'autorizzazione del ruolo predefinito sysadmin.|  
  
> [!IMPORTANT]  
> In tutti i casi, il proprietario dell'assembly deve essere l'utente utilizzato per distribuire l'assembly oppure deve essere un ruolo di cui tale utente è un membro. Questo requisito si applica anche a qualsiasi assembly a cui fa riferimento l'assembly da distribuire.  
  
## <a name="see-also"></a>Vedere anche  
[Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
  
