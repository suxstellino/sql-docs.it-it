---
description: Gestione degli errori in ADO
title: Gestione degli errori | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- reporting errors [ADO]
- errors [ADO]
- ADO, error handling
ms.assetid: 4909e413-f3b0-4183-8ad3-67b1434df742
author: rothja
ms.author: jroth
ms.openlocfilehash: 003c8412bdc9865290be9cb506cca41c2df358ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033221"
---
# <a name="error-handling-in-ado"></a>Gestione degli errori in ADO
ADO utilizza diversi metodi per notificare a un'applicazione gli errori che si verificano. In questa sezione vengono illustrati i tipi di errori che possono verificarsi quando si utilizza ADO e il modo in cui l'applicazione riceve una notifica. Concludendo con suggerimenti su come gestire tali errori.  
  
## <a name="how-does-ado-report-errors"></a>In che modo ADO segnala gli errori?  
 ADO invia notifiche sugli errori in diversi modi:  
  
-   Gli errori ADO generano un errore in fase di esecuzione. Gestire un errore ADO allo stesso modo di qualsiasi altro errore di run-time, ad esempio l'utilizzo di un'istruzione **On Error** in Visual Basic.  
  
-   Il programma può ricevere errori dal OLE DB. Un errore di OLE DB genera anche un errore in fase di esecuzione.  
  
-   Se l'errore è specifico del provider di dati, uno o più oggetti **Error** vengono inseriti nella raccolta **Errors** dell'oggetto **Connection** utilizzato per accedere all'archivio dati quando si è verificato l'errore.  
  
-   Se anche il processo che ha generato un evento ha generato un errore, le informazioni sull'errore vengono inserite in un oggetto **Error** e passate come parametro all'evento. Per ulteriori informazioni sugli eventi, vedere [gestione degli eventi ADO](./handling-ado-events.md) .  
  
-   I problemi che si verificano durante l'elaborazione degli aggiornamenti batch o di altre operazioni bulk che coinvolgono un **Recordset** possono essere indicati dalla proprietà **status** del **Recordset**. Ad esempio, le violazioni dei vincoli dello schema o le autorizzazioni insufficienti possono essere specificate dai valori **RecordStatusEnum** .  
  
-   I problemi che si verificano che coinvolgono un particolare **campo** del record corrente sono indicati anche dalla proprietà **status** di ogni **campo** nella raccolta **Fields** del **record** o del **Recordset**. Ad esempio, gli aggiornamenti che non possono essere completati o tipi di dati incompatibili possono essere specificati dai valori **FieldStatusEnum** .  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Errori ADO](./ado-errors.md)  
  
-   [Errori del provider](./provider-errors.md)  
  
-   [Informazioni sugli errori correlati ai campi](./field-related-error-information.md)  
  
-   [Informazioni sugli errori correlati ai recordset](./recordset-related-error-information.md)  
  
-   [Gestione degli errori in altri linguaggi](./handling-errors-in-other-languages.md)  
  
-   [Prevenzione degli errori](./anticipating-errors.md)