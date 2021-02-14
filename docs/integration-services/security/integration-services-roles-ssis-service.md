---
description: Ruoli Integration Services (servizio SSIS)
title: Ruoli di Integration Services (servizio SSIS) | Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtsserver.packageroles.f1
helpviewer_keywords:
- security [Integration Services], roles
- db_ssisoperator role
- db_ssisadmin role
- fixed database roles [Integration Services]
- packages [Integration Services], security
- roles [Integration Services]
- db_ssisltduser role
ms.assetid: 9702e90c-fada-4978-a473-1b1423017d80
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d8801c1a4f7387f1ff5bc815b45897d5a4f68fdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340781"
---
# <a name="integration-services-roles-ssis-service"></a>Ruoli Integration Services (servizio SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] offre alcuni ruoli predefiniti a livello di database per consentire l'accesso sicuro ai pacchetti archiviati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I ruoli disponibili sono diversi a seconda di dove si salvano i pacchetti, nel database del catalogo SSIS (SSISDB) o nel database msdb.  
  
## <a name="roles-in-the-ssis-catalog-database-ssisdb"></a>Ruoli nel database del catalogo SSIS (SSISDB)  
 Il database del catalogo SSIS (SSISDB) fornisce i seguenti ruoli predefiniti a livello di database per proteggere l'accesso ai pacchetti e alle relative informazioni.  
  
-   **ssis_admin**. Questo ruolo fornisce l'accesso amministrativo completo al database del catalogo SSIS.  
  
-   **ssis_logreader** Questo ruolo fornisce le autorizzazioni per accedere a tutte le viste correlate ai log operativi di SSISDB.  
  
     L'elenco delle viste include: [catalog].[projects], [catalog].[packages], [catalog].[operations], [catalog].[extended_operation_info], [catalog].[operation_messages], [catalog].[event_messages], [catalog].[execution_data_statistics], [catalog].[execution_component_phases], [catalog].[execution_data_taps], [catalog].[event_message_context], [catalog].[executions], [catalog].[executables], [catalog].[executable_statistics], [catalog].[validations], [catalog].[execution_parameter_values] e [catalog].[execution_property_override_values].  
  
## <a name="roles-in-the-msdb-database"></a>Ruoli all'interno del database msdb  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] include i tre ruoli predefiniti a livello di database, **db_ssisadmin**, **db_ssisltduser** e **db_ssisoperator**, per il controllo dell'accesso ai pacchetti salvati nel database **msdb**. I ruoli vengono assegnati a un pacchetto in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Le assegnazioni dei ruoli vengono salvate nel database **msdb** .  
  
### <a name="read-and-write-actions"></a>Azioni di lettura e scrittura  
 La tabella seguente descrive le azioni di lettura e scrittura di Windows e i ruoli predefiniti del database in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Ruolo|Azione di lettura|Azione di scrittura|  
|----------|-----------------|------------------|  
|**db_ssisadmin**<br /><br /> oppure<br /><br /> **sysadmin**|Enumerazione dei propri pacchetti.<br /><br /> Enumerazione di tutti i pacchetti.<br /><br /> Visualizzazione dei propri pacchetti.<br /><br /> Visualizzazione di tutti i pacchetti.<br /><br /> Esecuzione dei propri pacchetti.<br /><br /> Esecuzione di tutti i pacchetti.<br /><br /> Esportazione dei propri pacchetti.<br /><br /> Esportazione di tutti i pacchetti.<br /><br /> Esecuzione di tutti i pacchetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|Importazione di pacchetti.<br /><br /> Eliminazione dei propri pacchetti.<br /><br /> Eliminazione di tutti i pacchetti.<br /><br /> Modifica dei propri ruoli di pacchetto.<br /><br /> Modifica di tutti i ruoli di pacchetto.<br /><br /> <br /><br /> **\*\* Avviso \*\*** I membri dei ruoli db_ssisadmin e dc_admin possono essere in grado di elevare i privilegi a sysadmin. Questa elevazione dei privilegi può verificarsi perché tali ruoli possono modificare i pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e i pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] possono essere eseguiti da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando il contesto di sicurezza sysadmin di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Per impedire questa elevazione dei privilegi durante l'esecuzione dei piani di manutenzione, set di raccolta dati e altri pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configurare i processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che eseguono pacchetti in modo da utilizzare un account proxy con privilegi limitati o aggiungere solo i membri sysadmin ai ruoli db_ssisadmin e dc_admin.|  
|**db_ssisltduser**|Enumerazione dei propri pacchetti.<br /><br /> Enumerazione di tutti i pacchetti.<br /><br /> Visualizzazione dei propri pacchetti.<br /><br /> Esecuzione dei propri pacchetti.<br /><br /> Esportazione dei propri pacchetti.|Importazione di pacchetti.<br /><br /> Eliminazione dei propri pacchetti.<br /><br /> Modifica dei propri ruoli di pacchetto.|  
|**db_ssisoperator**|Enumerazione di tutti i pacchetti.<br /><br /> Visualizzazione di tutti i pacchetti.<br /><br /> Esecuzione di tutti i pacchetti.<br /><br /> Esportazione di tutti i pacchetti.<br /><br /> Esecuzione di tutti i pacchetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|nessuno|  
|**Amministratori di Windows**|Visualizzazione delle informazioni di esecuzione di tutti i pacchetti in esecuzione.|Arresto di tutti i pacchetti in esecuzione.|  
  
