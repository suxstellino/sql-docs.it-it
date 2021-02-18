---
description: Controllo locale per la raccolta di dati di diagnostica e utilizzo di SSMS
title: Dati di diagnostica e utilizzo
ms.custom: seo-lt-2019
ms.date: 04/16/2019
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b2e188200daf0469f80cf74db901bcc240fd8706
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340693"
---
# <a name="local-audit-for-ssms-usage-and-diagnostic-data-collection"></a>Controllo locale per la raccolta di dati di diagnostica e utilizzo di SSMS
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Management Studio (SSMS) include funzionalità abilitate per Internet che consentono di raccogliere e inviare a Microsoft dati anonimi di diagnostica e di utilizzo delle funzionalità. SSMS può raccogliere informazioni standard sul computer e informazioni relative all'uso e alle prestazioni che possono venire trasmesse a Microsoft e analizzate al fine di migliorare la qualità, la sicurezza e l'affidabilità di SSMS. Microsoft non raccoglie il nome, l'indirizzo o altre informazioni di contatto degli utenti. Per informazioni dettagliate, vedere l'[Informativa sulla privacy di Microsoft](https://privacy.microsoft.com/privacystatement) e [Supplemento alla privacy di SQL Server](../sql-server/sql-server-privacy.md).

## <a name="audit-feature-usage-and-diagnostic-data"></a>Controllare i dati di diagnostica e utilizzo delle funzionalità

Per vedere i dati relativi all'utilizzo delle funzionalità raccolti da SSMS, seguire questa procedura:

1.  Avviare SSMS.
2.  Fare clic su **Visualizza** e quindi su **Output** nel menu principale per visualizzare la finestra **Output**. 
3.  Quando la finestra **Output** è visualizzata, scegliere **Telemetria** dal menu **Mostra output di**.

Quando si usa SSMS per interagire con i database, la finestra **Output** mostra i dati raccolti.

## <a name="enable-or-disable-usage-and-diagnostic-data-collection-in-ssms"></a>Abilitare o disabilitare la raccolta di dati di diagnostica e utilizzo in SSMS

Per acconsentire esplicitamente alla raccolta di dati di utilizzo per SQL Server Management Studio o per rifiutare in modo esplicito:

- Per SQL Server Management Studio 17:

  `Subkey = HKEY_CURRENT_USER\Software\Microsoft\SQL Server Management Studio\14.0`

  Nome RegEntry = `UserFeedbackOptIn`

  Tipo di voce `DWORD`: `0` rifiuto esplicito; `1` consenso esplicito

  SSMS 17.x si basa inoltre sulla shell di Visual Studio 2015 e l'installazione di Visual Studio consente di abilitare i commenti e suggerimenti utenti per impostazione predefinita.  

  Per configurare Visual Studio per disabilitare i commenti e suggerimenti dei clienti nei singoli computer, modificare il valore della sottochiave del Registro di sistema seguente in una stringa `0`: `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM OptIn`

  Modificare ad esempio la sottochiave nella stringa seguente:  
  `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM OptIn `=` 0`

  I Criteri di gruppo basati sul Registro di sistema per queste sottochiavi del Registro di sistema vengono rispettati dalla raccolta di dati di diagnostica e utilizzo di SQL Server 2017.

- Per SQL Server Management Studio 18:

  `Subkey = HKEY_CURRENT_USER\Software\Microsoft\SQL Server Management Studio\18.0_IsoShell`

  Nome RegEntry = `UserFeedbackOptIn`

  Tipo di voce `DWORD`: `0` rifiuto esplicito; `1` consenso esplicito

## <a name="see-also"></a>Vedi anche

- [Configurare la raccolta di dati di diagnostica e utilizzo per SQL Server](../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md)
- [Controllo locale per la raccolta di dati di diagnostica e utilizzo di SQL Server](../sql-server/usage-and-diagnostic-data-in-local-audit.md)