---
title: Creare e configurare un gruppo di disponibilità per SQL Server in Linux
description: Questa esercitazione mostra come creare e configurare i gruppi di disponibilità per SQL Server in Linux, oltre a creare endpoint e certificati dei gruppi di disponibilità.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/28/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: ecb634a770b4990bfbbc094b15498b03bdea4a57
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172003"
---
# <a name="create-and-configure-an-availability-group-for-sql-server-on-linux"></a>Creare e configurare un gruppo di disponibilità per SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questa esercitazione illustra come creare e configurare un gruppo di disponibilità per [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] in Linux. A differenza di [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] e versioni precedenti in Windows, è possibile abilitare i gruppi di disponibilità sia che si crei o non si crei prima il cluster Pacemaker sottostante. L'integrazione con il cluster, se necessaria, viene eseguita solo in un momento successivo.

L'esercitazione include le attività seguenti:
 
> [!div class="checklist"]
> * Abilitare i gruppi di disponibilità.
> * Creare endpoint e certificati per i gruppi di disponibilità.
> * Usare [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) o Transact-SQL per creare un gruppo di disponibilità.
> * Creare l'account di accesso di [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] e le autorizzazioni per Pacemaker.
> * Creare risorse dei gruppi di disponibilità in un cluster Pacemaker (solo di tipo Esterno).

## <a name="prerequisite"></a>Prerequisito
- Distribuire il cluster a disponibilità elevata Pacemaker come descritto in [Distribuire un cluster Pacemaker per SQL Server in Linux](sql-server-linux-deploy-pacemaker-cluster.md).


## <a name="enable-the-availability-groups-feature"></a>Abilitare la funzionalità dei gruppi di disponibilità

Diversamente da Windows, non è possibile usare PowerShell o [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Configuration Manager per abilitare la funzionalità dei gruppi di disponibilità. In Linux è necessario usare `mssql-conf` per abilitare la funzionalità. È possibile abilitare la funzionalità dei gruppi di disponibilità in due modi: usando l'utilità `mssql-conf` o modificando il file `mssql.conf` manualmente.

> [!IMPORTANT]
> La funzionalità dei gruppi di disponibilità deve essere abilitata per le repliche di sola configurazione, anche in [!INCLUDE[ssexpress-md](../includes/ssexpress-md.md)].

### <a name="use-the-mssql-conf-utility"></a>Usare l'utilità mssql-conf

Al prompt dei comandi eseguire il comando seguente:

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
```

### <a name="edit-the-mssqlconf-file"></a>Modificare il file mssql.conf

È anche possibile modificare il file `mssql.conf`, che si trova nella cartella `/var/opt/mssql`, aggiungendo le righe seguenti:

```
[hadr]

hadr.hadrenabled = 1
```

### <a name="restart-ssnoversion-md"></a>Riavviare [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]
Dopo aver abilitato i gruppi di disponibilità, come in Windows, è necessario riavviare [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. A tale scopo, eseguire il comando seguente:

```bash
sudo systemctl restart mssql-server
```

## <a name="create-the-availability-group-endpoints-and-certificates"></a>Creare endpoint e certificati del gruppo di disponibilità

Un gruppo di disponibilità usa gli endpoint TCP per le comunicazioni. In Linux gli endpoint per un gruppo di disponibilità sono supportati solo se per l'autenticazione vengono usati i certificati. Questo significa che il certificato di un'istanza deve essere ripristinato in tutte le altre istanze che saranno repliche che fanno parte dello stesso gruppo di disponibilità. Il processo di autenticazione tramite certificato è necessario anche per una replica di sola configurazione. 

La creazione degli endpoint e il ripristino dei certificati possono essere eseguiti solo tramite Transact-SQL. Si possono usare anche certificati non generati da [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Sarà necessario anche un processo per gestire e sostituire eventuali certificati scaduti.

> [!IMPORTANT]
> Se si prevede di usare la procedura guidata di [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] per creare il gruppo di disponibilità, è comunque necessario creare e ripristinare i certificati usando Transact-SQL in Linux.

Per la sintassi completa delle opzioni disponibili per i vari comandi, ad esempio per un livello di sicurezza aggiuntivo, vedere:

-   [BACKUP CERTIFICATE](../t-sql/statements/backup-certificate-transact-sql.md)
-   [CREATE CERTIFICATE](../t-sql/statements/create-certificate-transact-sql.md)
-   [CREATE ENDPOINT](../t-sql/statements/create-endpoint-transact-sql.md)

> [!NOTE]
> Anche se si sta creando un gruppo di disponibilità, il tipo di endpoint usa *FOR DATABASE_MIRRORING*, perché alcuni aspetti sottostanti erano condivisi con questa funzionalità ormai deprecata.

In questo esempio vengono creati i certificati per una configurazione a tre nodi. I nomi delle istanze sono LinAGN1, LinAGN2 e LinAGN3.

1.  Eseguire il codice seguente in LinAGN1 per creare la chiave master, il certificato e l'endpoint e per eseguire il backup del certificato. In questo esempio per l'endpoint viene usata la porta TCP standard 5022.
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN1_Cert
    WITH SUBJECT = 'LinAGN1 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN1_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN1_Cert,
        ROLE = ALL);
    
    GO
    ```
    
2.  Eseguire lo stesso codice in LinAGN2:
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    WITH SUBJECT = 'LinAGN2 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN2_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN2_Cert,
        ROLE = ALL);
    
    GO
    ```
    
3.  Infine, eseguire la stessa sequenza in LinAGN3:
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    WITH SUBJECT = 'LinAGN3 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN3_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN3_Cert,
        ROLE = ALL);
    
    GO
    ```
    