### <a name="sysssispackages-table"></a>Tabella Sysssispackages  
 La tabella **sysssispackages** del database **msdb** include i pacchetti che sono stati salvati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [sysssispackages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysssispackages-transact-sql.md).  
  
 Le colonne della tabella **sysssispackages** contengono informazioni sui ruoli assegnati ai pacchetti.  
  
-   Nella colonna **readerrole** è indicato il ruolo con accesso in lettura al pacchetto.  
  
-   Nella colonna **writerrole** è indicato il ruolo con accesso in scrittura al pacchetto.  
  
-   La colonna **ownersid** include l'ID di sicurezza (SID) univoco dell'utente che ha creato il pacchetto. Questa colonna definisce pertanto il proprietario del pacchetto.  
  
### <a name="permissions"></a>Autorizzazioni  
 Per impostazione predefinita, le autorizzazioni dei ruoli predefiniti a livello di database **db_ssisadmin** e **db_ssisoperator** e l'ID di sicurezza univoco dell'utente che ha creato il pacchetto si applicano al ruolo lettura per il pacchetto, mentre le autorizzazioni del ruolo **db_ssisadmin** e l'ID di sicurezza univoco dell'utente che ha creato il pacchetto si applicano al ruolo scrittura. Per disporre dell'accesso in lettura per il pacchetto, l'utente deve essere membro del ruolo **db_ssisadmin**, **db_ssisltduser** o **db_ssisoperator** . Per disporre dell'accesso in scrittura, l'utente deve essere membro del ruolo **db_ssisadmin** .  
  
### <a name="access-to-packages"></a>Accesso ai pacchetti  
 I ruoli predefiniti del database funzionano congiuntamente ai ruoli definiti dall'utente, ovvero i ruoli creati dall'utente in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e usati per l'assegnazione di autorizzazioni ai pacchetti. Per poter accedere a un pacchetto, un utente deve essere membro del ruolo definito dall'utente e del ruolo predefinito del database di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] appropriato. Se, ad esempio, gli utenti sono membri del ruolo definito dall'utente **AuditUsers** assegnato a un pacchetto, per poter accedere in lettura al pacchetto devono essere membri anche del ruolo **db_ssisadmin**, **db_ssisltduser** o **db_ssisoperator** .  
  
 Se non si assegna alcun ruolo definito dall'utente ai pacchetti, l'accesso verrà determinato dai ruoli predefiniti a livello di database.  
  
 Per poter assegnare i ruoli definiti dall'utente ai pacchetti, è prima necessario aggiungerli al database **msdb** . È possibile creare nuovi ruoli del database in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 I ruoli a livello di database di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] concedono diritti per le tabelle di sistema di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nel database msdb.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Per poter eseguire la connessione al Motore di database e accedere al database **msdb** , è necessario che si stato avviato il servizio MSSQLSERVER.  
  
 Per assegnare ruoli ai pacchetti, è necessario completare le attività seguenti.  
  
-   **Aprire Esplora oggetti e connettersi a Integration Services**  
  
     Per poter assegnare ruoli ai pacchetti in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], è necessario aprire Esplora oggetti in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e connettersi a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
     Per poter eseguire la connessione a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , è necessario che il servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]sia stato avviato.  
  
