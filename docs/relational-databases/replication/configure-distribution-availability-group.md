---
title: Configurare il database di distribuzione repliche nel gruppo di disponibilità
description: Configurare il database di distribuzione per le repliche di SQL Server con un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 01/16/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], distribution
- distribution configuration [SQL Server replication]
- remote Distributors [SQL Server replication]
- transactional replication, configuring distribution
- distribution databases [SQL Server replication], sizing
- Distributors [SQL Server replication], configuring
- distribution databases [SQL Server replication], about distribution databases
- distribution databases [SQL Server replication]
- merge replication [SQL Server replication], configuring distribution
ms.assetid: 94d52169-384e-4885-84eb-2304e967d9f7
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 36e2f12793e08211e285c907f26d99e0fcc9262d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348007"
---
# <a name="set-up-replication-distribution-database-in-always-on-availability-group"></a>Configurare il database di distribuzione repliche nel gruppo di disponibilità Always On
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo articolo viene illustrato come configurare i database di distribuzione repliche di SQL Server in un gruppo disponibilità Always On.

SQL Server 2017 CU6 e SQL Server 2016 SP2-CU3 introducono il supporto per i database di distribuzione repliche in un gruppo di disponibilità tramite i meccanismi seguenti:

- Il gruppo di disponibilità del database di distribuzione deve avere un listener. Quando il server di pubblicazione aggiunge il server di distribuzione, usa il nome del listener come nome del server di distribuzione.
- I processi di replica vengono creati con il nome del listener come nome del server di distribuzione. I processi di snapshot di replica, di lettura log e dell'agente di distribuzione (sottoscrizione push) creati nel server di distribuzione vengono creati in tutte le repliche secondarie del gruppo di disponibilità per il database di distribuzione.
 >[!NOTE]
 >I progetti dell'agente di distribuzione per le sottoscrizioni pull vengono creati nel server di sottoscrizione e non nel server di distribuzione. 
- Un nuovo processo consente di monitorare lo stato dei database di distribuzione (primari o secondari nel gruppo di disponibilità) e di disabilitare o abilitare i processi di replica in base allo stato dei database di distribuzione.

Dopo aver configurato un database di distribuzione nel gruppo di disponibilità in base ai passaggi descritti di seguito, i processi di runtime e la configurazione delle repliche possono essere eseguiti correttamente prima e dopo il failover del gruppo di disponibilità del database di distribuzione.

## <a name="supported-scenarios"></a>Scenari supportati

- Configurazione del database di distribuzione da includere in un gruppo di disponibilità.
- Configurazione delle repliche, come ad esempio pubblicazioni e sottoscrizioni prima e dopo il failover del gruppo di disponibilità.
- Processi di replica funzionali prima e dopo il failover.
- Rimozione della replica nel server di distribuzione e nel server di pubblicazione quando il database di distribuzione si trova nel gruppo di disponibilità.
- Aggiunta o rimozione di nodi nel gruppo di disponibilità del database di distribuzione esistente.
- Un'istanza del server di distribuzione può avere più database di distribuzione. Ogni database di distribuzione può essere nel proprio gruppo di disponibilità e non può essere in tutti i gruppi di disponibilità. Più database di distribuzione possono condividere un gruppo di disponibilità.
- Il server di pubblicazione e il server di distribuzione devono essere in istanze separate di SQL Server.
- Se il listener del gruppo di disponibilità che ospita il database di distribuzione è configurato per usare una porta non predefinita, è necessario configurare un alias per il listener e la porta non predefinita.

## <a name="limitations-or-exclusions"></a>Limitazioni o esclusioni

