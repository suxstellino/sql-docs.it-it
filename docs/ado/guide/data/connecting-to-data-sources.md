---
description: Connessione a origini dati
title: Connessione alle origini dati | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
ms.assetid: 82770486-37bd-4c90-885f-6817a7c77ad7
author: rothja
ms.author: jroth
ms.openlocfilehash: 4234cf0e54ae3f3427264835e9f88a715052c2fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037701"
---
# <a name="connecting-to-data-sources"></a>Connessione a origini dati
Un oggetto **connessione** ADO rappresenta una sessione univoca con un'origine dati, tra cui un DBMS, un archivio di file o un file di testo delimitato da virgole. Nel caso di un sistema di database client/server, la connessione ADO può essere una connessione di rete effettiva al server.  
  
 L'oggetto **Connection** supporta varie [proprietà e metodi](../../reference/ado-api/connection-object-properties-methods-and-events.md) per specificare le configurazioni di connessione, aprire e chiudere le connessioni, creare ed eseguire comandi sull'origine dati e fornire informazioni sulla progettazione dell'origine dati sottostante sotto forma di set di righe dello schema e così via. A seconda della funzionalità supportata dal provider, alcune raccolte, metodi o proprietà di un oggetto **connessione** potrebbero non essere disponibili.  
  
 È possibile connettersi a un'origine dati utilizzando un oggetto **connessione** o un oggetto **Recordset** .  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Uso di un oggetto Connection](./using-a-connection-object.md)  
  
-   [Uso di un oggetto Recordset](./using-a-recordset-object.md)  
  
-   [Creazione di una stringa di connessione](./creating-a-connection-string.md)  
  
-   [Specifica delle proprietà di connessione](./specifying-connection-properties.md)  
  
-   [Controllo delle transazioni](./controlling-transactions-ado.md)