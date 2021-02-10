---
description: Estensioni di Visual C++ per ADO
title: Estensioni Visual C++ per ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- ADO, Visual C++
- Visual C++ [ADO], VC++ extensions for ADO
ms.assetid: 2952ece0-7217-4448-bb09-f6b64f43b7e2
author: rothja
ms.author: jroth
ms.openlocfilehash: 837ecdb45ead79e9e957a6d2381ccfaf1de6fb2c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028666"
---
# <a name="visual-c-extensions-for-ado"></a>Estensioni di Visual C++ per ADO
Il metodo preferito per la programmazione di ADO con Visual C++ consiste nell'utilizzare la direttiva **#import** , come illustrato nella [programmazione Microsoft Visual C++ ADO](./visual-c-ado-programming.md). Tuttavia, le versioni precedenti di ADO venivano fornite con un metodo alternativo di programmazione utilizzando Visual C++: le estensioni Visual C++. In questa sezione viene illustrata questa funzionalità per gli utenti che devono gestire il codice delle estensioni Visual C++, ma è necessario scrivere un nuovo codice ADO utilizzando #**Import**.

 Uno dei processi più noiosi Visual C++ ai programmatori quando si recuperano i dati con ADO consiste nel convertire i dati restituiti come tipo di dati VARIANT in un tipo di dati C++ e quindi archiviare i dati convertiti in una classe o una struttura. Oltre a essere complessa, il recupero dei dati C++ tramite un tipo di dati VARIANT diminuisce le prestazioni.

 ADO fornisce un'interfaccia che supporta il recupero di dati in tipi di dati C/C++ nativi senza passare attraverso una variante e fornisce anche macro del preprocessore che semplificano l'utilizzo dell'interfaccia. Il risultato è uno strumento flessibile che è più facile da usare e offre ottime prestazioni.

 Uno scenario comune di client C/C++ consiste nell'associare un record in un [Recordset](../../reference/ado-api/recordset-object-ado.md) a una classe o a uno struct c/c++ contenente tipi nativi c/c++. Quando si passano attraverso le varianti, questo implica la scrittura di codice di conversione da VARIANT a tipi nativi C/C++. Le estensioni Visual C++ per ADO sono destinate a rendere questo scenario molto più semplice per il programmatore di Visual C++.

 Per ulteriori informazioni sulle estensioni Visual C++ per ADO, vedere gli argomenti seguenti.

-   [Uso delle estensioni Visual C++ per ADO](./using-visual-c-extensions.md)

-   [Intestazione delle estensioni di Visual C++](./visual-c-extensions-header.md)

-   [Esempio di ADO con estensioni Visual C++](./visual-c-extensions-example.md)

## <a name="see-also"></a>Vedere anche
 Esempio di [sintassi ADO for Visual C++ per](../../reference/ado-api/ado-for-visual-c-syntax-index-for-com.md) [estensioni Visual C++](./visual-c-extensions-example.md) COM uso dell'intestazione [Visual C++ Extensions](./using-visual-c-extensions.md) [Visual C++ Extensions](./visual-c-extensions-header.md)