4.  Usando `scp` o un'altra utilità, copiare i backup del certificato in ogni nodo che farà parte del gruppo di disponibilità.
    
    Per questo esempio:
    
    - Copiare LinAGN1_Cert.cer in LinAGN2 e LinAGN3.
    - Copiare LinAGN2_Cert.cer in LinAGN1 e LinAGN3.
    - Copiare LinAGN3_Cert.cer in LinAGN1 e LinAGN2.
    
5.  Cambiare la proprietà e il gruppo associato ai file di certificato copiati in `mssql`.
    
    ```bash
    sudo chown mssql:mssql <CertFileName>
    ```
    
6.  Creare gli account di accesso a livello di istanza e gli utenti associati a LinAGN2 e LinAGN3 in LinAGN1.
    
    ```SQL
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
7.  Ripristinare LinAGN2_Cert e LinAGN3_Cert in LinAGN1. La presenza dei certificati delle altre repliche è un aspetto importante della comunicazione e della sicurezza dei gruppi di disponibilità.
    
    ```SQL
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
8.  Grant the logins associated with LinAG2 and LinAGN3 permission to connect to the endpoint on LinAGN1.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
9.  Creare gli account di accesso a livello di istanza e gli utenti associati a LinAGN1 e LinAGN3 in LinAGN2.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
10. Ripristinare LinAGN1_Cert e LinAGN3_Cert in LinAGN2.
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    ```
    
11. Concedere agli account di accesso associati a LinAGN1 e LinAGN3 l'autorizzazione per la connessione all'endpoint in LinAGN2.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
12. Creare gli account di accesso a livello di istanza e gli utenti associati a LinAGN1 e LinAGN2 in LinAGN3.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    ```
    
13. Ripristinare LinAGN1_Cert e LinAGN2_Cert in LinAGN3. 
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    ```
    
14. Concedere agli account di accesso associati a LinAGN1 e LinAGN2 l'autorizzazione per la connessione all'endpoint in LinAGN3.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    ```

## <a name="create-the-availability-group"></a>Creare il gruppo di disponibilità

Questa sezione illustra come usare [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) o Transact-SQL per creare il gruppo di disponibilità per [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

### <a name="use-ssmanstudiofull-md"></a>Utilizzare [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)].

Questa sezione illustra come creare un gruppo di disponibilità con un tipo di cluster esterno usando SSMS con la Creazione guidata gruppo di disponibilità.

1.  In SSMS espandere **Disponibilità elevata Always On**, fare clic con il pulsante destro del mouse su **Gruppi di disponibilità** e scegliere **Disponibilità elevata Always On**.

2.  Nella finestra di dialogo Introduzione fare clic su **Avanti**.

3.  Nella finestra di dialogo Specificare le opzioni del gruppo di disponibilità immettere un nome per il gruppo di disponibilità e selezionare il tipo di cluster ESTERNO o NESSUNO nell'elenco a discesa. Per distribuire Pacemaker occorre usare il tipo Esterno. Il tipo Nessuno è riservato per scenari specializzati, come la scalabilità in lettura. La selezione dell'opzione per il rilevamento dell'integrità a livello di database è facoltativa. Per altre informazioni su questa opzione, vedere [Opzione di failover di rilevamento dell'integrità a livello di database di un gruppo di disponibilità](../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md). Fare clic su **Avanti**.

    ![Creare un gruppo di disponibilità 03](./media/sql-server-linux-create-availability-group/image3.png)

