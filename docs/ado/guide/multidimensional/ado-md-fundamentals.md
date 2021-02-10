---
description: Nozioni fondamentali su ADO MD
title: Nozioni fondamentali su ADO MD | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO MD, fundamentals
ms.assetid: f6a20d9f-c1ab-474c-b9f3-82277e2a126d
author: rothja
ms.author: jroth
ms.openlocfilehash: 7634d32b35eef7d0289ef778bee9646a050e8755
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032193"
---
# <a name="ado-md-fundamentals"></a>Nozioni fondamentali su ADO MD
Microsoft® ActiveX® Data Objects (multidimensionale) (ADO MD) consente di accedere facilmente ai dati multidimensionali da linguaggi quali Microsoft Visual Basic® Microsoft Visual C++. ADO MD estende Microsoft ActiveX® Data Objects (ADO) per includere gli oggetti specifici di dati multidimensionali, ad esempio gli oggetti [CubeDef](../../reference/ado-md-api/cubedef-object-ado-md.md) e [cellt](../../reference/ado-md-api/cellset-object-ado-md.md) . Con ADO MD è possibile esplorare uno schema multidimensionale, eseguire una query su un cubo e recuperare i risultati.  
  
 Come ADO, ADO MD USA un provider di OLE DB sottostante per ottenere l'accesso ai dati. Per lavorare con ADO MD, il provider deve essere un provider di dati multidimensionale (MDP) come definito dalla specifica OLE DB per OLAP. Un MDP presenta i dati nelle viste multidimensionali anziché nelle viste tabulari, che rappresenta il modo in cui un provider di dati tabulari (TDP) presenta i dati. Per ulteriori informazioni sulla sintassi e sul comportamento specifici supportati dal provider, vedere la documentazione relativa al provider di OLE DB OLAP.  
  
 In questo documento si presuppone una conoscenza approfondita del linguaggio di programmazione Visual Basic e una conoscenza generale di ADO e OLAP. Per ulteriori informazioni, vedere la [Guida per programmatori ADO](../ado-programmer-s-guide.md) e [OLE DB per l'elaborazione analitica in linea (OLAP)](/previous-versions/windows/desktop/ms717005(v=vs.85)).  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Panoramica di schemi e dati multidimensionali](./overview-of-multidimensional-schemas-and-data.md)  
  
-   [Utilizzo dei dati multidimensionali](./working-with-multidimensional-data.md)  
  
-   [Uso di ADO con ADO MD](./using-ado-with-ado-md.md)  
  
-   [Programmazione con ADO MD](./programming-with-ado-md.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Modello a oggetti ADO MD](../../reference/ado-md-api/ado-md-object-model.md)   
 [Guida per programmatori ADO](../ado-programmer-s-guide.md)   
 [Estensioni ADO per la sicurezza e il linguaggio di definizione dei dati (ADOX)](../extensions/ado-extensions-for-data-definition-language-and-security-adox.md)   
 [Panoramica di schemi e dati multidimensionali](./overview-of-multidimensional-schemas-and-data.md)   
 [Programmazione con ADO MD](./programming-with-ado-md.md)   
 [Utilizzo di ADO con ADO MD](./using-ado-with-ado-md.md)   
 [Utilizzo dei dati multidimensionali](./working-with-multidimensional-data.md)