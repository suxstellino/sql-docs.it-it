---
description: catalog.event_message_context
title: catalog.event_message_context | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 273a54f8-b107-4f36-9461-2b475644760d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 880f17c309718522d589b0fea453d490e7d01b3f
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835260"
---
# <a name="catalogevent_message_context"></a>catalog.event_message_context 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Visualizza le informazioni sulle condizioni associate ai messaggi di evento di esecuzione, per le esecuzioni nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|Context_id|bigint|ID univoco per il contesto dell'errore.|  
|Event_message_id|bigint|ID univoco per il messaggio a cui è correlato il contesto.|  
|Context_depth|INT|Con l'aumentare della profondità, il contesto è più lontano dall'errore. Quando si verifica un errore, la profondità del contesto parte da 1. Il valore 0 indica lo stato del pacchetto prima dell'avvio dell'esecuzione.|  
|Package_path|Nvarchar(max)|Percorso del pacchetto per l'origine del contesto.|  
|Context_type|SMALLINT|Tipo dell'oggetto che rappresenta l'origine del contesto. Per un elenco di tipi di contesto, vedere la sezione **Osservazioni**.|  
|Context_source_name|Nvarchar(4000)|Nome dell'oggetto che rappresenta l'origine del contesto.|  
|Context_source_id|Nvarchar(38)|ID univoco dell'oggetto che rappresenta l'origine del contesto.|  
|Property_name|Nvarchar(4000)|Nome della proprietà associata all'origine del contesto.|  
|Property_value|Sql_variant|Valore della proprietà associata all'origine del contesto.|  
  
## <a name="remarks"></a>Osservazioni  
 Nella tabella seguente sono elencati i tipi di contesto.  
  
|Valore tipo di contesto|Nome tipo|Descrizione|  
|-|-|-|  
|10|Attività|Stato di un'attività nel momento in cui si è verificato un errore.|  
|20|Pipeline|Errore è stato generato da un componente della pipeline: componente di trasformazione, origine o destinazione.|  
|30|Sequenza|Stato di una sequenza.|  
|40|Ciclo For|Stato di un ciclo For|  
|50|Ciclo Foreach|Stato di un ciclo Foreach|  
|60|Pacchetto|Stato del pacchetto nel momento in cui si è verificato un errore.|  
|70|Variabile|Valore di variabile|  
|80|Gestione connessione|Proprietà di una gestione connessione.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa vista è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione READ per l'operazione  
  
-   Appartenenza al ruolo del database **ssis_admin**.  
  
-   Appartenenza al ruolo del server **sysadmin**.  
  
  
