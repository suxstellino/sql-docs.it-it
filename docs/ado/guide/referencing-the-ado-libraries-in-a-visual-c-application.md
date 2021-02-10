---
description: Riferimenti alle librerie ADO in un'applicazione Visual C++
title: Riferimento alle librerie ADO in un'applicazione Visual C++ | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- libraries [ADO]
- referencing libraries in a Visual C++ application[ADO]
- ADO, libraries
ms.assetid: d3ea12ec-bca8-48c3-af57-ce14576108c9
author: rothja
ms.author: jroth
ms.openlocfilehash: 596e11a51a093be30fab7d9d3b273d6605b48b75
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032073"
---
# <a name="referencing-the-ado-libraries-in-a-visual-c-application"></a>Riferimenti alle librerie ADO in un'applicazione Visual C++
Per usare la versione più recente di ADO in un'applicazione Visual C++, usare la `#import` direttiva seguente:  
  
```cpp
#import "msado15.dll" \  
    no_namespace rename("EOF", "EndOfFile")  
```  
  
 Per usare ADO MD o ADOX, è necessario importare *msadomd.dll* o *msadox.dll* usando la sintassi precedente.  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 Per utilizzare una versione precedente di ADO, sostituire *msado15.dll* sopra con una delle librerie dei tipi seguenti.  
  
-   *Msado27. tlb*, libreria di tipi ADO 2,7  
  
-   *msado26. tlb*, libreria di tipi ADO 2,6  
  
-   *Msado25. tlb*, libreria di tipi ADO 2,5  
  
-   *Msado21. tlb*, libreria di tipi ADO 2,1  
  
-   *msado20. tlb*, libreria di tipi ADO 2,0
