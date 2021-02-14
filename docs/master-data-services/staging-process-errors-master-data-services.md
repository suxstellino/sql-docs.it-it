---
title: Errori del processo di gestione temporanea
description: In questo articolo vengono illustrati i codici di errore aggiunti a tutti i record elaborati durante il processo di gestione temporanea in Master Data Services.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- staging process [Master Data Services], error messages
ms.assetid: 0d9be0dd-638f-4dd4-92b2-253fda655455
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 84a5a66cbb8bacb3e1df896fedfa3a051e2f4dbb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336262"
---
# <a name="staging-process-errors-master-data-services"></a>Errori del processo di gestione temporanea (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Al termine del processo di staging, per tutti i record elaborati è presente un valore nella colonna ErrorCode delle tabelle di staging. Questi valori sono elencati nella seguente tabella.  
  
|Codice|Errore|Si verifica quando/Dettagli|Si applica alla tabella|  
|----------|-----------|--------------------------|----------------------|  
|210001|Lo stesso codice membro è presente più volte nella tabella di staging.|Nel batch di gestione temporanea lo stesso codice membro è presente più volte. Il membro non è stato né creato né aggiornato.|Foglia<br /><br /> Consolidata<br /><br /> Relationship|  
|210003|I valori degli attributi fanno riferimento a un membro inesistente o inattivo.|Quando si gestiscono temporaneamente gli attributi basati su dominio, è necessario utilizzare il codice, piuttosto che il nome. Si applica a **ImportType0**, **1** e **2**.|Foglia<br /><br /> Consolidata|  
|210006|Il codice membro è inattivo.|**ImportType**  =  **1** ed è stato specificato un codice membro che non esiste.|Foglia<br /><br /> Consolidata<br /><br /> Relationship|  
|210032|Il nome della gerarchia è mancante o non valido.|La gerarchia esplicita non è stata trovata o il valore **HierarchyName** è vuoto.|Consolidata<br /><br /> Relationship|  
|210035|Poiché non esiste una regola di business per la generazione di codice, **MemberCode** è obbligatorio.|In caso di creazione o aggiornamento dei membri, **MemberCode** è sempre obbligatorio, a meno che non si usi la generazione di codice automatica. Per ulteriori informazioni, vedere [creazione automatica di codice &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).|Foglia<br /><br /> Consolidata|  
|210036|Poiché esiste una regola di business per la generazione di codice, **MemberCode** non è obbligatorio.|In caso di creazione o aggiornamento dei membri, **MemberCode** non è obbligatorio quando si usa la generazione di codice automatica. È tuttavia possibile specificare un codice. Per ulteriori informazioni, vedere [creazione automatica di codice &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).|Foglia<br /><br /> Consolidata|  
|210041|"ROOT" non è un codice membro valido.|Il valore **MemberCode** contiene la parola "root".|Foglia<br /><br /> Consolidata<br /><br /> Relationship|  
|210042|"MDMUNUSED" non è un codice membro valido.|Il valore **MemberCode** contiene la parola "MDMUNUSED".|Foglia<br /><br /> Consolidata<br /><br /> Relationship|  
|210052|MemberCode non può essere disattivato perché è utilizzato come valore di attributo basato su dominio.|Quando **ImportType**  =  **3** o **4**, la gestione temporanea ha esito negativo se il membro viene usato come valore di attributo per altri membri. Usare **ImportType5** o **6** per impostare il valore su NULL o modificare i valori prima di eseguire il processo di gestione temporanea.|Foglia<br /><br /> Consolidata|  
|300002|Il codice membro non è valido.|Relazioni: il codice membro padre o figlio non esiste.<br /><br /> Foglia o consolidata: **ImportType**  =  **3** o **4** e il codice membro non esiste.|Foglia<br /><br /> Consolidata<br /><br /> Relationship|  
|300004|Il codice membro esiste già.|**ImportType**  =  **1** ed è stato usato un codice membro già esistente nell'entità.|Foglia<br /><br /> Consolidata|  
|210011|Se **RelationshipType** è **1**, **ParentCode** non può essere un membro foglia.|Assicurarsi che il valore **ParentCode** sia un codice membro consolidato.|Relationship|  
|210015|Lo stesso codice membro è presente più volte nella tabella di staging per una gerarchia e un batch.|Per una gerarchia esplicita, si è specificata la posizione dello stesso membro più volte nello stesso batch.|Relationship|  
|210016|Impossibile creare la relazione poiché determinerebbe un riferimento circolare.|Ciò si verifica quando si tenta di assegnare un figlio come padre.|Relationship|  
|210046|Il membro non può essere di pari livello del nodo Radice.|Questo errore si verifica quando **RelationshipType**  =  **2** (di pari livello) e **ParentCode** o **ChildCode** sono **radice**. I membri non possono essere allo stesso livello del nodo Radice; possono essere solo elementi figlio.|Relationship|  
|210047|Il membro non può essere di pari livello del nodo Inutilizzato.|Ciò si verifica quando **RelationshipType**  =  **2** (di pari livello) e **ParentCode** o **ChildCode** è **inutilizzato**. I membri possono essere solo elementi figlio del nodo Inutilizzato.|Relationship|  
|210048|**ParentCode** e **ChildCode** non possono corrispondere.|Il valore **ParentCode** corrisponde al valore **ChildCode** . Questi valori devono essere differenti.|Relationship|  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare gli errori che si verificano durante la gestione temporanea &#40;Master Data Services&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)   
 [Panoramica: Importazione di dati da tabelle &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)  
  
  
