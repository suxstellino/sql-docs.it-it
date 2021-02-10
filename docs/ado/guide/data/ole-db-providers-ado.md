---
description: Provider OLE DB (ADO)
title: Provider di OLE DB (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 6e0488c3-934d-4976-99dc-65c580dc7a3c
author: rothja
ms.author: jroth
ms.openlocfilehash: 7e70762a2cbaa6e2dd3d8d91b824591799605748
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032663"
---
# <a name="ole-db-providers-ado"></a>Provider OLE DB (ADO)
OLE DB definisce un set di interfacce COM che consentono alle applicazioni di accedere in modo uniforme ai dati archiviati in origini dati diverse. Questo approccio consente a un'origine dati di condividere i dati attraverso le interfacce che supportano la quantità di funzionalità DBMS appropriate per l'origine dati. Per impostazione predefinita, l'architettura a prestazioni elevate di OLE DB si basa sull'utilizzo di un modello di servizi flessibile basato su componenti. Anziché avere un numero prescritto di livelli intermedi tra l'applicazione e i dati, OLE DB richiede solo il numero di componenti necessari per eseguire una determinata attività.  
  
 Si supponga, ad esempio, che un utente desideri eseguire una query. Prendere in considerazione gli scenari seguenti:  
  
-   I dati si trovano in un database relazionale per il quale esiste attualmente un driver ODBC ma non un provider di OLE DB nativo: l'applicazione utilizza ADO per comunicare con il provider OLE DB per ODBC, che quindi carica il driver ODBC appropriato. Il driver passa l'istruzione SQL al sistema DBMS, che recupera i dati.  
  
-   I dati si trovano in Microsoft SQL Server per cui è presente un provider di OLE DB nativo: l'applicazione usa ADO per comunicare direttamente con il provider di OLE DB per Microsoft SQL Server. Non è richiesto alcun intermediario.  
  
-   I dati si trovano in Microsoft Exchange Server, per il quale è disponibile un provider OLE DB ma che non espone un motore per elaborare le query SQL: l'applicazione utilizza ADO per comunicare con il provider OLE DB per Microsoft Exchange e chiama su un componente OLE DB query processor per gestire l'esecuzione delle query.  
  
-   I dati si trovano nel file system Microsoft NTFS sotto forma di documenti: i dati sono accessibili tramite un provider di OLE DB nativo sul servizio di indicizzazione Microsoft, che indicizza il contenuto e le proprietà dei documenti nel file system per consentire ricerche di contenuto efficienti.  
  
 In tutti gli esempi precedenti, l'applicazione può eseguire query sui dati. Le esigenze dell'utente sono soddisfatte da un numero minimo di componenti. In ogni caso, i componenti aggiuntivi vengono utilizzati solo se necessario e vengono richiamati solo i componenti richiesti. Questo caricamento su richiesta di componenti riutilizzabili e condivisibile contribuisce significativamente a prestazioni elevate quando si usa OLE DB.  
  
 I provider rientrano in due categorie, ovvero quelle che forniscono i dati e i servizi. Un provider di dati possiede i propri dati e li espone in forma tabulare all'applicazione. Un provider di servizi incapsula un servizio tramite la produzione e l'utilizzo di dati, aumentando le funzionalità nelle applicazioni ADO. Un provider di servizi può anche essere definito ulteriormente come un componente del servizio, che deve funzionare insieme ad altri provider di servizi o componenti.  
  
 ADO fornisce un'interfaccia coerente e di livello superiore ai vari provider di OLE DB.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Provider di dati](./data-providers.md)  
  
-   [Provider di servizi e componenti](./service-providers-and-components.md)