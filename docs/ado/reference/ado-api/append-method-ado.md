---
description: Metodo Append (ADO)
title: Metodo Append (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _DynaCollection::Append
helpviewer_keywords:
- Append method [ADO]
ms.assetid: f8a9bbed-ba9c-4698-945d-317ad22d2e92
author: rothja
ms.author: jroth
ms.openlocfilehash: 03e454407c1b01b34ea918f81a404d579f8a3809
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167891"
---
# <a name="append-method-ado"></a>Metodo Append (ADO)
Accoda un oggetto a una raccolta. Se la raccolta è [Fields](./fields-collection-ado.md), è possibile creare un nuovo oggetto [campo](./field-object.md) prima che venga aggiunto alla raccolta.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
collection.Append object  
fields.Append Name, Type, DefinedSize, Attrib, FieldValue  
```  
  
#### <a name="parameters"></a>Parametri  
 *raccolta*  
 Oggetto della raccolta.  
  
 *campi*  
 Raccolta di **campi** .  
  
 *object*  
 Variabile oggetto che rappresenta l'oggetto da accodare.  
  
 *Nome*  
 Valore **stringa** che contiene il nome del nuovo oggetto **campo** e non deve essere lo stesso nome di un altro oggetto nei *campi*.  
  
 *Tipo*  
 Valore [DataTypeEnum](./datatypeenum.md) , il cui valore predefinito è **adEmpty**, che specifica il tipo di dati del nuovo campo. I tipi di dati seguenti non sono supportati da ADO e non devono essere utilizzati per l'aggiunta di nuovi campi a un [oggetto recordset (ADO)](./recordset-object-ado.md): **adIDispatch**, **adIUnknown**, **adVariant**.  
  
 *DefinedSize*  
 facoltativo. Valore **Long** che rappresenta la dimensione definita, in caratteri o byte, del nuovo campo. Il valore predefinito per questo parametro è derivato dal *tipo*. I campi con un *DefinedSize* maggiore di 255 byte vengono trattati come colonne a lunghezza variabile. Il valore predefinito per *DefinedSize* non è specificato.  
  
 *Attrib*  
 facoltativo. Valore [FieldAttributeEnum](./fieldattributeenum.md) , il cui valore predefinito è **adFldDefault**, che specifica gli attributi per il nuovo campo. Se questo valore non è specificato, il campo conterrà gli attributi derivati dal *tipo*.  
  
 *FieldValue*  
 facoltativo. **Variant** che rappresenta il valore per il nuovo campo. Se non è specificato, il campo viene aggiunto con un valore null.  
  
## <a name="remarks"></a>Commenti  
  
## <a name="parameters-collection"></a>Raccolta Parameters  
 È necessario impostare la proprietà [Type](./type-property-ado.md) di un oggetto [Parameter](./parameter-object.md) prima di aggiungerla alla raccolta [Parameters](./parameters-collection-ado.md) . Se si seleziona un tipo di dati a lunghezza variabile, è necessario impostare anche la proprietà [size](./size-property-ado-parameter.md) su un valore maggiore di zero.  
  
 La descrizione dei parametri consente di ridurre al minimo le chiamate al provider e di conseguenza migliora le prestazioni quando si utilizzano stored procedure o query con parametri. Tuttavia, è necessario essere a conoscenza delle proprietà dei parametri associati alla stored procedure o alla query con parametri che si desidera chiamare.  
  
 Usare il metodo [CreateParameter](./createparameter-method-ado.md) per creare oggetti **Parameter** con le impostazioni delle proprietà appropriate e usare il metodo **Append** per aggiungerli alla raccolta [Parameters](./parameters-collection-ado.md) . In questo modo è possibile impostare e restituire i valori dei parametri senza dover chiamare il provider per le informazioni sui parametri. Se si sta scrivendo in un provider che non fornisce informazioni sui parametri, è necessario usare questo metodo per popolare manualmente la raccolta di **parametri** per poter usare i parametri.  
  
## <a name="fields-collection"></a>Raccolta Fields  
 Il parametro *FieldValue* è valido solo quando si aggiunge un oggetto **campo** a un oggetto [record](./record-object-ado.md) , non a un oggetto **Recordset** . Con un oggetto **record** è possibile accodare i campi e specificare contemporaneamente i valori. Con un oggetto **Recordset** , è necessario creare campi durante la chiusura del **Recordset** , quindi aprire il **Recordset** e assegnare i valori ai campi.  
  
> [!NOTE]
>  Per i nuovi oggetti **campo** aggiunti alla raccolta **Fields** di un oggetto **record** , è necessario impostare la proprietà [value](./value-property-ado.md) prima di poter specificare altre proprietà del **campo** . Per prima cosa, è necessario assegnare un valore specifico per la proprietà **value** e [aggiornarlo](./update-method.md) nella raccolta **Fields** denominata. È quindi possibile accedere ad altre proprietà, ad esempio il [tipo](./type-property-ado.md) o [gli attributi](./attributes-property-ado.md) . Gli oggetti **campo** dei tipi di dati seguenti **(DataTypeEnum**) non possono essere aggiunti alla **raccolta Fields** e si verificherà un errore: **adArray**, **adChapter**, **adEmpty**, **adPropVariant** e **adUserDefined**. Inoltre, i tipi di dati seguenti non sono supportati da ADO: **adIDispatch**, **adIUnknown** e **adIVariant**. Per questi tipi, non si verificherà alcun errore quando viene accodato, ma l'utilizzo può produrre risultati imprevedibili, incluse le perdite di memoria.  
  
## <a name="recordset"></a>recordset  
 Se non si imposta la proprietà [CursorLocation](./cursorlocation-property-ado.md) prima di chiamare il metodo **Append** , **CursorLocation** verrà impostato automaticamente su **adUseClient** (un valore [CursorLocationEnum](./cursorlocationenum.md) ) quando viene chiamato il metodo [Open](./open-method-ado-recordset.md) dell'oggetto [Recordset](./recordset-object-ado.md) .  
  
 Si verificherà un errore di run-time se il metodo **Append** viene chiamato sulla raccolta **Fields** di un **Recordset** aperto oppure su un **Recordset** in cui è stata impostata la proprietà [ActiveConnection](./activeconnection-property-ado.md) . È possibile aggiungere campi solo a un **Recordset** che non è aperto e non è ancora stato connesso a un'origine dati. Questa situazione si verifica in genere quando un oggetto **Recordset** viene fabbricato con il metodo [CreateRecordset](../rds-api/createrecordset-method-rds.md) o assegnato a una variabile oggetto.  
  
## <a name="record"></a>Registra  
 Se il metodo **Append** viene chiamato sulla raccolta **Fields** di un **record** aperto, non si verificherà un errore di run-time. Il nuovo campo verrà aggiunto alla raccolta **Fields** dell'oggetto **record** . Se il **record** è derivato da un **Recordset**, il nuovo campo non verrà visualizzato nella raccolta **Fields** dell'oggetto **Recordset** .  
  
 È possibile creare un campo inesistente e aggiungerlo alla raccolta **Fields** assegnando un valore all'oggetto campo come se fosse già presente nella raccolta. L'assegnazione attiverà la creazione e l'aggiunta automatica dell'oggetto **campo** , quindi l'assegnazione verrà completata.  
  
 Dopo l'aggiunta di un **campo** alla raccolta **Fields** di un oggetto **record** , chiamare il metodo **Update** della raccolta **Fields** per salvare la modifica.  
  
## <a name="applies-to"></a>Si applica a  
  
- [Raccolta Fields (ADO)](./fields-collection-ado.md)  
- [Raccolta Parameters (ADO)](./parameters-collection-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodi Append e CreateParameter (VB)](./append-and-createparameter-methods-example-vb.md)   
 [Esempio di metodi Append e CreateParameter (VC + +)](./append-and-createparameter-methods-example-vc.md)   
 [Metodo CreateParameter (ADO)](./createparameter-method-ado.md)   
 [Metodo Delete (raccolta di campi ADO)](./delete-method-ado-fields-collection.md)   
 [Metodo Delete (raccolta Parameters ADO)](./delete-method-ado-parameters-collection.md)   
 [Metodo Delete (recordset ADO)](./delete-method-ado-recordset.md)   
 [Metodo Update](./update-method.md)