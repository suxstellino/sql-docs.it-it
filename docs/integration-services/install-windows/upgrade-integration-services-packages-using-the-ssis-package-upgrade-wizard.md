---
description: Aggiornare i pacchetti di Integration Services mediante l'Aggiornamento guidato pacchetti SSIS
title: Aggiornare i pacchetti di Integration Services mediante l'Aggiornamento guidato pacchetti SSIS | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services packages, upgrading
- upgrading Integration Services packages
ms.assetid: 9359275a-48f5-4d1e-8ae7-e797759e3ccf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8f893df24e74e0367ab98545a9003de6d93f47f0
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835715"
---
# <a name="upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard"></a>Aggiornare i pacchetti di Integration Services mediante l'Aggiornamento guidato pacchetti SSIS

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  È possibile aggiornare i pacchetti creati nelle versioni precedenti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] al formato [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilizzato in [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce l'Aggiornamento guidato pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] che consente di semplificare il processo. Poiché è possibile configurare la procedura guidata in modo da eseguire il backup dei pacchetti originali, sarà possibile continuare a utilizzarli nel caso in cui si riscontrino difficoltà nell'aggiornamento.  
  
 L'Aggiornamento guidato pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] viene installato durante l'installazione di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
> [!NOTE]  
>  L'Aggiornamento guidato pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] è disponibile in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Standard Edition, Enterprise Edition e Developer Edition.  
  
 Per informazioni su come aggiornare i pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vedere [Aggiornare pacchetti di Integration Services](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
 Nella parte restante dell'argomento viene descritto come eseguire la procedura guidata e il backup dei pacchetti originali.  
  
## <a name="running-the-ssis-package-upgrade-wizard"></a>Esecuzione dell'Aggiornamento guidato pacchetti SSIS  
 È possibile eseguire l'Aggiornamento guidato pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] da [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]o dal prompt dei comandi.  
  
#### <a name="to-run-the-wizard-from-sql-server-data-tools"></a>Per eseguire la procedura guidata da SQL Server Data Tools  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]creare o aprire un progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  In Esplora soluzioni fare clic con il pulsante destro del mouse sul nodo **Pacchetti SSIS** , quindi scegliere **Aggiorna tutti i pacchetti** per aggiornare tutti i pacchetti all'interno di questo nodo.  
  
    > [!NOTE]  
    >  Quando si apre un progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenente pacchetti di [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] o versioni successive, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] apre automaticamente l'Aggiornamento guidato pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
#### <a name="to-run-the-wizard-from-sql-server-management-studio"></a>Per eseguire la procedura guidata da SQL Server Management Studio  
  
-   In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]connettersi a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], espandere il nodo **Pacchetti archiviati** e fare clic con il pulsante destro del mouse sul nodo **File system** o **MSDB** , quindi scegliere **Aggiorna pacchetti**.  
  
#### <a name="to-run-the-wizard-at-the-command-prompt"></a>Per eseguire la procedura guidata dal prompt dei comandi  
  
-   Al prompt dei comandi eseguire il file SSISUpgrade.exe dalla cartella **C:\Programmi\Microsoft SQL Server\130\DTS\Binn** .  
  
## <a name="backing-up-the-original-packages"></a>Esecuzione del backup dei pacchetti originali  
 Per eseguire il backup dei pacchetti originali, è necessario che i pacchetti originali e i pacchetti aggiornati siano archiviati nella stessa cartella del file system. A seconda di come viene eseguita la procedura guidata, è possibile che il percorso di archiviazione venga selezionato automaticamente.  
  
-   Quando si esegue l'Aggiornamento guidato pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] da [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], la procedura guidata archivia automaticamente i pacchetti originali e i pacchetti aggiornati nella stessa cartella del file system.  
  
-   Quando si esegue l'Aggiornamento guidato pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o dal prompt dei comandi, è possibile specificare percorsi di archiviazione diversi per i pacchetti originali e per quelli aggiornati. Per eseguire il backup dei pacchetti originali, accertarsi di specificare che sia i pacchetti originali che quelli aggiornati sono archiviati nella stessa cartella del file system. Se si specificano altre opzione di archiviazione, la procedura guidata non sarà in grado di eseguire il backup dei pacchetti originali.  
  
 Quando esegue il backup dei pacchetti originali, la procedura guidata archivia una copia di tali pacchetti nella cartella **SSISBackupFolder** . La cartella **SSISBackupFolder** viene creata come sottocartella della cartella che contiene i pacchetti originali e i pacchetti aggiornati.  
  
#### <a name="to-back-up-the-original-packages-in-sql-server-management-studio-or-at-the-command-prompt"></a>Per eseguire il backup dei pacchetti originali in SQL Server Management Studio o dal prompt dei comandi  
  
1.  Salvare i pacchetti originali in un percorso del file system.  
  
    > [!NOTE]  
    >  L'opzione di backup della procedura guidata funziona solo con i pacchetti archiviati nel file system.  
  
2.  Eseguire l'Aggiornamento guidato pacchetti [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] in [!INCLUDE[ssIS](../../includes/ssis-md.md)] o dal prompt dei comandi.  
  
3.  Nella pagina **Seleziona posizione di origine** della procedura guidata impostare la proprietà **Origine pacchetto** su **File system**.  
  
4.  Nella pagina **Seleziona posizione di destinazione** della procedura guidata selezionare **Salva in posizione di origine** per salvare i pacchetti aggiornati nello stesso percorso dei pacchetti originali.  
  
    > [!NOTE]  
    >  L'opzione di backup della procedura guidata è disponibile solo se i pacchetti aggiornati vengono archiviati nella stessa cartella dei pacchetti originali.  
  
5.  Nella pagina **Seleziona opzioni di gestione pacchetti** della procedura guidata selezionare l'opzione **Esegui backup pacchetti originali** .  
  
#### <a name="to-back-up-the-original-packages-in-sql-server-data-tools"></a>Per eseguire il backup dei pacchetti originali in SQL Server Data Tools  
  
1.  Salvare i pacchetti originali in un percorso del file system.  
  
2.  Nella pagina **Seleziona opzioni di gestione pacchetti** della procedura guidata selezionare l'opzione **Esegui backup pacchetti originali** .  
  
    > [!WARNING]  
    >  L'opzione **Esegui backup pacchetti originali** non viene visualizzata quando si apre un progetto di [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] o versioni successive in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], con il conseguente avvio automatico della procedura guidata.  
  
3.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]eseguire l'Aggiornamento guidato pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
  
