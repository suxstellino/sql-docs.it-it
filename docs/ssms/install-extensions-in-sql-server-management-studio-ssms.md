---
description: Installare le estensioni in SQL Server Management Studio (SSMS)
title: Installare le estensioni in SQL Server Management Studio (SSMS)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
keywords:
- Estensioni
- vsix
- installare l'estensione
- install vsix
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 07/29/2020
ms.openlocfilehash: d9fe47b283eb95ed624b68bc9c22abd9258bad15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344717"
---
# <a name="install-extensions-in-sql-server-management-studio-ssms"></a>Installare le estensioni in SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Le estensioni di SQL Server Management Studio (SSMS) vengono create con C# usando il carico di lavoro "Sviluppo di estensioni di Visual Studio" in Visual Studio. SSMS 18.x si basa sulla shell di Visual Studio 2017 ed è soggetto alle limitazioni di tale ambiente.

È possibile installare le estensioni in SSMS 18.x distribuendo VSIX tramite Visual Studio o un programma di installazione indipendente del pacchetto gestito.  La distribuzione di Visual Studio è descritta di seguito.

> [!NOTE]
> Non è possibile installare le estensioni di SQL Server Management Studio tramite VSIXInstaller in SSMS 18.x.
  
## <a name="visual-studio-deployment-of-an-extension-for-ssms-18x"></a>Distribuzione di un'estensione di Visual Studio per SSMS 18.x

Per installare manualmente le estensioni, copiare i file di estensione (VSIX) associati nella cartella predefinita delle estensioni SSMS.  SSMS controlla automaticamente la cartella delle estensioni all'avvio.  La distribuzione VSIX può essere completata da Visual Studio durante la compilazione del progetto. 

  
1.  Individuare la cartella di installazione di SSMS e delle estensioni predefinita.  Con le impostazioni di installazione di SSMS predefinite, il percorso della cartella è ```C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\Extensions\```.  


2. Avviare Visual Studio come amministratore.

3.  Il processo di copia dei file può essere completato da Visual Studio durante la compilazione selezionando la casella di controllo "Copia contenuto VSIX nella posizione seguente" nella scheda VSIX della finestra delle proprietà del progetto. Nella casella di testo sotto la casella di controllo immettere il percorso della cartella precedente con una cartella per l'estensione accodata.  ad esempio ```C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\Extensions\SampleExtension```
  
![Impostazioni VSIX nella finestra delle proprietà del progetto con tre caselle di controllo e una casella di testo](./media/install-extensions/vsix_ssms.png)

4. Compilare il progetto di estensione. Se la compilazione ha esito positivo, i file di estensione saranno trasferiti nella cartella delle estensioni di SSMS.

5.  Avviare SSMS e testare le funzionalità delle estensioni.
  
