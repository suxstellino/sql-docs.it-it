---
title: " Pagina Opzioni di SQL Server - Ambiente - Avvio"
description: Opzioni (Ambiente - Pagina Avvio)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.date: 11/05/2018
ms.openlocfilehash: 6db0e4b3cf24087b9441b9e3c6b83b59cbefe48f
ms.sourcegitcommit: af64e2b8d498af26b973e86db5c00f8d72991295
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2021
ms.locfileid: "98193048"
---
# <a name="options-environment---startup-page"></a>Opzioni (Ambiente - Pagina Avvio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Usare la finestra di dialogo **Opzioni** per configurare le azioni di avvio di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], le opzioni generali di gestione della finestra e altre impostazioni generali. Scegliere **Opzioni** dal menu **Strumenti**, espandere la cartella **Ambiente** e quindi fare clic su **Avvio**.

## <a name="ui-element-list"></a>Elenco di elementi dell'interfaccia utente

**All'avvio**

Consente di selezionare l'azione che deve essere eseguita da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] all'avvio. Le opzioni disponibili sono le seguenti:

- **Apri Esplora oggetti** richiede di stabilire una connessione e quindi apre Esplora oggetti.

- **Apri nuova finestra Query** richiede di stabilire una connessione e quindi apre Editor di query SQL.

- **Apri finestra Query e Esplora oggetti** richiede di stabilire una connessione e quindi la usa per aprire sia Esplora oggetti sia Editor di query SQL.

- **Apri Esplora oggetti e Monitoraggio attività**

- **Apri ambiente vuoto** apre [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] senza visualizzare una finestra dell'editor di query SQL né connettere Esplora oggetti a un server.

**Nascondi oggetti di sistema in Esplora oggetti**

Selezionare questa casella di controllo per rimuovere database, tabelle, viste e stored procedure di sistema dalla visualizzazione dell'albero in Esplora oggetti. Le funzioni e i tipi di dati di sistema non vengono nascosti. Questa opzione si applica solo alle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e non ha alcun effetto sui server già connessi in Esplora oggetti.

## <a name="see-also"></a>Vedere anche

- [Guida sensibile al contesto delle finestre di dialogo Opzioni](options-dialog-boxes-f1-help.md)
- [Opzioni (Ambiente - pagina Generale)](options-environment-general-page.md)
