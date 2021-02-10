---
description: Contrassegnare gli oggetti business come sicuri per lo scripting
title: Contrassegno di oggetti business come sicuri per lo scripting | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- business objects in RDS [ADO]
ms.assetid: 0be98d1a-ab3d-4dce-a166-dacda10d154a
author: rothja
ms.author: jroth
ms.openlocfilehash: 1866d72f438e263ec6cdb1f66cccb247d460a270
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031923"
---
# <a name="marking-business-objects-as-safe-for-scripting"></a>Contrassegnare gli oggetti business come sicuri per lo scripting
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Per garantire un ambiente Internet sicuro, è necessario contrassegnare tutti gli oggetti business di cui è stata creata un'istanza con [RDS. ](../../reference/rds-api/dataspace-object-rds.md) Metodo [CreateObject](../../reference/rds-api/createobject-method-rds.md) dell'oggetto DataSpace come "Safe per lo scripting". È necessario assicurarsi che siano contrassegnati come tali nell'area di licenza del registro di sistema prima di poter essere usati in DCOM.  
  
> [!NOTE]
>  È possibile creare un'istanza degli oggetti business contrassegnati come "Safe per lo scripting" o sicuri per l'inizializzazione e inizializzarli da chiunque in rete. Il contrassegno di un oggetto business "Safe per lo scripting" non lo rende sicuro. È estremamente importante verificare che gli oggetti business siano codificati con la massima sicurezza per assicurarsi che tali oggetti non presentino un punto di accesso non protetto per i dati sensibili.  
  
 Per contrassegnare manualmente l'oggetto business come sicuro per gli script, creare un file di testo con estensione reg che contenga il testo seguente. In questo esempio \<*MyActiveXGUID*> è il numero GUID esadecimale dell'oggetto business. I due numeri seguenti abilitano la funzionalità Safe-for-scripting:  
  
```console
[HKEY_CLASSES_ROOT\CLSID\<MyActiveXGUID>\Implemented   
Categories\{7DD95801-9882-11CF-9FA9-00AA006C42C4}]  
[HKEY_CLASSES_ROOT\CLSID\<MyActiveXGUID>\Implemented   
Categories\{7DD95802-9882-11CF-9FA9-00AA006C42C4}]  
```  
  
 Salvare il file e unirlo nel registro di sistema utilizzando l'editor del registro di sistema o facendo doppio clic sul file con estensione reg in Esplora risorse.  
  
 Gli oggetti business creati in Microsoft Visual Basic possono essere contrassegnati automaticamente come "sicuri per lo script" con il pacchetto e la distribuzione guidata. Quando la procedura guidata richiede di specificare le impostazioni di sicurezza, selezionare **Safe per l'inizializzazione** e **Safe per lo scripting**.  
  
 Nell'ultimo passaggio, l'installazione guidata dell'applicazione crea un file con estensione htm e CAB. È quindi possibile copiare questi due file nel computer di destinazione e fare doppio clic sul file con estensione htm per caricare la pagina e registrare correttamente il server.  
  
 Poiché l'oggetto business verrà installato nella directory Windows\System32\Occache per impostazione predefinita, spostarlo nella directory Windows\System32 e modificare la chiave del **\\** \<*MyActiveXGUID*> \\ Registro di sistemaHKEY_CLASSES_ROOT\CLSID **InprocServer32** in modo che corrisponda al percorso corretto.