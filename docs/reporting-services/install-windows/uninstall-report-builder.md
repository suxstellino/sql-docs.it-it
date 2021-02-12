---
description: Disinstallare Generatore report
title: Disinstallare Generatore report | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 009538c6-4941-4393-b14b-9144cffdbdaf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3f473c53a8e0a04603a391300fb2fc26dbd8210e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077482"
---
# <a name="uninstall-report-builder"></a>Disinstallare Generatore report

È possibile disinstallare la versione autonoma di Generatore report dal Pannello di controllo o dalla riga di comando.

La sintassi per la disinstallazione di Generatore report dalla riga di comando è identica a quella che si utilizza per l'installazione di Generatore report, con l'eccezione che si utilizza l'opzione /x anziché l'opzione /i. Per eseguire la disinstallazione dalle righe di comando è possibile anche includere l'opzione /quiet e altre opzioni standard. Se il pacchetto di Windows Installer di Generatore report (ReportBuilder3_x86.msi) è stato rimosso, non è possibile utilizzare facilmente la riga di comando per disinstallare Generatore report. Per altre informazioni su come rimuovere Generatore report usando il GUID corrispondente, vedere la documentazione per il programma msiexec in [Command-Line Options](/windows/desktop/Msi/command-line-options)(Opzioni della riga di comando).  

Se le cartelle utilizzate da Generatore report includono file personalizzati, le cartelle e i file vengono conservati quando Generatore report viene rimosso. Vengono rimossi solo i file di Generatore report.  

### <a name="to-uninstall-report-builder-from-the-control-panel"></a>Per disinstallare Generatore report dal Pannello di controllo

1.  Fare clic sul menu **Start** e scegliere **Pannello di controllo**.  
  
2.  Nel Pannello di controllo fare clic su **Programmi e caratteristiche**.  
  
3.  Individuare Generatore report di [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server nell'elenco **Nome** e fare clic sul nome del programma.  
  
4.  Fare clic su **Disinstalla**.  
  
5.  Se viene richiesto di confermare la disinstallazione di Generatore report, fare clic su **Sì**.  
  
### <a name="to-uninstall-report-builder-from-the-command-line"></a>Per disinstallare Generatore report dalla riga di comando  
  
1.  Fare clic sul menu **Start** e scegliere **Esegui**.  
  
2.  Nella casella di testo **Apri** digitare **cmd.**  
  
3.  Nella finestra del prompt dei comandi, passare alla cartella contenente ReportBuilder3_x86.msi.  
  
4.  Digitare una riga di comando di base simile alla seguente:  
  
 `msiexec /x ReportBuilder3_x86.msi /quiet /l*v install.log`  
  
 Per includere la registrazione, utilizzare una riga di comando simile alla seguente:  
  
 `msiexec /x ReportBuilder3_x86.msi /quiet /l*v c:\junk\install.log`  
  
5.  Premere **INVIO**.  

## <a name="next-steps"></a>Passaggi successivi

[Installare Generatore report](../../reporting-services/install-windows/install-report-builder.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
