---
description: Connettore Microsoft per Oracle
title: Connettore Microsoft per Oracle | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 70acf129fa608c1058e808d2193cf4c901198d6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343219"
---
# <a name="microsoft-connector-for-oracle"></a>Connettore Microsoft per Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Connettore Microsoft per Oracle consente di abilitare la capacità di esportare e caricare i dati in un'origine dati Oracle in un pacchetto SSIS.

## <a name="version-support"></a>Supporto versione

I prodotti Microsoft SQL Server seguenti sono supportati da Connettore Microsoft per Oracle:

- A partire da SQL Server 2019 CU1
- SQL Server Data Tools (SSDT) 15.9.3 o versioni successive per Visual Studio 2017
- Microsoft SQL Server Data Tools (SSDT) per Visual Studio 2019

Sono supportate le origini dati delle versioni seguenti di Oracle Database:

- Oracle 10.x
- Oracle 11.x
- Oracle 12c
- Oracle 18c (senza supporto per l'autenticazione di Windows)
- Oracle 19c (senza supporto per l'autenticazione di Windows)

Oracle Database è supportato in tutti i sistemi operativi e le piattaforme.
> [!NOTE]
>
> Il client Oracle non è necessario per Connettore Microsoft per database Oracle in SQL Server 2019.

## <a name="installation"></a>Installazione

Per installare il connettore per database Oracle, scaricare ed eseguire il programma di installazione dalla [versione più recente di Connettore Microsoft per Oracle](https://www.microsoft.com/download/details.aspx?id=58228). Quindi, seguire le indicazioni nell'installazione guidata.

Dopo aver installato il connettore, è necessario riavviare il servizio SQL Server Integration Services per assicurarsi che l'origine e la destinazione Oracle funzionino correttamente.

Per eseguire un pacchetto SSIS destinato a SQL Server 2017 e versioni precedenti, oltre a **Connettore Microsoft per Oracle** è necessario installare il **client Oracle** e il **connettore Microsoft per Oracle di Attunity** con la versione corrispondente dai collegamenti seguenti:

- [SQL Server 2017: Connettore Microsoft versione 5.0 per Oracle di Attunity](https://www.microsoft.com/download/details.aspx?id=55179)
- [SQL Server 2016: Connettore Microsoft versione 4.0 per Oracle di Attunity](https://www.microsoft.com/download/details.aspx?id=52950)
- [SQL Server 2014: Connettore Microsoft versione 3.0 per Oracle di Attunity](https://www.microsoft.com/download/details.aspx?id=44582)
- [SQL Server 2012: Connettore Microsoft versione 2.0 per Oracle di Attunity](https://www.microsoft.com/download/details.aspx?id=29283)

## <a name="limitations-and-known-issues"></a>Limitazioni e problemi noti

- Le viste non vengono elencate nell'origine *Nome tabella o vista* di Oracle. Come soluzione alternativa, usare il comando SQL ed eseguire un'operazione select * dalla vista o impostare il nome della vista sulla proprietà [Oracle Source].[TableName] in Editor avanzato.

## <a name="uninstallation"></a>Disinstallazione

È possibile eseguire la disinstallazione guidata per rimuovere Connettore Microsoft per database Oracle da SQL Server.

## <a name="next-steps"></a>Passaggi successivi

- Configurare [Gestione connessione Oracle](oracle-connection-manager.md).
- Configurare l'[origine Oracle](oracle-source.md).
- Configurare la [destinazione Oracle](oracle-destination.md).
- Per eventuali domande, visitare [TechCommunity](https://aka.ms/AA5u35j).
