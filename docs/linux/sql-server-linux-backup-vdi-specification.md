---
title: Specifica del backup VDI - SQL Server in Linux
description: Informazioni sulle interfacce fornite dall'SDK del client VDI (Virtual Device Interface) per SQL Server in Linux.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 0250ba2b-8cdd-450e-9109-bf74f70e1247
ms.openlocfilehash: dcdbd7ba3a00048b63836e52092835fe6f7a618f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343179"
---
# <a name="sql-server-on-linux-vdi-client-sdk-specification"></a>Specifica SDK client VDI per SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo documento illustra le interfacce fornite dall'SDK del client VDI (Virtual Device Interface) per SQL Server in Linux. I fornitori di software indipendenti (ISV) possono usare l'API Virtual Backup Device Interface (VDI) per integrare SQL Server nei propri prodotti. In generale, VDI in Linux si comporta in modo analogo a VDI in Windows con le modifiche seguenti:

- La memoria condivisa di Windows diventa la memoria condivisa POSIX.
- I semafori di Windows diventano i semafori POSIX.
- I tipi Windows come HRESULT e DWORD vengono sostituiti con gli equivalenti integer.
- Le interfacce COM vengono rimosse e sostituite con una coppia di classi C++.
- SQL Server in Linux non supporta le istanze denominate, quindi i riferimenti al nome dell'istanza sono stati rimossi. 
- La libreria condivisa è implementata in libsqlvdi.so installato nel percorso /opt/MSSQL/lib/libsqlvdi.so.

Questo documento è un supplemento di **vbackup.chm** che descrive in dettaglio la specifica VDI di MS SQL Serve in Windows. Scaricare la [specifica SQL VDI di Windows](https://www.microsoft.com/download/details.aspx?id=17282).

Vedere anche la soluzione di backup VDI di esempio nel [repository GitHub degli esempi di SQL Server](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sqlvdi-linux).

## <a name="user-permissions-setup"></a>Configurazione delle autorizzazioni utente

In Linux, le primitive POSIX sono di proprietà dell'utente che le crea e del relativo gruppo predefinito. Per impostazione predefinita, gli oggetti creati da SQL Server sono di proprietà dell'utente mssql e del gruppo mssql. Per consentire la condivisione tra SQL Server e il client VDI, è consigliabile usare uno dei due metodi seguenti:

1. Eseguire il client VDI come utente mssql
   
   Eseguire il comando seguente per passare all'utente mssql:
   
   ```bash
   sudo su mssql
   ```

2. Aggiungere l'utente mssql al gruppo vdiuser e vdiuser al gruppo mssql.
   
   Eseguire i comandi seguenti:

   ```bash
   sudo useradd vdiuser
   sudo usermod -a -G mssql vdiuser
   sudo usermod -a -G vdiuser mssql
   ```

   Riavviare il server per selezionare nuovi gruppi per SQL Server e vdiuser

## <a name="client-functions"></a>Funzioni client

Questa sezione contiene le descrizioni di ognuna delle funzioni client. Le descrizioni includono le informazioni seguenti:

- Scopo della funzione
- Sintassi della funzione
- Elenco parametri
- Valori restituiti
- Osservazioni

## <a name="clientvirtualdevicesetcreate"></a>ClientVirtualDeviceSet::Create

**Scopo** Questa funzione crea il set di dispositivi virtuali.

**Sintassi**
   ```
   int ClientVirtualDeviceSet::Create (
   char *   name,       // name for the set
   VDConfig   * cfg     // configuration for the set
   );
   ```

| Parametri | Argomento | Spiegazione
| ----- | ----- | ------ |
| | **nome** | Identifica il set di dispositivi virtuali. È necessario seguire le regole per i nomi usati da CreateFileMapping(). È possibile usare qualsiasi carattere eccetto la barra rovesciata (\)). Stringa di caratteri. È consigliabile anteporre alla stringa il nome del prodotto o della società dell'utente e il nome del database. |
| |**cfg** | Configurazione per il set di dispositivi virtuali. Per altre informazioni, vedere la sezione "Configurazione" più avanti in questo documento.

| Valori restituiti | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**NOERROR** | Funzione completata. |
| |**VD_E_NOTSUPPORTED** |Uno o più campi della configurazione non sono validi o altrimenti non supportati. |
| |**VD_E_PROTOCOL** | Il set di dispositivi virtuali esiste già.

**Osservazioni** Il metodo Create deve essere chiamato una sola volta per ogni operazione di backup o ripristino. Dopo aver richiamato il metodo Close, il client può riutilizzare l'interfaccia per creare un altro set di dispositivi virtuali.

## <a name="clientvirtualdevicesetgetconfiguration"></a>ClientVirtualDeviceSet::GetConfiguration

