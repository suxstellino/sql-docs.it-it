---
title: Avvia Configuration Manager
description: Istruzioni per l'avvio dello strumento di Configuration Manager per l'appliance del sistema della piattaforma di analisi.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: be69331ec9f9daa091035d6ef21142c4f40d6a84
ms.sourcegitcommit: 442fbe1655d629ecef273b02fae1beb2455a762e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235168"
---
# <a name="launch-the-configuration-manager-in-analytics-platform-system"></a>Avviare il Configuration Manager nel sistema della piattaforma Analytics
Questo argomento fornisce istruzioni per l'avvio del **Configuration Manager** per l'appliance del sistema della piattaforma di analisi.  
  
## <a name="before-you-begin"></a>Prima di iniziare  
  
### <a name="prerequisites"></a>Prerequisiti  
Il **Configuration Manager** di sistema della piattaforma Analytics può essere eseguito solo dall'amministratore di dominio dell'appliance. Per eseguire questo strumento, è necessaria la password per l'amministratore di dominio dell'appliance. Per creare amministratori APS aggiuntivi, vedere [creare un amministratore di dominio aps &#40;aps&#41;](create-an-aps-domain-administrator-aps.md).  
  
## <a name="launch-the-configuration-manager-tool"></a><a name="Accessing"></a>Avviare lo strumento di Configuration Manager  
Per eseguire il Configuration Manager, usare Desktop remoto per connettersi al nodo del nodo di controllo PDW ( **_PDW_region_ -CTL01** ) e accedere come _appliance_domain_**\Administrator** . Quando si avvia il programma di **Configuration Manager** , utilizzare l'opzione **Esegui come amministratore** per assicurarsi che vengano utilizzate le credenziali di amministratore.  
  
#### <a name="to-launch-from-a-browser-window"></a>Per avviare da una finestra del browser  
  
1.  Aprire un browser e passare alla directory `C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100` .  
  
2.  Fare clic con il pulsante destro del mouse `dwconfig.exe` e quindi scegliere **Esegui come amministratore** .  
  
#### <a name="to-launch-from-a-command-prompt"></a>Per avviare da un prompt dei comandi  
  
1.  Sul desktop, aprire il menu **Start** , fare clic su **programmi** , **Accessori** , fare clic con il pulsante destro del mouse su **prompt dei comandi** e quindi scegliere **Esegui come amministratore** .  
  
2.  Al prompt dei comandi, immettere il comando seguente per modificare le directory: `cd /d "C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100"` .  
  
3.  Al prompt dei comandi digitare `dwconfig.exe`.  
  
Una volta avviata la **Configuration Manager** , tutte le funzionalità disponibili vengono elencate nel riquadro sinistro. Nella parte restante di questa sezione viene illustrato come eseguire ogni azione disponibile nello strumento.  
  
Per chiudere e uscire **Configuration Manager** , fare clic su **Esci** nell'angolo in basso a destra della schermata.  
  
![Screenshot della finestra di dialogo piattaforma di strumenti analitici Microsoft Configuration Manager che mostra la topologia del dispositivo.](./media/launch-the-configuration-manager/SQL_Server_PDW_DWConfig_ApplTop.png "SQL_Server_PDW_DWConfig_ApplTop")  
  
## <a name="see-also"></a>Vedere anche  
[Monitorare l'appliance usando la console di amministrazione &#40;sistema della piattaforma di analisi&#41;](monitor-the-appliance-by-using-the-admin-console.md)  
  
