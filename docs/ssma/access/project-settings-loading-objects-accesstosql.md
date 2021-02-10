---
description: Impostazioni progetto (caricamento oggetti) (AccessToSQL)
title: Impostazioni progetto (caricamento oggetti) (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9ec1c1e8-a3e1-4e81-bf49-631f87daa209
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a2d119ac2c58d0bea25a6f21ba4bf97cb47a8421
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066366"
---
# <a name="project-settings-loading-objects-accesstosql"></a>Impostazioni progetto (caricamento oggetti) (AccessToSQL)
Le impostazioni del progetto caricamento oggetti consentono di configurare la modalità di sincronizzazione degli oggetti di database di Access con SQL Server oggetti di database.  
  
Le azioni predefinite specificano le impostazioni predefinite per l'aggiornamento degli oggetti dal database di Access e la sincronizzazione degli oggetti con il database SQL Server. Per ulteriori informazioni, vedere [Refresh from Database &#40;AccessToSQL&#41;](../../ssma/access/refresh-from-database-accesstosql.md)  
  
È possibile accedere a due diverse pagine di sincronizzazione che contengono le stesse impostazioni:  
  
-   Per specificare le impostazioni per tutti i progetti SSMA futuri, scegliere **Impostazioni DefaultProject** dal menu **strumenti** e quindi fare clic su **carica oggetti** nella parte inferiore del riquadro sinistro.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , quindi fare clic su **carica oggetti** nella parte inferiore del riquadro sinistro.  
  
## <a name="options"></a>Opzioni  
  
##### <a name="misc"></a>Varie  
  
##### <a name="attempts"></a>Tentativi  
Fornisce le informazioni sul numero di oggetti pass che devono essere caricati nel SQL Server. Il caricamento di oggetti in SQL Server viene in genere eseguito in più passaggi. Gli oggetti che non vengono caricati nel primo passaggio, ad esempio le chiavi esterne, potrebbero essere caricati correttamente nel passaggio successivo.  
  
Per impostazione predefinita, il valore è 2.  
  
## <a name="synchronization-for-sql-server"></a>Sincronizzazione per SQL Server  
  
##### <a name="default-action-on-local-and-remote-object-change"></a>Azione predefinita per la modifica di oggetti locali e remoti  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando la definizione dell'oggetto viene modificata in SSMA e nel server di database.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA aggiornerà gli oggetti nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
##### <a name="default-action-on-local-object-change"></a>Azione predefinita per la modifica di un oggetto locale  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando l'oggetto viene modificato in SSMA.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA aggiornerà l'oggetto nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
##### <a name="default-action-on-remote-object-change"></a>Azione predefinita per la modifica dell'oggetto remoto  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione in caso di modifica degli oggetti nel server di database.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA aggiornerà l'oggetto nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
##### <a name="default-action-when-local-object-metadata-is-missing"></a>Azione predefinita quando mancano i metadati degli oggetti locali  
Specifica l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando mancano i metadati locali.  
  
-   Se si seleziona **Aggiorna da database**, SSMA seleziona l'opzione Aggiorna da database quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA eliminerà l'oggetto dal database quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
