---
description: Impostazioni progetto (database SQL di Azure) (AccessToSQL)
title: Impostazioni progetto (database SQL di Azure) (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Project Settings dialog box, SQL Azure
- SQL Azure settings
ms.assetid: bbb8a204-d0e4-4f0b-9709-271feb1f136e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 6fa8b7f9940327d3a45c3b4f349892ae253f4738
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066456"
---
# <a name="project-settings-azure-sql-database-accesstosql"></a>Impostazioni progetto (database SQL di Azure) (AccessToSQL)
Le impostazioni del progetto SQL Azure consentono di configurare il suffisso del database SQL di Azure da aggiungere nella finestra di dialogo di connessione e consentire anche l'implementazione di un meccanismo di heartbeat in SQL Azure connessione.  
  
Il riquadro SQL Azure è disponibile nelle finestre di dialogo **Impostazioni progetto** e **Impostazioni progetto predefinite** .  
  
-   Utilizzare la finestra di dialogo Impostazioni progetto per impostare le opzioni di configurazione per il progetto corrente. Per accedere alle impostazioni di SQL Azure, nel menu **strumenti** selezionare **Impostazioni progetto**, fare clic su **generale** nella parte inferiore del riquadro a sinistra e quindi selezionare **SQL Azure**.  
  
-   Utilizzare la finestra di dialogo Impostazioni progetto predefinite per impostare le opzioni di configurazione per tutti i progetti. Per accedere alle impostazioni di SQL Azure, nel menu **strumenti** selezionare **Impostazioni DefaultProject**, selezionare il tipo di progetto come "SQL Azure" nella casella combinata **versione destinazione migrazione** per accedere alle impostazioni nel riquadro SQL Azure, fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi selezionare **SQL Azure**.  
  
## <a name="options"></a>Opzioni  
  
## <a name="connectivity"></a>Connettività  
**Intervallo heartbeat**  
  
Specifica un intervallo di tempo da usare per il meccanismo di heartbeat per tenere attiva la connessione SQL Azure nel formato "minuti: secondi".  
  
**Valore predefinito**:' 4:45'  
  
Il valore deve essere specificato nel formato ' m:SS ' (ad esempio,' 4:45' o ' 0:50').  
  
**Suffisso del server SQL Azure**  
  
Specifica il suffisso del server SQL Azure  
  
**Valore predefinito**:' database.Windows.NET '.  
  
