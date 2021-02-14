---
title: Definire la pianificazione per i pacchetti SSIS in Linux con cron
description: Questo articolo descrive come eseguire la pianificazione di pacchetti di SQL Server Integration Services (SSIS) in Linux con il servizio cron.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 75ac9be9d128f76157cee054674ecfe29de06063
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346437"
---
# <a name="schedule-sql-server-integration-services-package-execution-on-linux-with-cron"></a>Pianificare l'esecuzione del pacchetto SQL Server Integration Services in Linux con cron

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Se SQL Server Integration Services (SSIS) e SQL Server vengono eseguiti in Windows, è possibile automatizzare l'esecuzione dei pacchetti SSIS tramite SQL Server Agent. Se vengono eseguiti in Linux, tuttavia, l'utilità SQL Server Agent per la pianificazione dei processi non è disponibile. In questo caso si usa il servizio cron, ampiamente usato nelle piattaforme Linux per automatizzare l'esecuzione dei pacchetti.

Questo articolo offre alcuni esempi che illustrano come automatizzare l'esecuzione di pacchetti SSIS. Gli esempi sono destinati all'esecuzione in Red Hat Enterprise ma il codice per altre distribuzioni di Linux, ad esempio per Ubuntu, è simile.

## <a name="prerequisites"></a>Prerequisites

Prima di usare il servizio cron per eseguire i processi, controllare se è in esecuzione nel computer.

Per controllare lo stato del servizio cron, usare il comando seguente: `systemctl status crond.service`.

Se il servizio non è attivo, ovvero non è in esecuzione, rivolgersi all'amministratore per installare e configurare correttamente il servizio cron.

## <a name="create-jobs"></a>Creare processi

Un processo cron è un'attività di cui è possibile configurare l'esecuzione regolare in base a un intervallo specificato. Il processo può essere semplicemente un comando normalmente digitato direttamente nella console o può essere eseguito come script della shell.

Per semplificare la gestione e la manutenzione, è consigliabile inserire i comandi per l'esecuzione dei pacchetti in uno script con un nome descrittivo.

Ecco un esempio di un semplice script della shell per l'esecuzione di un pacchetto. Contiene un unico comando, ma è possibile aggiungere altri comandi in base alle esigenze.

```bash
# A simple shell script that contains a simple package execution command
# Script name: SSISpackageName.daily

/opt/ssis/bin/dtexec /F yourSSISpackageName.dtsx >> $HOME/tmp/out 2>&1
```

## <a name="schedule-jobs-with-the-cron-service"></a>Pianificare i processi con il servizio cron

Dopo aver definito i processi, è possibile pianificarne l'esecuzione automatica usando il servizio cron.

Per aggiungere il processo a cron per l'esecuzione, aggiungerlo nel file crontab. Per aprire il file crontab in un editor in cui sia possibile aggiungere o aggiornare il processo, usare il comando seguente `crontab -e`.

Per pianificare l'esecuzione del processo descritto in precedenza ogni giorno alle 2.10, aggiungere la riga seguente al file crontab:

```
# run <SSIS package name> at 2:10 AM every day
10 2 \* \* \* $/HOME/SSIS/jobs/SSISpackageName.daily
```

Salvare il file crontab e quindi uscire dall'editor.

Per comprendere il formato del comando di esempio, vedere le informazioni della sezione seguente.
 
## <a name="format-of-a-crontab-file"></a>Formato di un file crontab

L'immagine seguente offre la descrizione del formato della riga del processo aggiunta al file crontab.

![Descrizione del formato per la voce nel file crontab](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-job-definition.png)

Per una descrizione più dettagliata del formato del file crontab, usare il comando seguente: `man 5 crontab`.

Ecco un esempio parziale dell'output che facilita la comprensione dell'esempio in questo articolo:

![Descrizione parziale dettagliata del formato di crontab](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-crontab-format.png)

## <a name="related-content-about-ssis-on-linux"></a>Contenuto correlato su SSIS in Linux
-   [Estrarre, trasformare e caricare i dati in Linux con SSIS](sql-server-linux-migrate-ssis.md)
-   [Installare SQL Server Integration Services (SSIS) in Linux](sql-server-linux-setup-ssis.md)
-   [Configurare SQL Server Integration Services in Linux con ssis-conf](sql-server-linux-configure-ssis.md)
-   [Limitazioni e problemi noti di SSIS in Linux](sql-server-linux-ssis-known-issues.md)
