---
title: Esecuzione di un comando (OLE DB Driver) | Microsoft Docs
description: Informazioni sul modo in cui un consumer in OLE DB Driver per SQL Server esegue un comando creando prima una sessione, ottenendo un set di righe e usando il comando Esegui.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- commands [OLE DB Driver for SQL Server]
- OLE DB, executing commands
- sessions [OLE DB Driver for SQL Server]
- OLE DB extensions for XML
- OLE DB Driver for SQL Server, command execution
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 91c5377e84a47255bd079332c58835ec7413ce6d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749721"
---
# <a name="executing-a-command"></a>Esecuzione di un comando
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Una volta stabilita la connessione a un'origine dati, il consumer chiama il metodo **IDBCreateSession::CreateSession** per creare una sessione. La sessione funge da comando, set di righe o factory di transazioni.  
  
 Per lavorare direttamente con singoli indici o tabelle, il consumer richiede l'interfaccia **IOpenRowset**. Il metodo **IOpenRowset::OpenRowset** apre e restituisce un set di righe che include tutte le righe di un singolo indice o tabella di base.  
  
 Per eseguire un comando, ad esempio SELECT \* FROM Authors, il consumer richiede l'interfaccia **IDBCreateCommand**. Il consumer può eseguire il metodo **IDBCreateCommand::CreateCommand** per creare un oggetto comando e richiedere l'interfaccia **ICommandText**. Il metodo **ICommandText::SetCommandText** viene usato per specificare il comando da eseguire.  
  
 Per eseguire il comando viene usato il comando **Esegui**. Il comando può essere qualsiasi nome di istruzione o di procedura SQL. Non tutti i comandi producono un oggetto set di risultati (set di righe). Comandi come SELECT * FROM Authors producono un set di risultati.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di un driver OLE DB per applicazione SQL Server](../../oledb/ole-db-driver/creating-a-oledb-driver-for-sql-server-application.md)  
  
  
