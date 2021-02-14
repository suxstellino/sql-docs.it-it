---
description: Sviluppo di un'interfaccia utente per un enumeratore Foreach personalizzato
title: Sviluppo di un'interfaccia utente per un enumeratore Foreach personalizzato | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom user interface [Integration Services], custom foreach enumerators
- custom foreach enumerators [Integration Services], developing custom user interface
ms.assetid: 8aa4aa80-c9ba-42b3-ba87-ae5ea5d3cac3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e9344b0f3192d422d61da991de6fd7c3ca71344c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350312"
---
# <a name="developing-a-user-interface-for-a-custom-foreach-enumerator"></a>Sviluppo di un'interfaccia utente per un enumeratore Foreach personalizzato

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Dopo avere eseguito l'override dell'implementazione delle proprietà e dei metodi della classe di base per fornire la funzionalità personalizzata, è possibile creare un'interfaccia utente personalizzata per l'enumeratore Foreach. Se non si crea un'interfaccia utente personalizzata, gli utenti possono configurare il nuovo enumeratore ForEach solo utilizzando la finestra delle proprietà.  
  
 In un progetto o assembly di interfaccia utente personalizzata, creare una classe che implementa <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI>. Questa classe deriva dall'oggetto System.Windows.Forms.UserControl, che viene in genere usato per creare un controllo composito per ospitare altri controlli Windows Forms. Il controllo creato viene visualizzato nell'area **Configurazione enumeratore** della scheda **Raccolta** di **Editor ciclo Foreach**.  
  
> [!IMPORTANT]  
>  Dopo aver firmato, compilato e installato l'interfaccia utente nella Global Assembly Cache, come descritto in [Compilazione, distribuzione e debug di oggetti personalizzati](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md), specificare il nome completo di questa classe nella proprietà <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute.UITypeName%2A> dell'oggetto <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute>.  
  
## <a name="coding-the-user-interface-control-class"></a>Scrittura del codice della classe del controllo interfaccia utente  
  
### <a name="initializing-the-user-interface"></a>Inizializzazione dell'interfaccia utente  
 Eseguire l'override del metodo <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI.Initialize%2A> per memorizzare nella cache i riferimenti all'oggetto host e alle raccolte di gestioni connessioni e variabili definite nel pacchetto.  
  
### <a name="setting-properties-on-the-user-interface-control"></a>Impostazione di proprietà sul controllo interfaccia utente  
 La classe UserControl, da cui deriva la classe dell'interfaccia utente, viene usata come controllo composito per ospitare altri controlli Windows Forms. Poiché questa classe ospita altri controlli, è possibile progettare l'interfaccia utente personalizzata trascinando e rilasciando controlli, disponendoli, impostando le relative proprietà e rispondendo in fase di esecuzione agli eventi in qualsiasi applicazione Windows Form.  
  
### <a name="saving-settings"></a>Salvataggio delle impostazioni  
 Eseguire l'override del metodo <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI.SaveSettings%2A> per copiare i valori selezionati dall'utente dai controlli nelle proprietà dell'enumeratore quando l'utente chiude l'editor.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di un enumeratore Foreach personalizzato](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/creating-a-custom-foreach-enumerator.md)   
 [Scrittura del codice di un enumeratore Foreach personalizzato](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/coding-a-custom-foreach-enumerator.md)  
  
  
