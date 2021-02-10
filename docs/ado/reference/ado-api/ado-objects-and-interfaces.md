---
description: Interfacce e oggetti ADO
title: Oggetti e interfacce ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- ADO, objects and interfaces
- objects [ADO]
ms.assetid: d0b7e254-c89f-4406-b846-a060ef038c30
author: rothja
ms.author: jroth
ms.openlocfilehash: 64dc0e750a999fa1a333a3ec94576583850e511e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027944"
---
# <a name="ado-objects-and-interfaces"></a>Interfacce e oggetti ADO
Le relazioni tra questi oggetti sono rappresentate nel [modello a oggetti ADO](./ado-object-model.md).  
  
 Ogni oggetto può essere contenuto nella raccolta corrispondente. Un oggetto [Error](./error-object.md) , ad esempio, può essere contenuto in una raccolta [Errors](./errors-collection-ado.md) . Per ulteriori informazioni, vedere [raccolte ADO](./ado-collections.md) o un argomento specifico della raccolta.  
  
|Oggetto o interfaccia|Descrizione|  
|-|-|  
|[IADOCommandConstruction](/previous-versions/windows/desktop/aa965677(v=vs.85))|Utilizzato per recuperare il comando OLEDB sottostante da un oggetto ADOCommand.|  
|[ADORecordConstruction](./adorecordconstruction-interface.md)|Costruisce un oggetto **record** ADO da un oggetto OLE DB **riga** in un'applicazione C/C++.|  
|[ADORecordsetConstruction](./adorecordsetconstruction-interface.md)|Costruisce un oggetto **Recordset** ADO da un oggetto OLE DB **set di righe** in un'applicazione C/C++.|  
|[Interfaccia ADOStreamConstruction](./adostreamconstruction-interface.md)|Costruisce un oggetto **flusso** ADO da un oggetto OLE DB **IStream** in un'applicazione C/C++.|  
|[Comando](./command-object-ado.md)|Definisce un comando specifico che si desidera eseguire su un'origine dati.<br /><br /> L'oggetto **Command** non è sicuro per lo scripting.|  
|[Connection](./connection-object-ado.md)|Rappresenta una connessione aperta a un'origine dati.<br /><br /> L'oggetto **Connection** è sicuro per lo scripting.|  
|[Interfaccia IDSOShapeExtensions](./idsoshapeextensions-interface.md)|Ottiene l'oggetto origine dati OLEDB sottostante per il provider di forme.|  
|[Error (Errore) (Error (Errore)e)](./error-object.md)|Contiene informazioni dettagliate sugli errori di accesso ai dati relativi a una singola operazione che interessa il provider.<br /><br /> L'oggetto **errore** non è sicuro per lo scripting.|  
|[Campo](./field-object.md)|Rappresenta una colonna di dati con un tipo di dati comune.|  
|[Parametro](./parameter-object.md)|Rappresenta un parametro o un argomento associato a un oggetto **Command** basato su una query con parametri o stored procedure.<br /><br /> L'oggetto **Parameter** non è sicuro per lo scripting.|  
|[Proprietà](./property-object-ado.md)|Rappresenta una caratteristica dinamica di un oggetto ADO definito dal provider.|  
|[Record](./record-object-ado.md)|Rappresenta una riga di un **Recordset** o una directory o un file in un file System. L'oggetto **record** è sicuro per lo scripting.|  
|[Recordset](./recordset-object-ado.md)|Rappresenta il set di record di una tabella di base o i risultati di un comando eseguito. In qualsiasi momento, l'oggetto **Recordset** fa riferimento solo a un singolo record all'interno del set come record corrente.<br /><br /> L'oggetto **Recordset** è sicuro per lo scripting.|  
|[Flusso](./stream-object-ado.md)|Rappresenta un flusso di dati binario.<br /><br /> L'oggetto **flusso** è sicuro per lo scripting.|  
  
## <a name="see-also"></a>Vedere anche  
 [Riferimento all'API ADO](./ado-api-reference.md)   
 [Raccolte ADO](./ado-collections.md)   
 [Proprietà dinamiche ADO](./ado-dynamic-properties.md)   
 [Costanti enumerate ADO](./ado-enumerated-constants.md)   
 [Appendice B: errori ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Eventi ADO](./ado-events.md)   
 [Metodi ADO](./ado-methods.md)   
 [Modello a oggetti ADO](./ado-object-model.md)   
 [Proprietà ADO](./ado-properties.md)