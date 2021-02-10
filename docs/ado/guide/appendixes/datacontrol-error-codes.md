---
description: Codici di errore dell'oggetto DataControl
title: Codici di errore del controllo DataControl | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], DataControl
- DataControl errors [ADO]
ms.assetid: 293df9d5-e1a2-406d-9107-07bf7cdc6f96
author: rothja
ms.author: jroth
ms.openlocfilehash: dbcf0b6aef651e90c90df1cb49af2dbe6a29c674
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029435"
---
# <a name="datacontrol-object-error-codes"></a>Codici di errore dell'oggetto DataControl
Nella tabella seguente sono elencati i Servizi Desktop remoto [. ](../../reference/rds-api/datacontrol-object-rds.md) Codici di errore dell'oggetto DataControl. La conversione decimale positiva dei due byte bassi, la conversione decimale negativa del codice di errore completo e i valori esadecimali vengono visualizzati.

|RDS. Codici di errore di DataControl|Number|Descrizione|
|---------------------------------|------------|-----------------|
|**IDS_AsyncPending**|4107-2146824175 0x800A1011|Non è possibile eseguire l'operazione mentre l'operazione asincrona è in sospeso.|
|**IDS_BadInlineTablegram**|4105-2146824183 0x800A1009|TableGram inline non valido.|
|**IDS_CantConnect**|4099-2146824189 0x800A1003|Impossibile connettersi al server.|
|**IDS_CantCreateObject**|4100-2146824188 0x800A1004|Impossibile creare l'oggetto business.|
|**IDS_CantFindDataspace**|4102-2146824186 0x800A1006|Proprietà DataSpace non valida.|
|**IDS_CantInvokeMethod**|4101-2146824187 0x800A1005|Impossibile richiamare il metodo su un oggetto business.|
|**IDS_CrossDomainWarning**|4112-2146824170 0x800A1016|Questa pagina accede ai dati in un altro dominio. Si desidera consentire questa operazione? Per evitare questo messaggio in Internet Explorer, è possibile aggiungere un sito Web protetto all'area siti attendibili nella scheda **sicurezza** della finestra di dialogo **Opzioni Internet** .|
|**IDS_InvalidADCClientVersion**|4106-2146824176 0x800A1010|Versione client RDS non valida: il client è più recente del server.|
|**IDS_INVALIDARG**|5376-2147019520 0x80071500|Uno o più argomenti non sono validi.|
|**IDS_InvalidBindings**|4097-2146824191 0x800A1001|Errore nella proprietà Bindings.|
|**IDS_InvalidParam**|4110-2146824172 0x800A1014|Uno o più argomenti non sono validi.|
|**IDS_NOINTERFACE**|5377-2147019519 0x80071501|Questa interfaccia non è supportata.|
|**IDS_NotReentrant**|4111-2146824171 0x800A1015|Impossibile eseguire la richiesta mentre il gestore eventi è ancora in fase di elaborazione.|
|**IDS_ObjectNotSafe**|4103-2146824185 0x800A1007|Le impostazioni di sicurezza del computer non consentono la creazione di oggetti business.|
|**IDS_RecordsetNotOpen**|4109-2146824173 0x800A1013|**Recordset** non aperto.|
|**IDS_ResetInvalidField**|4108-2146824174 0x800A1012|La colonna specificata in **SortColumn** o **FilterColumn offrono** non esiste.|
|**IDS_RowsetNotUpdateable**|4104-2146824184 0x800A1008|Set di righe non aggiornabile.|
|**IDS_UnexpectedError**|4351-2146823937 0x800A10FF|Errore imprevisto.|
|**IDS_UpdatesFailed**|4098-2146824190 0x800A1002|Impossibile aggiornare il database.|
|**IDS_URLMONNotFound**|4119-2146824169 0x800A1017|Per la proprietà **URL** DataControl è necessario che il file di sistema Urlmon.dll, che non è stato trovato.|

## <a name="see-also"></a>Vedere anche
 [Oggetto DataControl (Servizi Desktop remoto)](../../reference/rds-api/datacontrol-object-rds.md)