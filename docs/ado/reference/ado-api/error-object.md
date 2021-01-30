---
description: Oggetto Error
title: Oggetto Error | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error
helpviewer_keywords:
- error object [ADO]
ms.assetid: a175d453-fa55-4f49-9ede-a26d83177919
author: rothja
ms.author: jroth
ms.openlocfilehash: eba5b63bc766161556779dd261393b0dfe06822d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171197"
---
# <a name="error-object"></a>Oggetto Error
Contiene informazioni dettagliate sugli errori di accesso ai dati relativi a una singola operazione che interessa il provider.  
  
## <a name="remarks"></a>Commenti  
 Tutte le operazioni che coinvolgono oggetti ADO possono generare uno o più errori del provider. Quando si verifica ogni errore, uno o più oggetti **Error** vengono inseriti nella raccolta [Errors](../../../ado/reference/ado-api/errors-collection-ado.md) dell'oggetto [Connection](../../../ado/reference/ado-api/connection-object-ado.md) . Quando un'altra operazione ADO genera un errore, la raccolta **Errors** viene cancellata e il nuovo set di oggetti **Error** viene inserito nella raccolta **Errors** .  
  
> [!NOTE]
>  Ogni oggetto **Error** rappresenta un errore specifico del provider, non un errore ADO. Gli errori ADO vengono esposti al meccanismo di gestione delle eccezioni in fase di esecuzione. In Microsoft Visual Basic, ad esempio, l'occorrenza di un errore specifico di ADO attiverà un evento **On Error** e verrà visualizzato nell'oggetto **Error** . Per un elenco completo degli errori ADO, vedere l'argomento [ErrorValueEnum](../../../ado/reference/ado-api/errorvalueenum.md) .  
  
 È possibile leggere le proprietà di un oggetto **errore** per ottenere dettagli specifici su ogni errore, inclusi i seguenti:  
  
-   Proprietà [Description](../../../ado/reference/ado-api/description-property.md) , che contiene il testo dell'errore. Si tratta della proprietà predefinita.  
  
-   Proprietà [Number](../../../ado/reference/ado-api/number-property-ado.md) , che contiene il valore **Long** Integer della costante Error.  
  
-   Proprietà di [origine](../../../ado/reference/ado-api/source-property-ado-error.md) , che identifica l'oggetto che ha generato l'errore. Questa operazione è particolarmente utile in presenza di diversi oggetti **Error** nella raccolta **Errors** che seguono una richiesta a un'origine dati.  
  
-   Proprietà [SQLSTATE](../../../ado/reference/ado-api/sqlstate-property.md) e [NativeError](../../../ado/reference/ado-api/nativeerror-property-ado.md) , che forniscono informazioni dalle origini dati SQL.  
  
 Quando si verifica un errore del provider, questo viene inserito nella raccolta **Errors** dell'oggetto **Connection** . ADO supporta la restituzione di più errori da parte di una singola operazione ADO per consentire informazioni sugli errori specifiche del provider. Per ottenere queste informazioni dettagliate sugli errori in un gestore errori, utilizzare le funzionalità di intercettazione degli errori appropriate del linguaggio o dell'ambiente che si sta utilizzando, quindi utilizzare i cicli annidati per enumerare le proprietà di ogni oggetto **errore** nella raccolta degli **errori** .  
  
> [!NOTE]
>  **Utenti Microsoft Visual Basic e VBScript** Se non è presente alcun oggetto **Connection** valido, sarà necessario recuperare le informazioni sull'errore dall'oggetto **Error** .  
  
 Analogamente a quanto avviene per i provider, ADO cancella l'oggetto **informazioni sull'errore OLE** prima di effettuare una chiamata che potrebbe potenzialmente generare un nuovo errore del provider. Tuttavia, la raccolta di **errori** nell'oggetto **connessione** viene cancellata e popolata solo quando il provider genera un nuovo errore o quando viene chiamato il metodo [Clear](../../../ado/reference/ado-api/clear-method-ado.md) .  
  
 Alcune proprietà e metodi restituiscono avvisi visualizzati come oggetti **Error** nella raccolta **Errors** ma non interrompono l'esecuzione di un programma. Prima di chiamare i metodi [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)o [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) su un oggetto [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) ; metodo [Open](../../../ado/reference/ado-api/open-method-ado-connection.md) su un oggetto **Connection** . in alternativa, impostare la proprietà [Filter](../../../ado/reference/ado-api/filter-property.md) per un oggetto **Recordset** , chiamare il metodo **Clear** sulla raccolta **Errors** . In questo modo, è possibile leggere la proprietà [count](../../../ado/reference/ado-api/count-property-ado.md) della raccolta **Errors** per verificare la presenza di avvisi restituiti.  
  
 L'oggetto **errore** non è sicuro per lo scripting.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Error](../../../ado/reference/ado-api/error-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Description, HelpContext, filelima, NativeError, Number, source e SQLState (VB)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Esempio di proprietà Description, HelpContext, filelima, NativeError, Number, source e SQLState (VC + +)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Oggetto Connection (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)   
 [Raccolta Errors (ADO)](../../../ado/reference/ado-api/errors-collection-ado.md)   
 [Appendice A: Provider](../../../ado/guide/appendixes/appendix-a-providers.md)
