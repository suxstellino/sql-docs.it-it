---
description: Interfaccia ADOStreamConstruction
title: Interfaccia ADOStreamConstruction | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADOStreamConstruction
helpviewer_keywords:
- ADOStreamConstruction interface [ADO]
ms.assetid: 92f5a939-3e1a-4b14-a9dd-90e6ce2dec74
author: rothja
ms.author: jroth
ms.openlocfilehash: 1138d00d291ffdabf415177a948bf87b1c6502a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171592"
---
# <a name="adostreamconstruction-interface"></a>Interfaccia ADOStreamConstruction
L'interfaccia **ADOStreamConstruction** viene utilizzata per costruire un oggetto **flusso** ADO da un oggetto OLE DB **IStream** in un'applicazione C/C++.  
  
## <a name="properties"></a>Proprietà  
  
|Proprietà|Descrizione|  
|-|-|  
|[Flusso](./stream-property.md)|Lettura/Scrittura. Ottiene o imposta un oggetto **flusso** OLE DB.|  
  
## <a name="methods"></a>Metodi  
 Nessuna.  
  
## <a name="events"></a>Eventi  
 No.  
  
## <a name="remarks"></a>Osservazioni  
 Dato un oggetto OLE DB **IStream** ( `pStream` ), la costruzione di un oggetto **flusso** ADO ( `adoStr` ) equivale alle tre operazioni di base seguenti:  
  
1.  Creazione di un oggetto **flusso** ADO:  
  
    ```  
    Stream20Ptr adoStr;  
    adoStr.CreateInstance(__uuidof(Stream));  
    ```  
  
2.  Eseguire una query sull'interfaccia **IADOStreamConstruction** sull'oggetto **Stream** :  
  
    ```  
    adoStreamConstructionPtr adoStrConstruct=NULL;  
    adoStr->QueryInterface(__uuidof(ADOStreamConstruction),  
                         (void**)&adoStrConstruct);  
    ```  
  
 Chiamare il `IADOStreamConstruction::get_Stream` Metodo Property per impostare il OLE DB oggetto **IStream** sull'oggetto **flusso** ADO:  
  
```  
IUnknown *pUnk=NULL;  
pRowset->QueryInterface(IID_IUnknown, (void**)&pUnk);  
adoStrConstruct->put_Stream(pUnk);  
```  
  
 L'oggetto risultante `adoStr` rappresenta ora l'oggetto **flusso** ADO costruito dal OLE DB oggetto **IStream** .  
  
## <a name="requirements"></a>Requisiti  
 **Versione:** ADO 2,0 o versione successiva  
  
 **Libreria:** msado15.dll  
  
 **UUID:** 00000283-0000-0010-8000-00AA006D2EA4  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sull'API ADO](./ado-api-reference.md)