**Scopo** Questa funzione viene usata per attendere che il server configuri il set di dispositivi virtuali.
**Sintassi**
   ```
   int ClientVirtualDeviceSet::GetConfiguration (
   time_t       timeout,    // in milliseconds
   VDConfig *       cfg // selected configuration
   );
   ```

| Parametri | Argomento | Spiegazione
| ----- | ----- | ------ |
| | **timeout** | Timeout in millisecondi. Usare INFINITE o un numero intero negativo per impedire il timeout.
| | **cfg** | Dopo la corretta esecuzione, contiene la configurazione selezionata dal server. Per altre informazioni, vedere la sezione "Configurazione" più avanti in questo documento.

| Valori restituiti | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**NOERROR** | La configurazione è stata restituita.
| |**VD_E_ABORT** |SignalAbort è stato richiamato.
| |**VD_E_TIMEOUT** |Timeout della funzione.

**Osservazioni** Questa funzione causa un blocco in uno stato di avviso. Una volta completata la chiamata, è possibile aprire i dispositivi nel set di dispositivi virtuali.


## <a name="clientvirtualdevicesetopendevice"></a>ClientVirtualDeviceSet::OpenDevice
**Scopo** Questa funzione apre uno dei dispositivi nel set di dispositivi virtuali.
**Sintassi**
   ```
   int ClientVirtualDeviceSet::OpenDevice (
   char *           name,       // name for the set
   ClientVirtualDevice **       ppVirtualDevice // returns interface to device
   );
   ```

| Parametri | Argomento | Spiegazione
| ----- | ----- | ------ |
| | **nome** |Identifica il set di dispositivi virtuali.
| | **ppVirtualDevice** |Quando la funzione riesce, viene restituito un puntatore al dispositivo virtuale. Questo dispositivo viene usato per GetCommand e CompleteCommand.

| Valori restituiti | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**NOERROR** |Funzione completata.
| |**VD_E_ABORT** | È stata richiesta l'interruzione.
| |**VD_E_OPEN** |  Tutti i dispositivi sono aperti.
| |**VD_E_PROTOCOL** |  Il set non è nello stato di inizializzazione o questo particolare dispositivo è già aperto.
| |**VD_E_INVALID** |Il nome del dispositivo non è valido. Non è uno dei nomi noti inclusi nel set.

**Osservazioni** VD_E_OPEN può essere restituito senza problemi. Il client può chiamare OpenDevice per mezzo di un ciclo finché questo codice non viene restituito.
Se sono configurati più dispositivi, ad esempio *n* dispositivi, il set di dispositivi virtuali restituirà *n* interfacce di dispositivo univoche.

La funzione `GetConfiguration` può essere usata per attendere che i dispositivi possano essere aperti.
Se la funzione non riesce, viene restituito un valore Null tramite ppVirtualDevice.
 
## <a name="clientvirtualdevicegetcommand"></a>ClientVirtualDevice::GetCommand

**Scopo** Questa funzione viene usata per ottenere il comando successivo accodato a un dispositivo. Quando richiesto, questa funzione attende il comando successivo.

**Sintassi**
   ```
   int ClientVirtualDevice::GetCommand (
   time_t       timeout,    // time-out in milliseconds
   VDC_Command**    ppCmd   // returns the next command
   );
   ```

| Parametri | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**timeout** |Tempo di attesa, in millisecondi. Usare INFINTE per un'attesa illimitata. Usare 0 per eseguire il polling di un comando. Se non è attualmente disponibile alcun comando, viene restituito VD_E_TIMEOUT. Se si verifica il timeout, il client decide l'azione successiva.
| |**Timeout** |Tempo di attesa, in millisecondi. Usare INFINTE o un valore negativo per un'attesa illimitata. Usare 0 per eseguire il polling di un comando. Se non è disponibile alcun comando prima della scadenza del timeout, viene restituito VD_E_TIMEOUT. Se si verifica il timeout, il client decide l'azione successiva.
| |**ppCmd** |Quando un comando viene restituito correttamente, il parametro restituisce l'indirizzo di un comando da eseguire. La memoria restituita è di sola lettura. Quando il comando viene completato, questo puntatore viene passato alla routine CompleteCommand. Per informazioni dettagliate su ogni comando, vedere la sezione "Comandi" più avanti in questo documento.
        
| Valori restituiti | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**NOERROR** |È stato recuperato un comando.
| |**VD_E_CLOSE** |Il dispositivo è stato chiuso dal server.
| |**VD_E_TIMEOUT** |Nessun comando disponibile e timeout scaduto.
| |**VD_E_ABORT** |Il client o il server ha usato SignalAbort per forzare l'arresto.

