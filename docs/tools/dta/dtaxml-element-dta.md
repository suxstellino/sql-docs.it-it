---
title: Elemento DTAXML (DTA)
description: Nell'utilità dta l'elemento DTAXML contiene tutti gli elementi che descrivono l'input e l'output di ottimizzazione generati da Ottimizzazione guidata motore di database.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- DTAXML element
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 9224b5ef28cd891e0dac3d5fdeb60c210f02cd71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338708"
---
# <a name="dtaxml-element-dta"></a>Elemento DTAXML (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L'elemento radice di un file di input o di output XML di Ottimizzazione guidata motore di database, **DTAXML** , contiene tutti gli elementi che descrivono l'input e l'output di ottimizzazione generati da Ottimizzazione guidata motore di database.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
<DTAXML   
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/dta">  
    ...code removed here...  
</DTAXML>  
```  
  
## <a name="element-attributes"></a>Attributi elemento  
  
|Attributo|Descrizione|  
|---------------|-----------------|  
|**xmlns:xsi**|Obbligatorio. Identifica lo spazio dei nomi di istanze di XML Schema. Gli attributi da questo spazio dei nomi sono utilizzati per fare riferimento allo schema utilizzato per convalidare il file XML di Ottimizzazione guidata motore di database.<br /><br /> Valore obbligatorio: [http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)|  
|**xmlns**|Obbligatorio. Identifica lo spazio dei nomi di Ottimizzazione guidata motore di database.<br /><br /> Se il file XML di Ottimizzazione guidata motore di database viene modificato utilizzando l'editor XML in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], questo valore è utilizzato da F1 Guida e Guida dinamica per individuare i possibili argomenti di riferimento nella documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Valore obbligatorio:<br /><br /> Spazio dei nomi[XML Schema di Ottimizzazione guidata motore di database](https://go.microsoft.com/fwlink/?LinkId=43100)|  
  
## <a name="element-characteristics"></a>Caratteristiche elemento  
  
|Caratteristica|Descrizione|  
|--------------------|-----------------|  
|**Tipo di dati e lunghezza**|No.|  
|**Valore predefinito**|No.|  
|**Occorrenza**|Obbligatorio una volta per ogni file DTA XML.|  
  
## <a name="element-relationships"></a>Relazioni elemento  
  
|Relazione|Elementi|  
|------------------|--------------|  
|**Elemento padre**|nessuno|  
|**Elementi figlio**|[Elemento DTAInput &#40;DTA&#41;](../../tools/dta/dtainput-element-dta.md)<br /><br /> Elemento **DTAOutput**. Per informazioni, vedere [Database Engine Tuning Advisor XML schema](https://schemas.microsoft.com/sqlserver/) (Schema XML dell'Ottimizzazione guidata motore di database).|  
  
## <a name="remarks"></a>Osservazioni  
 Per ulteriori informazioni sugli spazi dei nomi XML, vedere [spazi dei nomi in un documento XML](/dotnet/standard/data/xml/managing-namespaces-in-an-xml-document). 
  
## <a name="example"></a>Esempio  
 Per esempi di elementi **DTAXML** tipici, vedere [Esempi di file di input XML &#40;DTA&#41;](../../tools/dta/xml-input-file-samples-dta.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento ai file di input XML &#40;Ottimizzazione guidata motore di database&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)   
 [Avviare e usare Ottimizzazione guidata motore di database](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)  
  
