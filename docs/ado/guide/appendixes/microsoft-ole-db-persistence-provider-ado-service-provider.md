---
description: Provider di persistenza Microsoft OLE DB (provider di servizi ADO)
title: Provider di persistenza Microsoft OLE DB (provider di servizi ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], OLE DB persistence provider
- persistence provider [ADO]
- OLE DB persistence provider [ADO]
ms.assetid: e75ef0dc-2016-4fcc-8918-23311c0d4e02
author: rothja
ms.author: jroth
ms.openlocfilehash: 4ed23adb7922a18f53245f21b087414403ae61d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029365"
---
# <a name="microsoft-ole-db-persistence-provider-overview"></a>Panoramica del provider di persistenza di Microsoft OLE DB
Il provider di persistenza Microsoft OLE DB consente di salvare un oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) in un file e successivamente ripristinare l'oggetto **Recordset** dal file. Vengono mantenute le informazioni sullo schema, i dati e le modifiche in sospeso.

 È possibile salvare il **Recordset** nel formato ADTG (Advanced Data Table Gram) proprietario o nel formato Open Extensible Markup Language (XML).

## <a name="provider-keyword"></a>Parola chiave provider
 Per richiamare questo provider, specificare la parola chiave e il valore seguenti nella stringa di connessione.

```vb
"Provider=MSPersist"
```

## <a name="errors"></a>Errors
 Nell'applicazione è possibile rilevare gli errori seguenti rilasciati da questo provider.

|Costante|Descrizione|
|--------------|-----------------|
|E_BADSTREAM|Il formato del file aperto non è valido, ovvero il formato non è ADTG o XML.|
|E_CANTPERSISTROWSET|L'oggetto **Recordset** salvato presenta caratteristiche che ne impediscono l'archiviazione.|

## <a name="remarks"></a>Commenti
 Il provider di persistenza Microsoft OLE DB non espone proprietà dinamiche.

 Attualmente, non è possibile salvare solo oggetti **Recordset** gerarchici con parametri.

 Per ulteriori informazioni sull'archiviazione persistente di oggetti **Recordset** , vedere [persistenza recordset](../data/more-about-recordset-persistence.md).

 Quando si utilizza un flusso per aprire un **Recordset,** non devono essere specificati parametri diversi dal parametro *source* del metodo **Open** .
