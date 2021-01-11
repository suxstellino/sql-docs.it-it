---
title: Oggetti DbProviderFactory
description: Viene descritto il modello a livello di factory del provider e viene illustrato come usare le classi base nello spazio dei nomi `System.Data.Common`.
ms.date: 12/22/2020
ms.assetid: 2a8e2640-3a49-42a1-a3a9-b43026907ae1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1a373e7bde1e93d8dc92294f983a96de64e28ae1
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771771"
---
# <a name="dbproviderfactories"></a>Oggetti DbProviderFactory

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Lo spazio dei nomi <xref:System.Data.Common> fornisce classi per la creazione di istanze di <xref:System.Data.Common.DbProviderFactory> per l'uso di origini dati specifiche. Quando si crea un'istanza di <xref:System.Data.Common.DbProviderFactory> e si passano le informazioni sul provider di dati, `DbProviderFactory` è in grado di determinare l'oggetto connessione corretto, fortemente tipizzato, da restituire in base alle informazioni fornite.  

Il provider di dati <xref:Microsoft.Data.SqlClient> non è più elencato nel file machine.config ma i provider personalizzati continueranno a essere elencati.  

## <a name="in-this-section"></a>Contenuto della sezione  

[Ottenere un oggetto SqlClientFactory](obtain-sqlclientfactory.md)  
Viene illustrato come ottenere un oggetto `SqlClientFactory` dalla classe `DbProviderFactories` per lavorare con origini dati specifiche in .NET.  

## <a name="see-also"></a>Vedere anche

- [Recupero e modifica di dati in ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
