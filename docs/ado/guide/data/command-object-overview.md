---
description: Panoramica dell'oggetto Command
title: Cenni preliminari sull'oggetto Command | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- command object [ADO]
ms.assetid: e84a14b1-3c2a-4f7d-a966-9e08a93948df
author: rothja
ms.author: jroth
ms.openlocfilehash: c23ce0b24fcd5fb82f306c1746f7a277bed293ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033307"
---
# <a name="command-object-overview"></a>Panoramica dell'oggetto Command
Con un oggetto **Command** è possibile eseguire le operazioni seguenti:  
  
-   Definire il testo eseguibile del comando (ad esempio, un'istruzione SQL o un stored procedure) utilizzando la proprietà **CommandText** .  
  
-   Definire query con parametri o argomenti di stored procedure usando oggetti **parametro** e la raccolta **Parameters** .  
  
-   Eseguire un comando e restituire un oggetto **Recordset** , se appropriato, usando il metodo **Execute** .  
  
-   Specificare il tipo di comando utilizzando la proprietà **CommandType** prima dell'esecuzione per ottimizzare le prestazioni.  
  
-   Specificare informazioni specifiche sul testo del comando utilizzando la proprietà **dialetto** dell'oggetto **Command** .  
  
-   Controllare se il provider salva una versione preparata o compilata del comando prima dell'esecuzione utilizzando la proprietà **preparata** .  
  
-   Consente di impostare il numero di secondi di attesa per l'esecuzione di un comando da parte di un provider utilizzando la proprietà **CommandTimeout** .  
  
-   Associare una connessione aperta a un oggetto **Command** impostando la relativa proprietà **ActiveConnection** .  
  
-   Impostare la proprietà **Name** per identificare l'oggetto **Command** come metodo sull'oggetto **Connection** associato.  
  
-   Passare un oggetto **Command** alla proprietà **source** di un **Recordset** per ottenere i dati.  
  
-   Passare un oggetto **flusso** contenente un comando (ad esempio, un comando XML) a un provider che lo supporta.
