---
title: Creare un gruppo di disponibilità indipendente dal dominio
description: Procedura per la creazione di un gruppo di disponibilità che usa un cluster di gruppi di lavoro. In questo modo si consente a SQL Server 2016 (e versioni successive) di distribuire un gruppo di disponibilità Always On in un cluster WSFC che non richiede Active Directory Domain Services e quindi non richiede che ogni server faccia parte dello stesso dominio.
ms.custom: seodec18
ms.date: 09/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], domain independent
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 060e9e635aa7be4dd3c00e11b0f3a36073cdaa54
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100349287"
---
# <a name="create-a-domain-independent-availability-group"></a>Creare un gruppo di disponibilità indipendente dal dominio
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

I gruppi di disponibilità AlwaysOn richiedono un cluster di failover di Windows Server (WSFC) sottostante. Per distribuire un cluster WSFC tramite Windows Server 2012 R2 è necessario che i server che fanno parte di un cluster WSFC, anche detti nodi, siano aggiunti allo stesso dominio. Per altre informazioni su Active Directory Domain Services, vedere [questo articolo](/previous-versions/windows/it-pro/windows-server-2003/cc759073(v=ws.10)).

La dipendenza di WSFC e Active Directory Domain Services è più complessa di quella distribuita in precedenza con una configurazione di mirroring del database, dato che è possibile distribuire il mirroring del database tra più data center mediante i certificati, senza tale dipendenza.  Un gruppo di disponibilità tradizionale che comprende più di un data center richiede che tutti i server siano aggiunti allo stesso dominio di Active Directory. Non è compatibile con domini diversi, anche se si tratta di domini trusted. Tutti i server devono essere nodi dello stesso cluster WSFC. La figura seguente illustra questa configurazione. Anche SQL Server 2016 ha gruppi di disponibilità distribuiti, con cui è possibile ottenere questo risultato in modo diverso.


![Cluster WSFC che comprendono due data center connessi allo stesso dominio][1]

Con Windows Server 2012 R2 è stato introdotto un [cluster scollegato da Active Directory](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970(v=ws.11)), un particolare tipo di cluster di failover di Windows Server che può essere usato con i gruppi di disponibilità. Per questo tipo di cluster WSFC i nodi devono comunque essere aggiunti allo stesso dominio di Active Directory, ma in questo caso il cluster WSFC usa il DNS anziché il dominio. Dato che è ancora previsto l'uso di un dominio, un cluster scollegato da Active Directory non offre ancora un'esperienza completamente senza dominio.

Con Windows Server 2016 è stato introdotto un nuovo tipo di cluster di failover di Windows Server basato sull'elemento essenziale del cluster scollegato da Active Directory: un cluster di gruppi di lavoro. Il cluster di gruppi di lavoro permette a SQL Server 2016 di distribuire un gruppo di disponibilità in un cluster WSFC che non richiede Active Directory Domain Services. SQL Server richiede l'uso di certificati per la sicurezza degli endpoint, come avviene per lo scenario di mirroring del database.  Questo tipo di gruppo di disponibilità è detto indipendente dal dominio. La distribuzione di un gruppo di disponibilità con un cluster di gruppi di lavoro sottostante supporta le combinazioni seguenti per i nodi che costituiranno il cluster WSFC:
- Nessun nodo è aggiunto a un dominio.
- Tutti i nodi sono aggiunti a domini diversi.
- I nodi sono misti, con una combinazione di nodi aggiunti a un dominio e nodi non aggiunti a un dominio.

La figura seguente mostra un esempio di gruppo di disponibilità indipendente dal dominio in cui i nodi del data center 1 sono aggiunti a un dominio, mentre i nodi del data center 2 usano solo il DNS. In questo caso è possibile impostare il suffisso DNS in tutti i server che costituiranno i nodi del cluster WSFC. Ogni applicazione e ogni server che accedono al gruppo di disponibilità devono visualizzare le stesse informazioni DNS.


![Cluster di gruppi di lavoro con due nodi aggiunti a un dominio][2]

L'uso dei gruppi di disponibilità indipendenti dal dominio non si limita a scenari di ripristino di emergenza o multisito. Possono essere distribuiti in un data center singolo ed essere usati con un [gruppo di disponibilità di base](basic-availability-groups-always-on-availability-groups.md), anche detto gruppo di disponibilità Standard Edition, per fornire un'architettura simile a quella che si poteva ottenere con il mirroring del database con certificati, come illustrato.


![Panoramica generale di un gruppo di disponibilità Standard Edition][3]

