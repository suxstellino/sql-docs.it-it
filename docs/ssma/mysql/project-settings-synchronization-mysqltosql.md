---
description: Impostazioni del progetto (sincronizzazione) (MySQLToSQL)
title: Impostazioni progetto (sincronizzazione) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 42061ff7-e6e7-497b-a0d9-440b9cf5986c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3f778b2bd361fab5ac1210005076692c813bb4b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017311"
---
# <a name="project-settings-synchronization-mysqltosql"></a>Impostazioni del progetto (sincronizzazione) (MySQLToSQL)
Le **impostazioni del progetto** di sincronizzazione consentono di configurare la modalità di sincronizzazione degli oggetti di database MySQL con SQL Server oggetti di database.  
  
Le azioni predefinite specificano le impostazioni predefinite per l'aggiornamento degli oggetti dal database MySQL e la sincronizzazione degli oggetti con il database SQL Server. Per ulteriori informazioni, vedere [Refresh from database &#40;MySQLToSQL&#41;](../../ssma/mysql/refresh-from-database-mysqltosql.md)  
  
È possibile accedere a due diverse pagine di sincronizzazione che contengono le stesse impostazioni:  
  
-   Per specificare le impostazioni per tutti i progetti SSMA futuri, scegliere **Impostazioni DefaultProject** dal menu **strumenti** e quindi fare clic su **sincronizzazione** nella parte inferiore del riquadro sinistro.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , quindi fare clic su **Sincronizza** nella parte inferiore del riquadro sinistro.  
  
## <a name="options"></a>Opzioni  
  
##### <a name="misc"></a>Varie  
  
##### <a name="attempts"></a>Tentativi  
Fornisce le informazioni sul numero di oggetti pass che devono essere caricati nel SQL Server. Il caricamento di oggetti in SQL Server viene in genere eseguito in più passaggi. Gli oggetti che non vengono caricati nel primo passaggio, ad esempio le chiavi esterne, potrebbero essere caricati correttamente nel passaggio successivo.  
  
Per impostazione predefinita, il valore è 2.  
  
## <a name="synchronization-for-mysql"></a>Sincronizzazione per MySQL  
  
##### <a name="action-on-local-and-remote-object-change"></a>Azione per la modifica di oggetti locali e remoti  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando la definizione dell'oggetto viene modificata in SSMA e nel server di database.  
  
-   Se si seleziona Aggiorna da database, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona Ignora, SSMA non eseguirà alcuna azione di aggiornamento.  
  
##### <a name="action-on-local-object-change"></a>Azione per la modifica di un oggetto locale  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando l'oggetto viene modificato in SSMA.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
##### <a name="action-on-remote-object-change"></a>Azione per la modifica di un oggetto remoto  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione in caso di modifica degli oggetti nel server di database.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
##### <a name="action-when-local-object-metadata-is-missing"></a>Azione quando mancano i metadati degli oggetti locali  
Specifica l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando i metadati locali risultano mancanti.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento  
  
## <a name="synchronization-for-sql-server"></a>Sincronizzazione per SQL Server  
  
##### <a name="action-on-local-and-remote-object-change"></a>Azione per la modifica di oggetti locali e remoti  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando la definizione dell'oggetto viene modificata in SSMA e nel server di database.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA aggiornerà gli oggetti nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
##### <a name="action-on-local-object-change"></a>Azione per la modifica di un oggetto locale  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando l'oggetto viene modificato in SSMA.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA aggiornerà l'oggetto nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
##### <a name="action-on-remote-object-change"></a>Azione per la modifica di un oggetto remoto  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione in caso di modifica degli oggetti nel server di database.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA aggiornerà l'oggetto nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
##### <a name="action-when-local-object-metadata-is-missing"></a>Azione quando mancano i metadati degli oggetti locali  
Specifica l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando mancano i metadati locali.  
  
-   Se si seleziona **Aggiorna da database**, SSMA seleziona l'opzione Aggiorna da database quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA eliminerà l'oggetto dal database quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
