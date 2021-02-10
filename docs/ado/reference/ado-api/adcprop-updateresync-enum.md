---
description: ADCPROP_UPDATERESYNC_ENUM
title: ADCPROP_UPDATERESYNC_ENUM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADCPROP_UPDATERESYNC_ENUM
helpviewer_keywords:
- ADCPROP_UPDATERESYNC_ENUM [ADO]
ms.assetid: bc9e1a37-e969-47e9-8382-0bbfffa2034f
author: rothja
ms.author: jroth
ms.openlocfilehash: ec0ef44d25f76e701142aa1c81077de2a93d27d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035871"
---
# <a name="adcprop_updateresync_enum"></a>ADCPROP_UPDATERESYNC_ENUM
Specifica se il metodo [UpdateBatch](./updatebatch-method.md) è seguito da un'operazione implicita di [Risincronizzazione](./resync-method.md) del metodo e, in tal caso, dall'ambito di tale operazione.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adResyncAll**|15|Richiama la **Risincronizzazione** con il valore combinato di tutti gli altri membri di ADCPROP_UPDATERESYNC_ENUM.|  
|**adResyncAutoIncrement**|1|Valore predefinito. Tenta di recuperare il nuovo valore Identity per le colonne che vengono incrementate automaticamente o generate dall'origine dati, ad esempio i campi del numero automatico Microsoft Jet o le colonne Identity Microsoft SQL Server.|  
|**adResyncConflicts**|2|Richiama la **Risincronizzazione** per tutte le righe in cui l'operazione di aggiornamento o eliminazione non è riuscita a causa di un conflitto di concorrenza.|  
|**adResyncInserts**|8|Richiama la **Risincronizzazione** per tutte le righe inserite correttamente. Tuttavia, i valori della colonna AutoIncrement non vengono risincronizzati. Il contenuto delle righe appena inserite viene invece risincronizzato in base al valore della chiave primaria esistente. Se la chiave primaria è un valore di incremento automatico, la **Risincronizzazione** non recupererà il contenuto della riga desiderata. Per incrementare automaticamente i valori di chiave primaria di incremento automatico, chiamare **UpdateBatch** con il valore combinato **adResyncAutoIncrement**  +  **adResyncInserts**.|  
|**adResyncNone**|0|Non richiama la **Risincronizzazione**.|  
|**adResyncUpdates**|4|Richiama la **Risincronizzazione** per tutte le righe aggiornate correttamente.|  
  
## <a name="applies-to"></a>Si applica a  
 [Proprietà dinamica Update Resync (ADO)](./update-resync-property-dynamic-ado.md)