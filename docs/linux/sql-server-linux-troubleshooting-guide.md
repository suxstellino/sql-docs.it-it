---
title: Risolvere i problemi di SQL Server in Linux
description: Informazioni su come risolvere i problemi di Microsoft SQL Server in esecuzione in Linux o in un contenitore Docker. Informazioni su dove trovare dettagli sulle funzionalità supportate e sulle limitazioni note.
author: VanMSFT
ms.author: vanto
ms.date: 05/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 99636ee8-2ba6-4316-88e0-121988eebcf9S
ms.openlocfilehash: 56961a0dd9f57085ebe2d937668fe32c7e28dc73
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346221"
---
# <a name="troubleshoot-sql-server-on-linux"></a>Risolvere i problemi di SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo documento descrive come risolvere i problemi di Microsoft SQL Server in esecuzione in Linux o in un contenitore Docker. Quando si risolvono i problemi di SQL Server in Linux, ricordarsi di esaminare le funzionalità supportate e le limitazioni note nelle [Note sulla versione di SQL Server in Linux](sql-server-linux-release-notes.md).

> [!TIP]
> Per le risposte alle domande frequenti, vedere [Domande frequenti su SQL Server in Linux](sql-server-linux-faq.md).

## <a name="troubleshoot-connection-failures"></a><a id="connection"></a> Risolvere i problemi relativi agli errori di connessione
In caso di difficoltà di connessione a SQL Server Linux, è necessario eseguire alcuni controlli.

- Se non è possibile connettersi in locale usando **localhost**, provare a usare invece l'indirizzo IP 127.0.0.1. **Localhost** potrebbe non essere correttamente associato a questo indirizzo.

- Verificare che il nome del server o l'indirizzo IP sia raggiungibile dal computer client.

   > [!TIP]
   > Per trovare l'indirizzo IP del computer Ubuntu, è possibile eseguire il comando ifconfig come nell'esempio seguente:
   >
   >   ```bash
   >   sudo ifconfig eth0 | grep 'inet addr'
   >   ```
   > Per Red Hat, è possibile usare ip addr come nell'esempio seguente:
   >
   >   ```bash
   >   sudo ip addr show eth0 | grep "inet"
   >   ```
   > Un'eccezione a questa tecnica è relativa alle macchine virtuali di Azure. Per le macchine virtuali di Azure, [trovare l'IP pubblico per la macchina virtuale nel portale di Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#connect).

- Se applicabile, verificare di avere aperto la porta di SQL Server (per impostazione predefinita, 1433) sul firewall.

