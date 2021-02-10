---
title: Connettersi a Sybase (SybaseToSQL) | Microsoft Docs
description: Connettersi all'istanza di SAP ASE per avviare la migrazione usando SSMA per Sybase (SAP ASE). Utilizzare la finestra di dialogo Connetti a Sybase.
author: nahk-ivanov
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 524f95ef-10bd-497c-84ca-c06a0ae794fb
ms.author: alexiva
ms.openlocfilehash: 8d43e6a48746fbb1587327b430b67c6959731013
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064096"
---
# <a name="connect-to-sybase-sybasetosql"></a>Connettersi a Sybase (SybaseToSQL)

Utilizzare la finestra di dialogo **Connetti a Sybase** per connettersi all'istanza di Sybase Adaptive Server Enterprise (ASE) di cui si desidera eseguire la migrazione.

Per accedere a questa finestra di dialogo, scegliere **Connetti a Sybase** dal menu **file** . Se è già stata effettuata la connessione, il comando viene **riconnesso a Sybase**.

## <a name="options"></a>Opzioni

**Provider**  
Selezionare uno dei provider installati nel computer per la connessione al Server Sybase.

**Modalità**  
Selezionare la modalità di connessione standard o avanzata. In modalità standard, immettere o selezionare i valori per nome server, porta, nome utente e password. In modalità avanzata è possibile specificare una stringa di connessione.

**Nome server**  
Immettere o selezionare il nome o l'indirizzo IP del server adattivo. Il nome del server predefinito corrisponde al nome del computer. Si tratta di un'opzione della modalità standard.

**Porta server**  
Se si usa una porta non predefinita per le connessioni all'ambiente del servizio app, immettere il numero di porta. Il numero di porta predefinito è 5000. Si tratta di un'opzione della modalità standard.
  
**Nome utente**  
Immettere il nome utente usato per la connessione all'ambiente del servizio app. Si tratta di un'opzione della modalità standard.

**Password**  
Immettere il nome utente e la password Si tratta di un'opzione della modalità standard.

**Stringa di connessione**  
Immettere la stringa di connessione completa per la connessione all'ambiente del servizio app.

Le stringhe di connessione sono costituite da coppie di nome e valore del parametro. I nomi dei parametri variano a seconda del provider in uso.

**I parametri di connessione per i diversi provider sono i seguenti:**

1. Parametri di connessione per **provider di OLE DB**

   |Impostazione|Parametro Sybase 12,5|Parametro Sybase 15|
   |-----------|-------------------------|-----------------------|
   |Nome server|Nome server|Server|
   |Porta|Indirizzo porta server|Porta|
   |Nome utente|ID utente|ID utente|
   |Password|Password|Password|
   |Provider|Provider|Provider|

   Per Sybase ASE 12,5, una stringa di connessione di esempio è la seguente:

   `Server Name=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=Sybase.ASEOLEDBProvider;`

   Per Sybase ASE 15, una stringa di connessione di esempio è la seguente:

   `Server=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=ASEOLEDB;Port=5000;`

2. Parametri di connessione per il **provider ODBC**

   |Impostazione|Parametro Sybase 12.5/15|
   |-----------|-----------------------------|
   |Nome del driver|driver|
   |Nome server|Server|
   |Nome utente|UID|
   |Password|Pwd|
   |Numero porta|Porta|

   Per Sybase ASE 12,5 o 15, una stringa di connessione di esempio è la seguente:

   `driver=Adaptive Server Enterprise;Server=sybserver;uid=MyUserID;pwd=MyP@$$word;Port=5000;`

3. Parametri di connessione per il **Provider ADO.NET**

   |Impostazione|Parametro Sybase 12.5/15|
   |-----------|-----------------------------|
   |Nome server|Server|
   |Nome utente|UID|
   |Password|Pwd|
   |Numero porta|Porta|

   Di seguito è riportato un esempio della stringa di connessione per il provider ADO.NET:

   `Server=sybserver;Port=5000;uid=MyUserID;pwd=MyP@$$word;`

   Per ulteriori informazioni, vedere la documentazione dell'ambiente del servizio app.

   Si tratta di un'opzione della modalità avanzata.

## <a name="next-steps"></a>Passaggi successivi

Il passaggio successivo del processo di migrazione consiste nel [connettersi a SQL Server](connect-to-sql-server-sybasetosql.md).