- Il server di distribuzione locale non è supportato. Ad esempio, il server di pubblicazione e il server di distribuzione devono trovarsi in istanze diverse di SQL Server. Queste istanze possono essere ospitate nello stesso set di nodi.  Un server di pubblicazione che usa se stesso come server di distribuzione, ovvero come server di distribuzione locale, non supporta i database di distribuzione in un gruppo di disponibilità.
- Il server di pubblicazione Oracle non è supportato.
- ma non la replica di tipo merge.
- La replica transazionale con sottoscrittori ad aggiornamento immediato o in coda non è supportata.
- La replica peer-to-peer non è supportata.
- Tutte le istanze di SQL Server 2017 che ospitano le repliche del database di distribuzione devono essere SQL Server 2017 CU 6 o versione successiva. 
- Tutte le istanze di SQL Server 2016 che ospitano le repliche del database di distribuzione devono essere SQL Server 2016 SP2-CU3 o versione successiva.
- Tutte le istanze di SQL Server che ospitano le repliche del database di distribuzione devono essere della stessa versione, tranne nel breve intervallo di tempo durante il quale viene eseguito l'aggiornamento.
- Il database di distribuzione deve essere in modalità di recupero con registrazione completa.
- Per il ripristino e per consentire il troncamento del log delle transazioni, configurare il backup del log delle transazioni e il backup completo.
- Il gruppo di disponibilità del database di distribuzione deve avere un listener configurato.
- Le repliche secondarie in un gruppo di disponibilità del database di distribuzione possono essere sincrone o asincrone. La modalità sincrona è consigliata e preferita.
- La replica transazionale bidirezionale non è supportata.
- SSMS non visualizza il database di distribuzione come in fase di sincronizzazione/sincronizzato quando il database di distribuzione viene aggiunto a un gruppo di disponibilità.


   >[!NOTE]
   >Prima di eseguire qualsiasi stored procedure di replica (ad esempio, `sp_dropdistpublisher`, `sp_dropdistributiondb`, `sp_dropdistributor`, `sp_adddistributiondb`, `sp_adddistpublisher`) nella replica secondaria, verificare che la replica sia completamente sincronizzata.

- Tutte le repliche secondarie in un gruppo di disponibilità del database di distribuzione devono essere leggibili. Se una replica secondaria non è leggibile, non è possibile accedere alle proprietà del server di distribuzione in SQL Server Management Studio nella replica secondaria specifica, ma la replica continuerà a funzionare correttamente. 
- Tutti i nodi del gruppo di disponibilità del database di distribuzione devono usare lo stesso account di dominio per eseguire SQL Server Agent, e tale account di dominio deve avere lo stesso privilegio in ogni nodo.
- Se vi sono agenti di replica che vengono eseguiti in un account proxy, l'account proxy deve esistere in ogni nodo del gruppo di disponibilità del database di distribuzione e avere lo stesso privilegio in ogni nodo.
- Apportare modifiche alle proprietà del server di distribuzione o del database di distribuzione in tutte le repliche del gruppo di disponibilità del database di distribuzione.
- Modificare i processi di replica tramite stored procedure msdb o SQL Server Management Studio in tutte le repliche del gruppo di disponibilità del database di distribuzione.
- La configurazione del server di distribuzione nel server di pubblicazione deve essere eseguita con gli script. Non è possibile usare la procedura guidata di replica. Le procedure guidate di replica e le finestre delle proprietà sono supportate per altri scopi.
- La configurazione del gruppo di disponibilità per i database di distribuzione può essere eseguita solo tramite script.
- Per configurare i database di distribuzione in un gruppo di disponibilità è necessario configurare una nuova replica. Il passaggio di un database di distribuzione esistente a un gruppo di disponibilità non è supportato. Una volta che un database di distribuzione viene tolto da un gruppo di disponibilità, non può più essere usato come database di distribuzione valido e deve essere eliminato.

## <a name="configuration-architecture"></a>Architettura della configurazione

Negli esempi in questo articolo vengono usati i nomi dei server e le impostazioni seguenti.

- DIST1, DIST2, DIST3 sono server di distribuzione;
- PUB è il server di pubblicazione;
- Dopo che è stato creato il gruppo di disponibilità, il nome del listener è DISTLISTENER;
- DIST1 è la replica primaria iniziale del gruppo di disponibilità del database di distribuzione.

## <a name="configure-distributor-distribution-database-and-publisher"></a>Configurare server di distribuzione, database di distribuzione e server di pubblicazione

In questo esempio vengono configurati un nuovo server di distribuzione e un nuovo server di pubblicazione e il database di distribuzione viene aggiunto a un gruppo di disponibilità.

