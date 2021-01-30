---
description: Proprietà Recordset e SourceRecordset (Servizi Desktop remoto)
title: Recordset, proprietà SourceRecordset (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Recordset property [ADO]
ms.assetid: a29e3fb9-306d-497a-9a59-1856a914e5e9
author: rothja
ms.author: jroth
ms.openlocfilehash: b4f8f1db220bdf68205e3bd4628a27d0b57705b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168812"
---
# <a name="recordset-sourcerecordset-properties-rds"></a>Proprietà Recordset e SourceRecordset (Servizi Desktop remoto)
Indica l'oggetto **Recordset** restituito da un oggetto business personalizzato.  
  
 **Si applica a:** [oggetto DataControl (RDS)](./datacontrol-object-rds.md)  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DataControl.SourceRecordset = Recordset  
Recordset = DataControl.Recordset   
```  
  
#### <a name="parameters"></a>Parametri  
 *DataControl*  
 Variabile oggetto che rappresenta un Servizi Desktop remoto [. Oggetto DataControl](./datacontrol-object-rds.md) .  
  
 *Recordset*  
 Variabile oggetto che rappresenta un oggetto **Recordset** .  
  
## <a name="remarks"></a>Commenti  
 È possibile impostare la proprietà **SourceRecordset** su un [Recordset](../ado-api/recordset-object-ado.md) restituito da un oggetto business personalizzato.  
  
 Queste proprietà consentono a un'applicazione di gestire il processo di associazione per mezzo di un processo personalizzato. Ricevono un set di righe racchiuso in un **Recordset** in modo che sia possibile interagire direttamente con il **Recordset**, eseguendo azioni quali l'impostazione delle proprietà o l'iterazione del **Recordset**.  
  
 È possibile impostare la proprietà **SourceRecordset** o leggere la proprietà **Recordset** in fase di esecuzione nel codice di scripting.  
  
 **SourceRecordset** è una proprietà di sola scrittura, a differenza della proprietà **Recordset** , che è una proprietà di sola lettura.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Recordset e SourceRecordset (VBScript)](./recordset-and-sourcerecordset-properties-example-vbscript.md)   
 [Metodo CreateRecordset (RDS)](./createrecordset-method-rds.md)   
 [Metodo Query (Servizi Desktop remoto)](./query-method-rds.md)