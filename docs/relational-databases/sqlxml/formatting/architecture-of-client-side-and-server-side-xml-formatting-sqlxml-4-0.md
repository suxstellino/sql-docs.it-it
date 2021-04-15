---
title: Architettura di XML lato client e server (SQLXML)
description: Informazioni sull'architettura della formattazione XML sul lato client e sul lato server in SQLXML 4.0.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- providers [SQLXML], XML formatting architecture
- SQLOLEDB provider
- client-side XML formatting
- data providers [SQLXML], XML formatting architecture
- SQLNCLI, XML
- server-side XML formatting
- SQL Server Native Client, XML
- SQLXMLOLEDB Provider, XML formatting architecture
ms.assetid: 52440d9e-89fd-4c15-a008-a1ea99f41387
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2d82b9f0c5ebdc7dda410e3deaf63f8b6cf5e6f5
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490496"
---
# <a name="architecture-of-client-side-and-server-side-xml-formatting-sqlxml-40"></a>Architettura della formattazione XML sul lato client e sul lato server (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Nell'illustrazione seguente viene mostrata l'architettura della formattazione XML sul lato server.  
  
 ![Architettura della formattazione XML sul lato server](../../../relational-databases/sqlxml/formatting/media/serversidexml.gif "Architettura della formattazione XML sul lato server")  
  
 In questo esempio il comando specificato sul client viene inviato al server. Il server produce un documento XML e lo restituisce al client. In questo caso, il server ha un'istanza di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Grazie alla formattazione XML sul lato server, è possibile utilizzare il provider SQLXMLOLEDB o il provider SQLOLEDB.  Il provider SQLXMLOLEDB utilizza Sqlxml4.dll che è incluso in SQLXML 4.0. Quando si utilizza il provider SQLOLEDB, per impostazione predefinita si ottiene la funzionalità SQLXML fornita dal file Sqlxmlx.dll incluso in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows o in Microsoft Data Access Components (MDAC) 2.6 o versione successiva. Per usare Sqlxml4.dll con SQLOLEDB, è necessario impostare la proprietà Versione SQLXML su "SQLXML.4.0" nell'oggetto Connessione SQLOLEDB. In entrambi i casi, il server produce il documento XML e lo invia al client.  
  
> [!NOTE]  
>  Gli updategram e le query XPath vengono analizzati sul client. Per ottenere il modello XPath o la funzionalità degli updategram in SQLXML 4.0, utilizzare Sqlxml4.dll.  
  
 Nell'illustrazione seguente viene mostrata l'architettura della formattazione XML sul lato client.  
  
 ![Architettura della formattazione XML sul lato client](../../../relational-databases/sqlxml/formatting/media/clientsidexml.gif "Architettura della formattazione XML sul lato client")  
  
 In questo esempio il client utilizza il provider SQLXMLOLEDB. Nella stringa di connessione la proprietà provider di dati deve essere impostata su SQLOLEDB. Questo è l'unico valore accettato in SQLXML 4.0. Il comando eseguito nel client viene inviato al server. Il set di righe generato nel server viene inviato al client. Sul client viene eseguita la formattazione del documento XML dal set di righe.  
  
 In SQLXML 4.0 è possibile utilizzare [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (SQLNCLI11) o il provider SQLOLEDB come provider di dati. È possibile accedere a qualsiasi origine dati. La trasformazione XML può essere applicata al client purché la query restituisca un singolo set di righe.  
  
  
