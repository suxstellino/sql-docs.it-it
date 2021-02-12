---
title: Convertire progetti di database di Visual Studio 2010 in progetti di database di SQL Server
description: Convertire progetti di database di Visual Studio 2010 in progetti di database di SQL Server e destinarli di nuovo a piattaforme diverse. Visualizzare gli oggetti che SSDT può convertire o meno.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.projectconversion.dialog
- sql.data.tools.ImportDAC
ms.assetid: 7e5acf94-5c46-44c7-9ff5-ca7926f5332a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: bf8db10c508ed2cdf5fd2f4bc96c0d1d2508d83d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018381"
---
# <a name="how-to-convert-a-visual-studio-2010-database-projects-to-sql-server-database-projects-and-retarget-to-a-different-platform"></a>Procedura: Convertire progetti di database Visual Studio 2010 in progetti di database di SQL Server e destinarli di nuovo a una piattaforma differente

In SQL Server Data Tools (SSDT) è possibile convertire progetti di database di SQL Server, CLR e di applicazione livello dati esistenti creati in Visual Studio 2010 nel nuovo progetto di database di SQL Server. In questo modo, è possibile sfruttare il nuovo strumento di sviluppo del database fornito da SSDT, ad esempio un editor Transact\-SQL aggiornato, e la possibilità di destinare di nuovo il progetto in Microsoft SQL Server 2012 e in SQL Azure con convalida del codice. Il processo di conversione consente di convertire oggetti (tabelle, viste, stored procedure, file delle proprietà o script) che dispongono di un tipo equivalente in SSDT, inclusi i file di criteri di applicazione livello dati e le autorizzazioni. Gli elementi che non possono essere convertiti sono evidenziati in un report di log della conversione.  
  
Nella tabella seguente sono elencati tutti gli elementi di progetto che possono essere o meno convertiti da SSDT.  
  
|Elementi di progetto convertibili|Elementi di progetto non convertibili|  
|-------------------------------------------|----------------------------------------------|  
|File di progetto<br /><br />File di progetto con estensione dbproj (progetti server e di database di Visual Studio 2010, progetti di applicazione livello dati)<br /><br />File di progetto CLR con estensione csproj e vbproj convertibili ma con possibile perdita di dati|Progetti di unit test del database<br /><br />Progetti parziali, quali elementi con estensione files|  
|File delle proprietà<br /><br />File *.sqldeployment e con estensioni sqlsettings e sqlpolicy convertiti nelle relative pagine delle proprietà del progetto corrispondenti<br /><br />File con estensione sqlpermissions convertiti in script Transact\-SQL|Proprietà progetto<br /><br />Server.sqlsettings<br /><br />Variabili SQLCMD definite in file con estensione sqlcmd|  
|File con estensione sql importati utilizzando la struttura della cartella esistente.|File di estensibilità.|  
|Script di pre-distribuzione e di post-distribuzione|Riferimenti al database che dovranno essere ristabiliti manualmente dopo la conversione del progetto.|  
|File di confronto schema|File di generazione dati.|  
  
### <a name="to-convert-a-project"></a>Per convertire un progetto  
  
1.  Aprire un progetto di database di SQL Server 2005 o 2008.  
  
2.  Verrà aperta automaticamente la procedura guidata **Converti in progetto di database di SQL Server**. Selezionare **Converti in progetto di database di SQL Server** e scegliere **OK**. Mantenere selezionata l'impostazione predefinita per eseguire il backup dei file esistenti.  
  
3.  Viene generato automaticamente un report di conversione in cui sono elencati tutti i file convertiti. Fare clic sul segno **+** accanto al nome del file del progetto per leggere altre informazioni sul processo di conversione.  
  
4.  Si noti che in **Esplora soluzioni** il file di progetto, i file delle proprietà e gli oggetti dello schema sono tutti convertiti.  
  
### <a name="to-change-a-projects-target-platform"></a>Per modificare la piattaforma di destinazione di un progetto  
  
1.  Fare clic con il pulsante destro del mouse sul progetto appena convertito in **Esplora soluzioni** e selezionare **Proprietà** per accedere alla finestra di dialogo **Impostazioni progetto**.  
  
2.  Selezionare una delle piattaforme supportate da SSDT nell'elenco a discesa **Piattaforma di destinazione**.  
  
## <a name="see-also"></a>Vedere anche  
[Procedura: Modificare la piattaforma di destinazione e pubblicare un progetto di database](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)  
  
