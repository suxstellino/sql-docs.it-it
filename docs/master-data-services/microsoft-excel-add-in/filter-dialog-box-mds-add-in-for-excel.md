---
description: Finestra di dialogo Filtro (componente aggiuntivo MDS per Excel)
title: Finestra di dialogo Filtro
ms.custom: microsoft-excel-add-in
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b987b141-5abf-4161-a073-4cfc3e7f5aae
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e650bd924e893b21cbc6b154887d9cd6e66565b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352776"
---
# <a name="filter-dialog-box-mds-add-in-for-excel"></a>Finestra di dialogo Filtro (componente aggiuntivo MDS per Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Nel [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]usare la finestra di dialogo **Filtro** per limitare l'elenco di dati gestiti da MDS prima di caricarlo in Excel.  
  
 Questa finestra di dialogo contiene tre sezioni: **Colonne**, **Righe** e **Riepilogo**.  
  
## <a name="columns"></a>Colonne  
 Usare la sezione **Colonne** per determinare quali attributi (colonne) visualizzare in Excel.  
  
|Nome del controllo|Descrizione|  
|------------------|-----------------|  
|Tipo di attributo|Un tipo di attributo descrive il tipo di membri che si desidera utilizzare. Nella maggior parte dei casi, si tratta di **Foglia**. Per altre informazioni sui tipi di membro, vedere [Membri &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md).|  
|Gerarchia esplicita|Se si sceglie il tipo di attributo **Consolidato** , scegliere la gerarchia alla quale appartengono i membri consolidati. Per altre informazioni, vedere [Gerarchie esplicite &#40;Master Data Services&#41;](../../master-data-services/explicit-hierarchies-master-data-services.md).|  
|Gruppi di attributi|I gruppi di attributi consentono il raggruppamento di subset di attributi. Scegliere un gruppo di attributi se si desidera mostrare un subset di attributi disponibili. Per altre informazioni sui gruppi di attributi, vedere [Gruppi di attributi &#40;Master Data Services&#41;](../../master-data-services/attribute-groups-master-data-services.md).|  
|Seleziona tutto|Fare clic per selezionare tutti gli attributi visualizzati nell'elenco.|  
|Cancella tutto|Fare clic per cancellare gli attributi selezionati visualizzati nell'elenco.<br /><br /> Non è possibile deselezionare **Nome** e **Codice**.|  
|Freccia in giù, freccia in su|Consente di spostare l'attributo selezionato verso l'alto e verso il basso nell'elenco. L'ordine dall'alto verso il basso corrisponde all'ordine da sinistra a destra con cui le colonne vengono visualizzate nel foglio di lavoro.|  
  
## <a name="rows"></a>Righe  
 Usare la sezione **Righe** per determinare i membri (righe) da visualizzare in Excel. È possibile eseguire questa operazione definendo i criteri per filtrare le righe che saranno visualizzate.  
  
|Nome del controllo|Descrizione|  
|------------------|-----------------|  
|Attributo|Visualizza un attributo in base al quale si desidera filtrare. Se non è elencato alcun attributo, è perché non sono stati aggiunti attributi.<br /><br /> Nota: è possibile filtrare in base agli attributi che non si intende mostrare nel foglio di lavoro.|  
|Operatore|Visualizza gli operatori che corrispondono al tipo di attributo selezionato. Per altre informazioni, vedere [Operatori di filtro &#40;Master Data Services&#41;](../../master-data-services/filter-operators-master-data-services.md).|  
|Criteri|I criteri in base ai quali si desidera filtrare.|  
|Aggiornare il riepilogo|Quando si usano set di dati di grandi dimensioni, fare clic per aggiornare la sezione **Riepilogo** con i dettagli sulla quantità di dati che verrà caricata.|  
|Add|Quando si fa clic su un attributo nella sezione **Colonne** , quindi su **Aggiungi**, un attributo viene aggiunto all'elenco di filtri.|  
|Rimuovi tutto|Rimuove tutti i filtri dall'elenco.|  
|Rimuovi|Rimuove il filtro selezionato dall'elenco.|  
  
## <a name="summary"></a>Riepilogo  
 Usare la sezione **Riepilogo** per visualizzare i dettagli sulla quantità di dati che verrà caricata, prima di caricarla.  
  
|Nome del controllo|Descrizione|  
|------------------|-----------------|  
|Modello|Nome del modello.|  
|Versione|Il nome della versione.|  
|Entità|Il nome dell'entità.|  
|Righe|Il numero di righe che verrà caricato in Excel, in base ai filtri applicati nella sezione **Righe** .|  
|Colonne|Il numero di colonne che verrà caricato in Excel, in base agli attributi selezionati nella sezione **Colonne** .|  
  
## <a name="see-also"></a>Vedere anche  
 [Filtrare i dati prima dell'esportazione di &#40;Componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)   
 [Panoramica: Esportazione dei dati in Excel &#40;componente aggiuntivo MDS per Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
  
