---
title: Configurare l'opzione di configurazione del server cost threshold for parallelism | Microsoft Docs
description: Informazioni sull'opzione cost threshold for parallelism. Scoprire in che modo il suo valore determina se SQL Server esegue piani paralleli per le query e come impostarla.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- cost threshold for parallelism option
ms.assetid: dad21bee-fe28-41f6-9d2f-e6ababfaf9db
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ea4681fd02065129c4b565d0e9dbb744a321f9ae
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783303"
---
# <a name="configure-the-cost-threshold-for-parallelism-server-configuration-option"></a>Configurare l'opzione di configurazione del server cost threshold for parallelism
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento si illustra come configurare l'opzione di configurazione del server **cost threshold for parallelism** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Con l'opzione **cost threshold for parallelism** è possibile specificare la soglia oltre la quale in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono creati ed eseguiti piani paralleli per le query. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene creato ed eseguito un piano parallelo per una query solo quando il costo stimato per l'esecuzione di un piano seriale per la stessa query è più elevato del valore impostato in **cost threshold for parallelism**. Si tratta del costo stimato per l'esecuzione del piano seriale in una configurazione hardware specifica e non è riferito a una determinata unità di tempo. L'opzione **cost threshold for parallelism** può essere impostata su qualsiasi valore compreso tra 0 e 32767. Il valore predefinito è 5.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per configurare l'opzione cost threshold for parallelism utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [Dopo la configurazione dell'opzione cost threshold for parallelism](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Il costo indica un'unità di costo astratto e non un'unità di tempo stimata. Impostare l'opzione **cost threshold for parallelism** esclusivamente in sistemi basati su multiprocessori simmetrici.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] il valore dell'opzione **cost threshold for parallelism** verrà ignorato nei casi seguenti:  
  
    -   Il computer in uso è dotato di un solo processore.  
  
    -   È disponibile un solo processore logico per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a causa dell'opzione di configurazione **affinity mask** .  
  
    -   L'opzione **max degree of parallelism** è impostata su 1.  
  
Un processore logico è l'unità di base dell'hardware del processore che consente al sistema operativo di inviare un'attività o eseguire un contesto di thread. Ogni processore logico può eseguire un solo contesto di thread per volta. Il core del processore è il set di circuiti che consente di decodificare ed eseguire istruzioni. Un core di processore può contenere uno o più processori logici. La query [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente può essere utilizzata per l'acquisizione di informazioni sulla CPU del sistema.  
  
```sql  
SELECT (cpu_count / hyperthread_ratio) AS PhysicalCPUs,   
cpu_count AS logicalCPUs   
FROM sys.dm_os_sys_info  
```  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Questa opzione è avanzata e la relativa modifica è riservata ad amministratori di database esperti o a professionisti dotati di certificazione per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   In determinati casi il piano parallelo viene scelto anche se il piano costi di una query risulta inferiore al valore corrente di **cost threshold for parallelism** . La scelta tra piano parallelo o seriale è infatti basata su una stima dei costi fornita in precedenza nell'ottimizzazione. Per altre informazioni, vedere [Guida sull'architettura di elaborazione delle query](../../relational-databases/query-processing-architecture-guide.md#parallel-query-processing).  

-   Il valore predefinito 5 è adeguato per la maggior parte dei sistemi, ma nel caso specifico può risultare appropriato un valore diverso. Se necessario, eseguire il test dell'applicazione con valori superiori e inferiori per ottimizzare le prestazioni.
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_configure** senza alcun parametro o solo con il primo parametro vengono assegnate per impostazione predefinita a tutti gli utenti. Per eseguire **sp_configure** con entrambi i parametri per la modifica di un'opzione di configurazione o per l'esecuzione dell'istruzione RECONFIGURE, a un utente deve essere concessa l'autorizzazione a livello di server ALTER SETTINGS. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-configure-the-cost-threshold-for-parallelism-option"></a>Per configurare l'opzione cost threshold for parallelism  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un server e scegliere **Proprietà**.  
  
2.  Fare clic sul nodo **Avanzate** .  
  
3.  In **Parallelismo** impostare l'opzione **Soglia costo per parallelismo** sul valore desiderato. Digitare o selezionare un valore compreso tra 0 e 32767.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-configure-the-cost-threshold-for-parallelism-option"></a>Per configurare l'opzione cost threshold for parallelism  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Questo esempio illustra come usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) per impostare il valore dell'opzione `cost threshold for parallelism` su `10`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'cost threshold for parallelism', 10 ;  
GO  
RECONFIGURE  
GO  
```  
  
 Per altre informazioni, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)sia installato il servizio WMI.  
  
##  <a name="follow-up-after-you-configure-the-cost-threshold-for-parallelism-option"></a><a name="FollowUp"></a> Completamento: Dopo la configurazione dell'opzione cost threshold for parallelism  
 L'impostazione diventa effettiva immediatamente senza dover riavviare il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md)   
 [Hint di query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [Opzione di configurazione del server affinity mask](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
