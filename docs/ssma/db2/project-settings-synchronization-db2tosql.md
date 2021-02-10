---
description: Impostazioni progetto (sincronizzazione) (DB2ToSQL)
title: Impostazioni progetto (sincronizzazione) (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: a5629a72-8c17-46a4-bb4d-19d51a0b98a2
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4d96e3e3caeb1c40a9f4529f8d2c0ea7e0b1f717
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072125"
---
# <a name="project-settingssynchronization-db2tosql"></a>Impostazioni progetto (sincronizzazione) (DB2ToSQL)
La pagina sincronizzazione della finestra di dialogo **Impostazioni progetto** contiene impostazioni che consentono di personalizzare il modo in cui SSMA carica e aggiorna gli oggetti di database, ad esempio tabelle e stored procedure, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Le opzioni predefinite delle azioni specificano le impostazioni predefinite per l'aggiornamento degli oggetti dal database DB2 e la sincronizzazione degli oggetti con il database SQL Server. Per ulteriori informazioni, vedere [Refresh from Database &#40;DB2ToSQL&#41;](../../ssma/db2/refresh-from-database-db2tosql.md).  
  
È possibile accedere a due diverse pagine di sincronizzazione che contengono le stesse impostazioni:  
  
-   Per specificare le impostazioni per tutti i progetti SSMA futuri, scegliere **Impostazioni progetto predefinite** dal menu **strumenti** , quindi fare clic su **Sincronizza** nella parte inferiore del riquadro sinistro.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , quindi fare clic su **Sincronizza** nella parte inferiore del riquadro sinistro.  
  
## <a name="miscellaneous-options"></a>Opzioni varie  
**Tentativi**  
Specifica il numero di tentativi che SSMA deve eseguire quando carica gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Gli oggetti che non vengono caricati nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tentativo corrente verranno ritentati fino a quando SSMA non raggiunge il numero massimo di tentativi nel processo di sincronizzazione corrente. Il valore predefinito impostato è **2**  
  
## <a name="synchronization-for-db2-options"></a>Sincronizzazione per le opzioni DB2  
**Azione per la modifica di oggetti locali e remoti**  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando la definizione dell'oggetto viene modificata in SSMA e nel server di database. Il valore predefinito impostato è **Aggiorna da database**.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
**Azione per la modifica di un oggetto locale**  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando l'oggetto viene modificato in SSMA. Il valore predefinito impostato è **Skip**.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
**Azione per la modifica di un oggetto remoto**  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione in caso di modifica degli oggetti nel server di database. Il valore predefinito impostato è **Aggiorna da database**.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
**Azione quando mancano i metadati degli oggetti locali**  
Specifica l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando mancano i metadati locali. Il valore predefinito impostato è **Aggiorna da database**.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
## <a name="synchronization-for-sql-server-options"></a>Sincronizzazione per le opzioni di SQL Server  
**Azione per la modifica di oggetti locali e remoti**  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando la definizione dell'oggetto viene modificata in SSMA e nel server di database. Il valore predefinito impostato è **Scrivi nel database**.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA aggiornerà gli oggetti nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
**Azione per la modifica di un oggetto locale**  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando l'oggetto viene modificato in SSMA. Il valore predefinito impostato è **Scrivi nel database**.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA aggiornerà l'oggetto nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
**Azione per la modifica di un oggetto remoto**  
Consente di specificare l'impostazione predefinita nella finestra di dialogo di sincronizzazione in caso di modifica degli oggetti nel server di database.  Il valore predefinito impostato è **Aggiorna da database**.  
  
-   Se si seleziona **Aggiorna da database**, SSMA caricherà le definizioni del database nei metadati quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA aggiornerà l'oggetto nel database in base al contenuto dei metadati SSMA quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
**Azione quando mancano i metadati degli oggetti locali**  
Specifica l'impostazione predefinita nella finestra di dialogo di sincronizzazione quando mancano i metadati locali. Il valore predefinito impostato è **Aggiorna da database**.  
  
-   Se si seleziona **Aggiorna da database**, SSMA seleziona l'opzione **Aggiorna da database** quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Scrivi nel database**, SSMA eliminerà l'oggetto dal database quando viene soddisfatta la condizione.  
  
-   Se si seleziona **Ignora**, SSMA non eseguirà alcuna azione di aggiornamento.  
  