### <a name="distributors-workflow"></a>Flusso di lavoro dei server di distribuzione

1. Configurare DIST1, DIST2, DIST3 come server di distribuzione con `sp_adddistributor @@servername`. Specificare la password per `distributor_admin` mediante l'elemento `@password`. L'elemento `@password` deve essere identico in DIST1, DIST2, DIST3.
2. Creare il database di distribuzione in DIST1 con `sp_adddistributiondb`. Il nome del database di distribuzione è `distribution`. Modificare la modalità di ripristino del database `distribution` da recupero con registrazione minima a recupero con registrazione completa.
3. Creare un gruppo di disponibilità per il database `distribution` con le repliche in DIST1, DIST2 e DIST3. È preferibile che tutte le repliche siano sincrone. Configurare le repliche secondarie in modo che siano leggibili o che consentano la lettura. A questo punto, i database di distribuzione sono nel gruppo di disponibilità, DIST1 è la replica primaria e DIST2 e DIST3 sono repliche secondarie.
4. Configurare un listener denominato `DISTLISTENER` per il gruppo di disponibilità.
5. Per il ripristino e per consentire il troncamento del log delle transazioni, configurare il backup del log delle transazioni e il backup completo.
6. In DIST2 e DIST3 eseguire:

   ```sql
   sp_adddistributiondb 'distribution'
   ```

1. Per aggiungere `PUB` come server di pubblicazione in DIST1, eseguire:
   
   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   Il valore di `@working_directory` deve essere un percorso di rete indipendente da DIST1, DIST2 e DIST3.

1. In DIST2 e DIST3, se la replica è leggibile come secondaria, eseguire il codice seguente:  

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   Se una replica non è leggibile come secondaria, eseguire il failover in modo che la replica diventi primaria, quindi eseguire il codice seguente: 

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```
   
   Il valore di `@working_directory` deve coincidere con quello del passaggio precedente.

### <a name="publisher-workflow"></a>Flusso di lavoro del server di pubblicazione

Per aggiungere il listener del gruppo di disponibilità del database `distribution` come server di distribuzione, eseguire in PUB: 

   ```sql
   sp_adddistributor @distributor = 'DISTLISTENER', @password = <distributor_admin password> 
   ```

   Il valore di @password deve essere quello specificato quando i server di distribuzione sono stati configurati nel flusso di lavoro del server di distribuzione.

## <a name="remove-distributor-and-publisher"></a>Rimuovere il server di distribuzione e il server di pubblicazione

In questo esempio vengono rimossi il server di pubblicazione e il server di distribuzione quando il database di distribuzione si trova nel gruppo di disponibilità.

### <a name="publisher-workflow"></a>Flusso di lavoro del server di pubblicazione

In PUB eliminare tutte le sottoscrizioni e le pubblicazioni per il server di pubblicazione e chiamare `sp_dropdistributor`.

### <a name="distributors-workflow"></a>Flusso di lavoro dei server di distribuzione

In questo esempio DIST1 è la replica primaria corrente del gruppo di disponibilità del database `distribution`. DIST2 e DIST3 sono repliche secondarie.

1. In DIST2 e DIST3 eseguire:

   ```sql
   sp_dropdistpublisher 'PUB',  @no_checks = 1
   ```

1. In DIST1 eseguire:

   ```sql
   sp_dropdistpublisher 'PUB'
   ```

1. Eliminare il gruppo di disponibilità.
2. In DIST2 e DIST3 modificare il database `distribution` in modalità read_write ripristinando il database con il recupero.
   
   ```sql
   RESTORE DATABASE distribution WITH RECOVERY, KEEP_REPLICATION
   ```

1. Per eliminare il database `distribution` e per mantenere la directory di snapshot, eseguire: 

   ```sql
   sp_dropdistributiondb 'distribution' , @former_ag_secondary=1
   ```

  Questa procedura rimuove tutti i processi tralasciati nella replica.

1. Per eliminare il database `distribution` in DIST1, eseguire

   ```sql
   sp_dropdistributiondb 'distribution'
   ``` 

1. Se non sono disponibili altri database di distribuzione nel gruppo di disponibilità, eseguire `sp_dropdistributor` in DIST1, DIST2 e DIST3.

## <a name="add-a-replica-to-distribution-database-ag"></a>Aggiungere una replica al gruppo di disponibilità del database di distribuzione

In questo esempio viene aggiunto un nuovo server di distribuzione a una configurazione di replica esistente con un database di distribuzione nel gruppo di disponibilità. In questo esempio un database di distribuzione esistente è in un gruppo di disponibilità. DIST1 e DIST2 sono i server di distribuzione, `distribution` è il database di distribuzione nel gruppo di disponibilità e PUB è il server di pubblicazione. Aggiungere DIST3 come replica del gruppo di disponibilità.

### <a name="distributors-workflow"></a>Flusso di lavoro dei server di distribuzione

1. DIST3 deve essere configurato come server di distribuzione tramite `sp_adddistributor @@servername`. La password per `distributor_admin` deve essere specificata tramite il parametro @password. La password deve essere quella specificata per DIST1 e DIST2.
2. Aggiungere DIST3 al gruppo di disponibilità per il database di distribuzione corrente.
3. In DIST3 eseguire:

   ```sql
   sp_adddistributiondb 'distribution'
   ```

4. In DIST3, se la replica è leggibile come secondaria, eseguire il codice seguente: 

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   Se la replica non è leggibile come secondaria, eseguire il failover in modo che la replica diventi primaria, quindi eseguire il codice seguente:

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```
   
   Il valore di `@working_directory` deve essere quello specificato per DIST1 e DIST2.