4.  Nella finestra di dialogo Seleziona database selezionare i database che faranno parte del gruppo di disponibilità. Prima di aggiungere ogni database a un gruppo di disponibilità è necessario effettuarne un backup completo. Fare clic su **Avanti**.

5.  Nella finestra di dialogo Specifica repliche fare clic su **Aggiungi replica**.

6.  Nella finestra di dialogo Connetti al server immettere il nome dell'istanza di Linux di [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] che costituirà la replica secondaria e le credenziali per la connessione. Fare clic su **Connetti**.

7.  Ripetere i due passaggi precedenti per l'istanza che conterrà una replica di sola configurazione o un'altra replica secondaria.

8.  Ora tutte e tre le istanze dovrebbero essere elencate nella finestra di dialogo Specifica repliche. Se si usa il tipo di cluster Esterno, per la replica secondaria assicurarsi che la modalità di disponibilità corrisponda a quella della replica primaria e che la modalità di failover sia impostata su Esterno. Per la replica di sola configurazione, selezionare la modalità di disponibilità Sola configurazione.

    L'esempio seguente illustra un gruppo di disponibilità con due repliche, un cluster di tipo Esterno e una replica di sola configurazione.

    ![Creare un gruppo di disponibilità 04](./media/sql-server-linux-create-availability-group/image4.png)

    L'esempio seguente illustra un gruppo di disponibilità con due repliche, un cluster di tipo Nessuno e una replica di sola configurazione.

    ![Creare un gruppo di disponibilità 05](./media/sql-server-linux-create-availability-group/image5.png)

9.  Se si vogliono modificare le preferenze di backup, fare clic sulla scheda Preferenze di backup. Per altre informazioni sulle preferenze di backup con i gruppi di disponibilità, vedere [Configurare backup in repliche secondarie per un gruppo di disponibilità Always On](../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).

10. Se si usano repliche secondarie leggibili o si crea un gruppo di disponibilità con un cluster di tipo Nessuno per la scalabilità in lettura, è possibile creare un listener selezionando la scheda Listener. Un listener può anche essere aggiunto in un secondo momento. Per creare un listener, scegliere l'opzione **Crea un listener del gruppo di disponibilità** e immettere un nome e una porta TCP/IP, oltre a specificare se usare un indirizzo IP DHCP statico o assegnato automaticamente. Tenere presente che per un gruppo di disponibilità con un tipo di cluster Nessuno l'indirizzo IP deve essere statico e impostato sull'indirizzo IP della replica primaria.

    ![Creare un gruppo di disponibilità 06](./media/sql-server-linux-create-availability-group/image6.png)

11. Se si crea un listener per scenari di repliche leggibili, SSMS 17.3 o versioni successive consente la creazione del routing di sola lettura nella procedura guidata. Il listener può anche essere aggiunto successivamente tramite SSMS o Transact-SQL. Per aggiungere subito il routing di sola lettura:

    a.  Selezionare la scheda Routing di sola lettura.

    b.  Immettere gli URL per le repliche di sola lettura. Questi URL sono simili agli endpoint, ad eccezione del fatto che usano la porta dell'istanza, non l'endpoint.

    c.  Selezionare ogni URL e nella parte inferiore selezionare le repliche leggibili. Per effettuare una selezione multipla, tenere premuto MAIUSC o fare clic e trascinare.

12. Fare clic su **Avanti**.

13. Specificare la modalità di inizializzazione delle repliche secondarie. L'impostazione predefinita prevede l'uso del [seeding automatico](../database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group.md), che richiede lo stesso percorso in tutti i server che fanno parte del gruppo di disponibilità. È anche possibile indicare alla procedura guidata di eseguire un backup, una copia e un ripristino (seconda opzione), di procedere con l'aggiunta se è stato eseguito manualmente il backup, la copia e il ripristino del database nelle repliche (terza opzione) o di aggiungere il database in un secondo momento (ultima opzione). Come accade con i certificati, se si eseguono manualmente i backup e li si copia, le autorizzazioni per i file di backup devono essere impostate sulle altre repliche. Fare clic su **Avanti**.

