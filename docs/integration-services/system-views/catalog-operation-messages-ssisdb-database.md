---
description: catalog.operation_messages (database SSISDB)
title: catalog.operation_messages (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- catalog.operation_messages view [Integration Services]
- operation_messages view [Integration Services]
ms.assetid: 0b3cbe38-ce24-47ca-83ef-6538a5299d1a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9bc7373e302effa18247882c48f7db922f74dae9
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835216"
---
# <a name="catalogoperation_messages-ssisdb-database"></a>catalog.operation_messages (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Vengono visualizzati i messaggi registrati durante le operazioni nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|operation_message_id|**bigint**|Identificatore (ID) univoco del messaggio.|  
|operation_id|**bigint**|ID univoco dell'operazione.|  
|message_time|**datetimeoffset(7)**|Ora di creazione del messaggio.|  
|message_type|**smallint**|Tipo di messaggio visualizzato.|  
|message_source_type|**smallint**|L'ID del tipo di origine del messaggio.|  
|message|**nvarchar(max)**|Testo del messaggio.|  
|extended_info_id|**bigint**|ID di informazioni aggiuntive correlate al messaggio dell'operazione, individuato nella vista [extended_operation_info](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md).|  
  
## <a name="remarks"></a>Osservazioni  
 In questa vista viene visualizzata una riga per ogni messaggio registrato durante un'operazione nel catalogo. Il messaggio può essere generato dal server, dal processo di esecuzione del pacchetto o dal motore di esecuzione.  
  
 In questa vista vengono visualizzati i tipi di messaggio seguenti:  
  
|Valore di **message_type**|Descrizione|  
|-----------------------------|-----------------|  
|-1|Unknown|  
|120|Errore|  
|110|Avviso|  
|70|Informazioni|  
|10|Pre-convalida|  
|20|Post-convalida|  
|30|Pre-execute|  
|40|Post-execute|  
|60|Avanzamento|  
|50|StatusChange|  
|100|QueryCancel|  
|130|TaskFailed|  
|90|Diagnostic|  
|200|Personalizzato|  
|140|DiagnosticEx<br /><br /> Ogni volta che un'attività Esegui pacchetto esegue un pacchetto figlio, l'evento viene registrato. Il messaggio di evento include i valori dei parametri passati ai pacchetti figlio.<br /><br /> Il valore della colonna di messaggio per DiagnosticEx è Testo XML.|  
|400|NonDiagnostic|  
|80|VariableValueChanged|  
  
 In questa vista vengono visualizzati i tipi di origine del messaggio seguenti.  
  
|**message_source_type**|Descrizione|  
|-------------------------------|-----------------|  
|10|API della voce, ad esempio stored procedure CLR e T-SQL|  
|20|Processo esterno utilizzato per eseguire il pacchetto (ISServerExec.exe)|  
|30|Oggetti a livello di pacchetto|  
|40|Attività Flusso di controllo|  
|50|Contenitori del flusso di controllo|  
|60|Attività Flusso di dati|  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa vista è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione READ per l'operazione  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
> [!NOTE]  
>  Quando si dispone delle autorizzazioni per eseguire un'operazione nel server, si dispone anche delle autorizzazioni per visualizzare le informazioni sull'operazione. È applicata la sicurezza a livello di riga, pertanto vengono visualizzate solo le righe per le quali si dispone delle autorizzazioni per la visualizzazione.  
  
  
