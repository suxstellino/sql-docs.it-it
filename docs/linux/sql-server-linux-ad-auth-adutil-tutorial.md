---
title: Configurare l'autenticazione di Active Directory con SQL Server in Linux usando adutil
description: Istruzioni dettagliate su come configurare l'autenticazione di Active Directory con SQL Server in Linux usando adutil
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: 6b68cb2b2ff3712502ef4e0862fe71ad78705d7c
ms.sourcegitcommit: c52a6aeb6fa6d7c3a86b3e84449361f4a0949ad0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/06/2021
ms.locfileid: "99623761"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux-using-adutil"></a>Esercitazione: Configurare l'autenticazione di Active Directory con SQL Server in Linux usando adutil

> [!NOTE]
> **adutil** è attualmente disponibile in **anteprima pubblica**

Questa esercitazione illustra come configurare l'autenticazione di Active Directory (AD) per SQL Server in Linux usando adutil. Per un altro metodo di configurazione dell'autenticazione di Active Directory tramite ktpass, vedere [Esercitazione: Usare l'autenticazione di Azure Active Directory con SQL Server in Linux](sql-server-linux-active-directory-authentication.md).

Questa esercitazione è costituita dalle attività seguenti:

> [!div class="checklist"]
> - Installare adutil-preview
> - Aggiungere il computer Linux al dominio AD
> - Creare un utente AD per SQL Server e impostare il nome dell'entità servizio (SPN) usando lo strumento adutil
> - Creare il file keytab del servizio SQL Server
> - Configurare SQL Server per usare il file keytab
> - Creare account di accesso di SQL Server basati su AD usando Transact-SQL
> - Connettersi a SQL Server usando l'autenticazione di Active Directory

## <a name="prerequisites"></a>Prerequisiti

Prima di configurare l'autenticazione di Active Directory, è necessario quanto segue:

- Avere un controller di dominio di Active Directory (Windows) nella rete.
- Installare lo strumento adutil-preview in un computer host Linux. Seguire la sezione qui sotto corrispondente alla distribuzione di Linux che si sta eseguendo per installare adutil-preview.

## <a name="install-adutil-preview"></a>Installare adutil-preview

Nel computer host Linux usare i comandi seguenti per installare adutil-preview.

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

1. Eseguire i comandi seguenti per installare adutil-preview. `ACCEPT_EULA=Y` accetta il contratto di licenza di anteprima per adutil. Il contratto di licenza è disponibile nel percorso "/usr/share/adutil/".

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

1. Eseguire il comando seguente per installare adutil-preview. `ACCEPT_EULA=Y` accetta il contratto di licenza di anteprima per adutil. Il contratto di licenza è disponibile nel percorso "/usr/share/adutil/".

    ```bash
    sudo apt-get update
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

1. Eseguire il comando seguente per installare adutil-preview. `ACCEPT_EULA=Y` accetta il contratto di licenza di anteprima per adutil. Il contratto di licenza è disponibile nel percorso "/usr/share/adutil/".

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="domain-machine-preparation"></a>Preparazione del computer di dominio

Assicurarsi che sia stata aggiunta una voce per l'host di inoltro (A) in Active Directory per l'indirizzo IP dell'host Linux. In questa esercitazione, l'indirizzo IP del computer host `myubuntu` è `10.0.0.10`. La voce per l'host di inoltro in Active Directory viene aggiunta come illustrato di seguito. La voce garantisce che, quando gli utenti si connettono a myubuntu.contoso.com, venga raggiunto l'host corretto.

:::image type="content" source="media/sql-server-linux-ad-auth-adutil-tutorial/host-a-record.png" alt-text="Aggiungere il record dell'host":::

Per questa esercitazione viene usato un ambiente in Azure con tre macchine virtuali. Una macchina virtuale che funge da controller di dominio Windows con il nome di dominio `contoso.com`. Il controller di dominio è denominato `adVM.contoso.com`. La seconda è un computer Windows denominato `winbox`, su cui è in esecuzione Windows 10 Desktop, che viene usato come client e ha SQL Server Management Studio (SSMS) installato. La terza è un computer Ubuntu 18.04 LTS denominato `myubuntu`, che ospita SQL Server.

## <a name="join-the-linux-host-machine-to-your-ad-domain"></a>Aggiungere il computer host Linux al dominio AD

Aggiungere l'host di SQL Server in Linux a un controller di dominio Active Directory. Aggiungere un host di SQL Server in Linux a un dominio di Active DirectoryPer informazioni sull'aggiunta a un dominio Active Directory, vedere [Aggiungere un host di SQL Server in Linux a un dominio di Active Directory](sql-server-linux-active-directory-join-domain.md).

## <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-spn-using-the-adutil-tool"></a>Creare un utente AD per SQL Server e impostare il nome dell'entità servizio (SPN) usando lo strumento adutil

1. Ottenere o rinnovare il ticket-granting ticket (TGT) Kerberos usando il comando `kinit`. Usare un account con privilegi per il comando `kinit`. L'account deve avere l'autorizzazione per connettersi al dominio e anche poter creare account e nomi delle entità servizio nel dominio.

    > [!IMPORTANT]
    > Prima di eseguire questo comando, il computer host deve già far parte del dominio, come illustrato nel passaggio precedente.

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
    > - Flag password: --password \<password\>
    > - Variabili di ambiente: `ADUTIL_ACCOUNT_PWD`
    > - Input interattivo
    >
    > La precedenza dei metodi di immissione delle password corrisponde all'ordine delle opzioni elencate in precedenza. È consigliabile fornire la password usando le variabili di ambiente o l'input interattivo, perché si tratta di opzioni più sicure rispetto al flag password.

    È possibile specificare il nome dell'account usando il nome distinto (`-distname`), come illustrato in precedenza, oppure è possibile usare il nome dell'unità organizzativa (OU). Il nome dell'unità organizzativa (`--ou`) ha la precedenza sul nome distinto nel caso in cui li si specifichi entrambi. Per altri dettagli, è possibile eseguire il comando seguente:

    ```bash
    adutil user create --help
    ```

3. Registrare i nomi delle entità servizio nell'entità di sicurezza creata in precedenza. Usare il nome di dominio completo del computer. In questa esercitazione viene usata la porta predefinita 1433 di SQL Server. Il numero di porta dell'utente potrebbe essere diverso.

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H myubuntu.contoso.com -p 1433
    ```

    > [!NOTE]
    >
    > - `addauto` creerà automaticamente i nomi delle entità servizio, purché siano presenti privilegi sufficienti per l'account kinit.
    > - `-n`: nome dell'account a cui verranno assegnati i nomi delle entità servizio.
    > - `-s`: nome del servizio da usare per la generazione dei nomi delle entità servizio. In questo caso, trattandosi del nome del servizio SQL Server, è `MSSQLSvc`.
    > - `-H`: nome host da usare per la generazione dei nomi delle entità servizio. Se non specificato, verrà usato il nome di dominio completo dell'host locale. In questo caso, il nome host è `myubuntu` e il nome di dominio completo è `myubuntu.contoso.com`.
    > - `-p`: porta da usare per la generazione dei nomi delle entità servizio. Se non specificata, i nomi delle entità servizio verranno generati senza porta. In questo caso, le connessioni SQL funzioneranno solo quando SQL Server è in ascolto sulla porta predefinita 1433.

## <a name="create-the-sql-server-service-keytab-file"></a>Creare il file keytab del servizio SQL Server

Creare il file keytab che contiene le voci per ognuno dei 4 nomi delle entità servizio creati in precedenza e quella per l'utente.

```bash
adutil keytab createauto -k /var/opt/mssql/secrets/mssql.keytab -p 1433 -H myubuntu.contoso.com --password 'P@ssw0rd' -s MSSQLSvc 
```

> [!NOTE]
>
> - `-k`: percorso in cui si vuole creare il file `mssql.keytab`. Nell'esempio precedente la directory `/var/opt/mssql/secrets/` deve esistere già nell'host.
> - `-p`: porta da usare per la generazione dei nomi delle entità servizio. Se non specificata, i nomi delle entità servizio verranno generati senza porta.
> - `-H`: nome host da usare per la generazione dei nomi delle entità servizio. Se non specificato, verrà usato il nome di dominio completo dell'host locale. In questo caso, il nome host è `myubuntu` e il nome di dominio completo è `myubuntu.contoso.com`.
> - `-s`: nome del servizio da usare per la generazione dei nomi delle entità servizio. In questo caso, trattandosi del nome del servizio SQL Server, è `MSSQLSvc`.
> - `--password`: si tratta della password dell'account utente AD con privilegi creato in precedenza.
> - `-e` oppure `--enctype`: tipi di crittografia per la voce keytab. Usare un elenco delimitato da virgole dei valori. Se non specificato, verrà visualizzato un prompt interattivo.

Quando viene data la possibilità di scegliere i tipi di crittografia, è possibile scegliere più di uno. Per questo esempio vengono scelti `aes256-cts-hmac-sha1-96` e `arcfour-hmac`. Assicurarsi di scegliere il tipo di crittografia supportato dall'host e dal dominio.

Se si preferisce scegliere in modo non interattivo il tipo di crittografia, è possibile specificare la scelta del tipo di crittografia con l'argomento -e nel comando precedente. Per altre informazioni sui comandi adutil, eseguire il comando seguente.

```bash
adutil keytab createauto --help
```

> [!NOTE]
> `arcfour-hmac` è un tipo di crittografia debole e non è consigliato per l'uso nell'ambiente di produzione.

Aggiungere nel file keytab una voce per il nome dell'entità e la password che verrà usata da SQL Server per connettersi ad Active Directory:

```bash
adutil keytab create -k /var/opt/mssql/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k`: percorso in cui si vuole creare il file `mssql.keytab`.
> - `-p`: entità da aggiungere al file keytab.

Il comando adutil keytab create/autocreate non sovrascrive i file precedenti, ma esegue solo l'accodamento al file se è già presente.

Verificare che il file keytab creato sia di proprietà dell'utente `mssql` e che solo l'utente `mssql` abbia l'accesso in lettura/scrittura al file. È possibile eseguire i comandi `chown` e `chmod`, come illustrato di seguito:

```bash
chown mssql. /var/opt/mssql/secrets/mssql.keytab
chmod 440 /var/opt/mssql/secrets/mssql.keytab
```

## <a name="configure-sql-server-to-use-the-keytab"></a>Configurare SQL Server per usare il file keytab

Eseguire i comandi seguenti per configurare SQL Server per usare il file keytab creato nel passaggio precedente e impostare l'account AD con privilegi come l'utente creato in precedenza. Nell'esempio il nome utente è `sqluser`.

```bash
/opt/mssql/bin/mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
/opt/mssql/bin/mssql-conf set network.privilegedadaccount sqluser
```

## <a name="restart-sql-server"></a>Riavviare SQL Server

Eseguire il comando seguente per riavviare il servizio SQL Server:

```bash
sudo systemctl restart mssql-server
```

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>Creare account di accesso di SQL Server basati su AD in Transact-SQL

Connettersi a SQL Server ed eseguire i comandi seguenti per creare l'account di accesso e verificare che risulti elencato.

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>Connettersi a SQL Server usando l'autenticazione di Active Directory.

Per connettersi usando [SSMS](../ssms/download-sql-server-management-studio-ssms.md) o [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md), accedere a SQL Server con le credenziali di Windows.

È anche possibile usare uno strumento come [sqlcmd](../tools/sqlcmd-utility.md) per connettersi a SQL Server usando l'autenticazione di Windows.

```bash
sqlcmd -E -S 'myubuntu.contoso.com'
```

## <a name="next-steps"></a>Passaggi successivi

- [Aggiungere un host di SQL Server in Linux a un dominio di Active Directory](sql-server-linux-active-directory-auth-overview.md)
- Per informazioni su come configurare l'autenticazione di Active Directory con i contenitori di SQL Server in Linux, vedere [Configurare l'autenticazione di Active Directory con i contenitori di SQL Server in Linux](sql-server-linux-containers-ad-auth-adutil-tutorial.md)
