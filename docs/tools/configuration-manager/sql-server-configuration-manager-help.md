---
title: Guida di Gestione configurazione SQL Server
description: Acquisire familiarità con Gestione configurazione SQL Server e su come usarlo per gestire i servizi SQL Server e configurare la connettività di rete.
ms.custom: seo-lt-2019
ms.date: 02/01/2018
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Configuration Manager, help
ms.assetid: 6e909911-39a6-469b-b22a-3afdfd08a30b
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 0e690630174bb9e8f92575f69b586b44c4dabbfc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345170"
---
# <a name="sql-server-configuration-manager-help"></a>Guida di Gestione configurazione SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Usare Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per configurare i servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e la connettività di rete. Per creare o gestire oggetti di database, configurare la sicurezza e scrivere query [!INCLUDE[tsql](../../includes/tsql-md.md)] , usare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per altre informazioni su [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vedere la documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

 > [!TIP]
 > Se è necessario configurare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Linux, usare lo strumento **mssql-conf**. Per altre informazioni, vedere [Configurare SQL Server in Linux con lo strumento mssql conf](../../linux/sql-server-linux-configure-mssql-conf.md).

 In questa sezione sono disponibili gli argomenti della Guida per le finestre di dialogo di Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager non è in grado di configurare versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedenti a [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
## <a name="services"></a>Servizi  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Gestione configurazione SQL Server gestisce i servizi relativi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sebbene molte delle funzioni di questo strumento equivalgano alle opzioni della finestra di dialogo Servizi di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, è importante sottolineare che Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue operazioni aggiuntive sui servizi che gestisce, ad esempio applicando le autorizzazioni corrette quando si cambia l'account del servizio. L'utilizzo della finestra di dialogo Servizi Windows standard per configurare i servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe provocare malfunzionamenti.  
  
 Usare Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le operazioni seguenti:  
  
-   Avviare, arrestare e sospendere i servizi  
  
-   Configurare i servizi in modo che vengano avviati automaticamente o manualmente, disabilitare i servizi o modificare altre impostazioni  
  
-   Modificare le password degli account usati dai servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Avviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante i flag di traccia (parametri della riga di comando)  
  
-   Visualizzare le proprietà dei servizi  
  
## <a name="sql-server-network-configuration"></a>Configurazione di rete SQL Server  
 Usare Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le operazioni seguenti relative ai servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer corrente:  
  
-   Abilitare o disabilitare un protocollo di rete di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Configurare un protocollo di rete di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
> [!NOTE]  
>  Per una breve esercitazione sulla configurazione dei protocolli e sulla connessione al [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], vedere [Esercitazione: Introduzione al motore di database](../../relational-databases/tutorial-getting-started-with-the-database-engine.md).  
  
## <a name="sql-server-native-client-configuration"></a>Configurazione SQL Server Native Client  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] I client SQL Server si connettono a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite la libreria di rete [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Utilizzare Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le operazioni seguenti relative alle applicazioni client nel computer corrente:  
  
-   Per le applicazioni client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer, specificare l'ordine dei protocolli quando si stabiliscono connessioni alle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Configurare i protocolli di connessione client.  
  
-   Per le applicazioni client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , creare alias per le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in modo che i client possano connettersi usando una stringa di connessione personalizzata.  
  
 Per ulteriori informazioni su tali attività, vedere la Guida sensibile al contesto.  
  
#### <a name="to-open-sql-server-configuration-manager"></a>Per aprire Gestione configurazione SQL Server  
  
-   Dal menu **Start** scegliere **Tutti i programmi**, **Microsoft SQL Server** (versione), **Strumenti di configurazione**, quindi fare clic su **Gestione configurazione SQL Server**.  
  
  
 **Per accedere a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager tramite [!INCLUDE[win8](../../includes/win8-md.md)]**  
  
 Poiché Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è uno snap-in per il programma [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console e non un programma autonomo, Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene visualizzato come applicazione durante l'esecuzione di [!INCLUDE[win8](../../includes/win8-md.md)]. Per aprire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager, nell'icona promemoria **Cerca** in **App** digitare **SQLServerManager12.msc** (per [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) o **SQLServerManager11.msc** per ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) e quindi premere **INVIO**.  
  

## <a name="see-also"></a>Vedere anche  
 [Servizi di SQL Server](../../tools/configuration-manager/sql-server-services.md)   
 [Configurazione di rete SQL Server](../../tools/configuration-manager/sql-server-network-configuration.md)   
 [Configurazione SQL Native Client 11.0](../../tools/configuration-manager/sql-native-client-11-0-configuration.md)   
 [Scelta di un protocollo di rete](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))  
  
