---
description: Definire le opzioni del passaggio di processo Transact-SQL
title: Definire le opzioni del passaggio di processo Transact-SQL
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL job step
- job steps [Transact-SQL]
- SQL Server Agent jobs, Transact-SQL step
ms.assetid: b2a47057-f6fb-432b-a7b6-5d61f33a5d9c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 6d129a33d373bf85c1773a65851d413f0503bcae
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234167"
---
# <a name="define-transact-sql-job-step-options"></a>Definire le opzioni del passaggio di processo Transact-SQL
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come definire le opzioni per i passaggi del processo [!INCLUDE[tsql](../../includes/tsql-md.md)] di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o SQL Server Management Objects.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-define-transact-sql-job-step-options"></a>Per definire le opzioni del passaggio di processo Transact-SQL  
  
1.  In **Esplora oggetti** espandere **SQL Server Agent** e **Processi**, fare clic con il pulsante destro del mouse sul processo da modificare e quindi selezionare **Proprietà**.  
  
2.  Selezionare la pagina **Passaggi** , fare clic su un passaggio processo e quindi su **Modifica**.  
  
3.  Nella finestra di dialogo **Proprietà passaggio processo** verificare che il tipo di processo è **Transact-SQL Script (TSQL)** e quindi selezionare la pagina **Avanzate** .  
  
4.  Specificare un'operazione da eseguire se il processo ha esito positivo dall'elenco **Azione in caso di esito positivo** .  
  
5.  Specificare un numero di tentativi digitando un numero compreso tra 0 e 9999 nella casella **Numero tentativi** .  
  
6.  Specificare un intervallo di tentativi digitando un numero di minuti compreso tra 0 e 9999 nella casella **Intervallo tentativi** .  
  
7.  Specificare un'operazione da eseguire se il processo ha esito negativo dall'elenco **Azione in caso di esito negativo** .  
  
8.  Se il processo è uno script [!INCLUDE[tsql](../../includes/tsql-md.md)] , è possibile scegliere una delle opzioni seguenti:  
  
    -   Immettere il nome di un **File di output**. Per impostazione predefinita il file viene sovrascritto a ogni esecuzione del passaggio processo. Per evitarlo, selezionare **Accoda output a file esistente**. L'opzione è disponibile solo ai membri del ruolo predefinito del server **sysadmin** . Si noti che [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] non consente agli utenti di visualizzare file arbitrari nel file system, quindi non è possibile utilizzare [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] per visualizzare i log dei passaggi scritti nel file system.  
  
    -   Selezionare **Registra nella tabella** per registrare il passaggio processo in una tabella di database. Per impostazione predefinita il contenuto della tabella viene sovrascritto a ogni esecuzione del passaggio processo. Per evitarlo, selezionare **Accoda output a voce esistente nella tabella**. Dopo l'esecuzione del passaggio processo, è possibile visualizzare il contenuto della tabella facendo clic su **Visualizza**.  
  
    -   Selezionare **Includi output passaggio nella cronologia** se si desidera includere l'output nella cronologia dei passaggi. L'output verrà visualizzato solo se non si sono verificati errori. È inoltre possibile che l'output sia troncato.  
  
9. Se l'utente è membro del ruolo predefinito del server **sysadmin** e intende eseguire questo passaggio di processo con un diverso account di accesso SQL, selezionare l'account di accesso SQL dall'elenco **Esegui come utente** .  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilizzo di SQL Server Management Objects  
**Per definire le opzioni del passaggio di processo Transact-SQL**  
  
Usare la classe **JobStep** tramite un linguaggio di programmazione come Visual Basic, Visual C# o PowerShell.  
