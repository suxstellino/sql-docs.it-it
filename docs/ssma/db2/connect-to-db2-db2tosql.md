---
description: Connettersi a DB2 (DB2ToSQL)
title: Connettersi a DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9d485fd0-ab5d-402a-a59a-e9982a61b7de
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: c590a76696f2729bf5d1c6c5d4ca5b0242a63df6
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596730"
---
# <a name="connect-to-db2-db2tosql"></a>Connettersi a DB2 (DB2ToSQL)
Utilizzare la finestra di dialogo **Connetti a DB2** per connettersi al database DB2 di cui si desidera eseguire la migrazione.  
  
Per accedere a questa finestra di dialogo, scegliere **Connetti a DB2** dal menu **file** . Se è già stata effettuata la connessione, il comando viene **riconnesso a DB2**.  
  
## <a name="options"></a>Opzioni  
**Provider**  
Selezionare il provider di accesso ai dati per la connessione al database DB2. I provider disponibili sono il provider client DB2 e il provider di OLE DB. Il valore predefinito è provider client DB2.  
  
**Modalità**  
Selezionare la modalità standard, TNSNAME o stringa di connessione.  
  
-   In modalità standard, immettere o selezionare i valori per provider, nome server, porta server, SID DB2, nome utente e password.  
  
-   In modalità TNSNAME immettere l'identificatore di connessione (alias TNS) del database DB2, il nome utente e la password.  
  
-   In modalità stringa di connessione specificare una stringa di connessione.  
  
    > [!IMPORTANT]  
    > Non è consigliabile usare la modalità stringa di connessione perché il testo potrebbe includere password e viene inviato come testo non crittografato.  
  
Il valore predefinito è la modalità standard.  
  
**Nome server**  
Immettere il nome del server DB2. Il nome del server predefinito corrisponde al nome del computer. Si tratta di un'opzione della modalità standard.  
  
**Porta server**  
Se si usa un numero di porta diverso da 1521 (impostazione predefinita) per le connessioni a DB2, immettere il numero di porta. Si tratta di un'opzione della modalità standard.  
  
**Connetti identificatore**  
Immettere l'identificatore di DB2 Connect. Si tratta dell'alias del database come definito nel file tnsnames. ora locale.  
  
Si tratta di un'opzione della modalità TNSNAME.  
  
**SID DB2**  
Immettere il SID per il database. Il SID è un identificatore che distingue il database DB2 in un computer. Il SID predefinito per un database è costituito dai primi otto caratteri del nome del database.  
  
Si tratta di un'opzione della modalità standard.  
  
**Nome utente**  
Immettere il nome utente che SSMA utilizzerà per connettersi al database DB2.  
  
**Password**  
Immettere il nome utente e la password  
  
**Stringa di connessione**  
> [!IMPORTANT]  
> Non è consigliabile usare la modalità stringa di connessione perché il testo potrebbe includere password e viene inviato come testo non crittografato.  
  
Se si usa la modalità stringa di connessione, immettere la stringa di connessione completa per la connessione a DB2.  
  
Le stringhe di connessione sono costituite da coppie di nome e valore del parametro.  
  
-   Per OLE DB informazioni sulle stringhe di connessione, vedere l'articolo relativo [provider Microsoft OLE DB per DB2](../../ado/guide/appendixes/microsoft-ole-db-provider-for-oracle.md) in MSDN Library.  
  
Per le stringhe di connessione SSMA, includere sempre il parametro provider. Inoltre, assicurarsi di includere il parametro della porta quando ci si connette a DB2.  
