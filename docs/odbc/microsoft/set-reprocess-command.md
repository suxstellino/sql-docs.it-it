---
description: SET REPROCESS (comando)
title: IMPOSTA comando di rielaborazione | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET REPROCESS command [ODBC]
ms.assetid: b0708757-b1d7-42f3-8988-787f2a806b8b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b45188db00eda553bb3443d791d889a4d8c31a77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208593"
---
# <a name="set-reprocess-command"></a>SET REPROCESS (comando)
Specifica il numero di volte o il periodo di blocco di un file o di un record dopo un tentativo di blocco non riuscito.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
SET REPROCESS TO nAttempts [SECONDS] | TO AUTOMATIC  
```  
  
## <a name="arguments"></a>Argomenti  
 A *nAttempts*[secondi]  
 Specifica il numero di secondi per il tentativo di bloccare un record o un file dopo un tentativo iniziale non riuscito. Il valore predefinito è 0. il valore massimo è 32.000.  
  
 SECONDI specifica che Visual FoxPro tenta di bloccare un file o un record per *nAttempts* secondi. È disponibile solo quando *nAttempts* è maggiore di zero.  
  
 Se, ad esempio, *nAttempts* è 30, Visual FoxPro tenta di bloccare un record o un file fino a 30 volte. Se si includono anche secondi (impostare rielaborazione su 30 secondi), Visual FoxPro tenterà continuamente di bloccare un record o un file per un massimo di 30 secondi.  
  
 Se è attiva una routine ON ERROR e se i tentativi da parte di un comando di bloccare il record o il file hanno esito negativo, viene eseguita la routine ON ERROR. Tuttavia, se una funzione tenta il blocco, non viene eseguita una routine ON ERROR e la funzione restituisce false (. F.).  
  
 Se una routine ON ERROR non è attiva, un comando tenta di bloccare il record o il file e il blocco non può essere inserito, viene generato un errore. Se una funzione tenta di inserire il blocco, l'avviso non viene visualizzato e la funzione restituisce false (. F.).  
  
 Se *nAttempts* è 0 (valore predefinito) e si esegue un comando o una funzione che tenta di bloccare un record o un file, Visual FoxPro tenta di bloccare il record o il file per un periodo illimitato. Se il record o il file diventa disponibile per il blocco durante l'attesa, il blocco viene inserito e il messaggio di sistema viene cancellato. Se una funzione tenta di inserire il blocco, la funzione restituisce true (. T.).  
  
 Se è attiva una routine ON ERROR e un comando tenta di bloccare il record o il file, la routine ON ERROR ha la precedenza su ulteriori tentativi di bloccare il record o il file. La routine ON ERROR viene immediatamente eseguita. Visual FoxPro non tenta un ulteriore blocco di record o file e non Visualizza il messaggio di sistema.  
  
 Se *nAttempts* è 1, Visual FoxPro tenta di bloccare il record o il file per un periodo illimitato e non viene eseguita alcuna routine on error.  
  
 Se un blocco è stato inserito da un altro utente nel record o nel file che si sta tentando di bloccare, è necessario attendere che l'utente rilasci il blocco.  
  
 A AUTOMATICO  
 Specifica che Visual FoxPro tenta di bloccare il record o il file per un periodo illimitato. (Impostare rielaborazione su-2 è un comando equivalente).  
  
## <a name="remarks"></a>Commenti  
 Il primo tentativo di bloccare un record o un file non ha sempre esito positivo. Spesso un record o un file è bloccato da un altro utente nella rete. SET REPROCESS determina se Visual FoxPro esegue ulteriori tentativi di blocco del record o del file quando il tentativo iniziale ha esito negativo. È possibile specificare il numero di volte in cui vengono eseguiti altri tentativi o per quanto tempo vengono eseguiti i tentativi. Una routine ON ERROR influiscono sul modo in cui vengono gestiti i tentativi di blocco non riusciti.
