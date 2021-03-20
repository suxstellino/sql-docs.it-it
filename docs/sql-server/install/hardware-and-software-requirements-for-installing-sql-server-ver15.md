---
title: 'SQL Server 2019: Requisiti hardware e software'
description: Elenco dei requisiti hardware, software e del sistema operativo per l'installazione e l'esecuzione di SQL Server 2019.
ms.custom: sqlfreshmay19
ms.date: 09/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Setup [SQL Server], software
- software [SQL Server]
- installing SQL Server, software
- operating systems [SQL Server], SQL Server requirements
- Setup [SQL Server], cross-language support
- operating systems [SQL Server], cross-language support
- network connections [SQL Server], requirements
- disk space [SQL Server], SQL Server installations
- WOW [SQL Server]
- Setup [SQL Server], hardware
- dependencies [SQL Server], SQL Server installations
- cluster hardware requirements [SQL Server]
- endpoints [SQL Server], SQL Server installations
- Internet [SQL Server], SQL Server installations
- hardware [SQL Server]
- Windows on Windows [SQL Server]
- installing SQL Server, hardware
- Setup Configuration Checker
- SCC [SQL Server]
- operating systems [SQL Server]
- space [SQL Server], SQL Server installations
- system configuration checker
- installing SQL Server, cross-language support
- Internet [SQL Server]
- space [SQL Server]
- extended system support [SQL Server]
- 64-bit edition [SQL Server]
- failover clustering [SQL Server]
- failover clustering [SQL Server], hardware requirements
- 32-bit edition [SQL Server]
- locales [SQL Server], SQL Server installations
- cross-language support
- disk space [SQL Server]
- localized SQL Server versions
ms.assetid: 09bcf20b-0a40-4131-907f-b61479d5e4d8
ms.author: chadam
author: cawrites
ms.openlocfilehash: b7def7a7f58d3fe2124545171c5858a012e4642b
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673985"
---
# <a name="sql-server-2019-hardware-and-software-requirements"></a>SQL Server 2019: Requisiti hardware e software
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Questo articolo elenca i requisiti hardware e software minimi per l'installazione e l'esecuzione di [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] nel sistema operativo Windows.

