---
description: RightsEnum
title: RightsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RightsEnum
helpviewer_keywords:
- RightsEnum enumeration [ADOX]
ms.assetid: 55ee67c7-a583-42aa-849a-78264b4cb614
author: rothja
ms.author: jroth
ms.openlocfilehash: d89daeb8d85e9e59e60f6a1aa6b918364ab915fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169220"
---
# <a name="rightsenum"></a>RightsEnum
Specifica i diritti o le autorizzazioni per un gruppo o un utente in un oggetto.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adRightCreate**|16384 (&H4000)|L'utente o il gruppo dispone dell'autorizzazione per la creazione di nuovi oggetti di questo tipo.|  
|**adRightDelete**|65536 (&H10000)|L'utente o il gruppo dispone dell'autorizzazione per eliminare i dati da un oggetto. Per oggetti quali **tabelle**, l'utente dispone delle autorizzazioni per eliminare i valori dei dati dai record.|  
|**adRightDrop**|256 (&H100)|L'utente o il gruppo dispone dell'autorizzazione per rimuovere oggetti dal catalogo. Ad esempio, le **tabelle** possono essere eliminate da un comando SQL Drop Table.|  
|**adRightExclusive**|512 (&H200)|L'utente o il gruppo dispone delle autorizzazioni necessarie per accedere all'oggetto in modo esclusivo.|  
|**adRightExecute**|536870912 (&H20000000)|L'utente o il gruppo dispone dell'autorizzazione per l'esecuzione dell'oggetto.|  
|**adRightFull**|268435456 (&H10000000)|L'utente o il gruppo dispone di tutte le autorizzazioni per l'oggetto.|  
|**adRightInsert**|32768 (&H8000)|L'utente o il gruppo dispone dell'autorizzazione per inserire l'oggetto. Per gli oggetti, ad esempio le **tabelle**, l'utente dispone delle autorizzazioni necessarie per inserire i dati nella tabella.|  
|**adRightMaximumAllowed**|33554432 (&H2000000)|L'utente o il gruppo ha il numero massimo di autorizzazioni consentite dal provider. Le autorizzazioni specifiche sono dipendenti dal provider.|  
|**adRightNone**|0|L'utente o il gruppo non dispone delle autorizzazioni per l'oggetto.|  
|**adRightRead**|-2147483648 (&H80000000)|L'utente o il gruppo dispone dell'autorizzazione per la lettura dell'oggetto. Per gli oggetti, ad esempio le [tabelle](./table-object-adox.md), l'utente dispone delle autorizzazioni per leggere i dati nella tabella.|  
|**adRightReadDesign**|1024 (&H400)|L'utente o il gruppo dispone dell'autorizzazione per leggere la progettazione per l'oggetto.|  
|**adRightReadPermissions**|131072 (&H20000)|L'utente o il gruppo può visualizzare, ma non modificare, le autorizzazioni specifiche per un oggetto nel catalogo.|  
|**adRightReference**|8192 (&H2000)|L'utente o il gruppo dispone dell'autorizzazione per fare riferimento all'oggetto.|  
|**adRightUpdate**|1073741824 (&H40000000)|L'utente o il gruppo dispone delle autorizzazioni per aggiornare l'oggetto. Per gli oggetti, ad esempio le **tabelle**, l'utente dispone delle autorizzazioni per aggiornare i dati nella tabella.|  
|**adRightWithGrant**|4096 (&H1000)|L'utente o il gruppo dispone dell'autorizzazione per concedere autorizzazioni per l'oggetto.|  
|**adRightWriteDesign**|2048 (&H800)|L'utente o il gruppo dispone dell'autorizzazione per modificare la progettazione per l'oggetto.|  
|**adRightWriteOwner**|524288 (&H80000)|L'utente o il gruppo dispone dell'autorizzazione per modificare il proprietario dell'oggetto.|  
|**adRightWritePermissions**|262144 (&H40000)|L'utente o il gruppo può modificare le autorizzazioni specifiche per un oggetto nel catalogo.|  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Metodo GetPermissions (ADOX)](./getpermissions-method-adox.md)  
    :::column-end:::
    :::column:::
        [Metodo SetPermissions (ADOX)](./setpermissions-method-adox.md)  
    :::column-end:::
:::row-end:::