---
description: Informazioni su come definire il nome e la descrizione nella creazione guidata origine dati per creare una nuova connessione ODBC a SQL Server.
title: Schermata 1 di Creazione guidata origine dati (driver ODBC per SQL Server)
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c7b60adb82915d8fb73138d194a125f478c0a039
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673945"
---
# <a name="data-source-wizard-screen-1"></a>Creazione guidata origine dati - Schermata 1

Specificare il nome e la descrizione dell'origine dati e il nome del server in cui viene eseguito SQL Server cui si connetterà l'origine dati.

## <a name="options"></a>Opzioni

### <a name="name"></a>NOME

Nome dell'origine dati utilizzato da un'applicazione ODBC quando richiede una connessione all'origine dati, ad esempio "Personale". Il nome dell'origine dati è visualizzato nella finestra di dialogo Amministrazione origine dati ODBC.

### <a name="description"></a>Descrizione

Descrizione facoltativa dell'origine dati, ad esempio "Date collaborazioni, cronologia stipendi e situazione corrente di tutti i dipendenti".

### <a name="select-or-enter-a-server-name"></a>Selezionare o immettere il nome di un server

Nome di un'istanza di SQL Server in rete. Sarà necessario specificare un server nella casella di modifica successiva.

Nella maggior parte dei casi, il driver ODBC può connettersi usando l'ordine dei protocolli predefinito e il nome del server specificato in questa casella. Usare Gestione configurazione SQL Server se si vuole creare un alias per il server o configurare librerie di rete client.

È possibile immettere "(locale)" nella casella del server quando si usa lo stesso computer in cui è presente SQL Server. L'utente può quindi connettersi all'istanza locale di SQL Server anche in caso di esecuzione di una versione non in rete di SQL Server. È possibile eseguire più istanze di SQL Server nello stesso computer. Per specificare un'istanza denominata di SQL Server, il nome del server deve essere specificato come _NomeServer_\\_NomeIstanza_.

Per ulteriori informazioni sui nomi dei server per diversi tipi di reti, vedere [accesso a SQL Server](../../../database-engine/configure-windows/logging-in-to-sql-server.md#format-for-specifying-the-name-of-sql-server).

### <a name="finish"></a>Finish

Se le informazioni specificate in questa schermata sono tutte necessarie per connettersi a SQL Server, è possibile selezionare **fine**. Per tutti gli attributi specificati nelle altre schermate della procedura guidata verranno utilizzate le impostazioni predefinite.

### <a name="next"></a>Prossima

Per passare alla schermata successiva della procedura guidata, fare clic su **Avanti**.

## <a name="next-steps"></a>Passaggi successivi

[Creazione guidata origine dati - Schermata 2](dsn-wizard-2.md)
