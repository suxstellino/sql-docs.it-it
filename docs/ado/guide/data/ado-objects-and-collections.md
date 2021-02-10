---
description: Oggetti e raccolte di ADO
title: Oggetti e raccolte ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, objects and collections
ms.assetid: 7a745aae-9372-49b6-8dae-b9c93e5f3216
author: rothja
ms.author: jroth
ms.openlocfilehash: c03e1b792e23552252eb84fae9a71e693b3781c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028066"
---
# <a name="ado-objects-and-collections"></a>Oggetti e raccolte di ADO
ADO è costituito dai nove oggetti seguenti e da quattro raccolte.  
  
|Oggetto o raccolta|Descrizione|  
|--------------------------|-----------------|  
|Oggetto **Connection**|Rappresenta una sessione univoca con un'origine dati. Nel caso di un sistema di database client/server, può essere equivalente a una connessione di rete effettiva al server. A seconda della funzionalità supportata dal provider, alcune raccolte, metodi o proprietà di un oggetto **connessione** potrebbero non essere disponibili.|  
|**Command**|Utilizzato per definire un comando specifico, ad esempio una query SQL, progettato per l'esecuzione su un'origine dati.|  
|Oggetto **Recordset**|Rappresenta l'intero set di record di una tabella di base o i risultati di un comando eseguito. Tutti gli oggetti **Recordset** sono costituiti da record (righe) e campi (colonne).|  
|Oggetto **record**|Rappresenta una singola riga di dati, da un **Recordset** o dal provider. Questo record potrebbe rappresentare un record di database o un altro tipo di oggetto, ad esempio un file o una directory, a seconda del provider.|  
|Oggetto **Stream**|Rappresenta un flusso di dati binari o di testo. Un documento XML, ad esempio, può essere caricato in un flusso per l'input del comando o restituito da determinati provider come risultato di una query. Un oggetto **flusso** può essere usato per modificare campi o record contenenti questi flussi di dati.|  
|**Parameter** (oggetto)|Rappresenta un parametro o un argomento associato a un oggetto **Command** , in base a una query con parametri o a una stored procedure.|  
|**Field** (oggetto)|Rappresenta una colonna di dati con un tipo di dati comune. Ogni oggetto **campo** corrisponde a una colonna nel **Recordset**.|  
|Oggetto **Property**|Rappresenta una caratteristica di un oggetto ADO definito dal provider. Gli oggetti ADO hanno due tipi di proprietà: incorporata e dinamica. Le proprietà predefinite sono quelle implementate in ADO e immediatamente disponibili per un nuovo oggetto. L'oggetto **Property** è un contenitore per le proprietà dinamiche, definito dal provider sottostante.|  
|**Error**|Contiene informazioni dettagliate sugli errori di accesso ai dati relativi a una singola operazione che interessa il provider.|  
|Raccolta **Fields**|Contiene tutti gli oggetti **campo** di un **Recordset** o di un oggetto **record** .|  
|Raccolta **Properties**|Contiene tutti gli oggetti **Property** per un'istanza specifica di un oggetto.|  
|Raccolta **Parameters**|Contiene tutti gli oggetti **Parameter** di un oggetto **Command** .|  
|Raccolta di **errori**|Contiene tutti gli oggetti **Error** creati in risposta a un singolo errore correlato al provider.|  
  
## <a name="see-also"></a>Vedere anche  
 [Modello a oggetti ADO](../../reference/ado-api/ado-object-model.md)