14. Se nella finestra di dialogo Convalida alcune operazioni sono indicate come non riuscite, ricercarne la causa. Alcuni avvisi sono accettabili e non indicano errori irreversibili, ad esempio nel caso in cui non si crea un listener. Fare clic su **Avanti**.

15. Nella finestra di dialogo Riepilogo fare clic su **Fine**. Verrà avviato il processo di creazione del gruppo di disponibilità.

16. Una volta completata la creazione del gruppo di disponibilità, fare clic su **Chiudi** nella finestra Risultati. Ora è possibile vedere il gruppi di disponibilità nelle viste a gestione dinamica e nella cartella Disponibilità elevata Always On in SSMS.

### <a name="use-transact-sql"></a>Usare Transact-SQL

Questa sezione illustra alcuni esempi di creazione di un gruppo di disponibilità tramite Transact-SQL. Il listener e il routing di sola lettura possono essere configurati dopo la creazione del gruppo di disponibilità. Il gruppo di disponibilità stesso può essere modificato con `ALTER AVAILABILITY GROUP`, ma il tipo di cluster non può essere modificato in [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. Se non si intende creare un gruppo di disponibilità con un cluster di tipo Esterno, è necessario eliminarlo e ricrearlo con un cluster di tipo Nessuno. Per altre informazioni e altre opzioni, vedere i collegamenti seguenti:

-   [CREATE AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/create-availability-group-transact-sql.md)
-   [ALTER AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/alter-availability-group-transact-sql.md)
-   [Configurare il routing di sola lettura per un gruppo di disponibilità (SQL Server)](../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)
-   [Creare o configurare un listener del gruppo di disponibilità (SQL Server)](../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)

#### <a name="example-one---two-replicas-with-a-configuration-only-replica-external-cluster-type"></a>Esempio 1 - Due repliche con una replica di sola configurazione (cluster di tipo Esterno)

Questo esempio illustra come creare un gruppo di disponibilità con due repliche che usa una replica di sola configurazione.

1.  Eseguire il codice di esempio sul nodo che sarà la replica primaria contenente la copia con funzioni di lettura/scrittura complete dei database. Questo esempio usa il seeding automatico.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1' WITH (
       ENDPOINT_URL = N' TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       SEEDING_MODE = AUTOMATIC),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       AVAILABILITY_MODE = CONFIGURATION_ONLY);
       
    GO
    ```
    
2.  In una finestra di query connessa all'altra replica eseguire il codice seguente per aggiungere la replica al gruppo di disponibilità e avviare il processo di seeding dalla replica primaria alla replica secondaria.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3. In una finestra di query connessa alla replica di sola configurazione aggiungere la replica al gruppo di disponibilità.
    
   ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
   ```

#### <a name="example-two---three-replicas-with-read-only-routing-external-cluster-type"></a>Esempio 2 - Tre repliche con routing di sola lettura (cluster di tipo Esterno)

Questo esempio mostra tre repliche complete e illustra il modo in cui il routing di sola lettura può essere configurato nell'ambito della creazione del gruppo di disponibilità iniziale.

1.  Eseguire il codice di esempio sul nodo che sarà la replica primaria contenente la copia con funzioni di lettura/scrittura complete dei database. Questo esempio usa il seeding automatico.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN2.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:1433')),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN2.FullyQualified.Name:1433')),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN3.FullyQualified.Name:1433'))
    LISTENER '<ListenerName>' (WITH IP = ('<IPAddress>', '<SubnetMask>'), Port = 1433);
    
    GO
    ```
    
    Ecco alcuni aspetti da considerare per questa configurazione:
    
    - *AGName* è il nome del gruppo di disponibilità.
    - *DBName* è il nome del database che verrà usato con il gruppo di disponibilità. Può anche essere un elenco di nomi separati da virgole.
    - *ListenerName* è un nome diverso da qualsiasi nodo/server sottostante. Verrà registrato nel DNS insieme a *IPAddress*.
    - *IPAddress* è un indirizzo IP associato a *ListenerName*. È inoltre univoco e non corrisponde ad alcuno dei server/nodi. Le applicazioni e gli utenti finali useranno *ListenerName* o *IPAddress* per connettersi al gruppo di disponibilità.
    - *SubnetMask* è la subnet mask di *IPAddress*, ad esempio 255.255.255.0.

2.  In una finestra di query connessa all'altra replica eseguire il codice seguente per aggiungere la replica al gruppo di disponibilità e avviare il processo di seeding dalla replica primaria alla replica secondaria.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3.  Ripetere il passaggio 2 per la terza replica.

#### <a name="example-three---two-replicas-with-read-only-routing-none-cluster-type"></a>Esempio 3 - Due repliche con routing di sola lettura (cluster di tipo Nessuno)

Questo esempio illustra la creazione di una configurazione a due repliche che usa un cluster di tipo Nessuno. Viene usata per lo scenario di scalabilità in lettura in cui non è previsto alcun failover. Viene creato il listener che è effettivamente la replica primaria, oltre al routing di sola lettura, usando la funzionalità di round robin.

1.  Eseguire il codice di esempio sul nodo che sarà la replica primaria contenente la copia con funzioni di lettura/scrittura complete dei database. Questo esempio usa il seeding automatico.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = NONE)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name: <PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name'.'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:<PortOfInstance>'));
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:<PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL =N'TCP://LinAGN2.FullyQualified.Name:<PortOfInstance>'));
    LISTENER '<ListenerName>' (WITH IP = ('<PrimaryReplicaIPAddress>', '<SubnetMask>'), Port = <PortOfListener>);
    
    GO
    ```
    
    Where
    - *AGName* è il nome del gruppo di disponibilità.
    - *DBName* è il nome del database che verrà usato con il gruppo di disponibilità. Può anche essere un elenco di nomi separati da virgole.
    - *PortOfEndpoint* è il numero di porta usato dall'endpoint creato.
    - *PortOfInstance* è il numero di porta usato dall'istanza di [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].
    - *ListenerName* è un nome diverso da qualsiasi replica sottostante, ma non viene effettivamente usato.
    - *PrimaryReplicaIPAddress* è l'indirizzo IP della replica primaria.
    - *SubnetMask* è la subnet mask di *IPAddress*. Ad esempio, 255.255.255.0.
    
