---
title: Configurare l'autenticazione di Active Directory con contenitori basati su SQL Server in Linux usando adutil
description: Istruzioni dettagliate su come configurare l'autenticazione di Active Directory con contenitori di SQL Server in Linux usando adutil
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: a3fc97f34a8d75557abb9ed5b46745dde93fe77d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033971"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux--containers"></a>Esercitazione: Configurare l'autenticazione di Active Directory con contenitori di SQL Server in Linux

> [!NOTE]
> **adutil** è attualmente disponibile in **anteprima pubblica**

Questa esercitazione illustra come configurare i contenitori di SQL Server in Linux per supportare l'autenticazione di Active Directory (AD), detta anche autenticazione integrata. Per una panoramica, vedere [Autenticazione di Active Directory per SQL Server in Linux](sql-server-linux-active-directory-auth-overview.md).

Questa esercitazione è costituita dalle attività seguenti:

> [!div class="checklist"]
> - Installare adutil-preview
> - Aggiungere l'host Linux al dominio di Active Directory
> - Creare un utente AD per SQL Server e impostare il nome dell'entità servizio (SPN) usando lo strumento adutil
> - Creare il file keytab del servizio SQL Server
> - Creare i file mssql.conf e krb5.conf che verranno usati dal contenitore di SQL Server
> - Montare i file di configurazione e distribuire il contenitore di SQL Server
> - Creare account di accesso di SQL Server basati su AD usando Transact-SQL
> - Connettersi a SQL Server usando l'autenticazione di Active Directory

## <a name="prerequisites"></a>Prerequisiti

Prima di configurare l'autenticazione di Active Directory, è necessario quanto segue:

