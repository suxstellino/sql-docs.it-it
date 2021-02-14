---
title: Configurare le condivisioni della cartella snapshot
titleSuffix: SQL Server on Linux
description: Informazioni su come configurare le condivisioni della cartella snapshot per la replica di SQL Server in Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/24/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: fdd43008abb9ba49ab3a5e3a2859a6366a3a6542
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346477"
---
# <a name="configure-replication-snapshot-folder-with-shares"></a>Configurare la cartella snapshot delle repliche con condivisioni

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

La cartella snapshot è una directory designata dall'utente come condivisione. Gli agenti che eseguono operazioni di lettura e scrittura in questa cartella devono avere autorizzazioni sufficienti per accedervi.

![diagramma della replica][1]

### <a name="replication-snapshot-folder-share-explained"></a>Spiegazione delle condivisioni delle cartelle snapshot delle repliche

Prima degli esempi, si esaminerà in dettaglio in che modo SQL Server usa le condivisioni Samba nella replica. Ecco un semplice esempio del loro funzionamento.

1. Le condivisioni Samba sono configurate in modo che i file scritti in `/local/path1` dagli agenti di replica nel server di pubblicazione sono visibili per il sottoscrittore
2. SQL Server viene configurato per l'uso di percorsi condivisione quando si configura il server di pubblicazione per il database di distribuzione, in modo che tutte le istanze puntano a `//share/path`
3. SQL Server cerca il percorso locale da `//share/path` per sapere dove cercare i file
4. SQL Server legge e scrive nei percorsi locali supportati da una condivisione Samba


## <a name="configure-a-samba-share-for-the-snapshot-folder"></a>Configurare una condivisione Samba per la cartella snapshot 

Per accedere alle cartelle snapshot in altri computer, gli agenti di replica hanno bisogno di una directory condivisa tra gli host di replica. Nella replica pull transazionale, ad esempio, l'agente di distribuzione risiede nel sottoscrittore, che per ottenere gli articoli richiede l'accesso al database di distribuzione. In questa sezione verrà illustrato un esempio di come configurare una condivisione Samba in due host di replica.


## <a name="steps"></a>Passaggi

A titolo di esempio, si configurerà tramite Samba una cartella snapshot nell'host 1 (il server di distribuzione) da condividere con l'host 2 (il sottoscrittore). 

### <a name="install-and-start-samba-on-both-machines"></a>Installare e avviare Samba in entrambi i computer 

In Ubuntu:

```bash
sudo apt-get -y install samba
sudo service smbd restart
```

In RHEL:

```bash
sudo yum install samba
sudo service smb start
sudo service smb status
```

### <a name="on-host-1-distributor-set-up-the-samba-share"></a>Host 1 (server di distribuzione): configurazione della condivisione Samba 

1. Impostare utente e password per Samba:

  ```bash
  sudo smbpasswd -a mssql 
  ```

1. Modificare `/etc/samba/smb.conf` in modo da includere la voce seguente e inserire un valore nei campi *share_name* e *path*
 ```bash
  <[share_name]>
  path = </local/path/on/host/1>
  writable = yes
  create mask = 770
  directory mask 
  valid users = mssql 
  ```

  **Esempio**

  ```bash
  [mssql_data]    <- Name of the shared directory
  path = /var/opt/mssql/repldata <- location of directory we wish to share
  writable = yes  <- determines if the share is writable from other hosts
  create mask = 770  <- Linux permissions for files created 
  directory mask = 770 <- Linux permissions for directories created
  valid users = mssql   <- list of users who can login to this share
  ```

### <a name="on-host-2-subscriber--mount-the-samba-share"></a>Host 2 (sottoscrittore): montare la condivisione Samba

Modificare il comando con i percorsi corretti ed eseguire il comando seguente nel computer2:

  ```bash
  sudo mount //<name_of_host_1>/<share_name> </local/path/on/host/2> -o user=mssql,uid=mssql,gid=mssql
  ```

  **Esempio**

  ```bash
  mount //host1/mssql_data /var/opt/mssql/repldata_shared -o user=mssql,uid=mssql,gid=mssql

  user=mssql <- sets the login name for samba
  uid=mssql   <- makes the mssql user as the owner of the mounted directory
  gid=mssql   <- sets the mssql group as the owner of the mounted directory
  ```

### <a name="on-both-hosts--configure-sql-server-on-linux-instances-to-use-snapshot-share"></a>Entrambi gli host: configurare le istanze di SQL Server in Linux per l'uso della condivisione snapshot

Aggiungere la sezione seguente a `mssql.conf` in entrambi i computer. Usare la condivisione Samba per //share/path. In questo esempio, è `//host1/mssql_data`

  ```bash
  [uncmapping]
  //share/path = /local/path/on/hosts/
  ```

  **Esempio**

  Nell'host 1:

  ```bash
  [uncmapping]
  //host1/mssql_data = /local/path/on/hosts/1
  ```

  Nell'host 2:
  
  ```bash
  [uncmapping]
  //host1/mssql_data = /local/path/on/hosts/2
  ```

### <a name="configuring-publisher-with-shared-paths"></a>Configurazione del server di pubblicazione con percorsi condivisi

* Quando si configura la replica, usare il percorso condivisione, ad esempio `//host1/mssql_data`
* Eseguire il mapping di `//host1/mssql_data` a una directory locale e al mapping aggiunto a `mssql.conf`.

## <a name="next-steps"></a>Passaggi successivi

[Concetti: replica di SQL Server in Linux](sql-server-linux-replication.md)

[Stored procedure per la replica](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).

[1]: ./media/sql-server-linux-replication-snapshot-shares/image1.png