4. In DIST3 è necessario ricreare i server collegati presso i sottoscrittori.

## <a name="remove-a-replica-from-distribution-database-ag"></a>Rimuovere una replica dal gruppo di disponibilità del database di distribuzione

In questo esempio viene rimosso un server di distribuzione da un gruppo di disponibilità del database di distribuzione corrente senza alcun effetto sulle repliche nel gruppo di disponibilità del database di distribuzione. In questo esempio il database di distribuzione è in un gruppo di disponibilità. DIST1, DIST2 e DIST3 sono i server di distribuzione, `distribution` è il database di distribuzione nel gruppo di disponibilità, e PUB è il server di pubblicazione. Rimuovere DIST3 dal gruppo di disponibilità.

### <a name="distributors-workflow"></a>Flusso di lavoro dei server di distribuzione

1. Assicurarsi che DIST3 sia un server secondario per il gruppo di disponibilità del database `distribution`.
2. Rimuovere DIST3 dal gruppo di disponibilità del database `distribution`.
3. In DIST3 modificare il database `distribution` in modalità read_write ripristinando il database con il recupero. Ad esempio, eseguire il comando seguente:  
   
   ```sql
   RESTORE DATABASE distribution WITH RECOVERY, KEEP_REPLICATION.
   ```
   
1. Per rimuovere tutti i processi orfani in DIST3, eseguire: 

   ```sql
   sp_dropdistpublisher 'PUB', @no_checks = 1
   ```

1. In DIST3 eseguire:

   ```sql
   sp_dropdistributiondb 'distribution', @former_ag_secondary=1
   ```

1. In DIST3 eseguire: 

   ```sql
   sp_dropdistributor
   ```

## <a name="remove-a-publisher-from-distribution-database-ag"></a>Rimuovere un server di pubblicazione dal gruppo di disponibilità del database di distribuzione

In questo esempio viene rimosso un server di pubblicazione da un gruppo di disponibilità del database di distribuzione corrente del server di distribuzione senza alcun effetto sui restanti server di pubblicazione gestiti dal gruppo di disponibilità del database di distribuzione. In questo esempio la configurazione esistente include un database di distribuzione in un gruppo di disponibilità. DIST1, DIST2 e DIST3 sono i server di distribuzione, `distribution` è il database di distribuzione nel gruppo di disponibilità e PUB1 e PUB2 sono i server di pubblicazione serviti dal database `distribution`. Nell'esempio PUB1 viene rimosso dai server di distribuzione.

