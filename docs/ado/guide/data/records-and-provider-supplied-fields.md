---
description: Record e campi specificati dal provider
title: Campi di record e Provider-Supplied | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- records-provided fields [ADO]
- provider-supplied fields [ADO]
ms.assetid: 77f95e0a-0cf2-411a-a792-593f77330fbd
author: rothja
ms.author: jroth
ms.openlocfilehash: a64168a8a1fec81d47c337978ea75664fec0a10c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037111"
---
# <a name="records-and-provider-supplied-fields"></a>Record e campi specificati dal provider
Quando un oggetto [record](../../../ado/reference/ado-api/record-object-ado.md) viene aperto, la relativa origine può essere la riga corrente di un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)aperto, un URL assoluto o un URL relativo insieme a un oggetto di [connessione](../../../ado/reference/ado-api/connection-object-ado.md) aperto.  
  
 Se il **record** viene aperto da un **Recordset**, la raccolta dei [campi](../../../ado/reference/ado-api/fields-collection-ado.md) dell'oggetto **record** conterrà tutti i campi del **Recordset**, più tutti i campi aggiunti dal provider sottostante.  
  
 Il provider può inserire campi aggiuntivi che fungano da caratteristiche supplementari del **record**. Di conseguenza, un **record** può includere campi univoci non nel **Recordset** come un intero o qualsiasi **record** derivato da un'altra riga del **Recordset**.  
  
 Ad esempio, tutte le righe di un **Recordset** derivato da un'origine dati di posta elettronica potrebbero contenere colonne quali from, to e Subject. Un **record** derivato da tale **Recordset** avrà gli stessi campi. Tuttavia, il **record** potrebbe avere anche altri campi univoci per il messaggio specifico rappresentato da tale **record**, ad esempio allegato e CC (copia carbone).  
  
 Anche se l'oggetto **record** e la riga corrente del **Recordset** hanno gli stessi campi, sono diversi perché gli oggetti **record** e **Recordset** hanno metodi e proprietà diversi.  
  
 Un campo contenuto in comune dal **record** e dal **Recordset** può essere modificato in entrambi gli oggetti. Tuttavia, il campo non può essere eliminato nell'oggetto **record** , sebbene il provider sottostante possa supportare l'impostazione del campo su null.  
  
 Una volta aperto il **record** , è possibile aggiungere campi a livello di codice. È anche possibile eliminare i campi che sono stati aggiunti, ma non è possibile eliminare i campi dal **Recordset** originale.  
  
 È anche possibile aprire l'oggetto **record** direttamente da un URL. In questo caso, i campi aggiunti al **record** dipendono dal provider sottostante. Attualmente, la maggior parte dei provider aggiunge un set di campi che descrivono l'entità rappresentata dal **record**. Se l'entità è costituita da un flusso di byte, ad esempio un file semplice, un oggetto [flusso](../../../ado/reference/ado-api/stream-object-ado.md) può in genere essere aperto dal **record**.  
  
## <a name="special-fields-for-document-source-providers"></a>Campi speciali per i provider di origine dei documenti  
 Una classe speciale di provider, detti *provider di origine del documento*, gestisce cartelle e documenti. Quando un oggetto **record** rappresenta un documento o un oggetto **Recordset** rappresenta una cartella di documenti, il provider di origine del documento compila tali oggetti con un set univoco di campi che descrivono le caratteristiche del documento anziché il documento effettivo. In genere, un campo contiene un riferimento al **flusso** che rappresenta il documento.  
  
 Questi campi costituiscono un **record** di risorse o un **Recordset** e sono elencati per i provider specifici che li supportano in [appendice a: Providers](../../../ado/guide/appendixes/appendix-a-providers.md).  
  
 Due costanti indicizzano la raccolta **Fields** di un **record** di risorse o di un **Recordset** per recuperare una coppia di campi usati di frequente. La proprietà **Field** Object [value](../../../ado/reference/ado-api/value-property-ado.md) restituisce il contenuto desiderato.  
  
-   Il campo a cui si accede con la costante **adDefaultStream** contiene un flusso predefinito associato all'oggetto **record** o **Recordset** . Il provider assegna un flusso predefinito a un oggetto.  
  
