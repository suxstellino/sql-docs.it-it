---
description: ErrorValueEnum
title: ErrorValueEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ErrorValueEnum
helpviewer_keywords:
- ErrorValueEnum enumeration [ADO]
ms.assetid: 9469ba3a-5e4f-4a10-bbb8-a51a6c9660ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 238c8bf3d0c21625b9317690f70adbe16bd5075f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171164"
---
# <a name="errorvalueenum"></a>ErrorValueEnum
Specifica il tipo di errore in fase di esecuzione ADO.  
  
 Sono elencate tre forme del numero di errore:  
  
-   Decimal positivo: i due byte bassi del numero intero in formato decimale. Questo numero viene visualizzato nella finestra di dialogo predefinita del messaggio di errore Visual Basic. Ad esempio, errore di run-time "3707".  
  
-   Decimal negativo: la conversione decimale del numero di errore completo.  
  
-   Hexadecimal-The rappresentazione esadecimale del numero di errore completo. Il codice della funzionalità di Windows è presente nella quarta cifra. Il codice di struttura per i *numeri di errore ADO è.* Ad esempio: 0x800 ***a** _0E7B.  
  
> [!NOTE]
>  OLE DB errori possono essere passati all'applicazione ADO. In genere, questi possono essere identificati da un codice di funzionalità di Windows _4 *. Ad esempio, 0x800 * * * 4* *_.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|_ *adErrBoundToCommand**|3707-2146824581 0x800A0E7B|Impossibile modificare la proprietà **ActiveConnection** di un oggetto **Recordset** con un oggetto **Command** come origine.|  
|**adErrCannotComplete**|3732-2146824556 0x800A0E94|Il server non è in grado di completare l'operazione.|  
|**adErrCantChangeConnection**|3748-2146824540 0x800A0EA4|La connessione è stata negata. La nuova connessione richiesta ha caratteristiche diverse rispetto a quella già in uso.|  
|**adErrCantChangeProvider**|3220-2146825068 0X800A0C94|Il provider specificato è diverso da quello già in uso.|  
|**adErrCantConvertvalue**|3724-2146824564 0x800A0E8C|Non è possibile convertire il valore di dati per motivi diversi dalla non corrispondenza di segno o dall'overflow dei dati. Ad esempio, la conversione avrebbe dati troncati.|  
|**adErrCantCreate**|3725-2146824563 0x800A0E8D|Impossibile impostare o recuperare il valore dei dati perché il tipo di dati del campo è sconosciuto oppure il provider non dispone di risorse sufficienti per eseguire l'operazione.|  
|**adErrCatalogNotSet**|3747-2146824541 0x800A0EA3|Per l'operazione è necessario un **ParentCatalog** valido.|  
|**adErrColumnNotOnThisRow**|3726-2146824562 0x800A0E8E|Il record non contiene questo campo.|  
|**adErrDataConversion**|3421-2146824867 0x800A0D5D|L'applicazione usa un valore di tipo errato per l'operazione corrente.|  
|**adErrDataOverflow**|3721-2146824567 0x800A0E89|Il valore dei dati è troppo grande per essere rappresentato dal tipo di dati del campo.|  
|**adErrDelResOutOfScope**|3738-2146824550 0x800A0E9A|L'URL dell'oggetto da eliminare esula dall'ambito del record corrente.|  
|**adErrDenyNotSupported**|3750-2146824538 0x800A0EA6|Il provider non supporta le restrizioni di condivisione.|  
|**adErrDenyTypeNotSupported**|3751-2146824537 0x800A0EA7|Il provider non supporta il tipo di restrizione di condivisione richiesto.|  
|**adErrFeatureNotAvailable**|3251-2146825037 0x800A0CB3|L'oggetto o il provider non è in grado di eseguire l'operazione richiesta.|  
|**adErrFieldsUpdateFailed**|3749-2146824539 0x800A0EA5|Aggiornamento campi non riuscito. Per ulteriori informazioni, esaminare la proprietà **status** dei singoli oggetti Field.|  
|**adErrIllegalOperation**|3219-2146825069 0x800A0C93|Operazione non consentita in questo contesto.|  
|**adErrIntegrityViolation**|3719-2146824569 0x800A0E87|Il valore dei dati è in conflitto con i vincoli di integrità del campo.|  
|**adErrInTransaction**|3246-2146825042 0x800A0CAE|Impossibile chiudere in modo esplicito l'oggetto **connessione** in una transazione.|  
|**adErrInvalidArgument**|3001-2146825287 0x800A0BB9|Gli argomenti sono di tipo errato, non sono compresi nell'intervallo accettabile oppure sono in conflitto tra loro.|  
|**adErrInvalidConnection**|3709-2146824579 0x800A0E7D|Impossibile utilizzare la connessione per eseguire questa operazione. È chiuso o non è valido in questo contesto.|  
|**adErrInvalidParamInfo**|3708-2146824580 0x800A0E7C|L'oggetto **Parameter** non è definito correttamente. Sono state fornite informazioni incoerenti o incomplete.|  
|**adErrInvalidTransaction**|3714-2146824574 0x800A0E82|La transazione di coordinamento non è valida o non è stata avviata.|  
|**adErrInvalidURL**|3729-2146824559 0x800A0E91|L'URL contiene caratteri non validi. Verificare che l'URL sia digitato correttamente.|  
|**adErrItemNotFound**|3265-2146825023 0x800A0CC1|Impossibile trovare l'elemento nella raccolta che corrisponde al nome o all'ordinale richiesto.|  
|**adErrNoCurrentRecord**|3021-2146825267 0x800A0BCD|**BOF** o **EOF** è true oppure il record corrente è stato eliminato. L'operazione richiesta richiede un record corrente.|  
|**adErrNotExecuting**|3715-2146824573 0x800A0E83|Non è possibile eseguire l'operazione durante l'esecuzione.|  
|**adErrNotReentrant**|3710-2146824578 0x800A0E7E|Impossibile eseguire l'operazione durante l'elaborazione dell'evento.|  
|**adErrObjectClosed**|3704-2146824584 0x800A0E78|Operazione non consentita quando l'oggetto viene chiuso.|  
|**adErrObjectInCollection**|3367-2146824921 0x800A0D27|L'oggetto è già presente nella raccolta. Impossibile accodare.|  
|**adErrObjectNotSet**|3420-2146824868 0x800A0D5C|Oggetto non più valido.|  
|**adErrObjectOpen**|3705-2146824583 0x800A0E79|Operazione non consentita quando l'oggetto è aperto.|  
|**adErrOpeningFile**|3002-2146825286 0x800A0BBA|Non è stato possibile aprire il file.|  
|**adErrOperationCancelled**|3712-2146824576 0x800A0E80|L'operazione è stata annullata dall'utente.|  
|**adErrOutOfSpace**|3734-2146824554 0x800A0E96|Impossibile eseguire l'operazione. Il provider non può ottenere spazio di archiviazione sufficiente.|  
|**adErrPermissionDenied**|3720-2146824568 0x800A0E88|L'autorizzazione insufficiente impedisce la scrittura nel campo.|  
|**adErrProviderFailed**|3000-2146825288 0x800A0BB8|Il provider non ha eseguito l'operazione richiesta.|  
|**adErrProviderNotFound**|3706-2146824582 0x800A0E7A|Impossibile trovare il provider. Potrebbe non essere installato correttamente.|  
|**adErrReadFile**|3003-2146825285 0x800A0BBB|Impossibile leggere il file.|  
|**adErrResourceExists**|3731-2146824557 0x800A0E93|Non è possibile eseguire l'operazione di copia. L'oggetto denominato dall'URL di destinazione esiste già. Specificare **adCopyOverwrite** per sostituire l'oggetto.|  
|**adErrResourceLocked**|3730-2146824558 0x800A0E92|L'oggetto rappresentato dall'URL specificato è bloccato da uno o più processi. Attendere il completamento del processo, quindi ripetere l'operazione.|  
|**adErrResourceOutOfScope**|3735-2146824553 0x800A0E97|L'URL di origine o di destinazione non rientra nell'ambito del record corrente.|  
|**adErrSchemaViolation**|3722-2146824566 0x800A0E8A|Il valore dei dati è in conflitto con il tipo di dati o i vincoli del campo.|  
|**adErrSignMismatch**|3723-2146824565 0x800A0E8B|La conversione non è riuscita perché il valore dei dati è firmato e il tipo di dati del campo utilizzato dal provider è senza segno.|  
|**adErrStillConnecting**|3713-2146824575 0x800A0E81|Impossibile eseguire l'operazione durante la connessione asincrona.|  
|**adErrStillExecuting**|3711-2146824577 0x800A0E7F|Non è possibile eseguire l'operazione durante l'esecuzione asincrona.|  
|**adErrTreePermissionDenied**|3728-2146824560 0x800A0E90|Le autorizzazioni non sono sufficienti per accedere ad albero o sottoalbero.|  
|**adErrUnavailable**|3736-2146824552 0x800A0E98|L'operazione non è stata completata e lo stato non è disponibile. Il campo potrebbe non essere disponibile oppure l'operazione non è stata tentata.|  
|**adErrUnsafeOperation**|3716-2146824572 0x800A0E84|Le impostazioni di sicurezza di questo computer impediscono l'accesso a un'origine dati in un altro dominio.|  
|**adErrURLDoesNotExist**|3727-2146824561 0x800A0E8F|L'URL di origine o l'elemento padre dell'URL di destinazione non esiste.|  
|**adErrURLNamedRowDoesNotExist**|3737-2146824551 0x800A0E99|Il record denominato da questo URL non esiste.|  
|**adErrVolumeNotFound**|3733-2146824555 0x800A0E95|Il provider non è in grado di individuare il dispositivo di archiviazione indicato dall'URL. Verificare che l'URL sia digitato correttamente.|  
|**adErrWriteFile**|3004-2146825284 0x800A0BBC|Scrittura nel file non riuscita.|  
|**adWrnSecurityDialog**|3717-2146824571 0x800A0E85|Solo per uso interno. Non usare.|  
|**adWrnSecurityDialogHeader**|3718-2146824570 0x800A0E86|Solo per uso interno. Non usare.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
 Vengono definiti solo i subset seguenti di equivalenti ADO/WFC.  
  