2.  Aggiungere la replica secondaria al gruppo di disponibilità e avviare il seeding automatico.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = NONE);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```

## <a name="create-the-ssnoversion-md-login-and-permissions-for-pacemaker"></a>Creare l'account di accesso di [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] e le autorizzazioni per Pacemaker

Un cluster a disponibilità elevata Pacemaker sottostante a [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] in Linux deve poter accedere all'istanza di [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] e deve avere le autorizzazioni per il gruppo di disponibilità stesso. La procedura seguente crea l'account di accesso e le autorizzazioni associate, oltre a un file che indica a Pacemaker come accedere a [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

1.  In una finestra di query connessa alla prima replica eseguire il codice seguente:

    ```SQL
    CREATE LOGIN PMLogin WITH PASSWORD ='<StrongPassword>';
    
    GO
    
    GRANT VIEW SERVER STATE TO PMLogin;
    
    GO
    
    GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::<AGThatWasCreated> TO PMLogin;
    
    GO
    ```
    
2.  Nel nodo 1 immettere il comando 
    ```bash
    sudo emacs /var/opt/mssql/secrets/passwd
    ```
    
    Verrà aperto l'editor Emacs.
    
3.  Immettere le due righe seguenti nell'editor:

    ```
    PMLogin
    <StrongPassword>
    ```
    
4.  Tenere premuto CTRL e premere X, quindi C per uscire e salvare il file.

5.  Execute 
    ```bash
    sudo chmod 400 /var/opt/mssql/secrets/passwd
    ```
    
    per bloccare il file.

6.  Ripetere i passaggi 1-5 negli altri server che fungeranno da repliche.

## <a name="create-the-availability-group-resources-in-the-pacemaker-cluster-external-only"></a>Creare le risorse dei gruppi di disponibilità nel cluster Pacemaker (solo di tipo Esterno)

Dopo aver creato un gruppo di disponibilità in [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], è necessario creare le risorse corrispondenti in Pacemaker, quando viene specificato un tipo di cluster Esterno. A un gruppo di disponibilità sono associate due risorse: il gruppo di disponibilità stesso e un indirizzo IP. La configurazione della risorsa indirizzo IP è facoltativa se non si usa la funzionalità del listener, ma è tuttavia consigliata.

La risorsa gruppo di disponibilità creata è un tipo speciale di risorsa chiamata clone. Sono presenti copie di questa risorsa in ogni nodo, con una risorsa con funzioni di controllo detta risorsa master. La risorsa master è associata al server che ospita la replica primaria. Le altre risorse ospitano le repliche secondarie (normali o di sola configurazione) e possono essere alzate al livello master in un failover.

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

1.  Per creare la risorsa gruppo di disponibilità, usare la sintassi seguente:

    **Red Hat Enterprise Linux (RHEL) e Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failure-timeout=30s --master meta notify=true
    ```

    >[!NOTE]
    >In RHEL 7.4 è possibile che venga visualizzato un avviso con l'uso di --master. Per evitare questo avviso, usare `sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failover-timeout=30s master notify=true`
   
    **SUSE Linux Enterprise Server (SLES)**
    
    ```bash
    primitive <NameForAGResource> \
    ocf:mssql:ag \
    params ag_name="<AGName>" \
    meta failure-timeout=60s \
    op start timeout=60s \
    op stop timeout=60s \
    op promote timeout=60s \
    op demote timeout=10s \
    op monitor timeout=60s interval=10s \
    op monitor timeout=60s interval=11s role="Master" \
    op monitor timeout=60s interval=12s role="Slave" \
    op notify timeout=60s
    ms ms-ag_cluster <NameForAGResource> \
    meta master-max="1" master-node-max="1" clone-max="3" \
    clone-node-max="1" notify="true" \
    commit
    ```
    
    dove *NameForAGResource* è il nome univoco assegnato a questa risorsa cluster per il gruppo di disponibilità e *AGName* è il nome del gruppo di disponibilità creato.
 
