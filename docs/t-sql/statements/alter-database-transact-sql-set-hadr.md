---
description: ALTER DATABASE SET HADR (Transact-SQL)
title: ALTER DATABASE SET HADR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET HADR
- SET_HADR_TSQL
- HADR_TSQL
- HADR
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER DATABASE statement, AlwaysOn Availability Group
- ALTER DATABASE statement, SET HADR options
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], Transact-SQL statements
- Availability Groups [SQL Server], databases
ms.assetid: 20e6e803-d6d5-48d5-b626-d1e0a73d174c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7d62a25ccfdf4abb6231f226e7a846418680a155
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184520"
---
# <a name="alter-database-transact-sql-set-hadr"></a>ALTER DATABASE (Transact-SQL) SET HADR 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento è inclusa la sintassi di ALTER DATABASE correlata all'impostazione delle opzioni [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] in un database secondario. È consentita una sola opzione SET HADR per ogni istruzione ALTER DATABASE. Queste opzioni sono supportate solo su repliche secondarie.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER DATABASE database_name  
   SET HADR   
   {  
        { AVAILABILITY GROUP = group_name | OFF }  
   | { SUSPEND | RESUME }  
   }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *database_name*  
 Nome del database secondario da modificare.  
  
 SET HADR  
 Esegue il comando [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] indicato nel database specificato.  
  
 { AVAILABILITY GROUP **=** _group_name_ | OFF }  
 Crea un join del database di disponibilità o lo rimuove dal gruppo di disponibilità specificato come segue:  
  
 *group_name*  
 Crea un join del database specificato nella replica secondaria ospitata dall'istanza del server in cui si esegue il comando al gruppo di disponibilità specificato da group_name.  
  
 I prerequisiti per questa operazione sono i seguenti:  
  
-   È necessario che il database sia già stato aggiunto al gruppo di disponibilità nella replica primaria.  
  
-   La replica primaria deve essere attiva. Per informazioni su come risolvere i problemi relativi a una replica primaria inattiva, vedere l'articolo [Risolvere i problemi relativi alla configurazione di Gruppi di disponibilità AlwaysOn (SQL Server)](/previous-versions/sql/sql-server-2012/ff878308(v=sql.110)).  
  
-   La replica primaria deve essere online e quella secondaria deve essere connessa alla primaria.  
  
