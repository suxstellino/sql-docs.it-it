---
description: Funzionalità ADO per ogni versione
title: Cronologia ADO | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, what's new
ms.assetid: 667673f2-3151-432b-894a-3fc60b704ea4
author: rothja
ms.author: jroth
ms.openlocfilehash: 7247523daf68116c949d4ec8ad3eec90fe4c8fe2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030619"
---
# <a name="ado-features-for-each-release"></a>Funzionalità ADO per ogni versione

In questo argomento vengono elencate le nuove funzionalità introdotte da ogni versione di ADO, ADO MD e ADOX.

## <a name="ado-60"></a>ADO 6,0

ADO 6,0 è incluso in Windows Vista, come parte di Windows Data Access Components (Windows DAC) 6,0. ADO 6,0 è funzionalmente equivalente a ADO 2,8.

## <a name="ado-28"></a>ADO 2,8

ADO 2,8 è stato incluso in Windows XP e Windows Server 2003 come parte di Microsoft Data Access Components (MDAC) 2,8. È disponibile anche una versione ridistribuibile di MDAC 2,8; Si noti che questa versione ridistribuibile deve essere installata solo in Windows 2000. ADO 2,8 risolve diversi problemi relativi alla sicurezza:

*L'accesso al disco rigido non è consentito all'esterno di una zona attendibile.*
Nello scripting tra domini che coinvolgono siti non attendibili, le operazioni seguenti sono disabilitate: **Stream. SaveToFile**, **Stream. LoadFromFile**, **Recordset. Save** e **Recordset. Open**, usati insieme al flag **adCmdFile** o con il provider di persistenza Microsoft OLE DB (MSPersist).

**Recordset. Open** _,_  **Recordset. Save** _,_  **Stream. SaveToFile** _e_  **Stream. LoadFromFile**  _funzionano solo su file fisici._
Questi metodi ora verificano che gli handle di file puntino solo ai file fisici.

**Recordset. ActiveCommand**  _restituisce un errore quando viene richiamato da una pagina HTML/ASP._
In questo modo si impedisce che l'oggetto **comando** venga usato in modo improprio.

_Il numero di_**Recordset**_restituiti da un comando Shape annidato__ha un limite superiore._        
Un comando Shape annidato ora restituisce un massimo di 512 **Recordset**. Ciò significa che non è più possibile annidare un comando **Shape** a una profondità. Al contrario, la profondità massima del livello è 512, se ogni comando restituisce un singolo **Recordset**(figlio). Se, a qualsiasi livello, un comando **Shape** restituisce più **Recordset**, il livello massimo di profondità sarà inferiore a 512.

## <a name="ado-27"></a>ADO 2,7

*supporto della piattaforma a 64 bit* ADO 2,7 introduce il supporto per i processori a 64 bit.

## <a name="ado-26"></a>ADO 2,6

Il _Metodo_ **CubDef. GetSchemaObject** che inizia con ADO 2,6, ADO MD gli oggetti possono essere recuperati usando nomi univoci, come specificato dalla [Proprietà UniqueName (ADO MD)](../reference/ado-md-api/uniquename-property-ado-md.md).   Non è necessario che i nomi degli oggetti padre siano noti e non è necessario popolare le raccolte padre per recuperare un oggetto dello schema. Vedere [Metodo GetSchemaObject (ADO MD)](../reference/ado-md-api/getschemaobject-method-ado-md.md).

*Flussi di comandi* L'oggetto **Command** supporta i comandi in formato flusso come alternativa all'utilizzo della proprietà **CommandText** . La [Proprietà CommandStream (ADO)](../reference/ado-api/commandstream-property-ado.md) può essere utilizzata per specificare modelli o updategram XML come input del **comando** con il provider di Microsoft OLE DB per SQL Server.

Dialetto _Proprietà_ **dialetto** 
 [](../reference/ado-api/dialect-property.md) è una nuova proprietà che definisce la sintassi e le regole generali utilizzate dal provider per analizzare la stringa o il flusso.  

**Command.Exe**  _Metodo_ carino il [metodo Execute](../reference/ado-api/execute-method-ado-command.md) dell'oggetto **comando** ADO è stato migliorato in modo da usare i flussi per l'input e l'output.

*Campo statusvalues* Se si verifica un errore DB_E_ERRORSOCCURRED durante la modifica di un **campo** di un **Recordset**, ADO compilerà la proprietà **Field. status** con le informazioni di stato appropriate, in modo che l'utente disponga di ulteriori informazioni su ciò che si è verificato. Vedere [proprietà Status (campo ADO)](../reference/ado-api/status-property-ado-field.md).

La proprietà **namedParameters** 
 [namedParameters](../reference/ado-api/namedparameters-property-ado.md) è una nuova proprietà dell'oggetto **Command** che indica che il provider deve utilizzare parametri denominati.  

*Set di risultati nei flussi* ADO può restituire set di risultati da un'origine dati in un **flusso**, anziché un oggetto **Recordset** . Utilizzando la versione più recente del provider di Microsoft OLE DB per SQL Server, è possibile ottenere risultati XML dal provider eseguendo una query "for XML". Un **flusso** che riceve il set di risultati può essere aperto con un comando "for XML" come origine. Vedere [recupero di set di risultati in flussi](./data/retrieving-resultsets-into-streams.md).

*Set di risultati a riga singola* È ora possibile aprire l'oggetto **record** ADO su una stringa di comando o un oggetto **comando** che restituisce una riga di dati dal provider. Ciò comporta un miglioramento delle prestazioni con i provider MDAC 2,6. Vedere [metodo Open (record ADO)](../reference/ado-api/open-method-ado-record.md).

## <a name="ado-25"></a>ADO 2,5

L' _oggetto_ **record** ADO 2,5 introduce l'oggetto **record** per rappresentare e gestire una riga da un **Recordset** o da un provider di dati oppure da un oggetto che incapsula i dati semistrutturati, ad esempio un file o una directory.

L' _oggetto_ **flusso** ADO 2,5 introduce anche l'oggetto Stream * * per rappresentare *un flusso di* dati binari o di testo.

*Binding URL* ADO 2,5 introduce l'uso di un URL, in alternativa a una stringa di connessione e un testo del comando, per assegnare un nome agli oggetti dell'archivio dati. È possibile utilizzare un URL con la **connessione** esistente e gli oggetti **Recordset** , nonché con i nuovi oggetti **record** e **flusso** .

*Provider di dati che supportano l'associazione URL* ADO 2,5 supporta provider OLE DB che riconoscono gli schemi URL. Sono inclusi OLE DB provider per Internet Publishing, che accede al file system di Windows 2000 e riconosce lo schema HTTP esistente.
