---
title: Configurare SSIS in Linux con ssis-conf
description: Questo articolo descrive come configurare SQL Server Integration Services (SSIS) in Linux con l'utilità ssis-conf.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: da546bddb3b7b45753b224539a546f96663704c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348194"
---
# <a name="configure-sql-server-integration-services-on-linux-with-ssis-conf"></a>Configurare SQL Server Integration Services in Linux con ssis-conf

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Lo script di configurazione `ssis-conf` viene eseguito quando si installa SQL Server Integration Services (SSIS) per Red Hat Enterprise Linux e Ubuntu. Per altre informazioni sull'installazione di SSIS, vedere [Installare SQL Server Integration Services (SSIS) in Linux](sql-server-linux-setup-ssis.md).

È anche possibile usare l'utilità `ssis-conf` per configurare le proprietà seguenti:

| Comando | Descrizione |
|-------------|---------------------------------------------------------------------|
| set-edition | Imposta l'edizione di SQL Server                                       |
| telemetry   | Abilita o disabilita il servizio di telemetria di SQL Server Integration Services |
| installazione       | Installa e configura Microsoft SQL Server Integration Services      |
|||

## <a name="run-ssis-conf"></a>Eseguire ssis-conf

Gli esempi in questo articolo eseguono `ssis-conf` specificando il percorso completo: `/opt/ssis/bin/ssis-conf`. Se si passa a tale percorso prima di eseguire `ssis-conf`, è possibile eseguire l'utilità nel contesto della directory corrente: `./ssis-conf`.

Assicurarsi di eseguire i comandi descritti in questo articolo con privilegi radice. Eseguire ad esempio `sudo /opt/ssis/bin/ssis-conf setup` e non `/opt/ssis/bin/ssis-conf setup`.

Per eseguire questi comandi con prompt nella lingua preferita, è possibile specificare le impostazioni locali corrispondenti. Per visualizzare i prompt in cinese, ad esempio, eseguire il comando seguente: `sudo LC_ALL=zh_CN.UTF-8 /opt/ssis/bin/ssis-conf setup`.

## <a name="use-set-edition-to-set-the-edition-of-sql-server-integration-services"></a>Usare set-edition per impostare l'edizione di SQL Server Integration Services

L'edizione di SSIS è allineata all'edizione di SQL Server.

Immettere il comando seguente: `$ sudo /opt/ssis/bin/ssis-conf set-edition`.

Dopo l'immissione del comando, viene visualizzato il prompt seguente:

```
Choose an edition of SQL Server:

1) Evaluation (free, no production use rights, 180-day limit)

2) Developer (free, no production use rights)

3) Express (free)

4) Web (PAID)

5) Standard (PAID)

6) Enterprise (PAID)

7) Enterprise Core (PAID)

8) I bought a license through a retail sales channel and have a product key to enter.

Details about editions can be found at https://go.microsoft.com/fwlink/?LinkId=852748&clcid=0x409.

Use of PAID editions of this software requires separate licensing through a Microsoft Volume Licensing program.

By choosing a PAID edition, you are verifying that you have the appropriate number of licenses in place to install and run this software.

Enter your edition (1-8):
```

Se si immette un valore compreso tra 1 e 7, il sistema configura un'edizione gratuita o a pagamento. Se si immette il valore 8, l'utilità chiede di immettere il codice Product Key acquistato:

```
Enter the 25-character product key:
```

## <a name="use-telemetry-to-configure-customer-feedback"></a>Usare la telemetria per configurare il feedback dei clienti

Il comando `telemetry` determina se SSIS deve inviare feedback a Microsoft.

Per le edizioni gratuite, ovvero per le edizioni Express Edition, Developer Edition ed Evaluation Edition, il servizio di telemetria è sempre abilitato. Se si ha un'edizione gratuita, non è possibile usare il comando `telemetry` per disabilitare la telemetria.

Immettere il comando seguente: `$ sudo /opt/ssis/bin/ssis-conf telemetry`.

Per le edizioni a pagamento, dopo l'immissione del comando viene visualizzato il prompt seguente:

```
Send feature usage data to Microsoft. Feature usage data includes information about your hardware configuration and how you use SQL Server Integration Services.

[Yes/No]:
```

Se si seleziona **Sì**, il servizio di telemetria viene abilitato e ne viene avviata l'esecuzione. Il servizio si avvia automaticamente dopo ogni avvio. Se si seleziona **No**, il servizio di telemetria si arresta e viene disabilitato.

## <a name="use-setup-to-initialize-and-set-up-microsoft-sql-server-integration-services"></a>Usare setup per installare e configurare Microsoft SQL Server Integration Services

Usare il comando `setup` ogni volta che si installa SSIS.

Immettere il comando seguente: `sudo /opt/ssis/bin/ssis-conf setup`.

L'utilità richiede di confermare o specificare i valori per gli elementi seguenti:
-   Licenza del prodotto
-   Contratto di licenza con l'utente finale
-   Servizio di telemetria
-   Lingua usata da Integration Services

Per eseguire il comando `setup` con prompt nella lingua preferita, è possibile specificare le impostazioni locali corrispondenti. Per visualizzare i prompt in cinese, ad esempio, eseguire il comando seguente: `sudo LC_ALL=zh_CN.UTF-8 /opt/ssis/bin/ssis-conf setup`.

## <a name="ssisconf-format"></a>formato di ssis.conf

Il file `/var/opt/ssis/ssis.conf` seguente offre un esempio per ogni impostazione.

Per SQL Server è possibile modificare le impostazioni di sistema cambiando i valori nel file `mssql.conf`. Per SSIS *non* è possibile modificare le impostazioni di sistema cambiando i valori nel file `ssis.conf`. Il file `ssis.conf` si limita a visualizzare i risultati dell'installazione. Se si vogliono modificare le impostazioni per SSIS, è possibile eliminare il file `ssis.conf` ed eseguire di nuovo il comando `setup`.

Ecco un file `ssis.conf` di esempio. Ogni campo corrisponde al risultato di un passaggio di setup.

```
[LICENSE]
                       
registered = Y        
                       
pid = enterprisecore  
                       
[EULA]
                       
accepteula = Y        
                       
[TELEMETRY]
                       
enabled = Y           
                       
[language]
                       
lcid = 2052
```

## <a name="related-content-about-ssis-on-linux"></a>Contenuto correlato su SSIS in Linux
-   [Estrarre, trasformare e caricare i dati in Linux con SSIS](sql-server-linux-migrate-ssis.md)
-   [Installare SQL Server Integration Services (SSIS) in Linux](sql-server-linux-setup-ssis.md)
-   [Limitazioni e problemi noti di SSIS in Linux](sql-server-linux-ssis-known-issues.md)
-   [Pianificare l'esecuzione del pacchetto SQL Server Integration Services in Linux con cron](sql-server-linux-schedule-ssis-packages.md)