|Costante|  
|--------------|  
|AdoEnums. ErrorValue. BOUNDTOCOMMAND|  
|AdoEnums. ErrorValue. dataconversione|  
|AdoEnums. ErrorValue. FEATURENOTAVAILABLE|  
|AdoEnums. ErrorValue. ILLEGALOPERATION|  
|AdoEnums. ErrorValue. InTransaction|  
|AdoEnums. ErrorValue. INVALIDARGUMENT|  
|AdoEnums. ErrorValue. INVALIDCONNECTION|  
|AdoEnums. ErrorValue. INVALIDPARAMINFO|  
|AdoEnums. ErrorValue. ITEMNOTFOUND|  
|AdoEnums. ErrorValue. NOCURRENTRECORD|  
|AdoEnums. ErrorValue. NOTEXECUTING|  
|AdoEnums. ErrorValue. NOTREENTRANT|  
|AdoEnums. ErrorValue. OBJECTCLOSED|  
|AdoEnums. ErrorValue. OBJECTINCOLLECTION|  
|AdoEnums. ErrorValue. OBJECTNOTSET|  
|AdoEnums. ErrorValue. OBJECTOPEN|  
|AdoEnums. ErrorValue. DISPLAYCOMPOSITEBITMAP|  
|AdoEnums. ErrorValue. PROVIDERNOTFOUND|  
|AdoEnums. ErrorValue. STILLCONNECTING|  
|AdoEnums. ErrorValue. STILLEXECUTING|  
|AdoEnums. ErrorValue. UNSAFEOPERATION|  
  
## <a name="applies-to"></a>Si applica a  
 [Proprietà Number (ADO)](../../../ado/reference/ado-api/number-property-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Codici errore ADO](../../../ado/guide/appendixes/ado-error-codes.md)
