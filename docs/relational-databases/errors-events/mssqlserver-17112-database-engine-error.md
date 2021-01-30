---
description: MSSQLSERVER_17112
title: MSSQLSERVER_17112
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17112 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 29f6dd1b508a5833c465ef5af013e69de8d44077
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196802"
---
# <a name="mssqlserver_17112"></a>MSSQLSERVER_17112
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|17112|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|INIT_INVCOMMAND|
|Testo del messaggio|Specificata un'opzione di avvio 'a' non valida dal Registro di sistema o dal prompt dei comandi. Correggere o rimuovere l'opzione.|
||

## <a name="explanation"></a>Spiegazione

Questo errore indica che è stata specificata un'[opzione di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md) non valida. Quando un'opzione di avvio non viene specificata correttamente, l'avvio di SQL Server non riesce oppure l'esecuzione potrebbe essere diversa dal previsto. Viene generato anche l'errore 17112.

In alcuni casi, è possibile che l'istanza venga avviata, ma quando si esamina il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], i parametri di avvio non risultano corretti:

> \<Datetime> Parametri di avvio del Registro di sistema:  
\<Datetime> Server -d D:\Programmi\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\master.mdf  
\<Datetime> Server -e D:\Programmi\Microsoft SQL Server\MSSQL.1\MSSQL\LOG\ERRORLOG  
\<Datetime> Server -l D:\Programmi\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\mastlog.ldf  
\<Datetime> Server -T1118 -g512

Si noti che entrambi gli ultimi due parametri di avvio sono sulla stessa riga.

È anche possibile notare che in alcuni casi l'aggiunta dei parametri di avvio necessari non ha avuto l'effetto desiderato sul comportamento del server.

## <a name="possible-causes"></a>Possibili cause

Si possono riscontrare questi problemi a causa dei motivi seguenti:

- Uso di parametri di avvio non presenti nell'elenco valido di parametri di avvio
- Specifica di parametri di avvio senza i delimitatori appropriati [;]
- Copia/incolla di parametri di avvio da editor di testo con introduzione di alcuni caratteri speciali invisibili, ad esempio uno spazio prima di -T
- Combinazione di maiuscole/minuscole non corretta per il parametro di avvio

## <a name="user-action"></a>Azione utente

Usare lo strumento Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per fornire e convalidare i parametri di avvio specificati per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Assicurarsi che ogni parametro di avvio sia delimitato in modo corretto e che non siano presenti caratteri speciali.

## <a name="more-information"></a>Ulteriori informazioni

Per altre informazioni su questo argomento, vedere gli argomenti seguenti:

- [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md)
- [Configurazione delle opzioni di avvio del server (Gestione configurazione SQL Server)](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)