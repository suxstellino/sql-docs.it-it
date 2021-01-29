---
description: Proprietà personalizzate dell'origine CDC
title: Proprietà personalizzate dell'origine CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 744e9357-94a9-4202-abe8-1d3d202697e9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: afc10a2cfaa58565ac1ea45521f2eb0370ee48c8
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049250"
---
# <a name="cdc-source-custom-properties"></a>Proprietà personalizzate dell'origine CDC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Nella tabella seguente vengono descritte le proprietà personalizzate dell'origine CDC. Tutte le proprietà sono di lettura/scrittura.  
  
|Nome proprietà|Tipo di dati|Descrizione|  
|-------------------|---------------|-----------------|  
|Connessione|ADO.Net Connection|Connessione ADO.NET al database CDC di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] per l'accesso alle tabelle delle modifiche.|  
|StateVariable|string|Variabile del pacchetto di stringhe SSIS che gestisce lo stato CDC dell'esecuzione CDC corrente.|  
|CdcProcessingMode|Integer (enumerazione)|Questa modalità determina il modo in cui viene gestita l'elaborazione. Le opzioni possibili sono **All**, **All with old values**, **Net**, **Net with update mask** e **Net with merge**.<br /><br /> Le modalità il cui nome inizia con All restituiscono tutte le modifiche, mentre quelle il cui nome inizia con Net restituiscono solo le modifiche delta.<br /><br /> Le tabelle senza una chiave primaria possono accettare solo i valori ALL.<br /><br /> **Net with Update Mask** consente di aggiungere colonne booleane con il modello di nome **__$\<column-name>\__Changed**, che indica le colonne modificate nella riga delle modifiche corrente.<br /><br /> Per altre informazioni sui valori per questa proprietà, vedere [Editor origine CDC &#40;pagina Gestione connessione&#41;](./cdc-source.md).|  
|CaptureInstance|string|Nome dell'istanza di acquisizione CDC con la tabella CDC da leggere. Una tabella di origine acquisita può contenere una o due istanze acquisite per gestire la transizione senza problemi della definizione di tabella mediante modifiche dello schema. Se per la tabella di origine in corso di acquisizione sono definite più istanze di acquisizione, selezionare l'istanza di acquisizione che si desidera utilizzare a questo punto. Il nome dell'istanza di acquisizione predefinito per una tabella [schema].[tabella] è \<schema>_\<table>, ma i nomi delle istanze di acquisizione effettivi in uso possono essere diversi. L'effettiva tabella da cui viene eseguita la lettura è la tabella CDC **cdc .\<capture-instance>_CT**.|  
|ReprocessingIndicator|Boolean|Valore che specifica se aggiungere la colonna **__$reprocessing** . Questa colonna di output speciale consente allo sviluppatore di SSIS di gestire gli errori di consistenza in modo diverso quando si utilizza l'intervallo di elaborazione iniziale.<br /><br /> Se il valore è **true**, viene aggiunta la colonna  **__$reprocessing** .<br /><br /> Il valore di questa colonna è **true** quando l'intervallo di elaborazione CDC si sovrappone all'intervallo di elaborazione iniziale (intervallo di LSN che corrisponde al periodo di caricamento iniziale) o quando un intervallo di elaborazione CDC viene rielaborato a causa di un errore in un'esecuzione precedente. Questa colonna indicatore consente agli sviluppatori di SSIS di gestire gli errori in modo diverso durante la rielaborazione delle modifiche. Azioni quali l'eliminazione di una riga non esistente e l'inserimento non riuscito su una chiave duplicata, ad esempio, possono essere ignorate.<br /><br /> Il valore predefinito è **false**.|  
|CommandTimeout|Integer|Questo valore indica il timeout (in secondi) da usare quando si comunica con il database di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . Questo valore viene utilizzato quando il tempo di risposta dal database è molto lento e il valore predefinito (30 secondi) non è sufficiente.|  
  
 Per altre informazioni sull'origine CDC, vedere [Origine CDC](../../integration-services/data-flow/cdc-source.md).  
  
