---
title: Configurare le impostazioni di SQL Server in Linux
description: Questo articolo descrive come usare lo strumento mssql-conf per configurare le impostazioni di SQL Server in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 08/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 06798dff-65c7-43e0-9ab3-ffb23374b322
ms.openlocfilehash: 72d5179d64cda06a95e531d8cbc847b33ce9b92f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237253"
---
# <a name="configure-sql-server-on-linux-with-the-mssql-conf-tool"></a>Configurare SQL Server in Linux con lo strumento mssql conf

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

**mssql-conf** è uno script di configurazione che viene installato con SQL Server 2017 per Red Hat Enterprise Linux, SUSE Linux Enterprise Server e Ubuntu. Modifica il [**file mssql.conf**](#mssql-conf-format) in cui vengono archiviati i valori di configurazione. È possibile usare l'utilità **mssql-conf** per impostare i parametri seguenti:

|Parametro|Descrizione|
|---|---|
| [Agent](#agent) | Abilitare SQL Server Agent. |
| [Regole di confronto](#collation) | Impostare nuove regole di confronto per SQL Server in Linux. |
| [Commenti e suggerimenti degli utenti](#customerfeedback) | Scegliere se in SQL Server è possibile o meno inviare commenti e suggerimenti a Microsoft. |
| [Profilo di Posta elettronica database](#dbmail) | Impostare il profilo predefinito di Posta elettronica database per SQL Server in Linux. |
| [Directory predefinita dati](#datadir) | Cambiare la directory predefinita per i nuovi file di dati (con estensione mdf) di un database di SQL Server. |
| [Directory predefinita log](#datadir) | Cambiare la directory predefinita per i nuovi file di log (con estensione ldf) di un database di SQL Server. |
| [Directory predefinita del database master](#masterdatabasedir) | Cambiare la directory predefinita per il database master e i file di log.|
| [Nome file predefinito del database master](#masterdatabasename) | Modificare il nome dei file del database master. |
| [Directory dump predefinita](#dumpdir) | Cambiare la directory predefinita per i dump della memoria e altri file per la risoluzione dei problemi. |
| [Directory predefinita del log degli errori](#errorlogdir) | Cambiare la directory predefinita per i nuovi file di log degli errori di SQL Server, Analisi profiler predefinita, file XE di sessione di integrità del sistema e file XE di sessioni Hekaton. |
| [Directory di backup predefinita](#backupdir) | Cambiare la directory predefinita per i nuovi file di backup. |
| [Tipo di dump](#coredump) | Scegliere il tipo di file di dump della memoria da raccogliere. |
| [Disponibilità elevata](#hadr) | Abilitare i gruppi di disponibilità. |
| [Directory del controllo locale](#localaudit) | Impostare una directory per aggiungere i file del controllo locale. |
| [Impostazioni locali](#lcid) | Specificare le impostazioni locali da usare in SQL Server. |
| [Limite memoria](#memorylimit) | Impostare il limite di memoria per SQL Server. |
| [Impostazioni di rete](#network) | Impostazioni di rete aggiuntive per SQL Server. |
| [Microsoft Distributed Transaction Coordinator](#msdtc) | Configurare e risolvere i problemi relativi a MSDTC in Linux. |
| [Porta TCP](#tcpport) | Modificare la porta su cui SQL Server è in ascolto delle connessioni. |
| [TLS](#tls) | Configurare Transport Layer Security. |
| [Flag di traccia](#traceflags) | Impostare i flag di traccia che verranno usati dal servizio. |

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

**mssql-conf** è uno script di configurazione che viene installato con [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] per Red Hat Enterprise Linux, SUSE Linux Enterprise Server e Ubuntu. È possibile usare questa utilità per impostare i parametri seguenti:

|Parametro|Descrizione|
|---|---|
| [Agent](#agent) | Abilitare SQL Server Agent |
| [Regole di confronto](#collation) | Impostare nuove regole di confronto per SQL Server in Linux. |
| [Commenti e suggerimenti degli utenti](#customerfeedback) | Scegliere se in SQL Server è possibile o meno inviare commenti e suggerimenti a Microsoft. |
| [Profilo di Posta elettronica database](#dbmail) | Impostare il profilo predefinito di Posta elettronica database per SQL Server in Linux. |
| [Directory predefinita dati](#datadir) | Cambiare la directory predefinita per i nuovi file di dati (con estensione mdf) di un database di SQL Server. |
| [Directory predefinita log](#datadir) | Cambiare la directory predefinita per i nuovi file di log (con estensione ldf) di un database di SQL Server. |
| [Directory file predefinita del database master](#masterdatabasedir) | Cambiare la directory predefinita per i file di database master in un'installazione di SQL esistente.|
| [Nome file predefinito del database master](#masterdatabasename) | Modificare il nome dei file del database master. |
| [Directory dump predefinita](#dumpdir) | Cambiare la directory predefinita per i dump della memoria e altri file per la risoluzione dei problemi. |
| [Directory predefinita del log degli errori](#errorlogdir) | Cambiare la directory predefinita per i nuovi file di log degli errori di SQL Server, Analisi profiler predefinita, file XE di sessione di integrità del sistema e file XE di sessioni Hekaton. |
| [Directory di backup predefinita](#backupdir) | Cambiare la directory predefinita per i nuovi file di backup. |
| [Tipo di dump](#coredump) | Scegliere il tipo di file di dump della memoria da raccogliere. |
| [Disponibilità elevata](#hadr) | Abilitare i gruppi di disponibilità. |
| [Directory del controllo locale](#localaudit) | Impostare una directory per aggiungere i file del controllo locale. |
| [Impostazioni locali](#lcid) | Specificare le impostazioni locali da usare in SQL Server. |
| [Limite memoria](#memorylimit) | Impostare il limite di memoria per SQL Server. |
| [Microsoft Distributed Transaction Coordinator](#msdtc) | Configurare e risolvere i problemi relativi a MSDTC in Linux. |
| [Contratti di licenza con l'utente finale di MLServices](#mlservices-eula) | Accettare i Contratti di licenza con l'utente finale di R e Python per i pacchetti mlservices. Si applica solo a SQL Server 2019.|
| [Impostazioni di rete](#network) | Impostazioni di rete aggiuntive per SQL Server. |
| [outboundnetworkaccess](#mlservices-outbound-access) |Abilitare l'accesso alla rete in uscita per le estensioni R, Python e Java di [mlservices](sql-server-linux-setup-machine-learning.md).|
| [Porta TCP](#tcpport) | Modificare la porta su cui SQL Server è in ascolto delle connessioni. |
| [TLS](#tls) | Configurare Transport Layer Security. |
| [Flag di traccia](#traceflags) | Impostare i flag di traccia che verranno usati dal servizio. |

::: moniker-end

> [!TIP]
> Alcune di queste impostazioni possono anche essere configurate tramite le variabili di ambiente. Per altre informazioni, vedere [Configurare le impostazioni di SQL Server con variabili di ambiente](sql-server-linux-configure-environment-variables.md).

## <a name="usage-tips"></a>Suggerimenti per l'uso

* Per i gruppi di disponibilità Always On e i cluster di dischi condivisi, apportare sempre le stesse modifiche di configurazione in ogni nodo.

* Per lo scenario con cluster di dischi condivisi, non tentare di riavviare il servizio **mssql-server** per applicare le modifiche. SQL Server viene eseguito come applicazione. In alternativa, portare la risorsa offline e quindi riportarla online.

* In questi esempi viene eseguito mssql-conf specificando il percorso completo: **/opt/mssql/bin/mssql-conf**. Se invece si sceglie di passare a tale percorso, eseguire mssql-conf nel contesto della directory corrente: **./mssql-conf**.

## <a name="enable-sql-server-agent"></a><a id="agent"></a> Abilitare SQL Server Agent

L'impostazione **sqlagent.enabled** consente di abilitare [SQL Server Agent](sql-server-linux-run-sql-server-agent-job.md). Per impostazione predefinita, SQL Server Agent è disabilitato. Se **sqlagent.enabled** non è presente nel file di impostazioni mssql.conf, SQL Server internamente presuppone che SQL Server Agent sia disabilitato.

Per modificare questa impostazione, eseguire la procedura descritta di seguito:

1. Abilitare SQL Server Agent:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   ```

2. Riavviare il servizio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

### <a name="set-the-default-database-mail-profile-for-sql-server-on-linux"></a><a id="dbmail"></a> Impostare il profilo predefinito di Posta elettronica database per SQL Server in Linux

**sqlpagent.databasemailprofile** consente di impostare il profilo predefinito di Posta elettronica database per gli avvisi di posta elettronica.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.databasemailprofile <profile_name>
```

### <a name="sql-agent-error-logs"></a><a id="agenterrorlog"></a> Log degli errori di SQL Agent

Le impostazioni **sqlpagent.errorlogfile** e **sqlpagent.errorlogginglevel** consentono di configurare rispettivamente il percorso file di log e il livello di registrazione di SQL Agent. 

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.errorfile <path>
```

I livelli di registrazione di SQL Agent sono valori di maschera di bit che equivalgono a quanto segue:

- 1 = Errori
- 2 = Avvisi
- 4 = Informazioni

Se si vogliono acquisire tutti i livelli, usare `7` come valore.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.errorlogginglevel <level>
```

## <a name="change-the-sql-server-collation"></a><a id="collation"></a> Modificare le regole di confronto di SQL Server

L'opzione **set-collation** consente di modificare il valore delle regole di confronto in una delle regole di confronto supportate.

1. Eseguire innanzitutto il [backup di tutti i database utente](sql-server-linux-backup-and-restore-database.md) nel server.

1. Usare quindi la stored procedure [sp_detach_db](../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md) per scollegare i database utente.

1. Eseguire l'opzione **set-collation** e seguire le istruzioni:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-collation
   ```

1. L'utilità mssql-conf tenterà di impostare il valore specificato per le regole di confronto e di riavviare il servizio. Se si verificano errori, viene eseguito il rollback delle regole di confronto al valore precedente.

1. Ripristinare i backup dei database utente.

Per un elenco delle regole di confronto supportate, eseguire la funzione [sys.fn_helpcollations](../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md): `SELECT Name from sys.fn_helpcollations()`.

## <a name="configure-customer-feedback"></a><a id="customerfeedback"></a> Configurare i commenti e suggerimenti degli utenti

L'impostazione **telemetry.customerfeedback** specifica se in SQL Server è possibile inviare commenti e suggerimenti a Microsoft. Per impostazione predefinita, questo valore è impostato su **true** per tutte le edizioni. Per modificare il valore, eseguire i comandi seguenti:

> [!IMPORTANT]
> Non è possibile disattivare la raccolta dei commenti e suggerimenti degli utenti per le edizioni gratuite di SQL Server, Express e Developer.

1. Eseguire lo script mssql-conf come radice con il comando **set** per **telemetry.customerfeedback**. L'esempio seguente disattiva la raccolta dei commenti e suggerimenti degli utenti specificando **false**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. Riavviare il servizio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

Per altre informazioni, vedere [Commenti e suggerimenti degli utenti per SQL Server in Linux](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md) e [Informativa sulla privacy di SQL Server](../sql-server/sql-server-privacy.md).

## <a name="change-the-default-data-or-log-directory-location"></a><a id="datadir"></a> Modificare il percorso predefinito della directory dei dati o dei log

Le impostazioni **filelocation.defaultdatadir** e **filelocation.defaultlogdir** consentono di modificare il percorso in cui vengono creati i nuovi file di database e di log. Per impostazione predefinita, questo percorso è /var/opt/mssql/data. Per modificare queste impostazioni, eseguire la procedura descritta di seguito:

1. Creare la directory di destinazione per i nuovi file di dati e di log del database. L'esempio seguente crea una nuova directory **/tmp/data**:

   ```bash
   sudo mkdir /tmp/data
   ```

1. Sostituire il proprietario e il gruppo della directory con l'utente **mssql**:

   ```bash
   sudo chown mssql /tmp/data
   sudo chgrp mssql /tmp/data
   ```

1. Usare mssql-conf per modificare la directory dei dati predefinita con il comando **set**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdatadir /tmp/data
   ```

1. Riavviare il servizio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

1. Ora tutti i file di database per i nuovi database creati verranno archiviati in questa nuova posizione. Se si vuole modificare il percorso dei file di log (con estensione ldf) dei nuovi database, è possibile usare il comando "set" seguente:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultlogdir /tmp/log
   ```

1. Questo comando inoltre presuppone che esista una directory /tmp/log, accessibile con l'utente e il gruppo **mssql**.


## <a name="change-the-default-master-database-file-directory-location"></a><a id="masterdatabasedir"></a> Modificare il percorso predefinito della directory dei file del database master

Le impostazioni **filelocation.masterdatafile** e **filelocation.masterlogfile** modificano la posizione in cui il motore di SQL Server cerca i file del database master. Per impostazione predefinita, questo percorso è /var/opt/mssql/data.

Per modificare queste impostazioni, eseguire la procedura descritta di seguito:

1. Creare la directory di destinazione per i nuovi file di log degli errori. L'esempio seguente crea una nuova directory **/tmp/masterdatabasedir**:

   ```bash
   sudo mkdir /tmp/masterdatabasedir
   ```

1. Sostituire il proprietario e il gruppo della directory con l'utente **mssql**:

   ```bash
   sudo chown mssql /tmp/masterdatabasedir
   sudo chgrp mssql /tmp/masterdatabasedir
   ```

1. Usare mssql-conf per modificare la directory predefinita del database master per i file di dati master e di log con il comando **set**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /tmp/masterdatabasedir/master.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterlogfile /tmp/masterdatabasedir/mastlog.ldf
   ```

   > [!NOTE]
   > Oltre a spostare i file di dati master e dei log, viene modificato il percorso predefinito anche per tutti gli altri database di sistema.

1. Arrestare il servizio SQL Server:

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Spostare i file master.mdf e masterlog.ldf:

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /tmp/masterdatabasedir/master.mdf 
   sudo mv /var/opt/mssql/data/mastlog.ldf /tmp/masterdatabasedir/mastlog.ldf
   ```

1. Avviare il servizio SQL Server:

   ```bash
   sudo systemctl start mssql-server
   ```

   > [!NOTE]
   > Se SQL Server non riesce a trovare i file master.mdf e mastlog.ldf nella directory specificata, verrà creata automaticamente in tale directory una copia basata su modelli dei database di sistema e SQL Server verrà avviato correttamente. Tuttavia, i metadati come i database utente, gli account di accesso al server, i certificati del server, le chiavi di crittografia, i processi di SQL Agent o la password di accesso SA precedente non saranno aggiornati nel nuovo database master. È necessario arrestare SQL Server, spostare i file master.mdf e mastlog.ldf nel nuovo percorso specificato e avviare SQL Server per continuare a usare i metadati esistenti.
 
## <a name="change-the-name-of-master-database-files"></a><a id="masterdatabasename"></a> Modificare il nome dei file del database master

Le impostazioni **filelocation.masterdatafile** e **filelocation.masterlogfile** modificano la posizione in cui il motore di SQL Server cerca i file del database master. È anche possibile usare tali impostazioni per modificare il nome del database master e dei file di log. 

Per modificare queste impostazioni, eseguire la procedura descritta di seguito:

1. Arrestare il servizio SQL Server:

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Usare mssql-conf per modificare i nomi previsti dei database master per i file di dati master e dei log con il comando **set**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /var/opt/mssql/data/masternew.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.mastlogfile /var/opt/mssql/data/mastlognew.ldf
   ```

   > [!IMPORTANT]
   > È possibile modificare il nome del database master e dei file di log solo dopo che SQL Server è stato avviato correttamente. Prima dell'esecuzione iniziale, SQL Server prevede che i file siano denominati master.mdf e mastlog.ldf.

1. Modificare il nome dei file di dati e di log del database master 

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /var/opt/mssql/data/masternew.mdf
   sudo mv /var/opt/mssql/data/mastlog.ldf /var/opt/mssql/data/mastlognew.ldf
   ```

1. Avviare il servizio SQL Server:

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="change-the-default-dump-directory-location"></a><a id="dumpdir"></a> Modificare il percorso predefinito della directory dump

L'impostazione **filelocation.defaultdumpdir** modifica il percorso predefinito in cui vengono generati i dump della memoria e di SQL quando si verifica un arresto anomalo. Per impostazione predefinita, questi file vengono generati in /var/opt/mssql/log.

Per configurare il nuovo percorso, usare i comandi seguenti:

1. Creare la directory di destinazione per i nuovi file di dump. L'esempio seguente crea una nuova directory **/tmp/dump**:

   ```bash
   sudo mkdir /tmp/dump
   ```

1. Sostituire il proprietario e il gruppo della directory con l'utente **mssql**:

   ```bash
   sudo chown mssql /tmp/dump
   sudo chgrp mssql /tmp/dump
   ```

1. Usare mssql-conf per modificare la directory dei dati predefinita con il comando **set**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdumpdir /tmp/dump
   ```

1. Riavviare il servizio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="change-the-default-error-log-file-directory-location"></a><a id="errorlogdir"></a> Modificare il percorso predefinito della directory dei file di log degli errori

L'impostazione **filelocation.errorlogfile** modifica il percorso in cui vengono creati i nuovi file di log degli errori, Analisi profiler predefinita, file XE di sessione di integrità del sistema e file XE di sessioni Hekaton. Per impostazione predefinita, questo percorso è /var/opt/mssql/log. La directory in cui è impostato il file del log degli errori SQL diventa la directory dei log predefinita per gli altri log.

Per modificare queste impostazioni:

1. Creare la directory di destinazione per i nuovi file di log degli errori. L'esempio seguente crea una nuova directory **/tmp/logs**:

   ```bash
   sudo mkdir /tmp/logs
   ```

1. Sostituire il proprietario e il gruppo della directory con l'utente **mssql**:

   ```bash
   sudo chown mssql /tmp/logs
   sudo chgrp mssql /tmp/logs
   ```

1. Usare mssql-conf per modificare il nome file predefinito del log degli errori con il comando **set**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.errorlogfile /tmp/logs/errorlog
   ```

1. Riavviare il servizio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

L'impostazione `errorlog.numerrorlogs` consentirà di specificare il numero di log degli errori conservati prima di eseguire il ciclo del log.

## <a name="change-the-default-backup-directory-location"></a><a id="backupdir"></a> Impostare il percorso predefinito della directory di backup

L'impostazione **filelocation.defaultbackupdir** modifica il percorso predefinito in cui vengono generati i file di backup. Per impostazione predefinita, questi file vengono generati in /var/opt/mssql/data.

Per configurare il nuovo percorso, usare i comandi seguenti:

1. Creare la directory di destinazione per i nuovi file di backup. L'esempio seguente crea una nuova directory **/tmp/backup**:

   ```bash
   sudo mkdir /tmp/backup
   ```

1. Sostituire il proprietario e il gruppo della directory con l'utente **mssql**:

   ```bash
   sudo chown mssql /tmp/backup
   sudo chgrp mssql /tmp/backup
   ```

1. Usare mssql-conf per modificare la directory di backup predefinita con il comando "set":

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultbackupdir /tmp/backup
   ```

1. Riavviare il servizio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="specify-core-dump-settings"></a><a id="coredump"></a> Specificare le impostazioni dei core dump

Se si verifica un'eccezione in uno dei processi di SQL Server, SQL Server crea un dump della memoria.

Sono disponibili due opzioni per controllare il tipo di dump della memoria raccolti da SQL Server: **coredump.coredumptype** e **coredump.captureminiandfull**. Sono correlate alle due fasi dell'acquisizione dei core dump. 

La prima fase di acquisizione è controllata dall'impostazione **coredump.coredumptype**, che determina il tipo di file di dump generato durante un'eccezione. La seconda fase è abilitata tramite l'impostazione **coredump.captureminiandfull**. Se **coredump.captureminiandfull** è impostato su true, viene generato il file dump specificato da **coredump.coredumptype** e viene generato anche un secondo minidump. L'impostazione di **coredump.captureminiandfull** su false consente di disabilitare il secondo tentativo di acquisizione.

1. È possibile decidere se acquisire sia i minidump che i dump completi con l'impostazione **coredump.captureminiandfull**.

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.captureminiandfull <true or false>
    ```

    Impostazione predefinita: **false**

1. Specificare il tipo di file di dump con l'impostazione **coredump.coredumptype**.

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.coredumptype <dump_type>
    ```

    Impostazione predefinita: **miniplus**

    Nella tabella seguente sono elencati i valori possibili per **coredump.coredumptype**.

    | Type | Descrizione |
    |-----|-----|
    | **mini** | Mini è il tipo di file di dump più piccolo. Usa le informazioni di sistema di Linux per determinare i thread e i moduli nel processo. Il dump contiene solo i moduli e gli stack di thread dell'ambiente host. Non contiene riferimenti alla memoria indiretta o elementi globali. |
    | **miniplus** | MiniPlus è simile a mini, ma include memoria aggiuntiva. Riconosce gli elementi interni di SQLPAL e l'ambiente host, aggiungendo al dump le aree di memoria seguenti:</br></br> - Vari elementi globali</br> - Tutta la memoria sopra 64 TB</br> - Tutte le aree denominate disponibili in **/proc/$pid/maps**</br> - Memoria indiretta da thread e stack</br> - Informazioni sui thread</br> - TEB e PEB associati</br> - Informazioni sui moduli</br> - Albero VMM e VAD |
    | **filtered** | Filtered usa una struttura basata sulla sottrazione in cui viene inclusa tutta la memoria del processo, a meno che non sia esplicitamente esclusa. La struttura riconosce gli elementi interni di SQLPAL e l'ambiente host, escludendo determinate aree dal dump.
    | **full** | Full è un dump di processo completo che include tutte le aree disponibili in **/proc/$pid/maps**. Non è controllato dall'impostazione **coredump.captureminiandfull**. |

## <a name="high-availability"></a><a id="hadr"></a> Disponibilità elevata

L'opzione **hadr.hadrenabled** abilita i gruppi di disponibilità nell'istanza di SQL Server. Il comando seguente abilita i gruppi di disponibilità impostando **hadr.hadrenabled** su 1. Per rendere effettiva l'impostazione, è necessario riavviare SQL Server.

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled  1
sudo systemctl restart mssql-server
```

Per informazioni su come usare questa opzione con i gruppi di disponibilità, vedere i due argomenti seguenti.

- [Configurare un gruppo di disponibilità Always On per SQL Server in Linux](sql-server-linux-availability-group-configure-ha.md)
- [Configurare un gruppo di disponibilità con scalabilità in lettura per SQL Server in Linux](sql-server-linux-availability-group-configure-rs.md)


## <a name="set-local-audit-directory"></a><a id="localaudit"></a> Impostare la directory del controllo locale

L'impostazione **telemetry.userrequestedlocalauditdirectory** abilita il controllo locale e consente di impostare la directory in cui vengono creati i log di controllo locale.

1. Creare una directory di destinazione per i nuovi log di controllo locale. L'esempio seguente crea una nuova directory **/tmp/audit**:

   ```bash
   sudo mkdir /tmp/audit
   ```

1. Sostituire il proprietario e il gruppo della directory con l'utente **mssql**:

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

1. Eseguire lo script mssql-conf come radice con il comando **set** per **telemetry.userrequestedlocalauditdirectory**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

1. Riavviare il servizio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

Per altre informazioni, vedere [Commenti e suggerimenti degli utenti per SQL Server in Linux](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md).

## <a name="change-the-sql-server-locale"></a><a id="lcid"></a> Modificare le impostazioni locali di SQL Server

L'impostazione **language.lcid** modifica le impostazioni locali di SQL Server in qualsiasi identificatore di lingua (LCID) supportato. 

1. Nell'esempio seguente le impostazioni locali vengono modificate in francese (1036):

   ```bash
   sudo /opt/mssql/bin/mssql-conf set language.lcid 1036
   ```

1. Riavviare il servizio SQL Server per applicare le modifiche:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="set-the-memory-limit"></a><a id="memorylimit"></a> Impostare il limite di memoria

L'impostazione **memory.memorylimitmb** controlla la quantità di memoria fisica (in MB) disponibile per SQL Server. Per impostazione predefinita, corrisponde all'80% della memoria fisica.

1. Eseguire lo script mssql-conf come radice con il comando **set** per **memory.memorylimitmb**. Nell'esempio seguente la memoria disponibile per SQL Server viene impostata su 3,25 GB (3328 MB).

   ```bash
   sudo /opt/mssql/bin/mssql-conf set memory.memorylimitmb 3328
   ```

1. Riavviare il servizio SQL Server per applicare le modifiche:

   ```bash
   sudo systemctl restart mssql-server
   ```

### <a name="additional-memory-settings"></a>Impostazioni aggiuntive per la memoria

Le opzioni seguenti sono disponibili per le impostazioni della memoria.

|Opzione |Descrizione |
|--- |--- |
| memory.disablememorypressure | Utilizzo elevato di memoria disabilitato da SQL Server. I valori possibili sono `true` o `false`. |
| memory.memory_optimized | Abilitare o disabilitare funzionalità ottimizzate per la memoria di SQL Server. Miglioramento del file di memoria permanente, protezione della memoria. I valori possibili sono `true` o `false`. |

## <a name="configure-msdtc"></a><a id="msdtc"></a> Configurare MSDTC

Le impostazioni **network.rpcport** e **distributedtransaction.servertcpport** vengono usate per configurare Microsoft Distributed Transaction Coordinator (MSDTC). Per modificare queste impostazioni, eseguire i comandi seguenti:

1. Eseguire lo script mssql-conf come radice con il comando **set** per "network.rpcport":

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.rpcport <rcp_port>
   ```

2. Configurare quindi l'impostazione "distributedtransaction.servertcpport":

   ```bash
   sudo /opt/mssql/bin/mssql-conf set distributedtransaction.servertcpport <servertcpport_port>
   ```

Oltre a impostare questi valori, è anche necessario configurare il routing e aggiornare il firewall per la porta 135. Per altre informazioni sull'esecuzione di queste operazioni, vedere [Come configurare MSDTC in Linux](sql-server-linux-configure-msdtc.md).

Sono disponibili diverse altre impostazioni per mssql-conf che è possibile usare per monitorare e risolvere i problemi relativi a MSDTC. Nella tabella seguente viene fornita una breve descrizione delle impostazioni disponibili. Per altre informazioni sul relativo uso, vedere i dettagli nell'articolo del supporto tecnico di Windows [Come attivare l'analisi diagnostica per MS DTC in un computer basato su Windows](https://support.microsoft.com/help/926099/how-to-enable-diagnostic-tracing-for-ms-dtc-on-a-windows-based-compute).

| Impostazione di mssql-conf | Descrizione |
|---|---|
| distributedtransaction.allowonlysecurerpccalls | Configurare solo chiamate RPC protette per le transazioni distribuite |
| distributedtransaction.fallbacktounsecurerpcifnecessary | Configurare solo le chiamate RPC di sicurezza per le transazioni distribuite |
| distributedtransaction.maxlogsize | Dimensioni del file di log delle transazioni DTC in MB. Il valore predefinito è 64 MB |
| distributedtransaction.memorybuffersize | Dimensioni del buffer circolare in cui sono archiviate le tracce. La dimensione è in MB e il valore predefinito è 10 MB |
| distributedtransaction.servertcpport | Porta server RPC MSDTC |
| distributedtransaction.trace_cm | Tracce della gestione connessione |
| distributedtransaction.trace_contact | Tracce di pool di contatti e contatti |
| distributedtransaction.trace_gateway | Tracce dell'origine gateway |
| distributedtransaction.trace_log | Traccia dei log |
| distributedtransaction.trace_misc | Tracce che non possono essere classificate nelle altre categorie |
| distributedtransaction.trace_proxy | Tracce generate nel proxy MSDTC |
| distributedtransaction.trace_svc | Tracce dell'avvio di servizi e file con estensione exe |
| distributedtransaction.trace_trace | Infrastruttura di traccia |
| distributedtransaction.trace_util | Tracce delle routine dell'utilità chiamate da più posizioni |
| distributedtransaction.trace_xa | Origine di traccia della gestione transazioni XA (XATM) |
| distributedtransaction.tracefilepath | Cartella in cui archiviare i file di traccia |
| distributedtransaction.turnoffrpcsecurity | Abilitare o disabilitare la sicurezza RPC per le transazioni distribuite |

::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="accept-mlservices-eulas"></a><a id="mlservices-eula"></a> Accettare i contratti di licenza con l'utente finale di MLServices

Per [aggiungere pacchetti R o Python di Machine Learning](sql-server-linux-setup-machine-learning.md) al motore di database, è necessario accettare le condizioni di licenza per le distribuzioni open source di R e Python. Nella tabella seguente sono enumerati tutti i comandi o le opzioni disponibili correlati ai contratti di licenza con l'utente finale di MLServices. Lo stesso parametro per il contratto di licenza con l'utente finale viene usato per R e Python, a seconda di ciò che è stato installato.

```bash
# For all packages: database engine and mlservices
# Setup prompts for mlservices EULAs, which you need to accept
sudo /opt/mssql/bin/mssql-conf setup

# Add R or Python to an existing installation
sudo /opt/mssql/bin/mssql-conf setup accept-eula-ml

# Alternative valid syntax
# Adds the EULA section to the INI and sets acceptulam to yes
sudo /opt/mssql/bin/mssql-conf set EULA accepteulaml Y

# Rescind EULA acceptance and removes the setting
sudo /opt/mssql/bin/mssql-conf unset EULA accepteulaml
```

È anche possibile aggiungere l'accettazione del contratto di licenza con l'utente finale direttamente al [file mssql.conf](#mssql-conf-format):

```ini
[EULA]
accepteula = Y
accepteulaml = Y
```
:::moniker-end
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="enable-outbound-network-access"></a><a id="mlservices-outbound-access"></a> Abilitare l'accesso alla rete in uscita

Per impostazione predefinita, l'accesso alla rete in uscita per le estensioni R, Python e Java nella funzionalità [SQL Server Machine Learning Services](sql-server-linux-setup-machine-learning.md) è disabilitato. Per abilitare le richieste in uscita, impostare la proprietà booleana "outboundnetworkaccess" usando mssql-conf.

Dopo aver impostato la proprietà, riavviare il servizio Launchpad di SQL Server per leggere i valori aggiornati dal file INI. Un messaggio di riavvio avverte ogni volta che viene modificata un'impostazione relativa all'estendibilità.

```bash
# Adds the extensibility section and property.
# Sets "outboundnetworkaccess" to true.
# This setting is required if you want to access data or operations off the server.
sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1

# Turns off network access but preserves the setting
sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 0

# Removes the setting and rescinds network access
sudo /opt/mssql/bin/mssql-conf unset extensibility.outboundnetworkaccess
```

È anche possibile aggiungere "outboundnetworkaccess" direttamente al [file mssql.conf](#mssql-conf-format):

```ini
[extensibility]
outboundnetworkaccess = 1
```
:::moniker-end

## <a name="change-the-tcp-port"></a><a id="tcpport"></a> Modificare la porta TCP

L'impostazione **network.tcpport** consente di modificare la porta TCP su cui SQL Server è in ascolto delle connessioni. Per impostazione predefinita, tale porta è impostata su 1433. Per modificare la porta, eseguire i comandi seguenti:

1. Eseguire lo script mssql-conf come radice con il comando "set" per "network.tcpport":

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.tcpport <new_tcp_port>
   ```

2. Riavviare il servizio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

3. Ora, al momento della connessione a SQL Server, è necessario specificare la porta personalizzata con una virgola (,) dopo il nome host o l'indirizzo IP. Ad esempio, per connettersi a SQLCMD, è necessario usare il comando seguente:

   ```bash
   sqlcmd -S localhost,<new_tcp_port> -U test -P test
   ```

## <a name="specify-tls-settings"></a><a id="tls"></a> Specificare le impostazioni TLS

Le opzioni seguenti consentono di configurare TLS per un'istanza di SQL Server eseguita in Linux.

|Opzione |Descrizione |
|--- |--- |
|**network.forceencryption** |Se è 1, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] impone la crittografia di tutte le connessioni. Per impostazione predefinita, questa opzione è 0. |
|**network.tlscert** |Percorso assoluto del file di certificato usato da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per TLS. Esempio:   `/etc/ssl/certs/mssql.pem` Il file del certificato deve essere accessibile dall'account mssql. Microsoft consiglia di limitare l'accesso al file usando `chown mssql:mssql <file>; chmod 400 <file>`. |
|**network.tlskey** |Percorso assoluto del file di chiave privata usato da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per TLS. Esempio:  `/etc/ssl/private/mssql.key` Il file del certificato deve essere accessibile dall'account mssql. Microsoft consiglia di limitare l'accesso al file usando `chown mssql:mssql <file>; chmod 400 <file>`. |
|**network.tlsprotocols** |Elenco delimitato da virgole dei protocolli TLS consentiti da SQL Server. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tenta sempre di negoziare il protocollo più sicuro consentito. Se un client non supporta alcun protocollo consentito, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] rifiuta il tentativo di connessione.  Per compatibilità, tutti i protocolli supportati sono consentiti per impostazione predefinita (1.2, 1.1, 1.0).  Se i client supportano TLS 1.2, Microsoft consiglia di consentire solo TLS 1.2. |
|**network.tlsciphers** |Specifica le crittografie consentite da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per TLS. Questa stringa deve essere formattata in base al [formato per l'elenco di crittografie di OpenSSL](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html). In generale, non è necessario modificare questa opzione. <br /> Per impostazione predefinita, sono consentite le crittografie seguenti: <br /> `ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA` |
| **network.kerberoskeytabfile** |Percorso del file keytab Kerberos |

Per un esempio di uso delle impostazioni TLS, vedere [Crittografia delle connessioni a SQL Server in Linux](sql-server-linux-encrypted-connections.md).

## <a name="network-settings"></a><a id="network"></a> Impostazioni di rete

Vedere [Esercitazione: Usare l'autenticazione di Active Directory con SQL Server in Linux](sql-server-linux-active-directory-authentication.md) per ottenere informazioni complete sull'uso dell'autenticazione di AD con SQL Server in Linux.

Le opzioni seguenti sono impostazioni di rete aggiuntive che possono essere configurate con `mssql-conf`.

|Opzione |Descrizione |
|--- |--- |
| network.disablesssd | Disabilitare l'esecuzione di query su SSSD per ottenere informazioni sull'account AD e usare le chiamate LDAP per impostazione predefinita. I valori possibili sono `true` o `false`. |
| network.enablekdcfromkrb5conf | Consentire la ricerca di informazioni KDC da krb5.conf. I valori possibili sono `true` o `false`. |
| network.forcesecureldap | Forzare l'uso di LDAPS per contattare il controller di dominio. I valori possibili sono `true` o `false`. |
| network.ipaddress | Indirizzo IP per le connessioni in ingresso. |
| network.kerberoscredupdatefrequency | Tempo in secondi tra i controlli per le credenziali Kerberos che devono essere aggiornate. Il valore è un numero intero.|
| network.privilegedadaccount | Utente AD con privilegi da usare per l'autenticazione AD. Il valore è `<username>`. Per altre informazioni, vedere [Esercitazione: Usare l'autenticazione di Azure Active Directory con SQL Server in Linux](sql-server-linux-active-directory-authentication.md#spn)|
| uncmapping | Mappa un percorso UNC a un percorso locale. Ad esempio: `sudo /opt/mssql/bin/mssql-conf set uncmapping //servername/sharename /tmp/folder`. |

## <a name="enabledisable-traceflags"></a><a id="traceflags"></a> Abilitare o disabilitare i flag di traccia

L'opzione **traceflag** consente di abilitare o disabilitare i flag di traccia per l'avvio del servizio SQL Server. Per abilitare/disabilitare un traceflag, usare i comandi seguenti:

1. Abilitare un flag di traccia usando il comando seguente. Ad esempio, per il flag di traccia 1234:

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 on
   ```

1. È possibile abilitare più flag di traccia specificandoli separatamente:

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 2345 3456 on
   ```

1. In modo analogo, è possibile disabilitare uno o più flag di traccia abilitati specificandoli e aggiungendo il parametro **off**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 2345 3456 off
   ```

1. Riavviare il servizio SQL Server per applicare le modifiche:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="remove-a-setting"></a>Rimuovere un'impostazione

Per annullare le impostazioni effettuate con `mssql-conf set`, chiamare **mssql-conf** con l'opzione `unset` e il nome dell'impostazione. In questo modo l'impostazione viene cancellata, ripristinandone il valore predefinito.

1. Nell'esempio seguente viene cancellata l'opzione **network.tcpport**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf unset network.tcpport
   ```

1. Riavviare il servizio SQL Server.

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="view-current-settings"></a>Visualizzare le impostazioni correnti

Per visualizzare le impostazioni configurate, eseguire il comando seguente per restituire il contenuto del file **mssql.conf**:

```bash
sudo cat /var/opt/mssql/mssql.conf
```

Le impostazioni non visualizzate in questo file vengono usati i valori predefiniti. Nella sezione seguente viene fornito un file **mssql.conf** di esempio.


## <a name="mssqlconf-format"></a><a id="mssql-conf-format"></a> Formato di mssql.conf

Il file **/var/opt/mssql/mssql.conf** seguente offre un esempio per ogni impostazione. È possibile usare questo formato per apportare manualmente modifiche al file **mssql.conf** in base alle esigenze. Se si modifica manualmente il file, è necessario riavviare SQL Server per applicare le modifiche. Per usare il file **mssql.conf** con Docker, è necessario che Docker [salvi in modo permanente i dati](./sql-server-linux-docker-container-deployment.md). Aggiungere un file **mssql.conf** completo alla directory host, quindi eseguire il contenitore. Un esempio è disponibile in [Commenti e suggerimenti degli utenti](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

```ini
[EULA]
accepteula = Y

[coredump]
captureminiandfull = true
coredumptype = full

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

```ini
[EULA]
accepteula = Y
accepteulaml = Y

[coredump]
captureminiandfull = true
coredumptype = full

[distributedtransaction]
servertcpport = 51999

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
rpcport = 13500
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

::: moniker-end

## <a name="next-steps"></a>Passaggi successivi

Per usare le variabili di ambiente per apportare alcune di queste modifiche di configurazione, vedere [Configurare le impostazioni di SQL Server con variabili di ambiente](sql-server-linux-configure-environment-variables.md).

Per altri strumenti e scenari di gestione, vedere [Gestire SQL Server in Linux](sql-server-linux-management-overview.md).