Per informazioni sui requisiti hardware e software per le altre versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere:
- [[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016 e 2017](hardware-and-software-requirements-for-installing-sql-server.md)
- [[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  in Linux](../../linux/sql-server-linux-setup.md#system)
- [Cluster Big Data](../../big-data-cluster/deployment-guidance.md)

##  <a name="hardware-requirements"></a><a name="pmosr"></a> Requisiti hardware  
 I seguenti requisiti di memoria e processore si applicano a tutte le edizioni di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]:  
  
|Componente|Requisito|  
|---------------|-----------------|  
|Disco rigido|Per[!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] sono necessari almeno 6 GB di spazio libero su disco rigido.<br/><br/> I requisiti di spazio su disco variano a seconda dei componenti di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] installati. Per altre informazioni, vedere [Requisiti di spazio su disco rigido](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#HardDiskSpace) più avanti in questo articolo. Per informazioni sui tipi di archiviazione supportati per i file di dati, vedere [Storage Types for Data Files](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes).|  
|Monitorare|Per[!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] è necessario un monitor con risoluzione Super-VGA (800 x 600) o superiore.|  
|Internet|Per la funzionalità Internet è necessario l'accesso a Internet (potrebbero essere applicati costi aggiuntivi).|  
|Memoria \*|**Minimo:**<br/><br/> Edizioni Express: 512 MB<br/><br/> Tutte le altre edizioni: 1 GB<br/><br/> **Consigliato:**<br/><br/> Edizioni Express: 1 GB<br/><br/> Tutte le altre edizioni: almeno 4 GB, che devono essere incrementati con l'aumentare delle dimensioni del database per garantire prestazioni ottimali.|  
|Velocità del processore|**Minima**: processore x64: 1,4 GHz<br/><br/> **Consigliato:** almeno 2 GHz|  
|Tipo di processore|Processore x64: AMD Opteron, AMD Athlon 64, Intel Xeon con supporto Intel EM64T, Intel Pentium IV con supporto EM64T|  
  
> [!NOTE]  
> L'installazione di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] è supportata solo su processori x64, non è più supportata su processori x86.  
  
 \* La quantità di memoria minima richiesta per l'installazione del componente [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] in [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) è 2 GB di RAM, diversa dal requisito minimo di memoria di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]. Per informazioni sull'installazione di DQS, vedere [Install Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md).  


##  <a name="software-requirements"></a><a name="hwswr"></a> Requisiti software  

I requisiti seguenti si applicano a tutte le installazioni:  
  
|Componente|Requisito|  
|---------------|-----------------|  
|Sistema operativo|Windows 10 TH1 1507 o versione successiva<br/><br>Windows Server 2016 o versione successiva<br/><br/>
|.NET Framework|I sistemi operativi minimi devono includere almeno .NET Framework.|  
|Software di rete|I sistemi operativi supportati per [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] includono software di rete integrato. Le istanze denominate e predefinite di un'installazione autonoma supportano i protocolli di rete seguenti: Memoria condivisa, named pipe e TCP/IP.<br/><br/> |  

Tramite il programma di installazione di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono installati i seguenti componenti software necessari per il funzionamento del prodotto:  
  
   - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client    
   - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] File di supporto per l'installazione  


> [!IMPORTANT]
> Vi sono altri requisiti hardware e software per la funzionalità PolyBase. Per altre informazioni, vedere [Get started with PolyBase](../../relational-databases/polybase/polybase-guide.md)(Introduzione a PolyBase).  
  

## <a name="operating-system-support"></a>Supporto dei sistemi operativi

Nella tabella seguente vengono illustrate le edizioni di [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] compatibili con le versioni di Windows:  
  

| [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] edizione               | Enterprise | Developer | Standard | Web | Express |  
| :------------------------         | :--------- | :-------- | :------- | :-- | :------ | 
| Windows Server 2019 Datacenter    |    Sì     |    Sì    |    Sì   | Sì |   Sì   |
| Windows Server 2019 Standard      |    Sì     |    Sì    |    Sì   | Sì |   Sì   |
| Windows Server 2019 Essentials    |    Sì     |    Sì    |    Sì   | Sì |   Sì   |
| Windows Server 2016 Datacenter    |    Sì     |    Sì    |    Sì   | Sì |   Sì   |
| Windows Server 2016 Standard      |    Sì     |    Sì    |    Sì   | Sì |   Sì   |
| Windows Server 2016 Essentials    |    Sì     |    Sì    |    Sì   | Sì |   Sì   |
| Windows 10 IoT Enterprise         |    No      |    Sì    |    Sì   | No  |   Sì   |
| Windows 10 Enterprise             |    No      |    Sì    |    Sì   | No  |   Sì   |
| Windows 10 Professional           |    No      |    Sì    |    Sì   | No  |   Sì   |
| Windows 10 Home                   |    No      |    Sì    |    Sì   | No  |   Sì   |
| &nbsp; | &nbsp; |

### <a name="server-core-support"></a>Supporto di Server Core

L'installazione di [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] nella modalità Server Core è supportata dalle edizioni di Windows Server seguenti:

:::row:::
    :::column:::
        Windows Server 2019 Core
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Windows Server 2016 Core
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
:::row-end:::

Per altre informazioni sull'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  in Server Core, vedere [installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  in Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md). 


##  <a name="cross-language-support"></a><a name="CrossLanguageSupport"></a> Supporto di lingue diverse  
 Per ulteriori informazioni sul supporto di lingue diverse e sulle considerazioni relative all'installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di nelle lingue localizzate, vedere [versioni in lingua locale in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ](../../sql-server/install/local-language-versions-in-sql-server.md).  
  
##  <a name="disk-space-requirements"></a><a name="HardDiskSpace"></a> Requisiti relativi allo spazio su disco  
 Durante l'installazione di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]vengono creati alcuni file temporanei nell'unità di sistema. Prima di eseguire il programma di installazione per installare o aggiornare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], verificare che nell'unità di sistema siano disponibili almeno 6,0 GB di spazio su disco per tali file. Tale requisito deve essere soddisfatto anche se tutti i componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono installati in un'unità di sistema non predefinita.  
  
 I requisiti effettivi di spazio su disco rigido possono variare in base alla configurazione del sistema e alle funzionalità selezionate per l'installazione. Nella tabella seguente vengono indicati i requisiti di spazio su disco dei componenti di [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] .  
  
