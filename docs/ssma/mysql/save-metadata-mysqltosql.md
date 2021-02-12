---
description: Salvare i metadati (MySQLToSQL)
title: Salvare i metadati (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9bc6273f-e8b1-430b-81a5-14330a783562
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: bc5f3e07439f79fd5746fc564f2b7c9fb87e69c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074522"
---
# <a name="save-metadata--mysqltosql"></a>Salvare i metadati (MySQLToSQL)
La finestra di dialogo **Salva metadati** richiede di caricare i metadati nel progetto SSMA prima di salvarli. In questo modo è possibile disporre di un file di progetto completo che può essere utilizzato offline e inviato ad altri utenti, ad esempio il personale del supporto tecnico.  
  
Per accedere alla finestra di dialogo **Salva metadati** , salvare il progetto. Se mancano metadati, in SSMA verrà visualizzata la finestra di dialogo **Salva metadati** .  
  
## <a name="options"></a>Opzioni  
**Nome**  
Nome di ogni database nel progetto.  
  
**Status**  
Indica se i metadati vengono caricati nel progetto SSMA o se i metadati risultano mancanti.  
  
SSMA carica i metadati nel progetto, se necessario. I metadati vengono caricati automaticamente quando si esplorano i metadati e si convertono gli schemi.  
  
**Seleziona tutto**  
Seleziona tutti i database elencati.  
  
**Cancella**  
Deseleziona la casella di controllo per tutti i database con metadati mancanti. Non è possibile deselezionare la casella di controllo se i metadati sono stati caricati.  
  
**Salva**  
Salva il progetto, caricando i metadati per i database selezionati che contengono metadati mancanti.  
  
**Annulla**  
Annulla l'operazione di salvataggio. I metadati mancanti non vengono caricati nel progetto.  
  
