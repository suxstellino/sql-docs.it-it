---
title: Esecuzione di passaggi di Windows PowerShell in SQL Server Agent
description: Informazioni su come eseguire passaggi di Windows PowerShell in un processo di SQL Server Agent.
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: f25f7549-c9b3-4618-85f2-c9a08adbe0e3
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.openlocfilehash: a4ca54d68e62c4d9691df15d20261ec850f499c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062426"
---
# <a name="run-windows-powershell-steps-in-sql-server-agent"></a>Esecuzione di passaggi di Windows PowerShell in SQL Server Agent

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

Usare SQL Server Agent per eseguire gli script di SQL Server PowerShell in orari pianificati.  
  
**Per eseguire PowerShell da SQL Server Agent tramite:**  [passaggio del processo di PowerShell](#PShellJob), [passaggio del processo del prompt dei comandi](#CmdExecJob)  
  
[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Sono disponibili molti tipi di passaggi del processo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent. Ogni tipo è associato a un sottosistema che implementa un ambiente specifico, ad esempio un agente di replica o un ambiente del prompt dei comandi. È possibile codificare gli script di Windows PowerShell, quindi utilizzare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent per includere gli script nei processi eseguiti in base a orari pianificati o in risposta a eventi di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . È possibile eseguire gli script di Windows PowerShell con un passaggio del processo del prompt dei comandi o di PowerShell.  

- Usare un passaggio del processo di PowerShell per fare in modo che il sottosistema [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent esegua l'utilità **sqlps** , che avvia PowerShell e importa il modulo **sqlps** .

- Usare un passaggio del processo del prompt dei comandi per eseguire PowerShell.exe e specificare uno script che importa il modulo **sqlps** .

### <a name="caution-about-memory-consumption"></a><a name="LimitationsRestrictions"></a> Attenzione all'utilizzo della memoria

Ogni passaggio del processo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent che esegue PowerShell con il modulo **sqlps** avvia un processo che usa circa **20 MB** di memoria. L'esecuzione simultanea di numerosi passaggi del processo di Windows PowerShell può avere un impatto negativo sulle prestazioni.  

[!INCLUDE[Freshness](../includes/paragraph-content/fresh-note-steps-feedback.md)]

##  <a name="create-a-powershell-job-step"></a><a name="PShellJob"></a> Creare un passaggio del processo di PowerShell  
 **Per creare un passaggio del processo di PowerShell**  
  
1.  Espandere **SQL Server Agent**, creare un nuovo processo oppure fare clic con il pulsante destro del mouse su un processo esistente e quindi scegliere **Proprietà**. Per ulteriori informazioni sulla creazione di un processo, vedere [Creazione di processi](../ssms/agent/create-jobs.md).  
  
2.  Nella finestra di dialogo **Proprietà processo** fare clic sulla pagina **Passaggi** e quindi su **Nuovo**.  
  
3.  Nella finestra di dialogo **Nuovo passaggio di processo** digitare il nome del passaggio del processo nella casella **Nome passaggio**.  
  
4.  Scegliere **PowerShell** dall'elenco **Tipo**.  
  
5.  Nell'elenco **Esegui come** selezionare l'account proxy con le credenziali che verranno utilizzate dal processo.  
  
6.  Nella casella **Comando** immettere la sintassi dello script di PowerShell che verrà eseguito per il passaggio di processo. In alternativa, è possibile fare clic su **Apri** e selezionare un file contenente la sintassi dello script.  
  
7.  Fare clic sulla pagina **Avanzate** per impostare le opzioni seguenti relative al passaggio di processo: l'azione da eseguire in caso di esito positivo o negativo del passaggio, il numero di tentativi di esecuzione del passaggio che devono essere effettuati da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent e gli intervalli tra tentativi successivi.  
  
##  <a name="create-a-command-prompt-job-step"></a><a name="CmdExecJob"></a> Creare un passaggio del processo del prompt dei comandi  
 **Per creare un passaggio del processo di CmdExec**  
  
1.  Espandere **SQL Server Agent**, creare un nuovo processo oppure fare clic con il pulsante destro del mouse su un processo esistente e quindi scegliere **Proprietà**. Per ulteriori informazioni sulla creazione di un processo, vedere [Creazione di processi](../ssms/agent/create-jobs.md).  
  
2.  Nella finestra di dialogo **Proprietà processo** fare clic sulla pagina **Passaggi** e quindi su **Nuovo**.  
  
3.  Nella finestra di dialogo **Nuovo passaggio di processo** digitare il nome del passaggio del processo nella casella **Nome passaggio**.  
  
4.  Nell'elenco **Tipo** selezionare **Sistema operativo (CmdExec)** .  
  
5.  Nell'elenco **Esegui come** selezionare l'account proxy con le credenziali che verranno utilizzate dal processo. Per impostazione predefinita, questi passaggi di processo vengono eseguiti nel contesto dell'account di servizio SQL Server Agent.  
  
6.  Nella casella **Elabora codice di uscita di un comando eseguito correttamente** digitare un valore compreso tra 0 e 999999.  
  
7.  Nella casella **Comando** , immettere powershell.exe con parametri che specificano lo script di PowerShell da eseguire.  
  
8.  Fare clic sulla pagina **Avanzate** per impostare le opzioni relative ai passaggi di processo, ad esempio l'operazione da eseguire se il passaggio di processo ha esito positivo o negativo, il numero di tentativi che devono essere eseguiti da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent per l'esecuzione del passaggio di processo e il file in cui [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent può scrivere l'output del passaggio di processo. L'output del passaggio di processo può essere scritto in un file di sistema unicamente dai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server PowerShell](sql-server-powershell.md)  
  
  
