---
description: Modifica di record esistenti
title: Modifica di record esistenti | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- editing data [ADO], existing records
ms.assetid: 17ce1263-5897-452a-9ea5-c7f96b33df65
author: rothja
ms.author: jroth
ms.openlocfilehash: 031d819e2c8eed63b9b8ff42aa58fb8e998a4273
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037501"
---
# <a name="editing-existing-records"></a>Modifica di record esistenti
Per modificare i record esistenti, passare alla riga che si desidera modificare e modificare la proprietà **value** dei campi che si desidera modificare. Per ulteriori informazioni sulla proprietà **value** dell'oggetto **campo** , vedere [analisi dei dati](./examining-data.md). A seconda del tipo di cursore, si utilizzerà **Update** o **UpdateBatch** per restituire le modifiche all'origine dati. Per ulteriori informazioni, vedere [aggiornamento e salvataggio permanente dei dati](./updating-and-persisting-data.md).  
  
 È in genere più efficiente usare un stored procedure con un oggetto Command per eseguire gli aggiornamenti, nonché per eseguire altre operazioni, perché una stored procedure non richiede la creazione di un cursore. Per ulteriori informazioni sui cursori, vedere [informazioni sui cursori e sui blocchi](./understanding-cursors-and-locks.md).