-   Il campo a cui si accede con la costante **adRecordURL** contiene l'URL assoluto che identifica il documento.  
  
 Un provider di origine del documento non supporta la raccolta [Properties](../../../ado/reference/ado-api/properties-collection-ado.md) di oggetti **record** e **Field** . Il contenuto della raccolta **Properties** è null per tali oggetti.  
  
 Un provider di origine del documento può aggiungere una proprietà specifica del provider, ad esempio **DataSource type** , per stabilire se si tratta di un provider di origine del documento. Per ulteriori informazioni su come determinare il tipo di provider, vedere la documentazione del provider.  
  
## <a name="resource-recordset-columns"></a>Colonne recordset di risorse  
 Un *Recordset di risorse* è costituito dalle colonne seguenti.  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|RESOURCE_PARSENAME|AdVarWChar|Di sola lettura. Indica l'URL della risorsa.|  
|RESOURCE_PARENTNAME|AdVarWChar|Di sola lettura. Indica l'URL assoluto del record padre.|  
|RESOURCE_ABSOLUTEPARSENAME|AdVarWChar|Di sola lettura. Indica l'URL assoluto della risorsa, ovvero la concatenazione di PARENTname e PARSEname.|  
|RESOURCE_ISHIDDEN|AdBoolean|True se la risorsa è nascosta. Non verrà restituita alcuna riga, a meno che il comando che crea il set di righe non selezioni in modo esplicito le righe in cui RESOURCE_ISHIDDEN è true.|  
|RESOURCE_ISREADONLY|AdBoolean|True se la risorsa è di sola lettura. Tenta di aprire questa risorsa con DBBINDFLAG_WRITE e avrà esito negativo con DB_E_READONLY. Questa proprietà può essere modificata anche quando la risorsa è stata aperta solo per la lettura.|  
|RESOURCE_CONTENTTYPE|AdVarWChar|Indica l'utilizzo probabile del documento, ad esempio il Brief di un avvocato. Questo può corrispondere al modello di Office usato per creare il documento.|  
|RESOURCE_CONTENTCLASS|AdVarWChar|Indica il tipo MIME del documento, che indica il formato, ad esempio " `text/html` ".|  
|RESOURCE_CONTENTLANGUAGE|AdVarWChar|Indica la lingua in cui è archiviato il contenuto.|  
|RESOURCE_CREATIONTIME|adFileTime|Di sola lettura. Indica una struttura FILETIME che contiene l'ora in cui è stata creata la risorsa. L'ora è indicata nel formato UTC (Coordinated Universal Time).|  
|RESOURCE_LASTACCESSTIME|AdFileTime|Di sola lettura. Indica una struttura FILETIME che contiene l'ora dell'ultimo accesso alla risorsa. L'ora è in formato UTC. I membri FILETIME sono zero se il provider non supporta questo membro di tempo.|  
|RESOURCE_LASTWRITETIME|AdFileTime|Di sola lettura. Indica una struttura FILETIME che contiene l'ora dell'ultima scrittura della risorsa. L'ora è in formato UTC. I membri FILETIME sono zero se il provider non supporta questo membro di tempo.|  
|RESOURCE_STREAMSIZE|asUnsignedBigInt|Di sola lettura. Indica la dimensione, in byte, del flusso predefinito della risorsa.|  
|RESOURCE_ISCOLLECTION|AdBoolean|Di sola lettura. True se la risorsa è una raccolta, ad esempio una directory. False se la risorsa è un file semplice.|  
|RESOURCE_ISSTRUCTUREDDOCUMENT|AdBoolean|True se la risorsa è un documento strutturato. False se la risorsa non è un documento strutturato. Potrebbe trattarsi di una raccolta o di un semplice file.|  
|DEFAULT_DOCUMENT|AdVarWChar|Di sola lettura. Indica che questa risorsa contiene un URL del documento semplice predefinito di una cartella o di un documento strutturato. Utilizzato quando il flusso predefinito viene richiesto da una risorsa. Questa proprietà è vuota per un file semplice.|  
|CHAPTERED_CHILDREN|AdChapter|Di sola lettura. facoltativo. Indica il capitolo del set di righe che contiene gli elementi figlio della risorsa. Il *provider di OLE DB per la pubblicazione Internet* non utilizza questa colonna.|  
|RESOURCE_DISPLAYNAME|AdVarWChar|Di sola lettura. Indica il nome visualizzato della risorsa.|  
|RESOURCE_ISROOT|AdBoolean|Di sola lettura. True se la risorsa è la radice di una raccolta o di un documento strutturato.|  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto record (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Appendice A: Provider](../../../ado/guide/appendixes/appendix-a-providers.md)