Per la distribuzione di un gruppo di disponibilità indipendente dal dominio occorre tenere presente alcuni fattori:
- Gli unici tipi di controllo disponibili per l'uso con il quorum sono disco e [cloud](/windows-server/failover-clustering/deploy-cloud-witness), introdotto con Windows Server 2016. Il tipo di controllo disco risulta problematico, perché molto probabilmente il gruppo di disponibilità non fa uso di dischi condivisi.
- La variante cluster di gruppi di lavoro sottostante di un cluster WSFC può essere creata solo tramite PowerShell, ma successivamente può essere gestita con Gestione cluster di failover.
- Se è richiesto Kerberos, è necessario distribuire un cluster WSFC standard collegato a un dominio di Active Directory e, probabilmente, un gruppo di disponibilità indipendente dal dominio non è la scelta ideale.
- È possibile configurare un listener, ma per essere utilizzabile deve essere registrato nel DNS. Come indicato in precedenza, non è previsto alcun supporto Kerberos per il listener.
- Le applicazioni che si connettono a SQL Server devono usare principalmente l'autenticazione di SQL Server, dato che i domini potrebbero non esistere o non essere configurati per l'interazione. 
- I certificati verranno usati nella configurazione del gruppo di disponibilità.

## <a name="set-and-verify-the-dns-suffix-on-all-replica-servers"></a>Impostare e verificare il suffisso DNS in tutti i server di replica

Per un cluster di gruppi di lavoro di un gruppo di disponibilità indipendente dal dominio è necessario un suffisso DNS comune. Per impostare e verificare il suffisso DNS in ogni istanza di Windows Server che ospiterà una replica per il gruppo di disponibilità, seguire questa procedura:

1. Usando i tasti di scelta rapida tasto WINDOWS + X, selezionare Sistema.
2. Se il nome del computer e il nome completo del computer sono uguali, il suffisso DNS non è stato impostato. Ad esempio, se il nome del computer è ALLAN, il valore del nome completo del computer non può essere semplicemente ALLAN, ma dovrebbe essere simile ad ALLAN.SQLHA.LAB. SQLHA.LAB è il suffisso DNS. Il valore del gruppo di lavoro deve essere GRUPPO DI LAVORO. Se è necessario impostare il suffisso DNS, selezionare Cambia impostazioni.
3. Nella finestra di dialogo Proprietà del sistema fare clic su Modifica nella scheda Nome computer.
4. Nella finestra di dialogo Cambiamenti dominio/nome computer fare clic su Altro.
5. Nella finestra di dialogo Suffisso DNS e nome NetBIOS del computer immettere il suffisso DNS comune come suffisso DNS primario. 
6. Fare clic su OK per chiudere la finestra di dialogo Suffisso DNS e nome NetBIOS del computer.
7. Fare clic su OK per chiudere la finestra di dialogo Cambiamenti dominio/nome computer.
8. Verrà richiesto di riavviare il server per rendere effettive le modifiche. Fare clic su OK per chiudere la finestra di dialogo Cambiamenti dominio/nome computer.
9. Fare clic su Chiudi per chiudere la finestra di dialogo Proprietà del sistema.
10. Verrà richiesto di riavviare. Se non si vuole riavviare immediatamente, fare clic su Riavvia in seguito, in caso contrario fare clic su Riavvia.
11. Dopo aver riavviato il server, assicurarsi che il suffisso DNS comune sia configurato esaminando di nuovo Sistema.

![Configurazione del suffisso DNS riuscita][4]

  > [!NOTE]
  > Se si usano più subnet e si dispone di un DNS statico, sarà necessario predisporre un processo per aggiornare il record DNS associato al listener prima di eseguire un failover. In caso contrario, il nome di rete non verrà portato online.

## <a name="create-a-domain-independent-availability-group"></a>Creare un gruppo di disponibilità indipendente dal dominio

Attualmente non è possibile creare un gruppo di disponibilità indipendente dal dominio interamente con SQL Server Management Studio. Anche se la procedura per la creazione del gruppo di disponibilità indipendente dal dominio è sostanzialmente identica alla creazione di un gruppo di disponibilità normale, alcuni aspetti come la creazione dei certificati sono possibili solo con Transact-SQL. L'esempio seguente presuppone la configurazione di un gruppo di disponibilità con due repliche, una primaria e una secondaria. 

