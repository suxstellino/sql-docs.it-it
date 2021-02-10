---
description: Provider di dati
title: Provider di dati | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data providers [ADO]
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 877b9f25-60c4-4ab6-8052-2c28a3849e89
author: rothja
ms.author: jroth
ms.openlocfilehash: 67abe2bd8d2af4e43b1f6d14aed6588b9ddd7a2e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033306"
---
# <a name="data-providers"></a>Provider di dati
I provider di dati rappresentano diverse origini di dati, ad esempio database SQL, file indicizzati sequenziali, fogli di calcolo, archivi di documenti e file di posta elettronica. I provider espongono i dati in modo uniforme usando un'astrazione comune denominata set di righe.  
  
 ADO è potente e flessibile perché può connettersi a uno dei diversi provider di dati e comunque esporre lo stesso modello di programmazione, indipendentemente dalle funzionalità specifiche di un determinato provider. Tuttavia, poiché ogni provider di dati è univoco, il modo in cui l'applicazione interagisce con ADO varierà in base al provider di dati.  
  
 Ad esempio, le funzionalità e le funzionalità del provider OLE DB per SQL Server, che viene utilizzato per accedere ai database Microsoft SQL Server, sono notevolmente diverse da quelle del provider Microsoft OLE DB per Internet Publishing, che viene utilizzato per accedere agli archivi di file in un server Web.
