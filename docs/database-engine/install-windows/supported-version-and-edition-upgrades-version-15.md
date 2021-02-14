---
title: Aggiornamenti di versione ed edizione supportati (SQL Server 2019)
description: Aggiornamenti di versione ed edizione supportati per SQL Server 2019.
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server], adding to existing installations
- versions [SQL Server], upgrading
- upgrading SQL Server, upgrades supported
- cross-language support
ms.assetid: 702359c4-6ca9-42a8-860c-a95a802898a1
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: f2ba32f7f3c8defa21fdb8b035fc96e64a023159
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341813"
---
# <a name="supported-version--edition-upgrades-sql-server-2019"></a>Aggiornamenti di versione ed edizione supportati (SQL Server 2019)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  È possibile eseguire l'aggiornamento da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]. In questo articolo sono elencati i percorsi di aggiornamento supportati da queste versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e gli aggiornamenti di edizione supportati per [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)].  
  
## <a name="pre-upgrade-checklist"></a>Elenco di controllo preliminare all'aggiornamento  

- Prima di effettuare l'aggiornamento da un'edizione all'altra di [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] , verificare che le funzionalità attualmente utilizzate siano supportate nell'edizione da aggiornare.  
- Verificare la disponibilità di [hardware e software](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md) supportati.
- Prima di aggiornare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], abilitare l'autenticazione di Windows per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent e verificare la configurazione predefinita, ovvero che l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sia membro del gruppo sysadmin di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
- Per effettuare l'aggiornamento a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)], è necessario che nel computer sia in esecuzione un sistema operativo supportato. Per altre informazioni, vedere [Requisiti hardware e software per l'installazione di SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md).  
- L'aggiornamento verrà bloccato se è un riavvio è sospeso.  
- L'aggiornamento verrà bloccato se il servizio Windows Installer non è in esecuzione.

## <a name="unsupported-scenarios"></a>Scenari non supportati

- La presenza di istanze di versioni diverse di [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] non è supportata. I numeri di versione dei componenti del [!INCLUDE[ssDE](../../includes/ssde-md.md)] devono essere identici in un'istanza di [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)].  
  
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] è disponibile solo per piattaforme a 64 bit. L'aggiornamento tra piattaforme diverse non è supportato. Non è possibile aggiornare un'istanza a 32 bit di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un'istanza a 64 bit nativa tramite il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Tuttavia, è possibile eseguire il backup o scollegare database da un'istanza a 32 bit di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], quindi ripristinarli o collegarli a una nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (64 bit) se i database non sono pubblicati in replica. È necessario ricreare tutti gli account di accesso e gli altri oggetti utente nei database di sistema master, msdb e model.  
  
- Non è possibile aggiungere nuove funzionalità durante l'aggiornamento dell'istanza esistente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dopo aver aggiornato un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)], è possibile aggiungere funzionalità tramite il programma di installazione di [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]. Per altre informazioni, vedere [Aggiungere funzionalità a un'istanza di SQL Server &#40;programma di installazione&#41;](./add-features-to-an-instance-of-sql-server-setup.md).  
 
## <a name="upgrades-from-earlier-versions-to-sssql19-md"></a>Aggiornamenti da versioni precedenti a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]  
 
[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] supporta l'aggiornamento dalle versioni di SQL Server seguenti:

- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 o versioni successive
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3 o versioni successive
- [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] SP2 o versioni successive
- [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]

Nella tabella seguente sono elencati gli scenari di aggiornamento supportati da versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)].  
  
|Aggiornamento da|Percorso di aggiornamento supportato|  
|:------|:------|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Enterprise|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Developer|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/>[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Standard|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Web|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Express |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express <br/> <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Business Intelligence|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Evaluation|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Copia di valutazione <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Enterprise|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Developer|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Standard|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Web|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Express |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Business Intelligence|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Valutazione di SP2|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Copia di valutazione <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Enterprise|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Developer|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Standard|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Web|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Express |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Business Intelligence|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Evaluation|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Copia di valutazione <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Enterprise|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Developer|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Standard|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Web|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Express |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Business Intelligence|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Copia di valutazione|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Copia di valutazione <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] versione finale candidata* |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise |  
|[!INCLUDE[sssqlv15_md](../../includes/sssql19-md.md)] Developer |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise | 

 \* Il supporto Microsoft per l'aggiornamento da software con versioni finali candidate è destinato ai clienti che hanno partecipato al programma Early Adopter.

