---
title: Applicazione sqlservr
description: L'applicazione sqlservr avvia, arresta, sospende e riprende un'istanza di SQL Server al prompt dei comandi.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], sqlservr
- command prompt [SQL Server], pausing/resuming instance of SQL Server
- starting instance of SQL Server
- command prompt [SQL Server], continuing instance of SQL Server
- sqlservr utility
- pausing instance of SQL Server
- stopping instance of SQL Server
- resuming SQL Server
- command prompt [SQL Server], stopping instance of SQL Server
- command prompt [SQL Server], starting instance of SQL Server
- continuing instance of SQL Server
ms.assetid: 60e8ef0a-0851-41cf-a6d8-cca1e04cbcdb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 793943c492766037adbce8c077b9a391c041c88c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338580"
---
# <a name="sqlservr-application"></a>Applicazione sqlservr

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

L'applicazione **sqlservr** avvia, arresta, sospende e riprende un'istanza di [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```cmd
sqlservr [-s instance_name] [-c] [-d master_path] [-f] 
     [-e error_log_path] [-l master_log_path] [-m]
     [-n] [-T trace#] [-v] [-x]
```

## <a name="arguments"></a>Argomenti

**-s** *instance_name* Specifica l'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] alla quale connettersi. Se non si specifica un'istanza denominata, **sqlservr** avvia l'istanza predefinita di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

> [!IMPORTANT]
>Per l'avvio di un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], è necessario usare l'applicazione **sqlservr** nella directory appropriata per l'istanza. Nel caso dell'istanza predefinita, eseguire **sqlservr** dalla directory \MSSQL\Binn. Nel caso dell'istanza denominata, eseguire **sqlservr** dalla directory \MSSQL$*nome_istanza*\Binn.

 **-c** Indica l'avvio di un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in modo indipendente da Gestione controllo servizi di Windows. Questa opzione viene utilizzata dal prompt dei comandi all'avvio di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per ridurre il tempo di avvio di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

> [!NOTE]
>Se si usa questa opzione, non è possibile arrestare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usando Gestione servizi [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] oppure il comando **net stop** e [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene arrestato se si disconnette il computer.

**-d** *master_path* Indica il percorso completo del file del database **master**. Non sono presenti spazi tra **-d** e *master_path*. Se non si imposta questa opzione, vengono utilizzati i parametri esistenti nel Registro di sistema.

**-f** Avvia un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con la configurazione minima. È utile nel caso in cui l'impostazione di un valore di configurazione, ad esempio un'allocazione eccessiva di memoria, abbia impedito l'avvio del server.

**-e** *error_log_path* Indica il percorso completo del file di log degli errori. Se non specificato, il percorso predefinito è `*\<Drive>*:\Program Files\Microsoft SQL Server\MSSQL\Log\Errorlog` per l'istanza predefinita e `*\<Drive>*:\Program Files\Microsoft SQL Server\MSSQL$*instance_name*\Log\Errorlog` per un'istanza denominata. Non sono presenti spazi tra **-e** e *error_log_path*.

**-l** *master_log_path* Indica il percorso completo del file del log delle transazioni del database **master**. Non sono presenti spazi tra **-l** e *master_log_path*.

**-m** Indica l'avvio di un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in modalità utente singolo. Se [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene avviato in modalità utente singolo, la connessione è consentita solo a un utente. Il meccanismo di CHECKPOINT, che assicura la regolare scrittura delle transazioni completate dalla cache del disco al database, non viene avviato. In genere, questa opzione viene utilizzata quando si riscontrano problemi che richiedono interventi nei database di sistema. L'impostazione abilita l'opzione **sp_configure allow updates**. Per impostazione predefinita, l'opzione **allow updates** è disabilitata.

**-n** Consente di avviare un'istanza denominata di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Se non si specifica il set di parametri **-s** , viene avviata l'istanza predefinita. Al prompt dei comandi è necessario passare alla directory BINN appropriata per l'istanza prima di avviare **sqlservr.exe**. Ad esempio, se Instance1 usa \mssql$Instance1 per i relativi file binari, l'utente deve passare alla directory \mssql$Instance1\binn per avviare **sqlservr.exe -s instance1**. Se si avvia un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con l'opzione **-n** , è consigliabile usare anche l'opzione **-e** . In caso contrario, gli eventi di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non vengono registrati.

**-T** *trace#* Indica l'avvio di un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con uno specifico flag di traccia (*trace#* ) attivo. I flag di traccia vengono utilizzati per avviare il server con un funzionamento non standard. Per altre informazioni, vedere [Flag di traccia &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

>[!IMPORTANT]
>Quando si specifica un flag di traccia, indicarne il numero usando **-T** . La lettera minuscola t ( **-t**) è accettata da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], ma **-t** imposta altri flag di traccia interni necessari per i tecnici del supporto tecnico per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

**-v** Visualizza il numero di versione del server.

**-x** Disabilita la registrazione delle statistiche relative al tempo CPU e alla frequenza di accesso alla cache. Consente l'ottimizzazione delle prestazioni.

## <a name="remarks"></a>Osservazioni
Nella maggior parte dei casi il programma sqlservr.exe viene utilizzato solo per la risoluzione dei problemi o interventi di manutenzione ordinaria. Se si avvia [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dal prompt dei comandi con sqlservr.exe, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non viene avviato come servizio. Quindi, non sarà possibile arrestare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usando i comandi **net** . Gli utenti possono connettersi a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], ma gli strumenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] indicano lo stato del servizio. Di conseguenza, Gestione configurazione [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] indica correttamente che il servizio è stato arrestato. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] può connettersi al server anche se indica che il servizio è stato arrestato.

## <a name="compatibility-support"></a>Informazioni sulla compatibilità
I parametri seguenti sono obsoleti e non sono supportati in [!INCLUDE[sssql19-md](../includes/sssql19-md.md)].

|Parametro | Ulteriori informazioni|
|:-----|:-----|
|**-h** | In versioni precedenti di istanze a 32 bit di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] questo parametro è stato usatao per riservare spazio di indirizzi della memoria virtuale per metadati della memoria a caldo quando AWE è abilitato. Supportato in [!INCLUDE[sssql14](../includes/sssql14-md.md)]. Per ulteriori informazioni, vedere [Funzionalità di SQL Server obsolete in SQL Server 2016](../database-engine/discontinued-database-engine-functionality-in-sql-server.md).|
|**-g** | *memory_to_reserve*<br/><br>Si applica alle versioni precedenti delle istanze a 32 bit di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Supportato in [!INCLUDE[sssql14](../includes/sssql14-md.md)]. Specifica un numero intero di megabyte (MB) di memoria che [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] riserva per le allocazioni di memoria all'interno del processo di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , ma esternamente al pool di memoria di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [la documentazione di SQL Server 2014 sulle opzioni di configurazione di Server Memory](/previous-versions/sql/2014/database-engine/configure-windows/server-memory-server-configuration-options?view=sql-server-2014&preserve-view=true).|
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Vedere anche
 [Opzioni di avvio del servizio del motore di database](../database-engine/configure-windows/database-engine-service-startup-options.md)
