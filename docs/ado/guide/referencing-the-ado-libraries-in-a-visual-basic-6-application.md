---
description: Riferimenti alle librerie ADO in un'applicazione Visual Basic 6
title: Riferimento alle librerie ADO in un'applicazione Visual Basic 6 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- libraries [ADO]
- referencing libraries in a Visual Basic application[ADO]
- ADO, libraries
ms.assetid: cfd37a82-aad2-41cd-8d13-1566c43d95f0
author: rothja
ms.author: jroth
ms.openlocfilehash: 431a58609ff26ace443b012bc2835e5ba89736db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036611"
---
# <a name="referencing-the-ado-libraries-in-a-visual-basic-6-application"></a>Riferimenti alle librerie ADO in un'applicazione Visual Basic 6
Per importare le librerie ADO in un'applicazione Microsoft Visual Basic 6, è necessario impostare un riferimento nel progetto Visual Basic.  
  
### <a name="to-set-a-reference-to-the-ado-libraries-in-a-visual-basic-project"></a>Per impostare un riferimento alle librerie ADO in un progetto Visual Basic  
  
1.  Creare un nuovo progetto di Visual Basic o aprirne uno esistente.  
  
2.  Fare clic sulla voce di menu **progetto** , quindi selezionare **riferimenti** dal pannello menu a discesa.  
  
3.  In **riferimenti disponibili** selezionare la casella per la **libreria Microsoft ActiveX Data Objects *n. n***, dove **_n. n_*_ rappresenta il numero di versione più recente. Il campo _* location** seguente dovrebbe identificare la scelta come *$installDir\msado15.dll*, dove *$InstallDir* rappresenta il percorso della directory in cui è stata installata la libreria ADO.  
  
4.  Se si intende utilizzare ADO MD, ripetere il passaggio 3 per selezionare la **libreria Microsoft ActiveX Data Objects (multidimensionale) *n. n***. Il campo **location** dovrebbe identificare la scelta come *$installDir\msadomd.dll*.  
  
5.  Se si intende usare ADOX, ripetere il passaggio 3 per selezionare **Microsoft ADO Ext. *n. n* per DDL e sicurezza**. Il campo **location** dovrebbe identificare la scelta come *$installDir\msadox.dll*.  
  
6.  Fare clic su **OK** per completare l'impostazione dei riferimenti.  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 L'installazione di ADO copia anche le librerie dei tipi seguenti di versioni precedenti:  
  
-   *Msado27. tlb*, libreria di tipi ADO 2,7  
  
-   *msado26. tlb*, libreria di tipi ADO 2,6  
  
-   *Msado25. tlb*, libreria di tipi ADO 2,5  
  
-   *Msado21. tlb*, libreria di tipi ADO 2,1  
  
-   *msado20. tlb*, libreria di tipi ADO 2,0  
  
 Se l'applicazione deve usare una di queste librerie ADO per motivi di compatibilità con le versioni precedenti, è necessario importare la versione appropriata della libreria dei tipi. A tale scopo, seguire le procedure descritte nella sezione precedente, sostituendo *msado15.dll* da *msadoXX. tlb*, dove *XX* rappresenta il numero di versione che è necessario importare.