|**Funzionalità**|**Requisito di spazio su disco**|  
|-----------------|--------------------------------|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] e file di dati, replica, ricerca full-text e Data Quality Services|1480 MB|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] (come sopra) con R Services (In-Database)|2744 MB|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] come sopra con Servizio Query PolyBase per i dati esterni|4194 MB|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] e file di dati|698 MB|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|967 MB|  
|[!INCLUDE[rsql_platform](../../includes/rsql-platform-md.md)] (Standalone)|280 MB|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] - SharePoint|1203 MB|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] - Componente aggiuntivo per prodotti SharePoint|325 MB|  
|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]|121 MB|  
|Connettività strumenti client|328 MB|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|306 MB|  
|Componenti client, diversi dai componenti della documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e dagli strumenti di Integration Services|445 MB|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]|280 MB|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Componenti della documentazione online per visualizzare e gestire il contenuto della Guida*|27 MB|  
|Tutte le funzionalità|8030 MB|  
  
 *Il requisito di spazio su disco per il contenuto della documentazione online scaricato è pari a 200 MB.  
  
##  <a name="storage-types-for-data-files"></a><a name="StorageTypes"></a> Tipi di archiviazione per i file di dati  
 I tipi di archiviazione supportati per i file di dati sono:  
  
- Disco locale 
    - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attualmente supporta unità disco con dimensioni del settore native standard di 512 byte e 4 KB.  I dischi rigidi con dimensioni del settore superiori a 4KB possono causare errori durante il tentativo di archiviazione dei file di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sui dischi.  Per ulteriori informazioni sul supporto delle dimensioni del settore del disco rigido in, vedere la pagina relativa ai [limiti [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di supporto delle dimensioni del settore unità disco rigido ](https://support.microsoft.com/kb/926930) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 
    - Il disco locale è supportato nell'installazione del cluster di failover di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo per l'installazione dei file tempdb. Assicurarsi che il percorso specificato per i file di dati tempdb e di log sia valido su tutti i nodi del cluster. Durante il failover, se le directory tempdb non sono disponibili nel nodo di destinazione del failover, la risorsa di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non verrà riportata online.
- Archiviazione condivisa  
- [Spazi di archiviazione diretta \(S2D\)](/windows-server/storage/storage-spaces/storage-spaces-direct-overview)  
- Condivisione file SMB  
    - L'archiviazione SMB non è supportata per i file di dati di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] per le installazioni autonome o cluster. Usare invece l'archiviazione collegata direttamente, una rete SAN o S2D. 
    - L'archiviazione SMB può essere ospitata da un file server di Windows o da un dispositivo di archiviazione SMB di terze parti. Nel primo caso, la versione del file server di Windows deve essere la versione 2008 o successiva. Per ulteriori informazioni sull'installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite condivisione file SMB come opzione di archiviazione, vedere [install [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  with SMB FileShare As a Storage Option](../../database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option.md).  
  
  
  
##  <a name="installing-ssnoversion-on-a-domain-controller"></a><a name="DC_support"></a> Installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un controller di dominio  
 Per motivi di sicurezza, è consigliabile non installare [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] in un controller di dominio. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ma verranno applicate le limitazioni seguenti:  
  
- Non è possibile eseguire servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un controller di dominio utilizzando un account Servizio locale.    
- Al termine dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un computer, non è possibile modificare il computer da membro di dominio a controller di dominio. Prima di modificare il computer host in controller di dominio, è necessario disinstallare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .    
- Al termine dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un computer, non è possibile modificare il computer da controller di dominio a membro di dominio. Prima di modificare il computer host in membro di dominio, è necessario disinstallare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .   
- Le istanze del cluster di failover di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non sono supportate quando i nodi del cluster sono controller di dominio.   
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è supportato in un controller di dominio di sola lettura. Tramite il programma di installazione di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non sarà possibile creare gruppi di sicurezza o fornire account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un controller di dominio di sola lettura. In questo scenario il programma di installazione non verrà completato. 
- Un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è supportata in un ambiente in cui si può accedere solo a un controller di dominio di sola lettura. 
  
## <a name="installation-media"></a>Supporto di installazione

È possibile ottenere il supporto di installazione pertinente nelle posizioni seguenti: 
  
- [[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  centro di valutazione](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019)
- [Aggiornamenti cumulativi più recenti](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

In alternativa, è possibile creare una [macchina virtuale di Azure già [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in esecuzione ](/azure/virtual-machines/windows/sql/quickstart-sql-vm-create-portal) Sebbene [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  in una macchina virtuale risulti più lenta rispetto all'esecuzione a livello nativo a causa dell'overhead della virtualizzazione.


## <a name="next-steps"></a>Passaggi successivi

Una volta esaminati i requisiti hardware e software per l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile iniziare a [pianificare un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  installazione](../../sql-server/install/planning-a-sql-server-installation.md) o rivedere le [considerazioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sulla sicurezza per ](../../sql-server/install/security-considerations-for-a-sql-server-installation.md).