### <a name="publisher-workflow"></a>Flusso di lavoro del server di pubblicazione

In PUB1 eliminare tutte le sottoscrizioni e le pubblicazioni per il server di pubblicazione e chiamare `sp_dropdistributor`.

### <a name="distributor-workflow"></a>Flusso di lavoro del server di distribuzione

DIST1 è la replica primaria corrente del gruppo di disponibilità del database `distribution`.

1. In DIST2 e DIST3 eseguire:

   ```sql
   sp_dropdistpublisher 'PUB1',  @no_checks = 1
   ```

1. In DIST1 eseguire:

   ```sql
   sp_dropdistpublisher 'PUB1'
   ```

1. A questo punto, potrebbero esservi processi orfani correlati a PUB1 in DIST2 o DIST3. Ogni volta che si verifica un failover in DIST2 e DIST3, i processi orfani correlati a tutte le pubblicazioni di PUB1 verranno rimossi durante il processo `Monitor and sync replication agent jobs`.

## <a name="add-subscription"></a>Aggiungere una sottoscrizione

Questo esempio illustra la configurazione corretta delle informazioni relative al sottoscrittore tra i server di distribuzione. Nell'esempio viene aggiunto un sottoscrittore. DIST1 è la replica primaria corrente del database di distribuzione nel gruppo di disponibilità, DIST2 e DIST3 sono repliche secondarie del database di distribuzione nel gruppo di disponibilità. Il nome del sottoscrittore è SUB.

### <a name="publisher-workflow"></a>Flusso di lavoro del server di pubblicazione

In PUB aggiungere una sottoscrizione come si farebbe normalmente al sottoscrittore `SUB`.

### <a name="distributor-workflow"></a>Flusso di lavoro del server di distribuzione

In DIST2 e DIST3 aggiungere un server collegato per "SUB" se non è stato registrato in precedenza con DIST2 o DIST3. Di seguito è riportato un TSQL di esempio per la creazione del server collegato.

   ```sql 
   EXEC master.dbo.sp_addlinkedserver@server =N'SUB', @srvproduct=N'SQL Server'
   /* For security reasons the linked server remote logins password is changed with ######## */
   EXEC master.dbo.sp_addlinkedsrvlogin@rmtsrvname=N'SUB', @useself=N'True',@locallogin=NULL,@rmtuser=NULL,@rmtpassword=NULL
   ```

## <a name="add-a-pull-subscription"></a>Aggiungere una sottoscrizione pull

### <a name="subscriber-workflow"></a>Flusso di lavoro sottoscrittore

Per aggiungere una sottoscrizione pull per una pubblicazione con il database di distribuzione in un gruppo di disponibilità, usare il nome del listener del gruppo di disponibilità nel parametro `@distributor` di `sp_addpullsubscription_agent`.

## <a name="sample-t-sql-create-distribution-db-in-ag"></a>Esempio T-SQL per creare un database di distribuzione nel gruppo di disponibilità

Lo script seguente crea un database di distribuzione in un gruppo di disponibilità. 

