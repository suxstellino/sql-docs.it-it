---
title: Aggiungere SQL Server in Linux ad Active Directory
titleSuffix: SQL Server
description: Questo articolo fornisce indicazioni per l'aggiunta di un computer host SQL Server Linux a un dominio AD. È possibile usare un pacchetto SSSD predefinito o usare provider di Active Directory di terze parti.
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 11/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 33ce3edfd7afee30e09db78b908bdaebd9a4e322
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345659"
---
# <a name="join-sql-server-on-a-linux-host-to-an-active-directory-domain"></a>Aggiungere un host di SQL Server in Linux a un dominio di Active Directory

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo fornisce indicazioni generali su come aggiungere un computer host di SQL Server in Linux a un dominio di Active Directory (AD). Sono disponibili due metodi: usare un pacchetto SSSD incorporato o usare provider Active Directory di terze parti. Esempi di prodotti di terze parti per l'aggiunta a un dominio sono [PowerBroker Identity Services (PBIS)](https://www.beyondtrust.com/), [One Identity](https://www.oneidentity.com/products/authentication-services/) e [Centrify](https://www.centrify.com/). Questa guida include i passaggi per verificare la configurazione di Active Directory. Non ha tuttavia lo scopo di fornire istruzioni su come aggiungere un computer a un dominio quando si usano utilità di terze parti.

## <a name="prerequisites"></a>Prerequisiti

Prima di configurare l'autenticazione di Active Directory, è necessario configurare un controller di dominio Active Directory, Windows, nella rete. Aggiungere quindi l'host di SQL Server in Linux a un dominio di Active Directory.

> [!IMPORTANT]
> Le procedure di esempio descritte in questo articolo sono puramente indicative e fanno riferimento ai sistemi operativi Ubuntu 16.04, Red Hat Enterprise Linux (RHEL) 7.x e SUSE Enterprise Linux (SLES) 12. Le procedure effettive possono essere leggermente diverse a seconda della configurazione dell'ambiente generale e della versione del sistema operativo. Ad esempio, Ubuntu 18.04 usa netplan, mentre Red Hat Enterprise Linux (RHEL) 8.x usa nmcli tra gli altri strumenti per gestire e configurare la rete. È consigliabile coinvolgere gli amministratori di sistema e di dominio dell'ambiente per le attività specifiche di installazione degli strumenti, configurazione e personalizzazione e per l'eventuale risoluzione dei problemi.

### <a name="reverse-dns-rdns"></a>DNS inverso (RDNS)

Quando si configura un computer che esegue Windows Server come controller di dominio, è possibile che non sia disponibile una zona RDNS per impostazione predefinita. Assicurarsi che esista una zona RDNS applicabile sia per il controller di dominio che per l'indirizzo IP del computer Linux che eseguirà SQL Server.

Assicurarsi anche che esista un record PTR che punta ai controller di dominio.

## <a name="check-the-connection-to-a-domain-controller"></a>Controllare la connessione a un controller di dominio

Verificare che sia possibile contattare il controller di dominio usando sia il nome breve che quello completo del dominio e usando il nome host del controller di dominio. Anche l'indirizzo IP del controller di dominio deve essere risolto nel nome di dominio completo del controller di dominio:

```bash
ping contoso
ping contoso.com
ping dc1.contoso.com
nslookup <IP address of dc1.contoso.com>
```

> [!TIP]
> Questa esercitazione usa **contoso.com** e **CONTOSO.COM** rispettivamente come nome di dominio e nome dell'area di autenticazione di esempio. Usa anche **DC1.CONTOSO.COM** come nome di dominio completo di esempio del controller di dominio. È necessario sostituire questi nomi con i valori effettivi.

Se la verifica di uno di questi nomi non riesce, aggiornare l'elenco di ricerca del dominio. Le sezioni seguenti forniscono istruzioni per Ubuntu, Red Hat Enterprise Linux (RHEL) e SUSE Linux Enterprise Server (SLES) rispettivamente.

### <a name="ubuntu-1604"></a>Ubuntu 16.04

1. Modificare il file **/etc/network/interfaces** in modo che il dominio di Active Directory sia incluso nell'elenco di ricerca del dominio:

   ```/etc/network/interfaces
   # The primary network interface
   auto eth0
   iface eth0 inet dhcp
   dns-nameservers **<AD domain controller IP address>**
   dns-search **<AD domain name>**
   ```

   > [!NOTE]
   > L'interfaccia di rete, `eth0`, potrebbe essere diversa a seconda del computer. Per individuare quella in uso, eseguire **ifconfig**. Copiare quindi l'interfaccia che ha un indirizzo IP e byte trasmessi e ricevuti.

