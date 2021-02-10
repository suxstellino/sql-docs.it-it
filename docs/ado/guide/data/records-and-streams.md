---
description: Record e flussi
title: Record e flussi | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- streams [ADO]
- streams [ADO], about streams
- records [ADO]
ms.assetid: 4d68868e-2611-4b5c-9a89-7caa5f753151
author: rothja
ms.author: jroth
ms.openlocfilehash: 2de6347377cbdeb464941d766d732c23443efa2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032563"
---
# <a name="records-and-streams"></a>Record e flussi
ADO fornisce attualmente l'oggetto [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) come mezzo principale per accedere alle informazioni nelle origini dati, ad esempio i database relazionali. Tuttavia, alcuni provider supportano gli oggetti [record](../../../ado/reference/ado-api/record-object-ado.md) e [flusso](../../../ado/reference/ado-api/stream-object-ado.md) come oggetti alternativi o complementari con i quali è possibile modificare i dati dei provider. Per informazioni specifiche sul comportamento dei **record** , vedere la documentazione del provider.  
  
## <a name="records"></a>Record  
 Gli oggetti **record** funzionano essenzialmente come **Recordset** di una riga. Tuttavia, i **record** hanno una funzionalità limitata rispetto ai **Recordset** e dispongono di proprietà e metodi diversi. L'origine dei dati in un oggetto **record** può essere un comando che restituisce una riga di dati dal provider. L'utilizzo di oggetti **record** anziché di oggetti **Recordset** per ricevere i risultati da una query che restituisce una riga di dati Elimina il sovraccarico dovuto alla creazione di un'istanza dell'oggetto **Recordset** più complesso.  
  
 Gli oggetti **record** possono essere utilizzati con un altro scopo, in particolare con i provider per le origini dati diverse dai database relazionali tradizionali, ad esempio il [provider Microsoft OLE DB per la pubblicazione Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Gran parte delle informazioni che devono essere elaborate esiste, non come tabelle nei database, ma come messaggi nei sistemi di posta elettronica e nei file nei file System moderni. Gli oggetti **record** e **flusso** facilitano l'accesso alle informazioni archiviate in origini diverse dai database relazionali.  
  
 L'oggetto **record** può rappresentare e gestire dati quali directory e file in un file System o cartelle e messaggi in un sistema di posta elettronica. Per questi scopi, l'origine del **record** può essere la riga corrente di un **Recordset** aperto, un URL assoluto o un URL relativo insieme a un oggetto di [connessione](../../../ado/reference/ado-api/connection-object-ado.md) aperto.  
  
 In genere, è possibile usare un **Recordset** per rappresentare un contenitore o un elemento padre in una gerarchia, ad esempio una cartella o una directory. È possibile usare un **record** per restituire informazioni specifiche su un nodo nel contenitore padre, ad esempio un file o un documento. Il motivo principale per rappresentare questo tipo di informazioni è che queste origini dei **dati sono** eterogenee. Ciò significa che ogni **record** può avere un set e un numero di campi diversi. I **Recordset** tradizionali contenenti righe da un database sono omogenei, il che significa che ogni riga ha lo stesso numero e tipo di campi.  
  
 Per ulteriori informazioni sull'utilizzo dell'oggetto **record** per l'elaborazione di questi dati eterogenei da provider quali Internet Publishing Provider, vedere [utilizzo di ADO per Internet Publishing](../../../ado/guide/data/using-ado-for-internet-publishing.md).  
  
## <a name="streams"></a>Flussi  
 L'oggetto **Stream** fornisce i mezzi per la lettura, la scrittura e la gestione di un flusso di byte. Questo flusso di byte può essere di tipo text o Binary ed è di dimensioni limitate solo per le risorse di sistema. In genere, gli oggetti **flusso** ADO vengono utilizzati per gli scopi seguenti:  
  
-   Per contenere i dati di un **Recordset** salvato in formato XML. Questi flussi XML da **Recordset** salvati possono essere utilizzati come origine per l'apertura di un nuovo **Recordset**. Per altre informazioni, vedere [flussi e persistenza](../../../ado/guide/data/streams-and-persistence.md).  
  
-   Per contenere [CommandStreams](../../../ado/reference/ado-api/commandstream-property-ado.md) da eseguire sul provider come alternativa a [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md). Ad esempio, gli updategram XML possono essere utilizzati come origine per un comando sul provider Microsoft OLE DB per SQL Server.  
  
-   Per ricevere i risultati del provider in un formato diverso da un **Recordset**, ad esempio i risultati XML del provider Microsoft OLE DB per SQL Server. Per altre informazioni, vedere [recupero di set di risultati in flussi](../../../ado/guide/data/retrieving-resultsets-into-streams.md).  
  
-   Per contenere il testo o i byte che costituiscono un file o un messaggio, in genere utilizzato con provider quali il provider Microsoft OLE DB per Internet Publishing. Per ulteriori informazioni sull'utilizzo di oggetti **flusso** , vedere [utilizzo di ADO per Internet Publishing](../../../ado/guide/data/using-ado-for-internet-publishing.md).  
  
 Un oggetto **flusso** può essere aperto in:  
  
-   File semplice specificato con un URL.  
  
-   Campo di un **record** o di un **Recordset** contenente un oggetto **flusso** .  
  
-   Flusso predefinito di un **record** o di un oggetto **Recordset** che rappresenta una directory o un file composto.  
  
-   Campo di risorse contenente l'URL di un file semplice.  
  
-   Nessuna origine particolare. In questo caso, un oggetto **flusso** viene aperto in memoria. È possibile scrivere i dati e salvarli in un altro **flusso** o file.  
  
-   Campo BLOB in un **Recordset**.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Flussi e persistenza](../../../ado/guide/data/streams-and-persistence.md)  
  
-   [Flussi di comandi](../../../ado/guide/data/command-streams.md)  
  
-   [Recupero di set di risultati nei flussi](../../../ado/guide/data/retrieving-resultsets-into-streams.md)  
  
-   [Uso di ADO per Internet Publishing](../../../ado/guide/data/using-ado-for-internet-publishing.md)