```sql
--- WorkFlow to Enable Distribution Database In AG.

-- SECTION 1 ---- CONFIGURE THE DISTRIBUTOR SERVERS

-- Step1 - Configure the Distribution DB nodes (AG Replicas) to act as a distributor
:Connect SQLNode1
sp_adddistributor @distributor = @@ServerName, @password = 'Pass@word1'
Go 
:Connect SQLNode2
sp_adddistributor @distributor = @@ServerName, @password = 'Pass@word1'
Go

-- Step2 - Configure the Distribution Database
:Connect SQLNode1
USE master
EXEC sp_adddistributiondb @database = 'DistributionDB', @security_mode = 1;
GO
Alter Database [DistributionDB] Set Recovery Full
Go
Backup Database [DistributionDB] to Disk = 'Nul'
Go
-- Step 3 - Create AG for the Distribution DB.
:Connect SQLNode1
USE [master]
GO
CREATE ENDPOINT [Hadr_endpoint] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)   
    FOR DATA_MIRRORING (ROLE = ALL, AUTHENTICATION = WINDOWS NEGOTIATE
, ENCRYPTION = REQUIRED ALGORITHM AES)
GO

:Connect SQLNode2
USE [master]
GO
CREATE ENDPOINT [Hadr_endpoint] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)   
    FOR DATA_MIRRORING (ROLE = ALL, AUTHENTICATION = WINDOWS NEGOTIATE
, ENCRYPTION = REQUIRED ALGORITHM AES)
GO

:Connect SQLNode1
-- Create the Availability Group
CREATE AVAILABILITY GROUP [DistributionDB_AG]
FOR DATABASE [DistributionDB]
REPLICA ON 'SQLNode1'
WITH (ENDPOINT_URL = N'TCP://SQLNode1.contoso.com:5022', 
         FAILOVER_MODE = AUTOMATIC, 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
         BACKUP_PRIORITY = 50, 
         SECONDARY_ROLE(ALLOW_CONNECTIONS = ALL), 
         SEEDING_MODE = AUTOMATIC),
N'SQLNode2' WITH (ENDPOINT_URL = N'TCP://SQLNode2.contoso.com:5022', 
     FAILOVER_MODE = AUTOMATIC, 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
     BACKUP_PRIORITY = 50, 
     SECONDARY_ROLE(ALLOW_CONNECTIONS = ALL), 
     SEEDING_MODE = AUTOMATIC);
 GO


:Connect SQLNode2
ALTER AVAILABILITY GROUP [DistributionDB_AG] JOIN
GO  
ALTER AVAILABILITY GROUP [DistributionDB_AG] GRANT CREATE ANY DATABASE
Go

--STEP4 - Create the Listener for the Availability Group. This is very important.
:Connect SQLNode1

USE [master]
GO
ALTER AVAILABILITY GROUP [DistributionDB_AG]
ADD LISTENER N'DistributionDBList' (
WITH IP
((N'10.0.0.8', N'255.255.255.0')) , PORT=1500);
GO

-- STEP 5 - Enable SQLNode2 also as a Distributor
:CONNECT SQLNODE2
EXEC sp_adddistributiondb @database = 'DistributionDB', @security_mode = 1;
GO

--STEP 6 - On all Distributor Nodes Configure the Publisher Details 
:CONNECT SQLNODE1
EXEC sp_addDistPublisher @publisher = 'SQLNode4', @distribution_db = 'DistributionDB', 
    @working_directory = '\\sqlfileshare\Dist_Work_Directory\'
GO
:CONNECT SQLNODE2
EXEC sp_addDistPublisher @publisher = 'SQLNode4', @distribution_db = 'DistributionDB', 
    @working_directory = '\\sqlfileshare\Dist_Work_Directory\'
GO

-- SECTION 2 ---- CONFIGURE THE PUBLISHER SERVER
:CONNECT SQLNODE4
EXEC sp_addDistributor @distributor = 'DistributionDBList', -- Listener for the Distribution DB.    
    @password = 'Pass@word1'
Go

-- SECTION 3 ---- CONFIGURE THE SUBSCRIBERS 
-- On Publisher, create the publication as one would normally do.
-- On the Secondary replicas of the Distribution DB, add the Subscriber as a linked server.
:CONNECT SQLNODE2
EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE5', @srvproduct=N'SQL Server'
 /* For security reasons the linked server remote logins password is changed with ######## */
EXEC master.dbo.sp_addlinkedsrvlogin @rmtsrvname=N'SQLNODE5',@useself=N'True',@locallogin=NULL,@rmtuser=NULL,@rmtpassword=NULL 
```

## <a name="see-also"></a>Vedere anche  
 [Pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Proteggere il database di distribuzione](../../relational-databases/replication/security/secure-the-distributor.md)  
  
## <a name="next-steps"></a>Passaggi successivi
 [Visualizzare e modificare le proprietà del server di pubblicazione e del database di distribuzione](view-and-modify-distributor-and-publisher-properties.md)  
 [Disabilitare la pubblicazione e la distribuzione](disable-publishing-and-distribution.md)  
 [Abilitare un database per la replica (SQL Server Management Studio)](enable-a-database-for-replication-sql-server-management-studio.md) 
