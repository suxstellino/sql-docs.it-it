---
description: CDC Splitter
title: Barra di divisione CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.cdcsplitter.f1
ms.assetid: 167bc5c6-fa36-439d-987c-b20acd1a77e2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9029051054c1fcec0e082da2e4e5883e0ff72375
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048786"
---
# <a name="cdc-splitter"></a>CDC Splitter

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La barra di divisione CDC suddivide un singolo flusso di righe delle modifiche da un flusso di dati dell'origine CDC in diversi flussi di dati per operazioni di inserimento, aggiornamento ed eliminazione. Il flusso di dati viene suddiviso in base alla colonna obbligatoria `__$operation` e ai relativi valori standard nelle tabelle delle modifiche di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
|Valore dell'operazione|Output|Descrizione|  
|------------------------|------------|-----------------|  
|1|Delete|Riga eliminata|  
|2|Insert|Riga inserita (non disponibile quando si usa la modalità CDC **Net with Merge** (Rete con unione))|  
|3|Aggiornamento|Riga prima dell'aggiornamento (disponibile solo quando si usa la modalità CDC **All with Old Values** (Tutto con valori precedenti))|  
|4|Aggiornamento|Riga dopo l'aggiornamento (segue l'operazione prima dell'aggiornamento)|  
|5|Aggiornamento|Unione della riga (disponibile solo quando si usa la modalità CDC **Net with Merge** (Rete con unione))|  
|Altri|Errore||  
  
 È possibile utilizzare la barra di divisione per connettersi ad output INSERT, DELETE e UPDATE predefiniti per l'ulteriore elaborazione.  
  
 La trasformazione CDC Splitter include un input normale e un output degli errori.  
  
## <a name="error-handling"></a>Gestione degli errori  
 La trasformazione CDC Splitter include un output degli errori. Le righe di input con un valore non valido della colonna $operation vengono considerate errate e vengono gestite in base alla proprietà **ErrorRowDisposition** dell'input.  
  
 L'output degli errori del componente include le colonne di output seguenti:  
  
-   **Error Code**: impostare su 1.  
  
-   **Error Column**(Colonna errore): colonna di origine che provoca l'errore (per gli errori di conversione).  
  
-   **Error Row Columns**: colonne di input della riga che ha provocato l'errore.  
  
## <a name="configuring-the-cdc-splitter"></a>Configurazione della barra di divisione CDC  
 Non sono disponibili proprietà configurabili per la barra di divisione CDC.  
  
 Per ulteriori informazioni sull'utilizzo della barra di divisione CDC, vedere Componenti CDC per Microsoft SQL Server Integration Services.  
  
 La finestra di dialogo **Editor avanzato** contiene le proprietà che è possibile impostare a livello di codice.  
  
 Per aprire la finestra di dialogo **Editor avanzato** :  
  
-   Nella schermata **Flusso di dati** del progetto di [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] fare clic con il pulsante destro del mouse sulla barra di divisione CDC e scegliere **Visualizza editor avanzato**.  
  
## <a name="see-also"></a>Vedere anche  
 [Indirizzare il flusso CDC in base al tipo di modifica](../../integration-services/data-flow/direct-the-cdc-stream-according-to-the-type-of-change.md)  
  
  