1. Seguendo la procedura illustrata in [questo post di blog](https://techcommunity.microsoft.com/t5/Failover-Clustering/Workgroup-and-Multi-domain-clusters-in-Windows-Server-2016/ba-p/372059), distribuire un cluster di gruppi di lavoro composto da tutti i server che faranno parte del gruppo di disponibilità. Prima di configurare il cluster di gruppi di lavoro, assicurarsi che il suffisso DNS comune sia già configurato.
2. [Abilitare la funzionalità Gruppi di disponibilità AlwaysOn](./enable-and-disable-always-on-availability-groups-sql-server.md) in ogni istanza che farà parte del gruppo di disponibilità. Sarà necessario riavviare ogni istanza di SQL Server.
3. Per ogni istanza che ospiterà la replica primaria è necessaria una chiave master del database. Se non esiste già una chiave master, eseguire questo comando:

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Strong Password';
   GO
   ```

4. Nell'istanza che fungerà da replica primaria creare il certificato che verrà usto per le connessioni in ingresso nelle repliche secondarie e per proteggere l'endpoint nella replica primaria.

   ```sql
   CREATE CERTIFICATE InstanceA_Cert 
   WITH SUBJECT = 'InstanceA Certificate';
   GO
   ``` 

5. Eseguire il backup del certificato. È anche possibile migliorare ulteriormente la protezione con una chiave privata. Questo esempio non fa uso di una chiave privata.

   ```sql
   BACKUP CERTIFICATE InstanceA_Cert 
   TO FILE = 'Backup_path\InstanceA_Cert.cer';
   GO
   ```

6. Ripetere i passaggi 4 e 5 per creare ed eseguire il backup dei certificati per ogni replica secondaria, usando nomi appropriati per i certificati, ad esempio InstanceB_Cert.
7. Nella replica primaria è necessario creare un account di accesso per ogni replica secondaria del gruppo di disponibilità. A questo account di accesso verrà concessa l'autorizzazione a connettersi all'endpoint usato dal gruppo di disponibilità indipendente dal dominio. Ad esempio, per una replica denominata InstanceB:

   ```sql
   CREATE LOGIN InstanceB_Login WITH PASSWORD = 'Strong Password';
   GO
   ```

8. In ogni replica secondaria creare un account di accesso per la replica primaria. A questo account di accesso verrà concessa l'autorizzazione a connettersi all'endpoint. Ad esempio, per una replica denominata InstanceA:

   ```sql
   CREATE LOGIN InstanceA_Login WITH PASSWORD = 'Strong Password';
   GO
   ```

9. In tutte le istanze occorre creare un utente per ogni account di accesso creato. Questo verrà usato durante il ripristino dei certificati. Ad esempio, per creare un utente per la replica primaria:

   ```sql
   CREATE USER InstanceA_User FOR LOGIN InstanceA_Login;
   GO
   ```

10. Per qualsiasi replica che potrebbe essere primaria, creare un account di accesso e un utente in tutte le relative repliche secondarie.
11. Ripristinare in ogni istanza i certificati per le altre istanze per cui sono stati creati un account di accesso e un utente. Nella replica primaria ripristinare tutti i certificati delle repliche secondarie. Ripristinare il certificato della replica primaria in ogni replica secondaria e anche in qualsiasi altra replica che potrebbe essere primaria. Ad esempio:

   ```sql
   CREATE CERTIFICATE [InstanceB_Cert]
   AUTHORIZATION InstanceB_User
   FROM FILE = 'Restore_path\InstanceB_Cert.cer'
   ```

12. Creare l'endpoint che verrà usato dal gruppo di disponibilità in ogni istanza che sarà una replica. Per i gruppi di disponibilità, l'endpoint deve essere di tipo DATABASE_MIRRORING. L'endpoint usa il certificato creato nel passaggio 4 per l'istanza di autenticazione. Di seguito è riportata la sintassi di esempio per creare un endpoint che usa un certificato. Usare il metodo di crittografia appropriato e altre opzioni specifiche dell'ambiente. Per altre informazioni sulle opzioni disponibili, vedere [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md).

   ```sql
   CREATE ENDPOINT DIAG_EP
   STATE = STARTED
   AS TCP (   
    LISTENER_PORT = 5022,
    LISTENER_IP = ALL
         )
   FOR DATABASE_MIRRORING (
    AUTHENTICATION = CERTIFICATE InstanceX_Cert,
    ROLE = ALL
         )
   ```

13. Assegnare diritti a ogni account di accesso creato nell'istanza nel passaggio 8 per potersi connettere all'endpoint. 

   ```sql
   GRANT CONNECT ON ENDPOINT::DIAG_EP TO [InstanceX_Login];
   GO
   ```

14. Dopo aver configurato i certificati sottostanti e la sicurezza dell'endpoint, creare il gruppo di disponibilità usando il metodo preferito. È consigliabile eseguire manualmente il backup, copiare e ripristinare il backup usato per inizializzare la replica secondaria oppure usare il [seeding automatico](automatically-initialize-always-on-availability-group.md). L'uso della procedura guidata per inizializzare le repliche secondarie comporta l'uso di file Server Message Block (SMB), che potrebbero non funzionare con un cluster di gruppi di lavoro non aggiunto a un dominio.
15. Se si crea un listener, assicurarsi che il nome e il relativo indirizzo IP siano registrati nel DNS.

### <a name="next-steps"></a>Passaggi successivi 

- [Usare la Creazione guidata Gruppo di disponibilità (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

- [Usare la finestra di dialogo Nuovo gruppo di disponibilità (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
- [Creare un gruppo di disponibilità con Transact-SQL](create-an-availability-group-transact-sql.md)

<!--Image references-->
[1]: ./media/diag-wsfc-two-data-centers-same-domain.png
[2]: ./media/diag-workgroup-cluster-two-nodes-joined.png
[3]: ./media/diag-high-level-view-ag-standard-edition.png
[4]: ./media/diag-successful-dns-suffix.png