-   **Assegnazione dei ruoli lettura e scrittura ai pacchetti**  
  
     È possibile assegnare un ruolo lettura o scrittura a ogni pacchetto.  

## <a name="assign-a-reader-and-writer-role-to-a-package"></a><a name="assign"></a> Assegnazione di un ruolo lettura e scrittura a un pacchetto
  È possibile assegnare un ruolo lettura o scrittura a ogni pacchetto.  
  
### <a name="assign-a-reader-and-writer-role-to-a-package"></a>Assegnazione di un ruolo lettura e scrittura a un pacchetto  
  
1.  In Esplora oggetti individuare la connessione a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  Espandere la cartella Pacchetti archiviati e quindi la sottocartella contenente il pacchetto a cui si desidera assegnare ruoli.  
  
3.  Fare clic con il pulsante destro del mouse sul pacchetto desiderato.  
  
4.  Nella finestra di dialogo **Ruoli pacchetto** selezionare un ruolo nell'elenco **Ruolo lettura** e un ruolo nell'elenco **Ruolo scrittura** .  
  
5.  Fare clic su **OK**.

## <a name="create-a-user-defined-role"></a><a name="create"></a> Creazione di un ruolo definito dall'utente
    
### <a name="to-create-a-user-defined-role"></a>Per creare un ruolo definito dall'utente  
  
1.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Scegliere **Esplora oggetti** dal menu **Visualizza** .  
  
3.  Nella barra degli strumenti di Esplora oggetti fare clic su **Connetti** e quindi su **Motore di database**.  
  
4.  Nella finestra di dialogo **Connetti al server** specificare un nome di server e selezionare una modalità di autenticazione. Per specificare il server locale, è possibile digitare un punto (.), (locale) o **localhost** .  
  
5.  Fare clic su **Connetti**.  
  
6.  Espandere i nodi Database, Database di sistema, msdb, Sicurezza e Ruoli.  
  
7.  Nel nodo Ruoli fare clic con il pulsante destro del mouse su Ruoli del database e quindi scegliere **Nuovo ruolo del database**.  
  
8.  Nella pagina Generale specificare un nome. Facoltativamente specificare un proprietario e gli schemi di proprietà e aggiungere i membri del ruoli.  
  
9. Facoltativamente, fare clic su **Autorizzazioni** e configurare le autorizzazioni per gli oggetti.  
  
10. Facoltativamente, fare clic su **Proprietà estese** e configurare le proprietà estese.  
  
11. Fare clic su **OK**.

## <a name="package-roles-dialog-box-ui-reference"></a><a name="roles_dialog"></a> Riferimento all'interfaccia utente della finestra di dialogo Ruoli pacchetto
  Usare la finestra di dialogo **Ruoli pacchetto** , disponibile in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], per specificare i ruoli a livello di database che dispongono dell'accesso in lettura al pacchetto e quelli che dispongono dell'accesso in scrittura. I ruoli a livello di database si applicano solo ai pacchetti archiviati nel database  **msdb** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 I ruoli elencati nella finestra di dialogo sono quelli attualmente disponibili nel database di sistema **msdb** . Se non viene selezionato alcun ruolo, viene applicato il ruolo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] predefinito. Per impostazione predefinita, il ruolo lettura include **db_ssisadmin**, **db_ssisoperator** e l'utente che ha creato il pacchetto. Gli utenti membri di uno di questi ruoli o creatori dei pacchetti possono enumerare, visualizzare, esportare ed eseguire i pacchetti. Per impostazione predefinita, il ruolo scrittura include **db_ssisadmin** e l'utente che ha creato il pacchetto. L'utente membro di questo ruolo e il creatore dei pacchetti possono importare, eliminare e modificare i pacchetti.  
  
 La colonna **ownersid** nella tabella **sysssispackages** include l'ID di sicurezza (SID) univoco dell'utente che ha creato il pacchetto.  
  
### <a name="options"></a>Opzioni  
 **Nome pacchetto**  
 Consente di specificare il nome del pacchetto.  
  
 **Ruolo lettura**  
 Consente di selezionare un ruolo nell'elenco.  
  
 **Ruolo scrittura**  
 Consente di selezionare un ruolo nell'elenco.  