2.  Creare la risorsa indirizzo IP del gruppo di disponibilità che verrà associata alla funzionalità del listener.

    **RHEL e Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForIPResource> ocf:heartbeat:IPaddr2 ip=<IPAddress> cidr_netmask=<Netmask>
    ```

    **SLES**
    
    ```bash
    crm configure \
    primitive <NameForIPResource> \
       ocf:heartbeat:IPaddr2 \
       params ip=<IPAddress> \
          cidr_netmask=<Netmask>
    ```
    
    dove *NameForIPResource* è il nome univoco della risorsa IP e *IPAddress* è l'indirizzo IP statico assegnato alla risorsa. In SLES è necessario fornire anche la netmask. Ad esempio, 255.255.255.0 avrebbe il valore 24 per *Netmask*.
    
3.  Per fare in modo che la risorsa indirizzo IP e la risorsa gruppo di disponibilità siano in esecuzione nello stesso nodo, è necessario configurare un vincolo di condivisione del percorso.

    **RHEL e Ubuntu**
    
    ```bash
    sudo pcs constraint colocation add <NameForIPResource> <NameForAGResource>-master INFINITY with-rsc-role=Master
    ```

    **SLES**
    
    ```bash
    crm configure <NameForConstraint> inf: \
    <NameForIPResource> <NameForAGResource>:Master 
    commit
    ```
    
    dove *NameForIPResource* è il nome della risorsa IP, *NameForAGResource* è il nome della risorsa gruppo di disponibilità e, in SLES, *NameForConstraint* è il nome del vincolo.

4.  Creare un vincolo di ordinamento per fare in modo che la risorsa del gruppo di disponibilità sia attiva e in esecuzione prima dell'indirizzo IP. Mentre il vincolo di condivisione del percorso implica un vincolo di ordinamento, questo lo applica.

    **RHEL e Ubuntu**
    
    ```bash
    sudo pcs constraint order promote <NameForAGResource>-master then start <NameForIPResource>
    ```
    
    **SLES**
    
    ```bash
    crm configure \
    order <NameForConstraint> inf: <NameForAGResource>:promote <NameForIPResource>:start
    commit
    ```
    
    dove *NameForIPResource* è il nome della risorsa IP, *NameForAGResource* è il nome della risorsa gruppo di disponibilità e, in SLES, *NameForConstraint* è il nome del vincolo.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come creare e configurare un gruppo di disponibilità per [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] in Linux. Si è appreso come:
> [!div class="checklist"]
> * Abilitare i gruppi di disponibilità.
> * Creare endpoint e certificati per i gruppi di disponibilità.
> * Usare [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) o Transact-SQL per creare un gruppo di disponibilità.
> * Creare l'account di accesso di [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] e le autorizzazioni per Pacemaker.
> * Creare le risorse del gruppo di disponibilità in un cluster Pacemaker.

Per informazioni sulla maggior parte delle attività di amministrazione dei gruppi di disponibilità, inclusi gli aggiornamenti e il failover, vedere:

> [!div class="nextstepaction"]
> [Gestire gruppi di disponibilità elevata per SQL Server in Linux](sql-server-linux-availability-group-failover-ha.md)

