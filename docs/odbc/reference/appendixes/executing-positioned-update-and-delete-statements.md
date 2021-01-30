---
description: Esecuzione di istruzioni di eliminazione e aggiornamento posizionato
title: Esecuzione di istruzioni Update e Delete posizionate | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- positioned deletes [ODBC]
- cursor library [ODBC], positioned update or delete
- positioned updates [ODBC]
- ODBC cursor library [ODBC], positioned update or delete
ms.assetid: 1d64f309-2a6e-4ad1-a6b5-e81145549c56
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d10b2e5c625a222e8e09a5e783291ce613a492b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194832"
---
# <a name="executing-positioned-update-and-delete-statements"></a>Esecuzione di istruzioni di eliminazione e aggiornamento posizionato
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 Dopo che un'applicazione ha recuperato un blocco di dati con **SQLFetchScroll**, può aggiornare o eliminare i dati nel blocco. Per eseguire un aggiornamento o un'eliminazione posizionata, l'applicazione:  
  
1.  Chiama **SQLSetPos** per posizionare il cursore sulla riga da aggiornare o eliminare.  
  
2.  Costruisce un'istruzione Update o DELETE posizionata con la sintassi seguente:  
  
     **Aggiorna** *nome tabella*  
  
     **Set** *column-identifier* **=** {*Expression* &#124; **null**}  
  
     [**,** *identificatore* **=** di colonna {*expression* &#124; **null**}]  
  
     **Where current of** *Cursor-Name*  
  
     **Elimina da** *nome tabella* in **cui Current of** *Cursor-Name*  
  
     Il modo più semplice per costruire la clausola **set** in un'istruzione UPDATE posizionata consiste nell'utilizzare marcatori di parametro per ogni colonna da aggiornare e utilizzare **SQLBindParameter** per associarli ai buffer del set di righe per la riga da aggiornare. In questo caso, il tipo di dati C del parametro sarà uguale al tipo di dati C del buffer del set di righe.  
  
3.  Aggiorna i buffer del set di righe per la riga corrente se eseguirà un'istruzione UPDATE posizionata. Una volta eseguita correttamente un'istruzione UPDATE posizionata, la libreria di cursori copia i valori di ogni colonna della riga corrente nella relativa cache.  
  
    > [!CAUTION]  
    >  Se l'applicazione non aggiorna correttamente i buffer del set di righe prima di eseguire un'istruzione UPDATE posizionata, i dati nella cache non saranno corretti dopo l'esecuzione dell'istruzione.  
  
4.  Esegue l'istruzione Update o DELETE posizionata utilizzando un'istruzione diversa rispetto all'istruzione associata al cursore.  
  
    > [!CAUTION]  
    >  La clausola **where** costruita dalla libreria di cursori per identificare la riga corrente può non essere in grado di identificare le righe, identificare una riga diversa o identificare più di una riga. Per ulteriori informazioni, vedere [creazione di istruzioni ricercate](../../../odbc/reference/appendixes/constructing-searched-statements.md).  
  
 Tutte le istruzioni Update e Delete posizionate richiedono un nome di cursore. Per specificare il nome del cursore, un'applicazione chiama **SQLSetCursorName** prima dell'apertura del cursore. Per utilizzare il nome del cursore generato dal driver, un'applicazione chiama **SQLGetCursorName** dopo l'apertura del cursore.  
  
 Dopo che la libreria di cursori ha eseguito un'istruzione Update o DELETE posizionata, la matrice di stato, i buffer dei set di righe e la cache gestiti dalla libreria di cursori contengono i valori indicati nella tabella seguente.  
  
|Istruzione utilizzata|Valore nella matrice di stato della riga|Valori in<br /><br /> buffer dei set di righe|Valori in<br /><br /> buffer della cache|  
|--------------------|-------------------------------|----------------------------------|---------------------------------|  
|Aggiornamento posizionato|SQL_ROW_UPDATED|Nuovi valori [1]|Nuovi valori [1]|  
|Eliminazione posizionata|SQL_ROW_DELETED|Valori precedenti|Valori precedenti|  
  
 [1] l'applicazione deve aggiornare i valori nei buffer del set di righe prima di eseguire l'istruzione UPDATE posizionata; dopo l'esecuzione dell'istruzione UPDATE posizionata, la libreria di cursori copia i valori nei buffer del set di righe nella relativa cache.
