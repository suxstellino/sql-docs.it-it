---
description: Raccolta Fields (ADO)
title: Raccolta Fields (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Fields
- Recordset15::Fields
- _Record::Fields
helpviewer_keywords:
- Fields collection [ADO]
ms.assetid: 7c371474-b88f-4730-afa5-44163a0488d5
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e16bf602bd0c54b425587fcf1a50d2cc60b2aa7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167354"
---
# <a name="fields-collection-ado"></a>Raccolta Fields (ADO)
Contiene tutti gli oggetti [campo](./field-object.md) di un [Recordset](./recordset-object-ado.md) o di un oggetto [record](./record-object-ado.md) .  
  
## <a name="remarks"></a>Commenti  
 Un oggetto **Recordset** include una raccolta **Fields** costituita da oggetti **Field** . Ogni oggetto **campo** corrisponde a una colonna nel **Recordset**. È possibile popolare la raccolta **Fields** prima di aprire il **Recordset** chiamando il metodo [Refresh](./refresh-method-ado.md) sulla raccolta.  
  
> [!NOTE]
>  Per una spiegazione più dettagliata dell'uso di oggetti **campo** , vedere l'argomento oggetto **campo** .  
  
 La raccolta **Fields** include un metodo [Append](./append-method-ado.md) , che consente di creare e aggiungere in modo provvisorio un oggetto **campo** alla raccolta e un metodo **Update** , che finalizza eventuali aggiunte o eliminazioni.  
  
 Un oggetto **record** dispone di due campi speciali che possono essere indicizzati con costanti [FieldEnum](./fieldenum.md) . Una costante accede a un campo contenente il flusso predefinito per il **record** e l'altro accede a un campo contenente la stringa URL assoluta per il **record**.  
  
 Alcuni provider, ad esempio il [provider Microsoft OLE DB per Internet Publishing](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md), possono popolare la **raccolta Fields** con un subset di campi disponibili per il **record** o il **Recordset**. Gli altri campi non verranno aggiunti alla raccolta fino a quando non vi viene fatto riferimento in base al nome o indicizzati dal codice.  
  
 Se si tenta di fare riferimento a un campo inesistente in base al nome, verrà aggiunto un nuovo oggetto **campo** alla raccolta **Fields** con [lo stato](./status-property-ado-field.md) **adFieldPendingInsert**. Quando si chiama [Update](./update-method.md), ADO creerà un nuovo campo nell'origine dati, se consentito dal provider.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi della raccolta Fields](./fields-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Field](./field-object.md)