**Osservazioni** La restituzione di VD_E_CLOSE indica che SQL Server ha chiuso il dispositivo. Fa parte della normale procedura di arresto. Una volta chiusi tutti i dispositivi, il client richiama ClientVirtualDeviceSet::Close per chiudere il set di dispositivi virtuali.
Quando questa routine deve bloccarsi per attendere un comando, il thread viene lasciato in una condizione di avviso.

## <a name="clientvirtualdevicecompletecommand"></a>ClientVirtualDevice::CompleteCommand

**Scopo** Questa funzione viene usata per notificare a SQL Server il completamento di un comando. Devono essere restituite le informazioni di completamento appropriate per il comando. Per altre informazioni, vedere la sezione "Comandi" più avanti in questo documento.

**Sintassi** 

   ```
   int ClientVirtualDevice::CompleteCommand (
   VDC_Command pCmd,        // the command
   int  completionCode,     // completion code
   unsigned long    bytesTransferred,   // bytes transferred
   int64_t  position        // current position
   );
   ```

| Parametri | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**pCmd** |Indirizzo di un comando restituito in precedenza da ClientVirtualDevice::GetCommand.
| |**completionCode** |Codice di stato che indica lo stato di completamento. Questo parametro deve essere restituito per tutti i comandi. Il codice restituito deve essere appropriato per il comando in esecuzione. ERROR_SUCCESS viene usato in tutti i casi per indicare un comando eseguito correttamente. Per l'elenco completo dei codici possibili, vedere il file vdierror.h. Un elenco dei codici di stato tipici per ogni comando è disponibile nella sezione "Comandi" più avanti in questo documento.
| |**bytesTransferred** |Numero di byte trasferiti correttamente. Viene restituito solo per i comandi di trasferimento dati Read e Write.
| |**position** |Risposta al solo comando GetPosition.
        
| Valori restituiti | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**NOERROR** |Il completamento è stato annotato correttamente.
| |**VD_E_INVALID** |pCmd non è un comando attivo.
| |**VD_E_ABORT** |Interruzione segnalata.
| |**VD_E_PROTOCOL** |Il dispositivo non è aperto.

**Osservazioni** Nessuna

## <a name="clientvirtualdevicesetsignalabort"></a>ClientVirtualDeviceSet::SignalAbort

**Scopo** Questa funzione viene usata per segnalare che dovrebbe verificarsi una chiusura anomala.

**Sintassi** 

   ```
   int ClientVirtualDeviceSet::SignalAbort ();
   ```

| Parametri | Argomento | Spiegazione
| ----- | ----- | ------ |
| |nessuno | Non applicabile
        
| Valori restituiti | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**NOERROR**|La notifica di interruzione è stata inviata correttamente.

**Osservazioni** In qualsiasi momento il client può scegliere di interrompere l'operazione di backup o ripristino. Questa routine segnala che tutte le operazioni devono essere interrotte. Il set globale di dispositivi virtuali entra in uno stato di chiusura anomala. Nessun altro comando viene restituito su nessun dispositivo. Tutti i comandi non completati vengono completati automaticamente, restituendo ERROR_OPERATION_ABORTED come codice di completamento. Il client deve chiamare ClientVirtualDeviceSet::Close dopo aver terminato in modo sicuro qualsiasi uso dei buffer forniti al client. Per altre informazioni, vedere "Chiusura anomala" più indietro in questo documento.

## <a name="clientvirtualdevicesetclose"></a>ClientVirtualDeviceSet::Close

**Scopo** Questa funzione chiude il set di dispositivi virtuali creato da ClientVirtualDeviceSet::Create. Il risultato è il rilascio di tutte le risorse associate al set di dispositivi virtuali.

**Sintassi** 

   ```
   int ClientVirtualDeviceSet::Close ();
   ```

| Parametri | Argomento | Spiegazione
| ----- | ----- | ------ |
| |nessuno |Non applicabile
        
| Valori restituiti | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**NOERROR** |Viene restituito dopo la corretta chiusura del set di dispositivi virtuali.
| |**VD_E_PROTOCOL** |Non è stata eseguita alcuna azione perché il set di dispositivi virtuali non era aperto.
| |**VD_E_OPEN** |I dispositivi erano ancora aperti.

**Osservazioni** La chiamata di Close è una dichiarazione del client che tutte le risorse usate dal set di dispositivi virtuali devono essere rilasciate. Il client deve verificare che tutte le attività che coinvolgono i buffer di dati e i dispositivi virtuali siano terminate prima di richiamare Close. Tutte le interfacce di dispositivo virtuale restituite da OpenDevice vengono invalidate da Close.
Il client è autorizzato a eseguire una chiamata Create sull'interfaccia del set di dispositivi virtuali dopo la restituzione della chiamata Close. Questa chiamata creerebbe un nuovo set di dispositivi virtuali per un'operazione di backup o ripristino successiva.
Se Close viene chiamato quando uno o più dispositivi virtuali sono ancora aperti, viene restituito VD_E_OPEN. In questo caso, SignalAbort viene attivato internamente per garantire un arresto corretto, se possibile. Le risorse VDI vengono rilasciate. Il client deve attendere un'indicazione VD_E_CLOSE su ogni dispositivo prima di richiamare ClientVirtualDeviceSet::Close. Se il client sa che il set di dispositivi virtuali si trova già in uno stato di chiusura anomala, non deve attendere un'indicazione VD_E_CLOSE da GetCommand e può richiamare ClientVirtualDeviceSet::Close appena terminano le attività sui buffer condivisi.
Per altre informazioni, vedere "Chiusura anomala" più indietro in questo documento.

