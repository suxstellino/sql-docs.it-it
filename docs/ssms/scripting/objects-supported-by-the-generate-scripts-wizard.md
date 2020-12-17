---
title: Oggetti supportati dalla procedura guidata di generazione script
description: Scoprire quali tipi di oggetto possono essere pubblicati in modo facilitato con la procedura guidata Genera e pubblica script.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 071eb2cb-f073-41ca-9f4d-11d3b8803495
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6ba406c4418979f4cdc57363806c9561b3480959
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474272"
---
# <a name="objects-supported-by-the-generate-scripts-wizard"></a>Oggetti supportati dalla procedura guidata di generazione script
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La procedura guidata Genera e pubblica script supporta un subset degli oggetti supportati da [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
## <a name="supported-objects"></a>Oggetti supportati  
 Nella tabella seguente vengono elencati gli oggetti pubblicabili supportati dalla procedura guidata Genera e pubblica script.  
  
:::row:::
    :::column:::
        Ruolo applicazione
    :::column-end:::
    :::column:::
        Ruolo del database
    :::column-end:::
    :::column:::
        SCHEMA
    :::column-end:::
    :::column:::
        Aggregazione definita dall'utente
    :::column-end:::
    :::column:::
        Visualizzazione*
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Assembly
    :::column-end:::
    :::column:::
        Vincolo DEFAULT
    :::column-end:::
    :::column:::
        Stored procedure*
    :::column-end:::
    :::column:::
        Tipo di dati definito dall'utente
    :::column-end:::
    :::column:::
        Raccolta di XML Schema
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Vincolo CHECK
    :::column-end:::
    :::column:::
        Catalogo full-text
    :::column-end:::
    :::column:::
        Sinonimo
    :::column-end:::
    :::column:::
        Funzione definita dall'utente
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Stored procedure CLR (Common Language Runtime)*
    :::column-end:::
    :::column:::
        Indice
    :::column-end:::
    :::column:::
        Tabella
    :::column-end:::
    :::column:::
        Tabella definita dall'utente
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Funzione CLR definita dall'utente
    :::column-end:::
    :::column:::
        Regola
    :::column-end:::
    :::column:::
        Utente**
    :::column-end:::
    :::column:::
        Tipo definito dall'utente
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

 *Pubblicato senza crittografia.  
  
 **Qualsiasi utente non di sistema esistente nel database viene pubblicato come ruolo.  
  
  
