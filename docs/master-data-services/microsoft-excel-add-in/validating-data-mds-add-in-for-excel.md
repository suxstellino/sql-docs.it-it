---
description: Convalida dei dati (componente aggiuntivo MDS per Excel)
title: Convalida dei dati
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 71eda98f-01a4-4fff-8246-be3133782523
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 27e6cc619206c108e17aa62c907cdea083526b48
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352963"
---
# <a name="validating-data-mds-add-in-for-excel"></a>Convalida dei dati (componente aggiuntivo MDS per Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Nel [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]quando si pubblicano i dati, vengono eseguiti due tipi di convalida:  
  
-   Tutte le regole business definite sono applicate ai dati.  
  
-   I dati vengono controllati in base ai valori di attributo consentiti (ad esempio il numero di caratteri o il tipo di dati).  
  
 In ogni caso, i dati validi vengono pubblicati nel repository MDS. I dati non validi vengono evidenziati e i dettagli dell'errore possono essere visualizzati nelle colonne di stato.  
  
## <a name="when-validation-occurs"></a>Esecuzione della convalida  
 In la [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] convalida viene eseguita quando si pubblicano dati nuovi o modificati oppure quando si applicano manualmente regole business.  
  
 Quando le regole business hanno esito negativo, i dati vengono comunque pubblicati nel repository MDS. Quando la convalida dell'input ha esito negativo, i dati non vengono pubblicati nel repository.  
  
## <a name="validation-statuses"></a>Stati di convalida  
 Nel [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]sono possibili gli stati di convalida seguenti.  
  
 Per informazioni su ulteriori stati, vedere [Stati di convalida &#40;Master Data Services&#41;](../../master-data-services/validation-statuses-master-data-services.md)  
  
|Stato|Descrizione|  
|------------|-----------------|  
|Convalida non riuscita|Si è verificato un errore di convalida di uno o più valori nella riga in base alle regole business definite da un amministratore di MDS.|  
|Convalida completata|Tutti i valori nella riga sono stati convalidati in base alle regole business.|  
  
## <a name="input-statuses"></a>Stati di input  
 Nel [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]sono possibili gli stati di input seguenti  
  
|Stato|Descrizione|  
|------------|-----------------|  
|Errore|Uno o più valori nella riga non soddisfano i requisiti di sistema, come la lunghezza o il tipo di dati. Il valore non è aggiornato nel repository MDS.|  
|Nuova riga|I valori nella riga non sono ancora stati pubblicati nel repository MDS.|  
|Sola lettura|L'utente che ha eseguito l'accesso dispone di autorizzazioni di sola lettura per uno o più valori nella riga e non è possibile aggiornare il valore o i valori.|  
|Non modificato|Nessun valore nella riga è stato modificato nel foglio di lavoro. Ciò non significa che i valori nel repository non sono stati modificati. Per ottenere i dati più recenti nel foglio, fare clic su **Carica o aggiorna** nel gruppo **Connetti e carica**.<br /><br /> Questa è l'impostazione predefinita per ogni riga.|  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Determinare quali valori non passano le regole business definite.|[Applicare le regole di business &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/apply-business-rules-mds-add-in-for-excel.md)|  
|Per correggere gli errori di convalida, visualizzare tutte le transazioni eseguite per un membro.|[Visualizzare tutte le annotazioni o transazioni per un membro &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/view-all-annotations-or-transactions-for-a-member-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   [Panoramica: Importazione di dati da Excel &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