- Avere un controller di dominio di Active Directory (Windows) nella rete.
- Installare lo strumento adutil-preview in un computer host Linux, che è stato aggiunto a un dominio. Seguire la sezione [Installare adutil-preview](#install-adutil-preview) qui sotto in base alla distribuzione di Linux che si sta eseguendo per installare lo strumento adutil-preview.

## <a name="container-deployment-and-preparation"></a>Preparazione e distribuzione del contenitore

Per configurare il contenitore, sarà necessario conoscere in anticipo la porta che verrà usata dal contenitore nell'host. Il mapping della porta predefinita 1433 potrebbe essere diverso nell'host del contenitore dell'utente. Per questa esercitazione, verrà eseguito il mapping della porta 5433 dell'host alla porta 1433 del contenitore. Per altre informazioni, vedere [Eseguire immagini del contenitore di SQL Server con Docker](quickstart-install-connect-docker.md)

Quando si registrano i nomi delle entità servizio (SPN), è possibile usare il nome host del computer o il nome del contenitore, ma è consigliabile configurarli in base a ciò che si vuole visualizzare quando ci si connette al contenitore dall'esterno.

Assicurarsi che sia stata aggiunta una voce per l'host di inoltro (A) in Active Directory per l'indirizzo IP dell'host Linux, eseguendo il mapping del nome al contenitore di SQL Server. In questa esercitazione, l'indirizzo IP del computer host `myubuntu` è `10.0.0.10` e il nome del contenitore di SQL Server è `sql1`. La voce per l'host di inoltro in Active Directory viene aggiunta come illustrato di seguito. La voce garantisce che, quando gli utenti si connettono a sql1.contoso.com, venga raggiunto l'host corretto.

:::image type="content" source="media/sql-server-linux-containers-ad-auth-adutil-tutorial/host-a-record.png" alt-text="Aggiungere il record dell'host":::

Per questa esercitazione viene usato un ambiente in Azure con tre macchine virtuali. Una macchina virtuale che funge da controller di dominio Windows con il nome di dominio `contoso.com`. Il controller di dominio è denominato `adVM.contoso.com`. La seconda è un computer Windows denominato `winbox`, su cui è in esecuzione Windows 10 Desktop, che viene usato come client e ha SQL Server Management Studio (SSMS) installato. La terza è un computer Ubuntu 18.04 LTS denominato `myubuntu`, che ospita i contenitori di SQL Server. Tutti i computer sono stati aggiunti al dominio `contoso.com`. Per altre informazioni, vedere [Aggiungere un host di SQL Server in Linux a un dominio di Active Directory](sql-server-linux-active-directory-join-domain.md).

> [!NOTE]
> L'aggiunta del computer del contenitore host al dominio non è obbligatoria, come si vedrà più avanti in questo articolo.

## <a name="install-adutil-preview"></a>Installare adutil-preview

Nel computer host Linux usare i comandi seguenti per installare adutil-preview in base alla distribuzione di Linux.

> [!NOTE]
> Per questa versione di anteprima, in alcune distribuzioni di Linux, se si prova a eseguire l'installazione di adutil senza il parametro `ACCEPT_EULA`, l'esperienza di installazione è compromessa. La raccomandazione seguente prevede l'installazione dello strumento adutil-preview con `ACCEPT_EULA=Y` impostato. È possibile leggere l'anteprima del [contratto di licenza](https://go.microsoft.com/fwlink/?linkid=2151376) prima dell'installazione. Microsoft sta lavorando attivamente a questo problema che dovrebbe essere risolto per la versione disponibile a livello generale.

### <a name="rhel"></a>RHEL

1. Scaricare il file di configurazione del repository Microsoft per Red Hat.

    ```bash
    sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/8/prod.repo
    ```

1. Se era già stata installata una versione precedente di adutil, rimuovere tutti i pacchetti adutil meno recenti.

    ```bash
    sudo yum remove adutil
    ```

1. Eseguire i comandi seguenti per installare adutil-preview. `ACCEPT_EULA=Y` accetta il contratto di licenza di anteprima per adutil. Il contratto di licenza è disponibile nel percorso `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y yum install -y adutil-preview
    ```

### <a name="ubuntu"></a>Ubuntu

1. Registrare il repository Microsoft per Ubuntu.

    ```bash
    sudo curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
    ```

1. Se era già stata installata una versione precedente di adutil, rimuovere tutti i pacchetti adutil meno recenti usando i comandi seguenti

    ```bash
    sudo apt-get remove adutil
    ```

1. Eseguire il comando seguente per installare adutil-preview. `ACCEPT_EULA=Y` accetta il contratto di licenza di anteprima per adutil. Il contratto di licenza è disponibile nel percorso `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y apt-get install -y adutil-preview
    ```

### <a name="sles"></a>SLES

1. Aggiungere il repository Microsoft SQL Server a Zypper.

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo
    ```

1. Se era già stata installata una versione precedente di adutil, rimuovere tutti i pacchetti adutil meno recenti.

    ```bash
    sudo zypper remove adutil
    ```

1. Eseguire il comando seguente per installare adutil-preview. `ACCEPT_EULA=Y` accetta il contratto di licenza di anteprima per adutil. Il contratto di licenza è disponibile nel percorso `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="creating-the-ad-user-spns-and-sql-server-service-keytab"></a>Creazione di un utente AD, dei nomi delle entità servizio e del file keytab del servizio SQL Server

Se non si vuole che l'host del contenitore di SQL Server in Linux faccia parte del dominio e non è stata seguita la procedura per aggiungere il computer al dominio, seguire questa procedura in un altro computer Linux che fa già parte del dominio AD:

 1. Creare un utente AD per SQL Server e impostare il nome dell'entità servizio usando lo strumento adutil.

 2. Creare e configurare il file keytab del servizio SQL Server.

Copiare il file mssql.keytab creato nel computer host che eseguirà il contenitore di SQL Server e configurare il contenitore per usare il file mssql.keytab. Facoltativamente, è anche possibile aggiungere l'host Linux che eseguirà il contenitore di SQL Server al dominio AD e seguire questa procedura nello stesso computer.

### <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-using-the-adutil-tool"></a>Creare un utente AD per SQL Server e impostare il nome dell'entità servizio usando lo strumento adutil

Per abilitare l'autenticazione di Active Directory nei contenitori di SQL Server in Linux, è necessario eseguire i passaggi da 1 a 3 indicati di seguito per l'esecuzione in un computer Linux che fa parte del dominio AD.

1. Ottenere o rinnovare il ticket-granting ticket (TGT) Kerberos usando il comando `kinit`. Usare un account con privilegi per il comando `kinit`. L'account deve avere l'autorizzazione per connettersi al dominio e anche poter creare account e nomi delle entità servizio nel dominio.

    > [!IMPORTANT]
    > Prima di eseguire questo comando, l'host deve già far parte del dominio, come illustrato nel passaggio precedente.

    ```bash
    kinit privilegeduser@DOMAIN.COM
    ```

    Esempio: per l'ambiente descritto in precedenza, l'account con privilegi è `amvin@CONTOSO.COM`

    ```bash
    kinit amvin@CONTOSO.COM
    ```

2. Usando lo strumento adutil, creare il nuovo utente che verrà usato come account AD con privilegi da SQL Server.

   ```bash
   adutil user create --name sqluser -distname CN=sqluser,CN=Users,DC=CONTOSO,DC=COM --password 'P@ssw0rd'
   ```

    > [!NOTE]
    > È possibile specificare le password in uno dei tre modi seguenti:
    >
    > - Flag password: password \<password\>
    > - Variabili di ambiente: `ADUTIL_ACCOUNT_PWD`
    > - Input interattivo
    >
    > La precedenza dei metodi di immissione delle password corrisponde all'ordine delle opzioni elencate in precedenza. È consigliabile fornire la password usando le variabili di ambiente o l'input interattivo, perché si tratta di opzioni più sicure rispetto al flag password.

    È possibile specificare il nome dell'account usando il nome distinto (`-distname`), come illustrato in precedenza, oppure è possibile usare il nome dell'unità organizzativa (OU). Il nome dell'unità organizzativa (`--ou`) ha la precedenza sul nome distinto nel caso in cui li si specifichi entrambi. Per altri dettagli, è possibile eseguire il comando seguente:

    ```bash
    adutil user create --help
    ```

3. Registrare i nomi delle entità servizio nell'utente creato in precedenza. È possibile usare il nome del computer host invece del nome del contenitore se lo si preferisce, a seconda di come si vuole che la connessione venga visualizzata dall'esterno. In questa esercitazione viene usata la porta 5433 invece della porta 1433. Questo è il mapping delle porte per il contenitore. Il numero di porta dell'utente potrebbe essere diverso.

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H sql1.contoso.com -p 5433
    ```

    > [!NOTE]
    >
    > - `addauto` creerà automaticamente i nomi delle entità servizio, purché siano presenti privilegi sufficienti per l'account kinit.
    > - `-n`: nome dell'account a cui verranno assegnati i nomi delle entità servizio.
    > - `-s`: nome del servizio da usare per la generazione dei nomi delle entità servizio. In questo caso, trattandosi del nome del servizio SQL Server, è MSSQLSvc.
    > - `-H`: nome host da usare per la generazione dei nomi delle entità servizio. Se non specificato, verrà usato il nome di dominio completo dell'host locale. Specificare anche il nome di dominio completo come nome del contenitore. In questo caso, il nome del contenitore è `sql1` e il nome di dominio completo è `sql1.contoso.com`.
    > - `-p`: porta da usare per la generazione dei nomi delle entità servizio. Se non specificata, i nomi delle entità servizio verranno generati senza porta. In questo caso, le connessioni SQL funzioneranno solo quando SQL Server è in ascolto sulla porta predefinita 1433.

### <a name="create-the-sql-server-service-keytab-file"></a>Creare il file keytab del servizio SQL Server

Creare il file keytab che contiene le voci per ognuno dei 4 nomi delle entità servizio creati in precedenza e quella per l'utente. Il file keytab verrà montato nel contenitore, in modo che possa essere creato in qualsiasi posizione nell'host. È possibile modificare il percorso senza problemi, purché il file keytab risultante venga montato correttamente quando si usa docker/podman per distribuire il contenitore.

Per creare il file keytab per tutti i nomi delle entità servizio, è possibile usare l'opzione `createauto`:

```bash
adutil keytab createauto -k /container/sql1/secrets/mssql.keytab -p 5433 -H sql1.contoso.com --password 'P@ssw0rd' -s MSSQLSvc
```

> [!NOTE]
>
> - `-k`: percorso in cui si vuole creare il file `mssql.keytab`. Nell'esempio precedente la directory "/container/sql1/secrets" deve esistere già nell'host.
> - `-p`: porta da usare per la generazione dei nomi delle entità servizio. Se non specificata, i nomi delle entità servizio verranno generati senza porta.
> - `-H`: nome host da usare per la generazione dei nomi delle entità servizio. Se non specificato, verrà usato il nome di dominio completo dell'host locale. Specificare anche il nome di dominio completo come nome del contenitore. In questo caso, il nome del contenitore è `sql1` e il nome di dominio completo è `sql1.contoso.com`.
> - `-s`: nome del servizio da usare per la generazione dei nomi delle entità servizio. In questo caso, trattandosi del nome del servizio SQL Server, è MSSQLSvc.
> - `--password`: si tratta della password dell'account utente AD con privilegi creato in precedenza.
> - `-e` oppure `--enctype`: tipi di crittografia per la voce keytab. Usare un elenco delimitato da virgole dei valori. Se non specificato, verrà visualizzato un prompt interattivo.

Quando viene data la possibilità di scegliere i tipi di crittografia, è possibile scegliere più di uno. Per questo esempio vengono scelti `aes256-cts-hmac-sha1-96` e `arcfour-hmac`. Assicurarsi di scegliere il tipo di crittografia supportato dall'host e dal dominio.

Se si preferisce scegliere in modo non interattivo il tipo di crittografia, è possibile specificare la scelta del tipo di crittografia con l'argomento -e nel comando precedente. Per altre informazioni sui comandi adutil, eseguire il comando seguente.

```bash
adutil keytab createauto --help
```

> [!NOTE]
> `arcfour-hmac` è un tipo di crittografia debole e non è consigliato per l'uso nell'ambiente di produzione.

Per creare il file keytab per l'utente, il comando è:

```bash
adutil keytab create -k /container/sql1/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k`: percorso in cui si vuole creare il file `mssql.keytab`. Nell'esempio precedente la directory "/container/sql1/secrets" deve esistere già nell'host.
> - `-p`: entità da aggiungere al file keytab.

Il comando adutil keytab create/autocreate non sovrascrive i file precedenti, ma esegue solo l'accodamento al file se è già presente.

Assicurarsi che il file keytab creato abbia le autorizzazioni corrette impostate quando si distribuisce il contenitore.

```bash
chmod 440 /container/sql1/secrets/mssql.keytab
```

> [!NOTE]
> A questo punto, è possibile copiare mssql.keytab dall'host Linux corrente all'host Linux in cui si distribuirà il contenitore di SQL Server e seguire i passaggi rimanenti nell'host Linux che eseguirà il contenitore di SQL Server. Se i passaggi precedenti sono stati eseguiti nello stesso host Linux in cui verranno distribuiti i contenitori di SQL, eseguire anche i passaggi successivi nello stesso host.

## <a name="create-the-config-files-to-be-used-by-the-sql-server-container"></a>Creare i file di configurazione che verranno usati dal contenitore di SQL Server

1. Creare un file `mssql.conf` con le impostazioni per AD. Questo file può essere creato ovunque nell'host e deve essere montato correttamente durante l'esecuzione del comando docker run. In questo esempio il file `mssql.conf` è stato inserito in `/container/sql1 `, che è la directory del contenitore. Il contenuto del file `mssql.conf` è il seguente:

    ```output
    [network]
    privilegedadaccount = sqluser
    kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
    ```

    > [!NOTE]
    >
    > - `privilagedadaccount`: Utente AD con privilegi da usare per l'autenticazione AD.
    > - `kerberoskeytabfile`: percorso nel contenitore in cui verrà inserito il file mssql.keytab.

1. Creare un file krb5.conf. Di seguito è illustrato un esempio. In questi file è importante distinguere tra lettere maiuscole e minuscole.

    ```output
    [libdefaults]
    default_realm = DOMAIN.COM

    [realms]
     CONTOSO.COM = {
         kdc = adVM.contoso.com
         admin_server = adVM.contoso.com
         default_domain = CONTOSO.COM
     }

    [domain_realm]
     .contoso.com = CONTOSO.COM
     contoso.com = CONTOSO.COM


1. Copy all files, `mssql.conf`, `krb5.conf`, `mssql.keytab` to a location that will be mounted to the SQL Server container. In this example, these files are placed on the host at the following locations: `mssql.conf` and `krb5.conf` at `/container/sql1/`. `mssql.keytab` is placed at the location `/container/sql1/secrets/`.

1. Make sure there's enough permission on these folders for the user running the docker/podman command. When the container starts, the user needs access to the folder path created. In this example, we provided the below permissions given to the folder path:

    ```bash
    sudo chmod 755 /container/sql1/
    ```

## <a name="mount-the-config-files-and-deploy-the-sql-server-container"></a>Montare i file di configurazione e distribuire il contenitore di SQL Server

Eseguire il contenitore di SQL Server e montare i file di configurazione AD corretti creati in precedenza, come illustrato di seguito:

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=\<YourStrong@Passw0rd\>" \
-p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
> Quando si esegue il contenitore in un modulo di sicurezza Linux, ad esempio negli host abilitati per SELinux, è necessario montare i volumi usando l'opzione `Z`, che indica a docker di applicare al contenuto un'etichetta privata non condivisa. Per altre informazioni, vedere [Configure the SE Linux label](https://docs.docker.com/storage/bind-mounts/#configure-the-selinux-label) (Configurare l'etichetta SE Linux).

L'esempio conterrà i comandi seguenti:

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql/ \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
--dns-search contoso.com \
--dns 10.0.0.4 \
--add-host adVM.contoso.com:10.0.0.4 \
--add-host contoso.com:10.0.0.4 \
--add-host contoso:10.0.0.4 \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
>
> - I file `mssql.conf` e `krb5.conf` si trovano nel percorso del file host `/container/sql1`.
> - Il file `mssql.keytab` creato si trova nel percorso del file host `/container/sql1/secrets`.
> - Poiché il computer host è in Azure, è necessario accodare i dettagli di AD nello stesso ordine al comando docker run. Nell'esempio il controller di dominio `adVM` è nel dominio `contoso.com`, con l'indirizzo IP `10.0.0.4`. Il controller di dominio esegue DNS e KDC.

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>Creare account di accesso di SQL Server basati su AD in Transact-SQL

Connettersi al contenitore di SQL ed eseguire i comandi seguenti per creare l'account di accesso e verificare che risulti elencato. È possibile eseguire questo comando da un computer client (Windows o Linux) che esegue SSMS, Azure Data Studio (ADS) o qualsiasi altro strumento dell'interfaccia della riga di comando.

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>Connettersi a SQL Server usando l'autenticazione di Active Directory.

Per connettersi usando [SSMS](../ssms/download-sql-server-management-studio-ssms.md) o [ADS](../azure-data-studio/download-azure-data-studio.md), accedere a SQL Server con le credenziali di Windows usando il nome e il numero di porta di SQL Server. Il nome potrebbe corrispondere al nome del contenitore o al nome host. In questo esempio il nome del server sarà `sql1.contoso.com, 5433`.

È anche possibile usare uno strumento come [sqlcmd](../tools/sqlcmd-utility.md) per connettersi a SQL Server nel contenitore.

```bash
sqlcmd -E -S 'sql1.contoso.com, 5433'
```

## <a name="next-steps"></a>Passaggi successivi

- [Avvio rapido: Eseguire immagini del contenitore di SQL Server con Docker](quickstart-install-connect-docker.md)
- [Aggiungere un host di SQL Server in Linux a un dominio di Active Directory](sql-server-linux-active-directory-auth-overview.md)
