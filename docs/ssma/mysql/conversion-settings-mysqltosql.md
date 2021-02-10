---
description: Impostazioni di conversione (MySQLToSQL)
title: Impostazioni di conversione (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f551cf6e-1575-4206-9cca-975b5b43a6b8
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 42f7d0f36f0e0adfcf067ad0bbe55c5e2c169e9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068861"
---
# <a name="conversion-settings-mysqltosql"></a>Impostazioni di conversione (MySQLToSQL)
La scheda **"Impostazioni"** consente all'utente di impostare le impostazioni a livello di nodo. La scheda sarà disponibile nei seguenti nodi della metabase:  
  
-   Nodo database  
  
-   Categoria funzioni  
  
-   Nodo Function  
  
-   Categoria tabelle  
  
-   Nodo tabella  
  
## <a name="specifications"></a>Specifiche  
Nella scheda **Impostazioni** sono disponibili due impostazioni utente, vale a dire:  
  
1.  Conversione di funzioni  
  
2.  Conversione tabella  
  
Queste impostazioni saranno disponibili in base al tipo di nodo della metabase. Ad esempio, l'impostazione relativa alla conversione di funzioni non sarà disponibile nel nodo della tabella  
  
> [!NOTE]  
> -   Le modifiche apportate dall'utente verranno salvate nell'area di lavoro progetto come file di preferenza separato.  
> -   L'estensione di questo file sarà **ccprefs**.  
  
1.  **Impostazione conversione funzione:**  
  
    1.  Questa scheda contiene l'opzione **' forza conversione della funzione '** . L'opzione può avere uno dei quattro valori seguenti:  
  
        -   Converti in base alle impostazioni del progetto [ereditato]  
  
        -   Converti sempre in una funzione  
  
        -   Converti sempre in una procedura  
  
        -   Converti in base alle impostazioni del progetto  
  
    2.  In base alle impostazioni, la funzione verrà convertita in una funzione o in una stored procedure.  
  
    3.  Le impostazioni apportate dall'utente vengono salvate nel file di preferenze a cascata facendo clic sul pulsante **applica** .  
  
2.  **Impostazione di conversione tabella:**  
  
    1.  Questa scheda contiene l'opzione **' Disattiva generazione colonna ausiliaria ROWID '** . L'opzione può avere uno dei quattro valori seguenti:  
  
        -   Converti in base alle impostazioni del progetto [ereditato]  
  
        -   Sì  
  
        -   No  
  
        -   Converti in base alle impostazioni del progetto  
  
    2.  Se **è impostata su' Yes '**, questa impostazione impedisce la creazione di una colonna ausiliaria ROWID per le tabelle di destinazione.  
  
    3.  Le impostazioni apportate dall'utente vengono salvate nel file di preferenze a cascata facendo clic sul pulsante **applica** .  
  
## <a name="see-also"></a>Vedere anche  
[Impostazioni progetto (conversione) (da MySQL a SQL)](./project-settings-conversion-mysqltosql.md)  
