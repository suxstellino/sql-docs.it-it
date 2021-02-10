---
description: Uso di ADO per Internet Publishing
title: Utilizzo di ADO per la pubblicazione Internet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, Internet publishing
- publishing to Internet [ADO]
- Internet publishing [ADO]
- urls [ADO]
ms.assetid: d399fce4-b70b-418f-8110-3deb3448863c
author: rothja
ms.author: jroth
ms.openlocfilehash: f7affce9c8596b9155f691ccf2ef61f9f0eca35d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032273"
---
# <a name="using-ado-for-internet-publishing"></a>Uso di ADO per Internet Publishing
[Il Provider OLE DB per Internet Publishing](../../../ado/guide/data/the-ole-db-provider-for-internet-publishing.md) Mostra un esempio specifico di accesso ai dati eterogenei con ADO. Sebbene gli esempi in questa sezione siano specifici dell'utilizzo del provider di pubblicazione Internet, i principi illustrati dovrebbero essere simili quando si utilizza ADO con altri provider per dati eterogenei, ad esempio un provider per un archivio di posta elettronica.  
  
## <a name="urls"></a>URL  
 I localizzatori di risorse (URL) uniformi possono essere usati come alternativa alle stringhe di connessione e al testo del comando per specificare le origini dati e il percorso di file e directory. È possibile utilizzare gli URL con la [connessione](../../../ado/reference/ado-api/connection-object-ado.md) esistente e gli oggetti **Recordset** e con gli oggetti **record** e **flusso** .  
  
 Per ulteriori informazioni sull'utilizzo degli URL, vedere [URL assoluto e relativo](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="record-fields"></a>Campi di record  
 La differenza tra dati eterogenei e dati omogenei consiste nel fatto che per il primo, ogni riga di dati, o **record**, può avere un set di colonne o **campi** diverso. Per i dati omogenei, ogni riga ha lo stesso set di colonne. Per ulteriori informazioni sui campi specifici del provider di pubblicazione Internet, vedere [record e Provider-Supplied campi aggiuntivi](../../../ado/guide/data/records-and-provider-supplied-fields.md).  
  
### <a name="appending-new-fields"></a>Aggiunta di nuovi campi  
 Diversi oggetti ADO sono stati migliorati per interagire con gli oggetti **record** e **flusso** .  
  
-   Il metodo [Append](../../../ado/reference/ado-api/append-method-ado.md) della raccolta [Fields](../../../ado/reference/ado-api/fields-collection-ado.md) , che crea e aggiunge un oggetto [campo](../../../ado/reference/ado-api/field-object.md) alla raccolta, può specificare anche il valore del **campo**.  
  
-   Il metodo [Update](../../../ado/reference/ado-api/update-method.md) finalizza l'aggiunta o l'eliminazione di campi nella raccolta.  
  
-   Come collegamento alternativo al metodo **Append** , è possibile creare campi assegnando un valore a un campo non definito o eliminato in precedenza.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Provider OLE DB per Internet Publishing](../../../ado/guide/data/the-ole-db-provider-for-internet-publishing.md)  
  
-   [Scenario di Internet Publishing](../../../ado/guide/data/internet-publishing-scenario.md)  
  
-   [URL relativi e assoluti](../../../ado/guide/data/absolute-and-relative-urls.md)  
  
-   [Record e campi specificati dal provider](../../../ado/guide/data/records-and-provider-supplied-fields.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto record (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Oggetto Stream (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)   
 [Cronologia di ADO](../../../ado/guide/ado-history.md)
