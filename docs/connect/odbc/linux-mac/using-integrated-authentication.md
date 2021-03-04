---
title: Uso dell'autenticazione integrata
description: Microsoft ODBC Driver for SQL Server in Linux e macOS supporta le connessioni che usano l'autenticazione integrata Kerberos.
ms.custom: ''
ms.date: 09/01/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- integrated authentication
ms.assetid: 9499ffdf-e0ee-4d3c-8bca-605371eb52d9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4e918602b3793d95d6192a832f110500454ee8a5
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837374"
---
# <a name="using-integrated-authentication"></a>Uso dell'autenticazione integrata
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

[!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Linux e macOS supporta le connessioni che usano l'autenticazione integrata Kerberos. Supporta il centro distribuzione chiavi di MIT Kerberos e può essere usato con GSSAPI (Generic Security Services Application Program Interface) e librerie Kerberos v5.

A partire dalla versione 17.6 il driver supporta anche l'autenticazione integrata con Azure Active Directory usando un account federato, nonostante le limitazioni della libreria di sistema. Per altre informazioni, vedere [Uso di Azure Active Directory](../using-azure-active-directory.md).

## <a name="using-integrated-authentication-to-connect-to-ssnoversion-from-an-odbc-application"></a>Uso dell'autenticazione integrata per la connessione a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] da un'applicazione ODBC  

È possibile abilitare l'autenticazione integrata di Kerberos specificando **Trusted_Connection = yes** nella stringa di connessione di **SQLDriverConnect** o **SQLConnect**. Ad esempio:  

```
Driver='ODBC Driver 17 for SQL Server';Server=your_server;Trusted_Connection=yes  
```
  
Quando si esegue la connessione con un DSN è anche possibile aggiungere **Trusted_Connection = yes** alla voce del DSN in `odbc.ini`.
  
È anche possibile usare l'opzione `-E` di `sqlcmd` e l'opzione `-T` di `bcp` per specificare l'autenticazione integrata. Per altre informazioni, vedere [Connecting with **sqlcmd**](connecting-with-sqlcmd.md) (Connessione con sqlcmd) e [Connecting with **bcp**](connecting-with-bcp.md) (Connessione con bcp).

Assicurarsi che l'entità di sicurezza del client che esegue la connessione a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sia già stata autenticata con Kerberos KDC.
  
**ServerSPN** e **FailoverPartnerSPN** non sono supportati.  
  
## <a name="deploying-a-linux-or-macos-odbc-driver-application-designed-to-run-as-a-service"></a>Distribuzione di un'applicazione driver ODBC per Linux o macOS progettata per l'esecuzione come servizio

Un amministratore di sistema può distribuire un'applicazione da eseguire come servizio che usa l'autenticazione Kerberos per connettersi a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
È necessario innanzitutto configurare Kerberos nel client e quindi verificare che l'applicazione possa usare le credenziali Kerberos dell'entità di sicurezza predefinita.

Assicurarsi di usare `kinit` o PAM (Pluggable Authentication Module) per ottenere e memorizzare nella cache il TGT per l'entità usata dalla connessione tramite uno dei metodi seguenti:  
  
-   Eseguire `kinit` specificando il nome di un'entità di sicurezza e la password.  
  
-   Eseguire `kinit` specificando il nome di un'entità di sicurezza e il percorso del file keytab contenente la chiave dell'entità creata da `ktutil`.  
  
-   Assicurarsi che l'accesso al sistema sia stato eseguito usando Kerberos PAM (Pluggable Authentication Module).

Quando un'applicazione viene eseguita come servizio, poiché le credenziali Kerberos hanno una scadenza per impostazione predefinita, rinnovare le credenziali per garantire una disponibilità continua del servizio. Il driver ODBC non rinnova le credenziali in modo autonomo. Assicurarsi che sia presente un processo `cron` o uno script eseguito regolarmente per rinnovare le credenziali prima della scadenza. Per evitare di richiedere la password per ogni rinnovo è possibile usare un file keytab.  
  
La pagina relativa a[configurazione e uso di Kerberos](https://commons.oreilly.com/wiki/index.php/Linux_in_a_Windows_World/Centralized_Authentication_Tools/Kerberos_Configuration_and_Use) fornisce informazioni dettagliate sull'uso di Kerberos per i servizi in Linux.
  
## <a name="tracking-access-to-a-database"></a>Rilevamento dell'accesso a un database

Un amministratore del database può creare un audit trail di accesso a un database quando vengono usati account di sistema per accedere a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con l'autenticazione integrata.  
  
L'accesso a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa l'account di sistema e non vi è alcuna funzionalità in Linux per rappresentare il contesto di sicurezza. Sono pertanto necessari più dati per determinare l'utente.
  
Per controllare le attività in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per conto di utenti diversi dagli account di sistema, è necessario che l'applicazione usi l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] **EXECUTE AS**.  
  
Per migliorare le prestazioni, l'applicazione può usare pool di connessioni con autenticazione integrata e controllo. Tuttavia, la combinazione di pool di connessioni, autenticazione integrata e controllo crea un rischio per la sicurezza perché il programma di gestione dei driver unixODBC consente a utenti diversi di riutilizzare le connessioni in pool. Per altre informazioni, vedere la pagina relativa ai [pool di connessioni ODBC](http://www.unixodbc.org/doc/conn_pool.html).  

Prima del riutilizzo, è necessario che l'applicazione reimposti le connessioni in pool eseguendo `sp_reset_connection`.  

## <a name="using-active-directory-to-manage-user-identities"></a>Uso di Active Directory per la gestione delle identità utente

Non è necessario che l'amministratore di sistema dell'applicazione gestisca più set di credenziali di accesso per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. È possibile configurare Active Directory come centro distribuzione chiavi (KDC) per l'autenticazione integrata. Per altre informazioni, vedere [Microsoft Kerberos](/windows/desktop/SecAuthN/microsoft-kerberos).

## <a name="using-linked-server-and-distributed-queries"></a>Uso di un server collegato e di query distribuite

Gli sviluppatori possono distribuire un'applicazione che usa un server collegato o query distribuite senza che l'amministratore di database gestisca più set di credenziali SQL. In questo caso, è necessario che lo sviluppatore configuri l'applicazione per l'uso dell'autenticazione integrata:  
  
-   L'utente accede a un computer client ed esegue l'autenticazione nel server applicazioni.  
  
-   Il server applicazioni esegue l'autenticazione come database diverso e si connette a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esegue l'autenticazione come utente di database in un altro database ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
Dopo aver configurato l'autenticazione integrata, le credenziali vengono passate al server collegato.  
  
## <a name="integrated-authentication-and-sqlcmd"></a>Autenticazione integrata e sqlcmd
Per accedere a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con l'autenticazione integrata, usare l'opzione `-E` di `sqlcmd`. Verificare che l'account che esegue `sqlcmd` sia associato al client dell'entità Kerberos predefinita.

## <a name="integrated-authentication-and-bcp"></a>Autenticazione integrata e bcp
Per accedere a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con l'autenticazione integrata, usare l'opzione `-T` di `bcp`. Verificare che l'account che esegue `bcp` sia associato al client dell'entità Kerberos predefinita. 
  
Non è possibile usare `-T` con l'opzione `-U` o `-P`.
  
## <a name="supported-syntax-for-an-spn-registered-by-ssnoversion"></a>Sintassi supportata per un nome SPN registrato da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

Di seguito è indicata la sintassi usata per i nomi SPN nella stringa di connessione o negli attributi di connessione:  

|Sintassi|Descrizione|  
|----------|---------------|  
|MSSQLSvc/*fqdn*:*port*|Nome SPN predefinito generato dal provider quando si utilizza il protocollo TCP. *port* è un numero di porta TCP. *fqdn* è un nome di dominio completo.|  
  
## <a name="authenticating-a-linux-or-macos-computer-with-active-directory"></a>Autenticazione di un computer Linux o macOS con Active Directory

<a name="configure-kerberos"></a>Per configurare Kerberos, immettere i dati nel `krb5.conf` file. `krb5.conf` è in `/etc/` ma è possibile fare riferimento a un altro file tramite la sintassi, ad esempio `export KRB5_CONFIG=/home/dbapp/etc/krb5.conf`. Di seguito è riportato un file `krb5.conf` di esempio:  
  
```  
[libdefaults]  
default_realm = YYYY.CORP.CONTOSO.COM  
dns_lookup_realm = false  
dns_lookup_kdc = true  
ticket_lifetime = 24h  
forwardable = yes  
  
[domain_realm]  
.yyyy.corp.contoso.com = YYYY.CORP.CONTOSO.COM  
.zzzz.corp.contoso.com = ZZZZ.CORP.CONTOSO.COM  
```  
  
Se il computer Linux o macOS è configurato per l'uso del protocollo DHCP (Dynamic Host Configuration Protocol) con un server DHCP di Windows che specifica i server DNS da usare, è possibile usare **dns_lookup_kdc=true**. A questo punto è possibile usare Kerberos per accedere al dominio attivando il comando `kinit alias@YYYY.CORP.CONTOSO.COM`. I parametri passati a `kinit` fanno distinzione tra maiuscole e minuscole e il computer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] configurato nel dominio deve includere l'utente `alias@YYYY.CORP.CONTOSO.COM` per l'accesso. È ora possibile usare connessioni trusted (**Trusted_Connection=YES** in una stringa di connessione, **bcp -T** o **sqlcmd -E**).  
  
L'ora nel computer Linux o macOS e l'ora nel Centro distribuzione chiavi (KDC) Kerberos devono essere simili. Verificare che l'ora di sistema sia impostata correttamente, ad esempio usando il protocollo NTP (Network Time Protocol).  

Se l'autenticazione Kerberos non riesce, il driver ODBC in Linux o macOS non usa l'autenticazione NTLM.  

Per altre informazioni sull'autenticazione di computer Linux o macOS con Active Directory, vedere [Autenticare client Linux con Active Directory](/previous-versions/technet-magazine/dd228986(v=msdn.10)#id0060048). Per altre informazioni sulla configurazione di Kerberos, vedere la [documentazione di MIT Kerberos](https://web.mit.edu/kerberos/krb5-1.12/doc/index.html).

## <a name="see-also"></a>Vedere anche  
[Linee guida per la programmazione](programming-guidelines.md)

[Note sulla versione](release-notes-odbc-sql-server-linux-mac.md)

[Uso di Azure Active Directory](../using-azure-active-directory.md)