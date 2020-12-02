---
description: Proprietà personalizzate del file non elaborato
title: Proprietà personalizzate del file non elaborato | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 7e81f7e1-fac0-4b57-b145-8f1b9e4720bf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5b1b7f0e1416b20ce641848abe47d3497f7c7b7f
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "92196400"
---
# <a name="raw-file-custom-properties"></a>Proprietà personalizzate del file non elaborato

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  **Proprietà personalizzate delle origini**  
  
 L'origine file non elaborato include sia proprietà personalizzate che le proprietà comuni a tutti i componenti del flusso di dati.  
  
 Nella tabella seguente vengono descritte le proprietà personalizzate dell'origine file non elaborato. Tutte le proprietà sono di lettura/scrittura.  
  
|Nome proprietà|Tipo di dati|Descrizione|  
|-------------------|---------------|-----------------|  
|AccessMode|Integer (enumerazione)|Modalità utilizzata per accedere ai dati non elaborati. I valori possibili sono **Nome file** (0) e **Nome file da variabile** (1). Il valore predefinito è **Nome file** (0).|  
|FileName|string|Percorso e nome del file di origine.|  
  
 L'output e le colonne di output dell'origine file non elaborato non includono proprietà personalizzate.  
  
 Per ulteriori informazioni, vedere [Raw File Source](../../integration-services/data-flow/raw-file-source.md).  
  
 **Proprietà personalizzate delle destinazioni**  
  
 La destinazione file non elaborato include sia proprietà personalizzate che le proprietà comuni a tutti i componenti del flusso di dati.  
  
 Nella tabella seguente vengono descritte le proprietà personalizzate della destinazione file non elaborato. Tutte le proprietà sono di lettura/scrittura.  
  
|Nome proprietà|Tipo di dati|Descrizione|  
|-------------------|---------------|-----------------|  
|AccessMode|Integer (enumerazione)|Valore che specifica se la proprietà FileName include un nome file o il nome di una variabile che contiene un nome file. Le opzioni sono **Nome file** (0) e **Nome file da variabile** (1).|  
|FileName|string|Nome del file in cui la destinazione file non elaborato scrive.|  
|WriteOption|Integer (enumerazione)|Valore che specifica se la destinazione file non elaborato elimina un file esistente con lo stesso nome. Le opzioni sono **Crea sempre** (0), **Crea una sola volta** (1), **Tronca e accoda** (3) e **Accoda** (2). Il valore predefinito della proprietà è **Crea sempre** (0).|  
  
> [!NOTE]  
>  È possibile eseguire un'operazione di aggiunta solo se i metadati dei dati aggiunti corrispondono a quelli dei dati già presenti nel file.  
  
 L'input e le colonne di input della destinazione file non elaborato non includono proprietà personalizzate.  
  
 Per altre informazioni, vedere [Destinazione file non elaborato](../../integration-services/data-flow/raw-file-destination.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà comuni](./set-the-properties-of-a-data-flow-component.md)  
  