## <a name="clientvirtualdevicesetopeninsecondary"></a>ClientVirtualDeviceSet::OpenInSecondary

**Scopo** Questa funzione apre il set di dispositivi virtuali in un client secondario. Il client primario deve avere già usato Create e GetConfiguration per configurare il set di dispositivi virtuali.

**Sintassi** 
   
   ```
   int ClientVirtualDeviceSet::OpenInSecondary (
   char *   setName         // name of the set
   );
   ```

| Parametri | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**setName** |Identifica il set. Questo nome fa distinzione tra maiuscole e minuscole e deve corrispondere al nome usato dal client primario per richiamare ClientVirtualDeviceSet::Create.

| Valori restituiti | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**NOERROR** |Funzione completata.
| |**VD_E_PROTOCOL** |Il set di dispositivi virtuali non è stato creato, è già stato aperto nel client oppure non è pronto ad accettare richieste di apertura da client secondari.
| |**VD_E_ABORT** |Operazione interrotta.

**Osservazioni** Quando si usa un modello a più processi, il client primario è responsabile del rilevamento della chiusura normale e anomala dei client secondari.

## <a name="clientvirtualdevicesetgetbufferhandle"></a>ClientVirtualDeviceSet::GetBufferHandle

**Scopo** Alcune applicazioni potrebbero richiedere più di un processo per operare sui buffer restituiti da ClientVirtualDevice::GetCommand. In questi casi, il processo che riceve il comando può usare GetBufferHandle per ottenere un handle indipendente dal processo che identifica il buffer. Questo handle può quindi essere comunicato a qualsiasi altro processo che ha lo stesso set di dispositivi virtuali aperto. Tale processo può quindi usare ClientVirtualDeviceSet::MapBufferHandle per ottenere l'indirizzo del buffer. L'indirizzo sarà probabilmente diverso da quello del partner, perché ogni processo può eseguire il mapping dei buffer a indirizzi diversi.

**Sintassi** 

   ```
   int ClientVirtualDeviceSet::GetBufferHandle (
   uint8_t*     pBuffer,        // in: buffer address
   unsigned int*        pBufferHandle   // out: buffer handle
   );
   ```

| Parametri | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**pBuffer** |Indirizzo di un buffer ottenuto da un comando Read o Write.
| |**BufferHandle** |Viene restituito un identificatore univoco per il buffer.

| Valori restituiti | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**NOERROR** |Funzione completata.
| |**VD_E_PROTOCOL** |Il set di dispositivi virtuali non è attualmente aperto.
| |**VD_E_INVALID** |pBuffer non è un indirizzo valido.

Osservazioni - Il processo che richiama la funzione GetBufferHandle è responsabile della chiamata a ClientVirtualDevice::CompleteCommand al termine del trasferimento dei dati.

## <a name="clientvirtualdevicesetmapbufferhandle"></a>ClientVirtualDeviceSet::MapBufferHandle

**Scopo** Questa funzione viene usata per ottenere un indirizzo del buffer valido da un handle del buffer ottenuto da un altro processo. 

**Sintassi** 

   ```
   int ClientVirtualDeviceSet::MapBufferHandle (
   i        nt  dwBuffer,   // in: buffer handle
   uint8_t**    ppBuffer        // out: buffer address
   );
   ```

| Parametri | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**dwBuffer** |Handle restituito da ClientVirtualDeviceSet::GetBufferHandle.
| |**ppBuffer** |Indirizzo del buffer valido nel processo corrente.

| Valori restituiti | Argomento | Spiegazione
| ----- | ----- | ------ |
| |**NOERROR** |Funzione completata.
| |**VD_E_PROTOCOL** |Il set di dispositivi virtuali non è attualmente aperto.
| |**VD_E_INVALID** |ppBuffer è un handle non valido.

**Osservazioni** Prestare attenzione a comunicare correttamente gli handle. Gli handle sono locali in un singolo set di dispositivi virtuali. I processi partner che condividono un handle devono assicurarsi che gli handle del buffer vengano usati solo nell'ambito del set di dispositivi virtuali da cui è stato originariamente ottenuto il buffer.


