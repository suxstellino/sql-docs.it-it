---
title: Raccolta dei dati di utilizzo e diagnostica di SSMA
description: Informazioni sulla raccolta dei dati di diagnostica e di utilizzo in SQL Server Migration Assistant.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 04/02/2021
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.author: alexiva
ms.openlocfilehash: d306193accdf97ab04b45724cc8a275c890bec80
ms.sourcegitcommit: f1a571b6ce02a39c385ad32508ceff23475ed9f5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/05/2021
ms.locfileid: "106387383"
---
# <a name="ssma-usage-and-diagnostic-data-collection"></a>Raccolta dei dati di utilizzo e diagnostica di SSMA

SQL Server Migration Assistant (SSMA) contiene funzionalità abilitate per Internet che consentono di raccogliere e inviare dati di diagnostica e di utilizzo delle caratteristiche anonimi a Microsoft.

## <a name="collected-data"></a>Dati raccolti

SSMA può raccogliere informazioni standard sul computer e informazioni sull'utilizzo e sulle prestazioni che possono essere trasmesse a Microsoft e analizzate per migliorare la qualità, la sicurezza e l'affidabilità di SSMA. Microsoft non raccoglie il nome, l'indirizzo o altre informazioni di contatto degli utenti. Per informazioni dettagliate, vedere l'[Informativa sulla privacy di Microsoft](https://privacy.microsoft.com/privacystatement) e [Supplemento alla privacy di SQL Server](../sql-server/sql-server-privacy.md).
## <a name="enable-or-disable-usage-and-diagnostic-data-collection-in-ssma"></a>Abilitare o disabilitare la raccolta di dati di utilizzo e diagnostica in SSMA

La seguente voce del registro di sistema consente di acconsentire o rifiutare esplicitamente la raccolta dei dati:

Nome RegEntry = `DisableTelemetry`  
Tipo di voce `STRING` : `True` rifiuto esplicito `False` o non presente

Inoltre, i controlli automatici per le versioni più recenti degli strumenti durante l'avvio possono essere disabilitati aggiungendo la voce seguente:

Nome RegEntry = `DisableAutoUpdate`  
Tipo di voce `STRING` : `True` rifiuto esplicito `False` o non presente

A seconda dell'origine della migrazione, la voce deve essere inserita in una delle chiavi del registro di sistema seguenti:

- Per SQL Server Migration Assistant per l'accesso:

  Sottochiave = `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Migration Assistant for Access`  
  Sottochiave (applicazione a 32 bit installata in un sistema operativo a 64 bit) = `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server Migration Assistant for Access`  

- Per SQL Server Migration Assistant per DB2:

  Sottochiave = `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Migration Assistant for DB2`  
  Sottochiave (applicazione a 32 bit installata in un sistema operativo a 64 bit) = `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server Migration Assistant for DB2`  

- Per SQL Server Migration Assistant per MySQL:

  Sottochiave = `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Migration Assistant for MySQL`  
  Sottochiave (applicazione a 32 bit installata in un sistema operativo a 64 bit) = `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server Migration Assistant for MySQL`  

- Per SQL Server Migration Assistant per Oracle:

  Sottochiave = `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Migration Assistant for Oracle`  
  Sottochiave (applicazione a 32 bit installata in un sistema operativo a 64 bit) = `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server Migration Assistant for Oracle`  

- Per SQL Server Migration Assistant per Sybase:

  Sottochiave = `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Migration Assistant for Sybase`  
  Sottochiave (applicazione a 32 bit installata in un sistema operativo a 64 bit) = `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server Migration Assistant for Sybase`  
