---
description: Connettersi al database SQL di Azure (SybaseToSQL)
title: Connettersi al database SQL di Azure (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 96538007-1099-40c8-9902-edd07c5620ee
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 93d1901cd5701cbdea537b947aa93de64fe59661
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056716"
---
# <a name="connect-to-azure-sql-database--sybasetosql"></a>Connettersi al database SQL di Azure (SybaseToSQL)
Usare la finestra di dialogo Connetti a database SQL di Azure per connettersi al database di database SQL di Azure di cui si vuole eseguire la migrazione.  
  
Per accedere a questa finestra di dialogo, scegliere **Connetti al database SQL di Azure** dal menu **file** . Se è già stata effettuata la connessione, il comando viene **riconnesso al database SQL di Azure.**  
  
## <a name="options"></a>Opzioni  
**Nome server**  
  
Selezionare o immettere il nome del server per la connessione al database SQL di Azure.  
  
**Database**  
  
Selezionare, immettere o **esplorare** il nome del database.  
  
> [!IMPORTANT]  
> SSMA per Sybase non supporta la connessione al database master nel database SQL di Azure.  
  
**Nome utente**  
  
Immettere il nome utente che SSMA userà per connettersi al database del database SQL di Azure  
  
**Password**  
  
Immettere il nome utente e la password  
  
**Encrypt**  
  
SSMA consiglia la connessione crittografata al database SQL di Azure.  
  
## <a name="create-azure-database"></a>Creare un database di Azure  
Se non sono presenti database nell'account del database SQL di Azure, è possibile creare il primo database.  
  
Per creare un nuovo database per la prima volta, attenersi alla procedura seguente:  
  
1.  Fare clic sul pulsante Sfoglia presente nella finestra di dialogo Connetti a database SQL di Azure  
  
2.  Se non sono presenti database, vengono visualizzate le due voci di menu seguenti.  
  
    1.  **(nessun database trovato)** disabilitato e disattivato per sempre  
  
    2.  **Creare un nuovo database** che è abilitato solo quando non sono presenti database nell'account del database SQL di Azure. Quando si fa clic su questa voce di menu, la finestra di dialogo Crea database di Azure è disponibile con nome e dimensioni del database.  
  
3.  Al momento della creazione del database, i due parametri seguenti vengono forniti come input:  
  
    1.  **Nome database:** Immettere il nome del database.  
  
    2.  **Dimensioni database:** Selezionare le dimensioni del database che è necessario creare nell'account del database SQL di Azure.  
  
