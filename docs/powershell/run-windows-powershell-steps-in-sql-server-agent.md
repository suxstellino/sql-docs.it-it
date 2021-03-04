---
title: Esecuzione di passaggi di Windows PowerShell in SQL Server Agent
description: Informazioni su come eseguire passaggi di Windows PowerShell in un processo di SQL Server Agent.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.openlocfilehash: 8029d18dee3dd49342c3c029fc78992900394ed4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839405"
---
# <a name="run-windows-powershell-steps-in-sql-server-agent"></a>Esecuzione di passaggi di Windows PowerShell in SQL Server Agent

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

Usare SQL Server Agent per eseguire gli script di SQL Server PowerShell in orari pianificati.

[!INCLUDE[sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

[!INCLUDE[sql-server-powershell-no-sqlps](../includes/sql-server-powershell-no-sqlps.md)]

## <a name="to-run-powershell-from-sql-server-agent"></a>Per eseguire PowerShell da SQL Server Agent

Sono disponibili diversi tipi di SQL Server Agent passaggi di processo. Ogni tipo è associato a un sottosistema che implementa un ambiente specifico, ad esempio un agente di replica o un ambiente del prompt dei comandi. È possibile codificare gli script di Windows PowerShell, quindi usare SQL Server Agent per includere gli script nei processi eseguiti in orari pianificati o in risposta a eventi di SQL Server. È possibile eseguire gli script di Windows PowerShell con un passaggio del processo del prompt dei comandi o di PowerShell.

- Usare un passaggio del processo di PowerShell per fare in modo che il SQL Server Agent sottosistema esegua l'utilità **sqlps** , che avvia PowerShell e importa il modulo **sqlps** . Se si esegue SQL Server 2019 o versione successiva, è consigliabile usare il modulo **[SqlServer](sql-server-powershell.md#sql-server-agent)** nel passaggio del processo di SQL Agent.

- Usare un passaggio del processo del prompt dei comandi per eseguire PowerShell.exe e specificare uno script che importa il modulo **sqlps** .

### <a name="caution-about-memory-consumption"></a><a name="LimitationsRestrictions"></a> Attenzione all'utilizzo della memoria

Ogni passaggio del processo di SQL Server Agent che esegue PowerShell con il modulo **sqlps** avvia un processo che utilizza circa **20 MB** di memoria. L'esecuzione simultanea di numerosi passaggi del processo di Windows PowerShell può avere un impatto negativo sulle prestazioni.

## <a name="create-a-powershell-job-step"></a><a name="PShellJob"></a> Creare un passaggio del processo di PowerShell

### <a name="to-create-a-powershell-job-step"></a>Per creare un passaggio del processo di PowerShell

1. Espandere **SQL Server Agent**, creare un nuovo processo o fare clic con il pulsante destro del mouse su un processo esistente, quindi scegliere **Proprietà**. Per ulteriori informazioni sulla creazione di un processo, vedere [Creazione di processi](../ssms/agent/create-jobs.md).

2. Nella finestra di dialogo **Proprietà processo** selezionare la pagina **passaggi** , quindi selezionare **nuovo**.

3. Nella finestra di dialogo **Nuovo passaggio di processo** digitare il nome del passaggio del processo nella casella **Nome passaggio**.

4. Nell'elenco **tipo** selezionare **PowerShell**.

5. Nell'elenco **Esegui come** selezionare l'account proxy con le credenziali che verranno utilizzate dal processo.

6. Nella casella **Comando** immettere la sintassi dello script di PowerShell che verrà eseguito per il passaggio di processo. In alternativa, selezionare **Apri** e selezionare un file contenente la sintassi dello script.

7. Selezionare la pagina **Avanzate** per impostare le opzioni seguenti relative al passaggio di processo: l'azione da eseguire se il passaggio del processo ha esito positivo o negativo, il numero di volte in cui SQL Server Agent deve tentare di eseguire il passaggio del processo e la frequenza con cui effettuare nuovi tentativi.

## <a name="create-a-command-prompt-job-step"></a><a name="CmdExecJob"></a> Creare un passaggio del processo del prompt dei comandi

### <a name="to-create-a-cmdexec-job-step"></a>Per creare un passaggio del processo di CmdExec

1. Espandere **SQL Server Agent**, creare un nuovo processo o fare clic con il pulsante destro del mouse su un processo esistente, quindi scegliere **Proprietà**. Per ulteriori informazioni sulla creazione di un processo, vedere [Creazione di processi](../ssms/agent/create-jobs.md).

2. Nella finestra di dialogo **Proprietà processo** selezionare la pagina **passaggi** , quindi selezionare **nuovo**.

3. Nella finestra di dialogo **Nuovo passaggio di processo** digitare il nome del passaggio del processo nella casella **Nome passaggio**.

4. Nell'elenco **Tipo** selezionare **Sistema operativo (CmdExec)** .

5. Nell'elenco **Esegui come** selezionare l'account proxy con le credenziali che verranno utilizzate dal processo. Per impostazione predefinita, questi passaggi di processo vengono eseguiti nel contesto dell'account di servizio SQL Server Agent.

6. Nella casella **Elabora codice di uscita di un comando eseguito correttamente** digitare un valore compreso tra 0 e 999999.

7. Nella casella **Comando** , immettere powershell.exe con parametri che specificano lo script di PowerShell da eseguire.

8. Selezionare la pagina **Avanzate** per impostare le opzioni del passaggio di processo, ad esempio l'azione da eseguire se il passaggio del processo ha esito positivo o negativo, il numero di volte in cui SQL Server Agent deve tentare di eseguire il passaggio del processo e il file in cui SQL Server Agent può scrivere l'output del passaggio di processo. L'output del passaggio di processo può essere scritto in un file di sistema unicamente dai membri del ruolo predefinito del server **sysadmin** .

## <a name="see-also"></a>Vedere anche

- [SQL Server PowerShell](sql-server-powershell.md)
- [SQL Server Agent con PowerShell](sql-server-powershell.md#sql-server-agent)