1. Dopo aver modificato il file, riavviare il servizio di rete:

   ```bash
   sudo ifdown eth0 && sudo ifup eth0
   ```

1. Verificare quindi che il file **/etc/resolv.conf** contenga una riga come quella nell'esempio seguente:

   ```/etc/resolv.conf
   search contoso.com com  
   nameserver **<AD domain controller IP address>**
   ```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

1. Modificare il file [sudo vi /etc/netplan/******.yaml] in modo che il dominio di Active Directory sia incluso nell'elenco di ricerca del dominio:

   ```/etc/netplan/******.yaml
   network:
     ethernets:
       eth0:
               dhcp4: true

               dhcp6: true
               nameservers:
                       addresses: [ **<AD domain controller IP address>**]
                       search: [**<AD domain name>**]
     version: 2
   ```

   > [!NOTE]
   > L'interfaccia di rete, `eth0`, potrebbe essere diversa a seconda del computer. Per individuare quella in uso, eseguire **ifconfig**. Copiare quindi l'interfaccia che ha un indirizzo IP e byte trasmessi e ricevuti.

1. Dopo aver modificato il file, riavviare il servizio di rete:

   ```bash
   sudo netplan apply
   ```

1. Verificare quindi che il file **/etc/resolv.conf** contenga una riga come quella nell'esempio seguente:

   ```/etc/resolv.conf
   search contoso.com com  
   nameserver **<AD domain controller IP address>**
   ```

### <a name="rhel-7x"></a>RHEL 7.x

1. Modificare il file **/etc/sysconfig/network-scripts/ifcfg-eth0** in modo che il dominio di Active Directory sia incluso nell'elenco di ricerca del dominio. In alternativa, modificare un altro file di configurazione dell'interfaccia nel modo appropriato:

   ```/etc/sysconfig/network-scripts/ifcfg-eth0
   PEERDNS=no
   DNS1=**<AD domain controller IP address>**
   DOMAIN="contoso.com com"
   ```

1. Dopo aver modificato il file, riavviare il servizio di rete:

   ```bash
   sudo systemctl restart network
   ```

1. Verificare quindi che il file **/etc/resolv.conf** contenga una riga come quella nell'esempio seguente:

   ```/etc/resolv.conf
   search contoso.com com  
   nameserver **<AD domain controller IP address>**
   ```

1. Se non è ancora possibile effettuare il ping del controller di dominio, trovare il nome di dominio completo e l'indirizzo IP del controller di dominio. Un nome di dominio di esempio è **DC1.CONTOSO.COM**. Aggiungere la voce seguente a **/etc/hosts**:

   ```/etc/hosts
   **<IP address>** DC1.CONTOSO.COM CONTOSO.COM CONTOSO
   ```

### <a name="sles-12"></a>SLES 12

1. Modificare il file **/etc/sysconfig/network/config** in modo che l'indirizzo IP del controller di dominio Active Directory venga usato per le query DNS e che il dominio di Active Directory sia incluso nell'elenco di ricerca del dominio:

   ```/etc/sysconfig/network/config
   NETCONFIG_DNS_STATIC_SEARCHLIST=""
   NETCONFIG_DNS_STATIC_SERVERS="**<AD domain controller IP address>**"
   ```

1. Dopo aver modificato il file, riavviare il servizio di rete:

   ```bash
   sudo systemctl restart network
   ```

1. Verificare quindi che il file **/etc/resolv.conf** contenga una riga come quella nell'esempio seguente:

   ```/etc/resolv.conf
   search contoso.com com
   nameserver **<AD domain controller IP address>**
   ```

## <a name="join-to-the-ad-domain"></a>Eseguire l'aggiunta al dominio di Active Directory

Una volta verificata la configurazione di base e la connettività con il controller di dominio, sono disponibili due opzioni per l'aggiunta di un computer host di SQL Server in Linux con il controller di dominio Active Directory:

- [Opzione 1: Usare un pacchetto SSSD](#option1)
- [Opzione 2: Usare le utilità del provider openldap di terze parti](#option2)

### <a name="option-1-use-sssd-package-to-join-ad-domain"></a><a id="option1"></a> Opzione 1: Usare il pacchetto SSSD per l'aggiunta a un dominio AD

Questo metodo aggiunge l'host di SQL Server a un dominio di Active Directory usando i pacchetti **realmd** e **sssd**.

> [!NOTE]
> È il metodo preferito per l'aggiunta di un host Linux a un controller di dominio AD.

Per aggiungere un host di SQL Server a un dominio di Active Directory, seguire questa procedura:

1. Usare [realmd](https://www.freedesktop.org/software/realmd/docs/guide-active-directory-join) per aggiungere il computer host al dominio AD. È necessario prima installare i pacchetti client **realmd** e Kerberos nel computer host di SQL Server usando lo strumento di gestione pacchetti della distribuzione Linux:

   **RHEL:**

   ```base
   sudo yum install realmd krb5-workstation
   ```
   
   **SLES 12:**
   
   Si noti che questi passaggi sono specifici per SLES 12, che è l'unica versione supportata ufficialmente di SUSE per Linux.

   ```bash
   sudo zypper addrepo https://download.opensuse.org/repositories/network/SLE_12/network.repo
   sudo zypper refresh
   sudo zypper install realmd krb5-client sssd-ad
   ```

   **Ubuntu 16.04:**

   ```bash
   sudo apt-get install realmd krb5-user software-properties-common python-software-properties packagekit
   ```

   **Ubuntu 18.04:**

   ```bash
   sudo apt-get install realmd krb5-user software-properties-common python3-software-properties packagekit
   sudo apt-get install adcli libpam-sss libnss-sss sssd sssd-tools
   ```

1. Se l'installazione del pacchetto client Kerberos richiede un nome dell'area di autenticazione, immettere il nome di dominio in lettere maiuscole.

1. Dopo aver verificato che il DNS sia configurato correttamente, aggiungere il dominio eseguendo il comando seguente. È necessario eseguire l'autenticazione con un account di Active Directory che disponga di privilegi sufficienti in Active Directory per aggiungere un nuovo computer al dominio. Questo comando crea un nuovo account computer in Active Directory, crea il file keytab host **/etc/krb5.keytab**, configura il dominio in **/etc/sssd/sssd.conf** e aggiorna **/etc/krb5.conf**.

   A causa di un problema con **realmd**, impostare prima di tutto il nome host del computer sul nome di dominio completo anziché sul nome del computer. In caso contrario, **realmd** potrebbe non creare tutti i nomi dell'entità servizio necessari per il computer e le voci DNS non verranno aggiornate automaticamente, anche se il controller di dominio supporta gli aggiornamenti dinamici di DNS.
   
   ```bash
   sudo hostname <old hostname>.contoso.com
   ```
   
   Dopo aver eseguito il comando precedente, il file /etc/hostname dovrebbe contenere <old hostname>.contoso.com.

   ```bash
   sudo realm join contoso.com -U 'user@CONTOSO.COM' -v
   ```

   Dovrebbe essere visualizzato il messaggio `Successfully enrolled machine in realm`.

   La tabella seguente elenca alcuni messaggi di errore che si potrebbero ricevere e suggerimenti per risolverli:

   | Messaggio di errore | Recommendation |
   |---|---|
   | `Necessary packages are not installed` | Installare i pacchetti usando lo strumento di gestione pacchetti della distribuzione Linux prima di eseguire di nuovo il comando realm join. |
   | `Insufficient permissions to join the domain` | Verificare con un amministratore di dominio di disporre di autorizzazioni sufficienti per aggiungere computer Linux al dominio. |
   | `KDC reply did not match expectations` | È possibile che non sia stato specificato il nome dell'area di autenticazione corretto per l'utente. I nomi dell'area di autenticazione fanno distinzione tra maiuscole e minuscole, in genere sono in maiuscolo e possono essere identificati con il comando realm discover contoso.com. |

   SQL Server usa SSSD e NSS per il mapping degli account utente e dei gruppi agli ID di sicurezza (SID). SSSD deve essere configurato e in esecuzione affinché SQL Server possa creare correttamente gli account di accesso di Active Directory. **realmd** in genere esegue questa operazione automaticamente nell'ambito dell'aggiunta del dominio, ma in alcuni casi è necessario eseguirla separatamente.

   Per altre informazioni, vedere gli articoli che spiegano come [configurare SSSD manualmente](https://access.redhat.com/articles/3023951) e come [configurare NSS per l'uso con SSSD](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system-level_authentication_guide/configuring_services#Configuration_Options-NSS_Configuration_Options).

1. Verificare che sia ora possibile raccogliere informazioni su un utente dal dominio e che sia possibile acquisire un ticket Kerberos come tale utente. A questo scopo, l'esempio seguente usa i comandi **id**, [kinit](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html) e [klist](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/klist.html).

   ```bash
   id user@contoso.com

   uid=1348601103(user@contoso.com) gid=1348600513(domain group@contoso.com) groups=1348600513(domain group@contoso.com)

   kinit user@CONTOSO.COM

   Password for user@CONTOSO.COM:

   klist
   Ticket cache: FILE:/tmp/krb5cc_1000
   Default principal: user@CONTOSO.COM
   ```

   > [!NOTE]
   > - Se **id user\@contoso.com** restituisce `No such user`, verificare che il servizio SSSD sia stato avviato correttamente eseguendo il comando `sudo systemctl status sssd`. Se il servizio è in esecuzione e viene ancora visualizzato l'errore, provare ad abilitare la registrazione dettagliata per SSSD. Per altre informazioni, vedere la documentazione di Red Hat relativa alla [risoluzione dei problemi di SSSD](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/System-Level_Authentication_Guide/trouble.html#SSSD-Troubleshooting).
   >
   > - Se **kinit user\@CONTOSO.COM** restituisce `KDC reply did not match expectations while getting initial credentials`, verificare di avere specificato il nome dell'area di autenticazione in maiuscolo.

Per altre informazioni, vedere la documentazione di Red Hat relativa all'[individuazione e aggiunta di domini di identità](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/realmd-domain.html).

### <a name="option-2-use-third-party-openldap-provider-utilities"></a><a id="option2"></a> Opzione 2: Usare le utilità del provider openldap di terze parti

È possibile usare utilità di terze parti come [PBIS](https://www.beyondtrust.com/), [VAS](https://www.oneidentity.com/products/authentication-services/) o [Centrify](https://www.centrify.com/). Questo articolo non include le procedure per ogni singola utilità. Prima di procedere, è necessario usare una di queste utilità per aggiungere l'host Linux per SQL Server al dominio.  

SQL Server non usa codice o librerie di integratori di terze parti per le query correlate ad Active Directory. Esegue sempre le query su Active Directory tramite chiamate alla libreria openldap direttamente in questa configurazione. Gli integratori di terze parti vengono usati solo per aggiungere l'host Linux al dominio di Active Directory e SQL Server non ha alcuna comunicazione diretta con queste utilità.

> [!IMPORTANT]
> Vedere i consigli per l'uso dell'opzione di configurazione **mssql-conf** `network.disablesssd` nella sezione **Opzioni di configurazione aggiuntive** dell'articolo [Usare l'autenticazione di Active Directory con SQL Server in Linux](sql-server-linux-active-directory-authentication.md#additionalconfig).

Verificare che **/etc/krb5.conf** sia configurato correttamente. Per la maggior parte dei provider di Active Directory di terze parti, questa configurazione viene eseguita automaticamente. Tuttavia, per evitare eventuali problemi futuri, verificare se in **/etc/krb5.conf** sono presenti i valori seguenti:

```/etc/krb5.conf
[libdefaults]
default_realm = CONTOSO.COM

[realms]
CONTOSO.COM = {
}

[domain_realm]
contoso.com = CONTOSO.COM
.contoso.com = CONTOSO.COM
```

## <a name="check-that-the-reverse-dns-is-properly-configured"></a>Verificare che il DNS inverso sia configurato correttamente

Il comando seguente deve restituire il nome di dominio completo (FQDN) dell'host che esegue SQL Server. Un esempio è **SqlHost.contoso.com**.

```bash
host **<IP address of SQL Server host>**
```

L'output di questo comando dovrebbe essere simile a `**<reversed IP address>**.in-addr.arpa domain name pointer SqlHost.contoso.com`. Se il comando non restituisce il nome di dominio completo dell'host o se il nome di dominio completo non è corretto, aggiungere una voce di DNS inverso per l'host di SQL Server in Linux al server DNS.

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha illustrato i prerequisiti per la configurazione di un computer host di SQL Server in Linux con l'autenticazione di Active Directory. Per completare la configurazione di SQL Server in Linux per il supporto degli account Active Directory, seguire le istruzioni in [Usare l'autenticazione di Azure Active Directory con SQL Server in Linux](sql-server-linux-active-directory-authentication.md).