-   Il database secondario deve essere stato ripristinato tramite WITH NORECOVERY da backup di database e di log recenti eseguiti sul database primario che terminano con un backup del log sufficientemente recente per consentire che il database secondario venga aggiornato in base a quello primario.  
  
    > [!NOTE]  
    >  Per aggiungere un database al gruppo di disponibilità, connettersi all'istanza del server che ospita la replica primaria e usare l'istruzione [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)*group_name* ADD DATABASE *database_name*.  
  
 Per altre informazioni, vedere [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
 OFF  
 Rimuove il database secondario specificato dal gruppo di disponibilità.  
  
 La rimozione di un database secondario può essere utile se tale database è in ritardo rispetto a quello primario e non si desidera attenderne l'aggiornamento. Dopo aver rimosso il database secondario, è possibile aggiornarlo ripristinando una sequenza di backup che termina con un backup del log recente (usando RESTORE ... WITH NORECOVERY).  
  
> [!IMPORTANT]  
>  Per rimuovere completamente un database di disponibilità da un gruppo di disponibilità, connettersi all'istanza del server che ospita la replica di disponibilità primaria e usare l'istruzione [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)*group_name* REMOVE DATABASE *availability_database_name*. Per altre informazioni, vedere [Rimuovere un database primario da un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).  
  
 SUSPEND  
 Sospende lo spostamento dei dati su un database secondario. Un comando SUSPEND viene restituito non appena è stato accettato dalla replica che ospita il database di destinazione, ma la sospensione effettiva del database avviene in modo asincrono.  
  
 L'ambito dell'impatto dipende dal punto in cui si esegue l'istruzione ALTER DATABASE:  
  
-   Se si sospende un database secondario su una replica secondaria, viene sospeso solo il database secondario locale. Le connessioni esistenti nel database secondario leggibile rimangono utilizzabili. Non sono consentite nuove connessioni al database sospeso nel database secondario leggibile finché non viene ripreso lo spostamento di dati.  
  
-   Se si sospende un database nella replica primaria, lo spostamento di dati viene sospeso nei database secondari corrispondenti in ogni replica secondaria. Le connessioni esistenti in un database secondario leggibile rimangono usabili e le nuove connessioni con finalità di lettura non si connettono a repliche secondarie leggibili.  
  
-   Quando lo spostamento di dati viene sospeso a causa di un failover manuale forzato, non sono consentite connessioni alla nuova replica secondaria e lo spostamento di dati è sospeso.  
  
 Quando un database in una replica secondaria viene sospeso, sia il database sia la replica non risultano più sincronizzati e vengono contrassegnati come NOT SYNCHRONIZED.  
  
> [!IMPORTANT]  
>  Durante la fase di sospensione di un database secondario, nella coda di invio del database primario corrispondente verranno accumulati record del log delle transazioni non inviati. Tramite le connessioni alla replica secondaria vengono restituiti i dati disponibili quando lo spostamento di dati è stato sospeso.  
  
> [!NOTE]  
>  La sospensione e la ripresa di un database secondario AlwaysOn non incide direttamente sulla disponibilità del database primario, anche se la sospensione di un database secondario può avere un impatto sulle funzionalità di ridondanza e failover del database primario, finché il database secondario sospeso non viene ripreso. Questo comportamento è diverso rispetto al mirroring del database, in cui lo stato del mirroring risulta sospeso sia sul database mirror che sul database principale, finché il mirroring non viene ripreso. La sospensione di un database primario AlwaysOn comporta la sospensione dello spostamento di dati su tutti i corrispondenti database secondari e le funzionalità di ridondanza e failover cessano per tale database finché non viene ripreso il database primario.  
  
 Per altre informazioni, vedere [Sospendere un database di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md).  
  
 RESUME  
 Riprende lo spostamento dei dati sospesi nel database secondario specificato. Un comando RESUME viene restituito non appena è stato accettato dalla replica che ospita il database di destinazione, ma la ripresa effettiva del database avviene in modo asincrono.  
  
 L'ambito dell'impatto dipende dal punto in cui si esegue l'istruzione ALTER DATABASE:  
  
-   Se si riprende un database secondario su una replica secondaria, viene ripreso solo il database secondario locale. Lo spostamento dei dati viene ripreso a meno che il database non sia stato sospeso anche sulla replica primaria.  
  
-   Se si riprende un database sulla replica primaria, lo spostamento dei dati viene ripreso su ogni replica secondaria su cui il database secondario corrispondente non è stato sospeso anche localmente. Per riprendere un database secondario è sospeso singolarmente su una replica secondaria, connettersi all'istanza del server che ospita la replica secondaria e riprendere il database da quel punto.  
  
     In modalità con commit sincrono modalità lo stato del database cambia su SYNCHRONIZING. Se attualmente non è sospeso alcun altro database, anche lo stato della replica viene modificato in SYNCHRONIZING.  
  
     Per ulteriori informazioni, vedere [Riprendere un database di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md).  
  
## <a name="database-states"></a>Stati del database  
 Quando per un database secondario viene creato un join a un gruppo di disponibilità, la replica secondaria locale modifica lo stato di tale database da RESTORING a ONLINE. Se un database secondario viene rimosso da un gruppo di disponibilità, lo stato viene reimpostato su RESTORING dalla replica secondaria locale. In questo modo è possibile applicare backup di log successivi dal database primario a quello secondario.  
  
## <a name="restrictions"></a>Restrizioni  
 Eseguire le istruzioni ALTER DATABASE all'esterno sia delle transazioni che dei batch.  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER per il database. Per la creazione di un join di un database a un gruppo di disponibilità è richiesta l'appartenenza al ruolo predefinito del database **db_owner** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene creato un join del database secondario, `AccountsDb1`, alla replica secondaria locale del gruppo di disponibilità `AccountsAG`.  
  
```sql  
ALTER DATABASE AccountsDb1 SET HADR AVAILABILITY GROUP = AccountsAG;  
```  
  
> [!NOTE]  
>  Per un esempio di questa istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] impiegata in un contesto, vedere [Creare un gruppo di disponibilità &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [Panoramica di Gruppi di disponibilità Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) [Risolvere i problemi relativi alla configurazione di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md) 
  