- Per le macchine virtuali di Azure, controllare di avere una [regola del gruppo di sicurezza di rete per la porta predefinita di SQL Server](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#remote).

- Verificare che il nome utente e la password non contengano errori di digitazione o spazi aggiuntivi o maiuscole e minuscole non corrette.

- Provare a impostare in modo esplicito il protocollo e il numero di porta con il nome del server, come nell'esempio seguente: **tcp:servername,1433**.

- Problemi di connettività di rete possono anche causare errori di connessione e timeout. Dopo aver verificato le informazioni di connessione e la connettività di rete, riprovare a stabilire la connessione.

## <a name="manage-the-sql-server-service"></a>Gestire il servizio SQL Server

Le sezioni seguenti illustrano come avviare, arrestare, riavviare e controllare lo stato del servizio SQL Server.

### <a name="manage-the-mssql-server-service-in-red-hat-enterprise-linux-rhel-and-ubuntu"></a>Gestire il servizio mssql-server in Red Hat Enterprise Linux (RHEL) e Ubuntu 

Controllare lo stato del servizio SQL Server usando questo comando:

   ```bash
   sudo systemctl status mssql-server
   ```

È possibile arrestare, avviare o riavviare il servizio SQL Server in base alle esigenze usando i comandi seguenti:

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

### <a name="manage-the-execution-of-the-mssql-docker-container"></a>Gestire l'esecuzione del contenitore Docker mssql

È possibile ottenere lo stato e l'ID dell'ultimo contenitore Docker di SQL Server creato eseguendo il comando seguente (l'ID è nella colonna **CONTAINER ID**):

   ```bash
   sudo docker ps -l
   ```
   
È possibile arrestare o riavviare il servizio SQL Server in base alle esigenze usando i comandi seguenti:
   
   ```bash
   sudo docker stop <container ID>
   sudo docker restart <container ID>
   ```

> [!TIP]
> Per altri suggerimenti sulla risoluzione dei problemi relativi a Docker, vedere [Risoluzione dei problemi dei contenitori Docker di SQL Server](./sql-server-linux-docker-container-troubleshooting.md).

## <a name="access-the-log-files"></a>Accedere ai file di log
   
Il motore di SQL Server accede al file /var/opt/mssql/log/errorlog nelle installazioni sia di Linux che di Docker. Per esplorare questa directory, è necessario essere in modalità "utente con privilegi avanzati".

Il programma di installazione accede a /var/opt/mssql/setup-<timestamp indicante l'ora di installazione> È possibile esplorare i file errorlog con qualsiasi strumento compatibile con UTF-16, ad esempio "vim" o "cat", come di seguito: 

   ```bash
   sudo cat errorlog
   ```

Se si preferisce, è anche possibile convertire i file in UTF-8 per leggerli con "more" o "less" con il comando seguente:
   
   ```bash
   sudo iconv -f UTF-16LE -t UTF-8 <errorlog> -o <output errorlog file>
   ```
## <a name="extended-events"></a>Eventi estesi

È possibile eseguire query sugli eventi estesi tramite un comando SQL.  Per altre informazioni sugli eventi estesi, vedere [qui](../relational-databases/extended-events/extended-events.md):

## <a name="crash-dumps"></a>Dump di arresto anomalo 

Cercare i dump nella directory log in Linux. Cercare nella directory /var/opt/mssql/log i core dump di Linux (estensione tar.gz2) o i minidump di SQL (estensione mdmp)

Per i core dump 
   ```bash
   sudo ls /var/opt/mssql/log | grep .tar.gz2 
   ```

Per i dump di SQL 
   ```bash
   sudo ls /var/opt/mssql/log | grep .mdmp 
   ```
   
## <a name="start-sql-server-in-minimal-configuration-or-in-single-user-mode"></a>Avviare SQL Server con la modalità configurazione minima o utente singolo

### <a name="start-sql-server-in-minimal-configuration-mode"></a>Avviare SQL Server con la modalità di configurazione minima
È utile nel caso in cui l'impostazione di un valore di configurazione, ad esempio un'allocazione eccessiva di memoria, abbia impedito l'avvio del server.
  
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -f
   ```

### <a name="start-sql-server-in-single-user-mode"></a>Avviare SQL Server in modalità utente singolo
In alcuni casi potrebbe essere necessario avviare un'istanza di SQL Server in modalità utente singolo usando l'opzione di avvio -m. Ad esempio, può risultare utile modificare le opzioni di configurazione del server oppure recuperare un database master o un altro database di sistema danneggiato. Ad esempio, può risultare utile modificare le opzioni di configurazione del server oppure recuperare un database master o un altro database di sistema danneggiato   

Avviare SQL Server in modalità utente singolo
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m
   ```

Avviare SQL Server in modalità utente singolo con SQLCMD
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m SQLCMD
   ```
  
> [!WARNING]  
>  Avviare SQL Server in Linux con l'utente "mssql" per evitare problemi di avvio futuri. Esempio "sudo -u mssql /opt/mssql/bin/sqlservr [STARTUP OPTIONS]" 

Se SQL Server è stato avviato accidentalmente con un altro utente, è necessario modificare la proprietà dei file di database di SQL Server impostandola di nuovo sull'utente "mssql" prima di avviare SQL Server con systemd. Per modificare, ad esempio, la proprietà di tutti i file di database in/var/opt/mssql impostandola sull'utente "mssql", eseguire il comando seguente

   ```bash
   chown -R mssql:mssql /var/opt/mssql/
   ```

## <a name="rebuild-system-databases"></a>Ricompilare i database di sistema
Se strettamente necessario, è possibile scegliere di ripristinare le versioni predefinite dei database master e modello.

> [!WARNING]
> Questa procedura **ELIMINA tutti i dati di sistema di SQL Server** configurati, incluse le informazioni sui database utente (ma non i database utente stessi). Elimina anche le altre informazioni archiviate nei database di sistema, incluse le seguenti: informazioni sulla chiave master, eventuali certificati caricati nel master, password di accesso dell'amministratore di sistema, informazioni relative ai processi da msdb, informazioni su Posta elettronica database da msdb e opzioni di sp_configure. Usarla solo se si conoscono le implicazioni.

1. Arrestare SQL Server.

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Eseguire **sqlservr** con il parametro **force-setup**. 

   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr --force-setup
   ```
   
   > [!WARNING]
   > Vedere l'avviso precedente. È anche necessario eseguire questa operazione come utente **mssql**, come illustrato qui.

1. Dopo aver visualizzato il messaggio "Recupero completato", premere CTRL+C. SQL Server verrà arrestato

1. Riconfigurare la password dell'amministratore di sistema.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```
   
1. Avviare SQL Server e riconfigurare il server. Ciò include il ripristino o la riconnessione di tutti i database utente.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="improve-performance"></a>Migliorare le prestazioni

Esistono molti fattori che influiscono sulle prestazioni, tra cui la progettazione del database, l'hardware e le esigenze dei carichi di lavoro. Per migliorare le prestazioni, iniziare a esaminare le procedure consigliate dell'articolo [Procedure consigliate per le prestazioni e linee guida per la configurazione per SQL Server in Linux](sql-server-linux-performance-best-practices.md). Esplorare quindi alcuni degli strumenti disponibili per la risoluzione dei problemi relativi alle prestazioni.

- [Archivio query](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Viste a gestione dinamica (DMV) di sistema](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [Performance Dashboard in SQL Server Management Studio](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in) (Performance Dashboard in SQL Server Management Studio)

## <a name="common-issues"></a>Problemi comuni

1. Non è possibile connettersi all'istanza di SQL Server remota.

   Vedere la sezione relativa alla risoluzione dei problemi nell'articolo [Connettersi a SQL Server in Linux](#connection).

2. ERRORE: il nome non può contenere più di 15 caratteri.

   Si tratta di un problema noto che si verifica ogni volta che il nome del computer che prova a installare il pacchetto Debian di SQL Server contiene più di 15 caratteri. L'unica soluzione alternativa attualmente disponibile consiste nel modificare il nome del computer. A questo scopo, è possibile modificare il file del nome host e riavviare il computer. La [guida del sito Web](https://www.cyberciti.biz/faq/ubuntu-change-hostname-command/) seguente illustra l'operazione in dettaglio.

3. Reimpostazione della password dell'amministratore di sistema.

   Se la password dell'amministratore di sistema è stata dimenticata o è necessario reimpostarla per un altro motivo, seguire questa procedura.

   > [!NOTE]
   > I passaggi seguenti arrestano temporaneamente il servizio SQL Server.

   Accedere al terminale host, eseguire i comandi seguenti e seguire le istruzioni per reimpostare la password dell'amministratore di sistema:

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf setup
   ```

4. Uso di caratteri speciali nella password.

   Se si usano alcuni caratteri nella password di accesso di SQL Server, potrebbe essere necessario farli precedere da una barra rovesciata quando li si usa in un comando di Linux nel terminale. È ad esempio necessario applicare un carattere di escape al simbolo del dollaro ($) ogni volta che lo si usa in uno script di comando/shell del terminale:

   Non valido:

   ```bash
   sudo sqlcmd -S myserver -U sa -P Test$$
   ```

   Valido:

   ```bash
   sqlcmd -S myserver -U sa -P Test\$\$
   ```

   Risorse: [Special characters](https://tldp.org/LDP/abs/html/special-chars.html) (Caratteri speciali)
   [Escaping](https://tldp.org/LDP/abs/html/escapingsection.html) (Escape)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]