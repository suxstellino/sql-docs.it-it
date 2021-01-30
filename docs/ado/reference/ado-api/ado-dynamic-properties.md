---
description: Proprietà dinamiche ADO
title: Proprietà dinamiche ADO | Microsoft Docs
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- dynamic properties [ADO]
- properties [ADO], dynamic
ms.assetid: d7b06d72-f792-4328-93a2-5006b9e2c581
author: rothja
ms.author: jroth
ms.openlocfilehash: f815c82b16ef31bc64e5524ec7114779b2ba3234
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161654"
---
# <a name="ado-dynamic-properties"></a>Proprietà dinamiche ADO
È possibile aggiungere proprietà dinamiche alle raccolte [Properties](./properties-collection-ado.md) degli oggetti [Connection](./connection-object-ado.md), [Command](./command-object-ado.md)o [Recordset](./recordset-object-ado.md) . L'origine di queste proprietà è un provider di dati, ad esempio il [provider di OLE DB per SQL Server](../../guide/appendixes/microsoft-ole-db-provider-for-sql-server.md), o un provider di servizi, ad esempio il [servizio Microsoft Cursor per OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md). Per ulteriori informazioni su una specifica proprietà dinamica, fare riferimento alla documentazione del provider di dati o del provider di servizi appropriato.  
  
 L' [indice della proprietà dinamica ADO](./ado-dynamic-property-index.md) fornisce un riferimento incrociato tra i nomi ADO e OLE DB per ogni proprietà dinamica del provider OLE DB standard.  
  
 Le seguenti proprietà dinamiche sono particolarmente interessanti e sono inoltre documentate nelle origini citate in precedenza. La funzionalità speciale con ADO è documentata negli argomenti della Guida ADO elencati di seguito.  
  
|Proprietà dinamica|Descrizione|  
|-|-|  
|[Optimize](./optimize-property-dynamic-ado.md) (Ottimizza)|Specifica se è necessario creare un indice in questo campo.|  
|[Messaggio di richiesta](./prompt-property-dynamic-ado.md)|Specifica se il provider di OLE DB deve richiedere all'utente le informazioni di inizializzazione.|  
|[Nome riforma](./reshape-name-property-dynamic-ado.md)|Specifica un nome per l'oggetto **Recordset** .|  
|[Comando di risincronizzazione](./resync-command-property-dynamic-ado.md)|Specifica una stringa di comando fornita dall'utente che il metodo di **Risincronizzazione** emette per aggiornare i dati nella tabella denominata nella proprietà dinamica della **tabella univoca** .|  
|[Tabella univoca, schema univoco, catalogo univoco](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|**Tabella univoca** Specifica il nome della tabella di base su cui sono consentiti aggiornamenti, inserimenti ed eliminazioni.<br /><br /> **Schema univoco** Specifica lo schema o il nome del proprietario della tabella.<br /><br /> **Catalogo univoco** Specifica il catalogo o il nome del database che contiene la tabella.|  
|[Aggiornamento della risincronizzazione](./update-resync-property-dynamic-ado.md)|Specifica se il metodo **UpdateBatch** è seguito da un'operazione implicita di **Risincronizzazione** del metodo e, in tal caso, dall'ambito di tale operazione.|
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Vedere anche  
 [Riferimento all'API ADO](./ado-api-reference.md)   
 [Raccolte ADO](./ado-collections.md)   
 [Costanti enumerate ADO](./ado-enumerated-constants.md)   
 [Appendice B: errori ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Eventi ADO](./ado-events.md)   
 [Metodi ADO](./ado-methods.md)   
 [Modello a oggetti ADO](./ado-object-model.md)   
 [Oggetti e interfacce ADO](./ado-objects-and-interfaces.md)   
 [Proprietà ADO](./ado-properties.md)