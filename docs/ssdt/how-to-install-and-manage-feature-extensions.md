---
title: Installare e gestire le estensioni delle funzionalità
description: Informazioni su come installare le estensioni delle funzionalità in modo da aumentare le funzionalità di SQL Server Data Tools. Scoprire dove installare i diversi tipi di estensioni.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9cdc8cd5-c36f-4bee-a191-87ed457803e7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: c469770ab64401498b35038f7b20e6f245793804
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020744"
---
# <a name="how-to-install-and-manage-feature-extensions"></a>Procedura: Installare e gestire le estensioni delle funzionalità

È possibile aggiungere regole per l'analisi del codice del database, condizioni per gli unit test del database e collaboratori alla compilazione/distribuzione per aumentare le funzionalità offerte dalle edizioni di Visual Studio che includono SQL Server Data Tools. Tuttavia, per potere usare un'estensione della funzionalità è necessario prima installarla, indipendentemente dal fatto che l'estensione sia stata creata dall'utente stesso o se ne stia installando una creata da un altro utente.  
  
Il percorso in cui installare l'estensione dipende dal tipo di estensione e da dove si intende usarla. Nelle versioni più recenti di Visual Studio il percorso di installazione di alcuni componenti è stato spostato dalla directory di installazione di SQL Server all'interno della directory di Visual Studio. Questa configurazione semplifica la possibilità di eseguire contemporaneamente diverse versioni del software, ma implica che potrebbe essere necessario installare l'estensione in più percorsi, se si vuole usarla in una versione diversa di SQL Server Data Tools e dalla riga di comando.  
  
### <a name="installing-extensions-for-use-inside-visual-studio"></a>Installazione delle estensioni da usare all'interno di Visual Studio  
  
|Tipo di estensione|Percorso di installazione|  
|------------------|--------------------|  
|Condizione di test personalizzata per unit test di SQL Server|<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\TestConditions|  
|Collaboratori alla compilazione<br /><br />Collaboratori alla distribuzione<br /><br />Regole di analisi del codice statica|<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions|  
  
<Visual Studio Install Dir> varia a seconda della versione di Visual Studio usata e da dove si è scelto di installarla. Per Visual Studio 2012, in genere è C:\Programmi (x86)\\MicrosoftVisual Studio 11.0. Per Visual Studio 2013, in genere è C:\Programmi (x86)\\MicrosoftVisual Studio 12.0.  
  
Le estensioni possono essere eseguite come parte dei servizi Microsoft da riga di comando:  
  
|Tipo di estensione|Servizio della riga di comando|Cartella di installazione|  
|------------------|------------------------|------------------|  
|Condizione di test personalizzata per unit test di SQL Server|MSBuild / MSTest può essere usato per eseguire gli unit test dal Prompt dei comandi per gli sviluppatori per Visual Studio 2013 e da altri strumenti da riga di comando simili.|Uguale a quella dell'esecuzione all'interno di Visual Studio.|  
|Collaboratori alla compilazione<br /><br />Collaboratori alla distribuzione|[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md) oppure usando le destinazioni di distribuzione o pubblicazione di MSBuild durante la compilazione di un progetto del database.|MSBuild: Uguale a quella dell'esecuzione all'interno di Visual Studio.<br /><br />[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md): se si trova all'interno della directory di Visual Studio, come in precedenza.<br /><br />Se SqlPackage.exe e le altre DLL di DacFx si trovano all'esterno di tale directory, le estensioni dovranno essere posizionate nella stessa directory o in C:\Programmi (x86)\\MicrosoftSQL Server\120\DAC\bin\Extensions.|  
|Regole di analisi del codice statica|MSBuild può essere usato per compilare il progetto ed eseguire l'analisi del codice statica.<br /><br />È anche possibile eseguire l'analisi del codice usando l'API CodeAnalysisService dalle applicazioni. Le regole di ricerca estensione funzionano in questo caso allo stesso modo di quando si usa SqlPackage.exe.|Uguale a quella dei collaboratori alla compilazione e alla distribuzione|  
  
> [!NOTE]  
> È necessario avere le autorizzazioni di amministratore sul computer per accedere a tutte le directory di installazione nella cartella Programmi. Se non si hanno le autorizzazioni appropriate, contattare l'amministratore di rete.  
  
**Security Considerations**  
  
Prima di installare un'estensione creata da altri, è necessario comprendere i rischi dell'operazione:  
  
-   Il programma di installazione dell'estensione potrebbe essere dannoso e ottenere l'accesso alle risorse protette in base alle autorizzazioni di installazione concesse.  
  
-   L'estensione in sé potrebbe essere dannosa e ottenere il controllo delle risorse protette se l'utente che usa l'estensione dispone delle autorizzazioni sufficienti.  
  
Per ridurre il rischio, è necessario installare un'estensione solo se proviene da un'origine nota. Se l'estensione proviene da un'origine non attendibile, è necessario controllare il codice sorgente dell'estensione e il programma di installazione (se disponibile) prima di installare e usare l'estensione.  
  
## <a name="to-install-a-custom-feature-extension"></a>Per installare un'estensione della funzionalità personalizzata  
Copiare l'assembly firmato (con estensione dll) nella cartella di installazione corretta. Chiudere e riaprire Visual Studio. Ora l'estensione dovrebbe essere disponibile.  
  
## <a name="see-also"></a>Vedere anche  
[Estensione delle funzionalità del database](../ssdt/extending-the-database-features.md)  
  
