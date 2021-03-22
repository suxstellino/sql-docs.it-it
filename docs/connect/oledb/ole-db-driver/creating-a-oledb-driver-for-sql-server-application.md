---
title: Creazione di un'applicazione del driver OLE DB per SQL Server
description: Informazioni sui passaggi necessari per creare un driver OLE DB per SQL Server applicazione e per trovare altre risorse.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, application creation
- applications [OLE DB Driver for SQL Server]
- OLE DB, creating applications
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fdf209a8bdb34dc5f22a60ee190a118d8a608995
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793088"
---
# <a name="creating-an-ole-db-driver-for-sql-server-application"></a>Creazione di un'applicazione del driver OLE DB per SQL Server

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  La creazione di un'applicazione OLE DB Driver per SQL Server prevede questi passaggi:

1. Avvio di una connessione a un'origine dati.
2. Esecuzione di un comando.
3. Elaborazione dei risultati.

> [!NOTE]
> Se possibile, usare l'autenticazione di Windows. Se non è disponibile, agli utenti verrà richiesto di immettere le credenziali in fase di esecuzione. Evitare di archiviare le credenziali in un file. Se è necessario rendere persistenti le credenziali, è consigliabile crittografarle usando [CryptoAPI Win32](/windows/win32/seccng/cng-portal).

## <a name="in-this-section"></a>Contenuto della sezione

- [Avvio di una connessione a un'origine dati](establishing-a-connection-to-a-data-source.md)
- [Esecuzione di un comando](executing-a-command.md)
- [Elaborazione dei risultati](processing-results.md)
- [Informazioni sulle proprietà OLE DB](about-ole-db-properties.md)
- [Uso della clausola OUTPUT con OLE DB nel driver OLE DB per SQL Server](using-the-output-clause-with-ole-db-in-oledb-driver-for-sql-server.md)

## <a name="see-also"></a>Vedi anche

[Driver OLE DB per programmazione con SQL Server](../ole-db/oledb-driver-for-sql-server-programming.md)
