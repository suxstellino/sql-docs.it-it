---
description: Connettersi al database SQL di Azure (AccessToSQL)
title: Connettersi al database SQL di Azure (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connect to SQL Azure dialog box
ms.assetid: bf44b236-d9be-41ae-a5fd-bd73038e505f
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 57fadc47d241658b96f2d9a7087bcb174d72dcce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076312"
---
# <a name="connect-to-azure-sql-database-accesstosql"></a>Connettersi al database SQL di Azure (AccessToSQL)
Usare la finestra di dialogo Connetti a SQL Azure per connettersi al database nel database SQL di Azure di cui si vuole eseguire la migrazione.  
  
Per accedere a questa finestra di dialogo, scegliere **Connetti a SQL Azure** dal menu **file** . Se è già stata effettuata la connessione, il comando viene **riconnesso a SQL Azure.**  
  
## <a name="options"></a>Opzioni  
**Nome server**  
  
Selezionare o immettere il nome del server per la connessione a SQL Azure.  
  
**Database**  
  
Selezionare, immettere o **esplorare** il nome del database.  
  
> [!IMPORTANT]  
> SSMA per Access non supporta la connessione al database master in SQL Azure.  
  
**Nome utente**  
  
Immettere il nome utente che SSMA userà per connettersi al database SQL di Azure  
  
**Password**  
  
Immettere il nome utente e la password  
  
**Encrypt**  
  
SSMA consiglia la connessione crittografata a SQL Azure.  
  
## <a name="create-database"></a>Creazione del database  
Per creare un nuovo database, attenersi alla procedura seguente:  
  
1.  fare clic sul pulsante Sfoglia presente nella finestra di dialogo Connetti a SQL Azure  
  
2.  Se non sono presenti database, vengono visualizzate due voci di menu  
  
    1.  **(nessun database trovato)** disabilitato e disattivato per sempre  
  
    2.  **Creare un nuovo database** che è sempre abilitato, consentendo all'utente di creare un nuovo database. Quando si fa clic su questa voce di menu, la finestra di dialogo Crea database è presente con il nome e le dimensioni del database.  
  
3.  Al momento della creazione del database, questi due parametri vengono forniti come input.  
  
    1.  **Nome database:** Immettere il nome del database.  
  
    2.  **Dimensioni database:** Selezionare le dimensioni del database che è necessario creare in SQL Azure account.  
  
