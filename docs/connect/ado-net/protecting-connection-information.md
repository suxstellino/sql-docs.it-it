---
title: Protezione delle informazioni di connessione
description: Informazioni sulle vulnerabilità di sicurezza nelle stringhe di connessione, che possono verificarsi a seconda di come le stringhe vengono costruite e rese persistenti e al tipo di autenticazione.
ms.date: 11/13/2020
ms.assetid: 1471f580-bcd4-4046-bdaf-d2541ecda2f4
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1d170d712269bf169d069ef7b93f975de855f8ec
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771403"
---
# <a name="protecting-connection-information"></a>Protezione delle informazioni di connessione

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La protezione dell'accesso all'origine dati è uno dei principali obiettivi da raggiungere quando si imposta la sicurezza di un'applicazione. Una stringa di connessione presenta una potenziale vulnerabilità se non è protetta. Se le informazioni di connessione vengono archiviate in testo normale o mantenute nella memoria, si rischia di compromettere l'intero sistema. Le stringhe di connessione incorporate nel codice sorgente possono essere lette tramite [Ildasm.exe (disassembler IL)](/dotnet/framework/tools/ildasm-exe-il-disassembler) per visualizzare il linguaggio MISL (Microsoft Intermediate Language Microsoft) in un assembly compilato.

Le vulnerabilità di sicurezza che riguardano le stringhe di connessione possono presentarsi in base al tipo di autenticazione usato, alla modalità con cui le stringhe di connessione vengono mantenute nella memoria e su disco e alle tecniche usate per crearle in fase di esecuzione.

## <a name="use-windows-authentication"></a>Usare l'autenticazione di Windows

Per limitare l'accesso all'origine dati, è necessario proteggere le informazioni di connessione quali, ad esempio, identificatore utente, password e nome dell'origine dati. Per evitare di esporre informazioni utente, si consiglia di usare l'autenticazione di Windows (definita anche *sicurezza integrata*) laddove possibile. L'autenticazione di Windows viene specificata in una stringa di connessione usando le parole chiave `Integrated Security` o `Trusted_Connection`, eliminando la necessità di usare un ID utente e una password. Tramite l'autenticazione di Windows, gli utenti vengono autenticati da Windows e l'accesso alle risorse di server e database viene determinato concedendo autorizzazioni a utenti e gruppi di Windows.

Nei casi in cui non sia possibile usare l'autenticazione di Windows, è necessario prestare una maggiore attenzione perché le credenziali utente sono esposte nella stringa di connessione. Nelle applicazioni ASP.NET è possibile configurare un account di Windows come identità fissa usata per le connessioni a database e ad altre risorse di rete. La rappresentazione viene abilitata nell'elemento identità nel file **web.config** e vengono specificati un nome utente e una password.

```xml  
<identity impersonate="true"
        userName="MyDomain\UserAccount"
        password="*****" />  
```  

L'account con identità fissa deve avere privilegi limitati che includono solo le autorizzazioni necessarie nel database. È inoltre necessario crittografare il file di configurazione in modo da non esporre il nome utente e la password in testo non crittografato.

## <a name="avoid-injection-attacks-with-connection-string-builders"></a>Evitare gli attacchi injection con i generatori di stringhe di connessione

Un attacco injection alle stringhe di connessione può verificarsi quando si usa la concatenazione dinamica di stringhe per compilare stringhe di connessione basate sull'input dell'utente. Se l'input dell'utente non viene convalidato e il testo o i caratteri dannosi non vengono convertiti in caratteri di escape, un utente non autorizzato potrebbe accedere a dati sensibili o ad altre risorse del server. Per risolvere questo problema, il provider di dati Microsoft SqlClient per SQL ha introdotto nuove classi di generatori di stringhe di connessione per convalidare la sintassi delle stringhe e assicurarsi che non vengano inseriti parametri aggiuntivi. Per altre informazioni, vedere [Compilatori di stringhe di connessione](connection-string-builders.md).

## <a name="use-persist-security-infofalse"></a>Usare "Persist Security Info=false"

Il valore predefinito per `Persist Security Info` è false; si consiglia di usare questo valore in tutte le stringhe di connessione. Se si imposta `Persist Security Info` su `true` o `yes`, è possibile che da una connessione aperta si ottengano informazioni sensibili, tra cui l'ID utente e la password. Se si imposta `Persist Security Info` su `false` o `no`, le informazioni di sicurezza vengono eliminate dopo essere state usate per aprire la connessione. In questo modo un'origine non attendibile non ha accesso alle informazioni sensibili per la sicurezza.

## <a name="encrypt-configuration-files"></a>Crittografare i file di configurazione

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

È anche possibile archiviare le stringhe di connessione in file di configurazione, eliminando la necessità di incorporarle nel codice dell'applicazione. I file di configurazione sono file XML standard per i quali in .NET Framework è stato definito un set comune di elementi. Le stringhe di connessione nei file di configurazione vengono in genere archiviate all'interno dell'elemento **\<connectionStrings>** in **app.config** per le applicazioni Windows o nel file **web.config** per le applicazioni ASP.NET. Per altre informazioni sull'archiviazione, il recupero e la crittografia di stringhe di connessione dai file di configurazione, vedere [Stringhe di connessione e file di configurazione](connection-strings-and-configuration-files.md).

## <a name="see-also"></a>Vedere anche

- [Crittografia delle informazioni di configurazione tramite la configurazione protetta](/previous-versions/aspnet/53tyfkaw(v=vs.100))
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
