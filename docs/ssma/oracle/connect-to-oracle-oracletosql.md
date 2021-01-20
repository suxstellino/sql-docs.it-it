---
title: Connettersi a Oracle (OracleToSQL) | Microsoft Docs
description: Informazioni su come connettersi a un database Oracle per avviare la migrazione tramite SSMA per Oracle. Utilizzare la finestra di dialogo Connetti a Oracle.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 23a48cb6-ff30-49bb-b4a7-603ebcab336f
ms.author: alexiva
ms.openlocfilehash: 8e13b9ea8bcd264d1668cf0979dc2956bd11bef7
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596725"
---
# <a name="connect-to-oracle-oracletosql"></a>Connettersi a Oracle (OracleToSQL)

Utilizzare la finestra di dialogo **Connetti a Oracle** per connettersi al database Oracle di cui si desidera eseguire la migrazione.

Per accedere a questa finestra di dialogo, scegliere **Connetti a Oracle** dal menu **file** . Se è già stata effettuata la connessione, il comando viene **riconnesso a Oracle**.

## <a name="options"></a>Opzioni

**Provider**  
Selezionare il provider di accesso ai dati per la connessione al database Oracle. I provider disponibili sono il provider client Oracle e il provider di OLE DB. Il valore predefinito è provider client Oracle.

**Modalità**  
Selezionare la modalità standard, TNSNAME o stringa di connessione.

- In modalità standard, immettere o selezionare i valori per il provider, il nome del server, la porta del server, il SID Oracle, il nome utente e la password.
- In modalità TNSNAME immettere l'identificatore di connessione (alias TNS) del database Oracle, il nome utente e la password.
- In modalità stringa di connessione specificare una stringa di connessione.

  > [!IMPORTANT]
  > Non è consigliabile usare la modalità stringa di connessione perché il testo potrebbe includere password e viene inviato come testo non crittografato.

Il valore predefinito è la modalità standard.

**Nome server**  
Immettere il nome del server Oracle. Il nome del server predefinito corrisponde al nome del computer. Si tratta di un'opzione della modalità standard.

**Porta server**  
Se si usa un numero di porta diverso da 1521 (impostazione predefinita) per le connessioni a Oracle, immettere il numero di porta. Si tratta di un'opzione della modalità standard.

**Connetti identificatore**  
Immettere l'identificatore Oracle Connect. Si tratta dell'alias del database come definito nel file tnsnames. ora locale.

Si tratta di un'opzione della modalità TNSNAME.

**SID Oracle**  
Immettere il SID per il database. Il SID è un identificatore che distingue il database Oracle in un computer. Il SID predefinito per un database è costituito dai primi otto caratteri del nome del database.

Si tratta di un'opzione della modalità standard.

**Nome utente**  
Immettere il nome utente che SSMA utilizzerà per connettersi al database Oracle.

**Password**  
Immettere il nome utente e la password

**Stringa di connessione**  
> [!IMPORTANT]
> Non è consigliabile usare la modalità stringa di connessione perché il testo potrebbe includere password e viene inviato come testo non crittografato.

Se si utilizza la modalità stringa di connessione, immettere la stringa di connessione completa per la connessione a Oracle.

Le stringhe di connessione sono costituite da coppie di nome e valore del parametro.

- Per OLE DB informazioni sulle stringhe di connessione, vedere l'articolo relativo [provider Microsoft OLE DB per Oracle](../../ado/guide/appendixes/microsoft-ole-db-provider-for-oracle.md) in MSDN Library.

Per le stringhe di connessione SSMA, includere sempre il parametro provider. Inoltre, assicurarsi di includere il parametro della porta quando ci si connette a Oracle.

## <a name="next-steps"></a>Passaggi successivi

Il passaggio successivo del processo di migrazione consiste nel [connettersi a SQL Server](connect-to-sql-server-oracletosql.md).