## <a name="migrate-to-sql-server-2019"></a>Eseguire la migrazione a SQL Server 2019

È possibile eseguire la migrazione dei database da versioni precedenti. È possibile, ad esempio, eseguire la migrazione dei database da [!INCLUDE [sskilimanjaro-md](../../includes/sskilimanjaro-md.md)] a SQL Server 2019.

Per informazioni, vedere [Guida alla migrazione del database di Azure](https://datamigration.microsoft.com/scenario/sql-to-sqlserver).

Per pianificare e implementare la migrazione, usare i suggerimenti e gli strumenti seguenti.

- Strumenti di migrazione: la migrazione è supportata tramite [Data Migration Assistant (DMA)](../../dma/dma-overview.md).
- Backup e ripristino: è possibile ripristinare in SQL Server 2019 una copia di backup effettuata in SQL Server 2008 o SQL Server 2008 R2.
- Log shipping: Il log shipping è supportato se l'istanza primaria esegue SQL Server 2008 SP3 o versione successiva oppure SQL Server 2008 R2 SP2 o versione successiva e l'istanza secondaria esegue SQL Server 2019. 

   > [!WARNING]
   > Se viene eseguito un failover automatico o manuale e l'istanza di SQL Server 2019 diventa primaria, l'istanza di SQL Server 2008 o SQL Server 2008 R2 diventa secondaria e non può ricevere modifiche da quella primaria.

- Caricamento bulk: è possibile eseguire una copia bulk di tabelle da SQL Server 2008 o SQL Server 2008 R2 a SQL Server 2019.

## <a name="sssql19-md-edition-upgrade"></a>[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Aggiornamento delle edizioni 

Nella tabella seguente sono elencati gli scenari di aggiornamento delle edizioni supportati in [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)].  

Per istruzioni dettagliate sull'esecuzione di un aggiornamento dell'edizione, vedere [Eseguire l'aggiornamento a un'edizione diversa di SQL Server &#40;programma di installazione&#41;](../../database-engine/install-windows/upgrade-to-a-different-edition-of-sql-server-setup.md).  
  
|Aggiornamento da|Aggiornamento a|  
|------------------|----------------|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (Server+CAL e Core)**|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise |  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Evaluation Enterprise**|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licenza Server+CAL o Core) <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> L'aggiornamento da Evaluation (edizione gratuita) a qualsiasi edizione a pagamento è supportato per le installazioni autonome, ma non per le installazioni cluster. Questa limitazione non si applica alle istanze autonome installate in un cluster di failover Windows che partecipa a un gruppo di disponibilità. |  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard**|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licenza Server+CAL o Core)|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer**|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licenza Server+CAL o Core) <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licenza Server+CAL o Core) <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express*|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licenza Server+CAL o Core) <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|  
  
 È inoltre possibile inoltre eseguire un aggiornamento dell'edizione tra [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (Server+licenza CAL) e [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licenza Core):  
  
|Aggiornamento dell'edizione da|Aggiornamento delle edizioni a|  
|--------------------------|------------------------|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licenza Server+CAL)**|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licenza Core)|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licenza Core)|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licenza Server+CAL)|  
  
 \* Applicabile anche a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express with Tools e [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express with Advanced Services.  
  
 ** La modifica dell'edizione di un cluster di failover di [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] prevede alcune limitazioni. Gli scenari seguenti non sono supportati per i cluster di failover di [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] :  
  
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Da Enterprise a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer, Standard o Evaluation.  
  
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Da Developer a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard o Evaluation.  
  
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Da Standard a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Evaluation.  
  
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Da Evaluation a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard.  
  
## <a name="see-also"></a>Vedere anche  

 [Edizioni e funzionalità supportate di [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]](../../sql-server/editions-and-components-of-sql-server-version-15.md)

 [Requisiti hardware e software per l'installazione di SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)

 [Eseguire l'aggiornamento di SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)