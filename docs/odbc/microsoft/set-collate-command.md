---
description: SET COLLATE (comando)
title: IMPOSTA comando COLLATE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set collate command [ODBC]
ms.assetid: 00efbcd4-fea8-4061-86a5-82de413cb753
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6158f79c589e446c2b3c106a1d14fd58715714f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208652"
---
# <a name="set-collate-command"></a>SET COLLATE (comando)
Specifica una sequenza di regole di confronto per i campi carattere nelle successive operazioni di indicizzazione e ordinamento.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
SET COLLATE TO cSequenceName  
```  
  
## <a name="arguments"></a>Argomenti  
 *cSequenceName*  
 Specifica una sequenza di regole di confronto. Nella tabella seguente sono descritte le opzioni disponibili per la sequenza delle regole di confronto.  
  
|Opzioni|Linguaggio|  
|-------------|--------------|  
|Olandese|Olandese|  
|GENERAL|Inglese, francese, tedesco, spagnolo moderno, portoghese e altre lingue europee occidentali|  
|Tedesco|Ordine telefonico tedesco (DIN)|  
|Islanda|Islandese|  
|MACCHINA|Machine (sequenza delle regole di confronto predefinite per le versioni precedenti di FoxPro)|  
|NORDAN|Norvegese, danese|  
|Spagnolo|Spagnolo tradizionale|  
|SWEFIN|Svedese, finlandese|  
|UNIQWT|Spessore univoco|  
  
> [!NOTE]  
>  Quando si specifica l'opzione spagnolo, *ch* è una singola lettera che ordina tra *c* e *d* e si *Ordina tra* *l* e *m*.  
  
 Se si specifica un'opzione della sequenza di regole di confronto come stringa di caratteri letterali, assicurarsi di racchiudere l'opzione tra virgolette:  
  
```  
SET COLLATE TO "SWEFIN"  
```  
  
 Il computer è l'opzione della sequenza di regole di confronto predefinita ed è la sequenza con cui gli utenti hanno familiarità con Xbase. I caratteri vengono ordinati come appaiono nella tabella codici corrente.  
  
 Il generale può essere preferibile per gli utenti degli Stati Uniti e dell'Europa occidentale. I caratteri vengono ordinati come appaiono nella tabella codici corrente. Nelle versioni di FoxPro precedenti alla 2,5, gli indici possono essere stati creati usando le funzioni **Upper**() o **Lower**() per convertire i campi di tipo carattere in un caso coerente. Nelle versioni di FoxPro successive alla 2,5, è invece possibile specificare l'opzione sequenza regole di confronto generale e omettere la conversione **superiore**().  
  
 Se si specifica un'opzione relativa alla sequenza delle regole di confronto diversa da MACHINE e si crea un file con estensione IDX, viene sempre creato un file Compact. idx.  
  
 Utilizzare SET ("COLLATE") per restituire la sequenza delle regole di confronto corrente.  
  
 È possibile specificare una sequenza di ordinamento per un'origine dati tramite la finestra di [dialogo di configurazione di ODBC Visual FoxPro](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md) oppure tramite la parola chiave COLLATE nella stringa di connessione con [SQLDriverConnect](../../odbc/microsoft/sqldriverconnect-visual-foxpro-odbc-driver.md). Questo è identico all'esecuzione del comando seguente:  
  
```  
SET COLLATE TO cSequenceName  
```  
  
## <a name="remarks"></a>Commenti  
 SET COLLATE consente di ordinare le tabelle contenenti caratteri accentati per qualsiasi lingua supportata. La modifica dell'impostazione di SET COLLATE non influisce sulla sequenza di confronto degli indici aperti in precedenza. Visual FoxPro gestisce automaticamente gli indici esistenti, offrendo la flessibilità necessaria per creare molti tipi diversi di indici, anche per lo stesso campo.  
  
 Se, ad esempio, viene creato un indice con SET COLLATE impostato su generale e l'impostazione SET COLLATE viene successivamente modificata in spagnolo, l'indice mantiene la sequenza generale delle regole di confronto.  
  
## <a name="see-also"></a>Vedere anche  
 [Finestra di dialogo di configurazione ODBC Visual FoxPro](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md)
