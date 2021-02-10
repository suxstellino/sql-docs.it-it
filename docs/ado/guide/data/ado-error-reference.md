---
description: Informazioni di riferimento sugli errori ADO
title: Guida di riferimento agli errori ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], number reference
- errors [ADO], ErrorValueEnum
- ErrorValueEnum enumeration [ADO]
ms.assetid: f653393e-d4b0-4c34-ad5f-2bdf56bc1305
author: rothja
ms.author: jroth
ms.openlocfilehash: cfb6a2ede665c939aa5824810c692bdbe02df6c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028373"
---
# <a name="ado-errors"></a>Errori ADO
La costante **ErrorValueEnum** descrive i valori di errore ADO. Per un elenco completo di queste costanti enumerate, inclusi i valori, vedere [Appendice B: errori ADO](../appendixes/appendix-b-ado-errors.md). In questa sezione vengono esaminati alcuni degli errori più interessanti e vengono illustrate alcune situazioni specifiche che possono sollevarle o soluzioni per risolvere il problema. Vengono elencate sia la costante **ErrorValueEnum** che il numero decimale positivo breve.

|Number|Costante ErrorValueEnum|Descrizione/possibili cause|
|------------|-----------------------------|----------------------------------|
|**3000**|**adErrProviderFailed**|Il provider non è riuscito a eseguire l'operazione richiesta.|
|**3001**|**adErrInvalidArgument**|Gli argomenti sono di tipo errato, non sono compresi nell'intervallo accettabile oppure sono in conflitto tra loro. Questo errore è spesso causato da un errore tipografico in un'istruzione SQL SELECT. Ad esempio, un nome di campo o un nome di tabella con errori di ortografia può generare questo errore. Questo errore può verificarsi anche quando un campo o una tabella denominata in un'istruzione SELECT non esiste nell'archivio dati.|
|**3002**|**adErrOpeningFile**|Non è stato possibile aprire il file. È stato specificato un nome di file con errori di ortografia oppure un file è stato spostato, rinominato o eliminato. In una rete, l'unità potrebbe essere temporaneamente non disponibile o il traffico di rete potrebbe impedire una connessione.|
|**3003**|**adErrReadFile**|Impossibile leggere il file. Il nome del file è stato specificato in modo errato, il file potrebbe essere stato spostato o eliminato oppure il file potrebbe essere stato danneggiato.|
|**3004**|**adErrWriteFile**|Scrittura nel file non riuscita. È possibile che sia stato chiuso un file e che si sia tentato di scrivervi oppure che il file sia danneggiato. Se il file si trova in un'unità di rete, le condizioni della rete transitoria potrebbero impedire la scrittura in un'unità di rete.|
|**3021**|**adErrNoCurrentRecord**|**BOF** o **EOF** è true oppure il record corrente è stato eliminato. L'operazione richiesta richiede un record corrente.<br /><br /> È stato effettuato un tentativo di aggiornare i record usando **trova** o **Cerca** per spostare il puntatore del record nel record desiderato. Se il record non viene trovato, **EOF** sarà true. Questo errore può verificarsi anche dopo un valore **AddNew** o **Delete** non riuscito perché non è presente alcun record corrente quando questi metodi hanno esito negativo.|
|**3219**|**adErrIllegalOperation**|Operazione non consentita in questo contesto.|
|**3220**|**adErrCantChangeProvider**|Il provider specificato è diverso da quello già in uso.|
|**3246**|**adErrInTransaction**|Impossibile chiudere in modo esplicito l'oggetto **connessione** in una transazione. Un **Recordset** o un oggetto **connessione** che attualmente partecipa a una transazione non può essere chiuso. Chiamare **RollbackTrans** o **CommitTrans** prima di chiudere l'oggetto.|
|**3251**|**adErrFeatureNotAvailable**|L'oggetto o il provider non è in grado di eseguire l'operazione richiesta. Alcune operazioni dipendono da una specifica versione del provider.|
|**3265**|**adErrItemNotFound**|Impossibile trovare l'elemento nella raccolta corrispondente al nome o al numero ordinale richiesto. È stato specificato un nome di campo o di tabella errato.|
|**3367**|**adErrObjectInCollection**|L'oggetto è già presente nella raccolta. Impossibile accodare. Non è possibile aggiungere due volte un oggetto alla stessa raccolta.|
|**3420**|**adErrObjectNotSet**|Oggetto non più valido.|
|**3421**|**adErrDataConversion**|L'applicazione usa un valore di tipo errato per l'operazione corrente. È possibile che sia stata fornita una stringa a un'operazione che prevede un flusso, ad esempio.|
|**3704**|**adErrObjectClosed**|Operazione non consentita quando l'oggetto viene chiuso. La **connessione** o il **Recordset** è stato chiuso. Ad esempio, un'altra routine potrebbe avere chiuso un oggetto globale. È possibile evitare questo errore controllando la proprietà **state** prima di provare a eseguire un'operazione.|
|**3705**|**adErrObjectOpen**|Operazione non consentita quando l'oggetto è aperto. Impossibile aprire un oggetto aperto. Non è possibile aggiungere campi a un **Recordset** aperto.|
|**3706**|**adErrProviderNotFound**|Impossibile trovare il provider. Potrebbe non essere installato correttamente.<br /><br /> Il nome del provider potrebbe non essere specificato correttamente, il provider specificato potrebbe non essere installato nel computer in cui è in esecuzione il codice o l'installazione potrebbe essere danneggiata.|
|**3707**|**adErrBoundToCommand**|Non è possibile modificare la proprietà **ActiveConnection** di un oggetto **Recordset** , che ha un oggetto **comando** come origine. L'applicazione ha tentato di assegnare un nuovo oggetto **connessione** a un **Recordset** che dispone di un oggetto **comando** come origine.|
|**3708**|**adErrInvalidParamInfo**|L'oggetto **Parameter** è definito in modo errato. Sono state fornite informazioni incoerenti o incomplete.|
|**3709**|**adErrInvalidConnection**|Impossibile utilizzare la connessione per eseguire questa operazione. È chiuso o non è valido in questo contesto.|
|**3710**|**adErrNotReentrant**|Impossibile eseguire l'operazione durante l'elaborazione dell'evento. Non è possibile eseguire un'operazione all'interno di un gestore eventi che fa sì che venga generato nuovamente l'evento. Ad esempio, i metodi di navigazione non devono essere chiamati dall'interno di un gestore eventi **WillMove** .|
|**3711**|**adErrStillExecuting**|Non è possibile eseguire l'operazione durante l'esecuzione asincrona.|
|**3712**|**adErrOperationCancelled**|L'operazione è stata annullata dall'utente. L'applicazione ha chiamato il metodo **CancelUpdate** o **CancelBatch** e l'operazione corrente è stata annullata.|
|**3713**|**adErrStillConnecting**|Impossibile eseguire l'operazione durante la connessione asincrona.|
|**3714**|**adErrInvalidTransaction**|La transazione di coordinamento non è valida o non è stata avviata.|
|**3715**|**adErrNotExecuting**|Non è possibile eseguire l'operazione durante l'esecuzione.|
|**3716**|**adErrUnsafeOperation**|Le impostazioni di sicurezza del computer non consentono l'accesso a un'origine dati in un altro dominio.|
|**3717**|**adWrnSecurityDialog**|Solo per uso interno. Non usare. (La voce è stata inclusa per motivi di completezza. Questo errore non dovrebbe essere visualizzato nel codice.|
|**3718**|**adWrnSecurityDialogHeader**|Solo per uso interno. Non usare. (Voce inclusa per motivi di completezza. Questo errore non dovrebbe essere visualizzato nel codice.|
|**3719**|**adErrIntegrityViolation**|Il valore dei dati è in conflitto con i vincoli di integrità del campo. Un nuovo valore per un **campo** provocherebbe una chiave duplicata. Un valore che costituisce un lato di una relazione tra due record potrebbe non essere aggiornabile.|
|**3720**|**adErrPermissionDenied**|L'autorizzazione insufficiente impedisce la scrittura nel campo. L'utente indicato nella stringa di connessione non dispone delle autorizzazioni appropriate per scrivere in un **campo**.|
|**3721**|**adErrDataOverflow**|Il valore dei dati è troppo grande per essere rappresentato dal tipo di dati del campo. È stato assegnato un valore numerico troppo grande per il campo previsto. Ad esempio, un valore long integer è stato assegnato a un campo short integer.|
|**3722**|**adErrSchemaViolation**|Il valore dei dati è in conflitto con il tipo di dati o i vincoli del campo. L'archivio dati presenta vincoli di convalida diversi dal valore del **campo** .|
|**3723**|**adErrSignMismatch**|La conversione non è riuscita perché il valore dei dati è firmato e il tipo di dati del campo utilizzato dal provider è senza segno.|
|**3724**|**adErrCantConvertvalue**|Non è possibile convertire il valore di dati per motivi diversi dalla non corrispondenza di segno o dall'overflow dei dati. Ad esempio, la conversione avrebbe dati troncati.|
|**3725**|**adErrCantCreate**|Impossibile impostare o recuperare il valore dei dati perché il tipo di dati del campo è sconosciuto oppure il provider non dispone di risorse sufficienti per eseguire l'operazione.|
|**3726**|**adErrColumnNotOnThisRow**|Il record non contiene questo campo. È stato specificato un nome di campo errato oppure è stato fatto riferimento a un campo non presente nella raccolta **Fields** del record corrente.|
|**3727**|**adErrURLDoesNotExist**|L'URL di origine o l'elemento padre dell'URL di destinazione non esiste. Si è verificato un errore tipografico nell'URL di origine o di destinazione. È possibile che si disponga di `https://mysite/photo/myphoto.jpg` in `https://mysite/photos/myphoto.jpg` alternativa. Errore tipografico nell'URL padre (in questo caso, *foto* anziché *foto*) ha causato l'errore.|
|**3728**|**adErrTreePermissionDenied**|Le autorizzazioni non sono sufficienti per accedere ad albero o sottoalbero. L'utente indicato nella stringa di connessione non dispone delle autorizzazioni appropriate.|
|**3729**|**adErrInvalidURL**|L'URL contiene caratteri non validi. Verificare che l'URL sia digitato correttamente. L'URL segue lo schema registrato per il provider corrente (ad esempio, il provider di pubblicazione Internet è registrato per http).|
|**3730**|**adErrResourceLocked**|L'oggetto rappresentato dall'URL specificato è bloccato da uno o più processi. Attendere il completamento del processo e ripetere l'operazione. L'oggetto a cui si sta tentando di accedere è stato bloccato da un altro utente o da un altro processo nell'applicazione. Questo è più probabile che si verifichi in un ambiente multiutente.|
|**3731**|**adErrResourceExists**|Non è possibile eseguire l'operazione di copia. L'oggetto denominato dall'URL di destinazione esiste già. Specificare **adCopyOverwrite** per sostituire l'oggetto. Se non si specifica **adCopyOverwrite** durante la copia dei file in una directory, la copia ha esito negativo quando si tenta di copiare un elemento già esistente nella posizione di destinazione.|
|**3732**|**adErrCannotComplete**|Il server non è in grado di completare l'operazione. Questo problema potrebbe essere dovuto al fatto che il server è occupato con altre operazioni o potrebbe avere risorse insufficienti.|
|**3733**|**adErrVolumeNotFound**|Il provider non è in grado di individuare il dispositivo di archiviazione indicato dall'URL. Verificare che l'URL sia digitato correttamente. L'URL del dispositivo di archiviazione potrebbe non essere corretto, ma questo errore può verificarsi per altri motivi. Il dispositivo potrebbe essere offline o un volume elevato di traffico di rete potrebbe impedire la connessione.|
|**3734**|**adErrOutOfSpace**|Impossibile eseguire l'operazione. Il provider non può ottenere spazio di archiviazione sufficiente. Potrebbe non essere disponibile memoria sufficiente per i file temporanei sul server.|
|**3735**|**adErrResourceOutOfScope**|L'URL di origine o di destinazione non rientra nell'ambito del record corrente.|
|**3736**|**adErrUnavailable**|Non è stato possibile completare l'operazione e lo stato non è disponibile. Il campo potrebbe non essere disponibile oppure l'operazione non è stata tentata. Un altro utente potrebbe aver modificato o eliminato il campo a cui si sta tentando di accedere.|
|**3737**|**adErrURLNamedRowDoesNotExist**|Il record denominato da questo URL non esiste. Durante il tentativo di aprire un file utilizzando un oggetto **record** , il nome del file o il percorso del file non è stato digitato in modo errato.|
|**3738**|**adErrDelResOutOfScope**|L'URL dell'oggetto da eliminare esula dall'ambito del record corrente.|
|**3747**|**adErrCatalogNotSet**|Per l'operazione è necessario un **ParentCatalog** valido.|
|**3748**|**adErrCantChangeConnection**|La connessione è stata negata. La nuova connessione richiesta ha caratteristiche diverse rispetto a quella già in uso.|
|**3749**|**adErrFieldsUpdateFailed**|Aggiornamento campi non riuscito. Per ulteriori informazioni, esaminare la proprietà **status** dei singoli oggetti Field. Questo errore può verificarsi in due situazioni: quando si modifica il valore di un oggetto **campo** nel processo di modifica o aggiunta di un record al database; e quando si modificano le proprietà dell'oggetto **campo** .<br /><br /> Aggiornamento del **record** o del **Recordset** non riuscito a causa di un problema con uno dei campi del record corrente. Enumerare la raccolta **Fields** e controllare la proprietà **status** di ogni campo per determinare la causa del problema.|
|**3750**|**adErrDenyNotSupported**|Il provider non supporta le restrizioni di condivisione. È stato effettuato un tentativo di limitare la condivisione dei file e il provider non supporta il concetto.|
|**3751**|**adErrDenyTypeNotSupported**|Il provider non supporta il tipo di restrizione di condivisione richiesto. È stato effettuato un tentativo di stabilire un particolare tipo di restrizione di condivisione file non supportata dal provider. Vedere la documentazione del provider per determinare quali restrizioni di condivisione file sono supportate.|