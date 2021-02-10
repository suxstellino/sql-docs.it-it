---
description: Recupero di dati
title: Recupero di dati | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, getting data
ms.assetid: 3931e7ec-f66b-4d5d-aad3-c4bf12e8b154
author: rothja
ms.author: jroth
ms.openlocfilehash: 9f53092d1677afd0c6eec25c2bdcde6f75b47657
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037421"
---
# <a name="getting-data"></a>Recupero di dati
Le [nozioni di base su ADO](./ado-fundamentals.md)e l'esempio [HelloData](./hellodata-a-simple-ado-application.md) , in particolare, hanno introdotto le quattro operazioni principali necessarie per la creazione di un'applicazione ADO, ovvero il recupero dei dati, l'analisi dei dati, la modifica dei dati e l'aggiornamento dei dati. Questa sezione illustra il recupero dei dati in modo più dettagliato.  
  
 A livello di base, diversi oggetti ADO contribuiscono alle operazioni di recupero dei dati. Per prima cosa è necessario connettersi a un'origine dati utilizzando un oggetto ADO **Connection** . Passare quindi le istruzioni all'origine dati usando un oggetto **comando** ADO. Infine, si ricevono spesso dati in un oggetto **Recordset** ADO.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Connessione a origini dati](./connecting-to-data-sources.md)  
  
-   [Preparazione ed esecuzione di comandi](./preparing-and-executing-commands.md)  
  
-   [Ricezione dei risultati](./receiving-results.md)