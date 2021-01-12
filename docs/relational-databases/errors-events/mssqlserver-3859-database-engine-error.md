---
description: MSSQLSERVER_3859
title: MSSQLSERVER_3859
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 3859 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 3228a21b43fd5b5ed2a23c5e13b47353e55cd028
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093276"
---
# <a name="mssqlserver_3859"></a>MSSQLSERVER_3859
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|3859|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|DBCC_CHECKCAT_DIRECT_UPDATE|
|Testo del messaggio|Avviso: il catalogo di sistema è stato aggiornato direttamente nel database con ID \%d, l'ultima volta alle ore %S_DATE|
||

## <a name="explanation"></a>Spiegazione

Questo errore indica modifiche eseguite dall'utente sulle tabelle di sistema. L'aggiornamento manuale delle tabelle di sistema non è supportato. Le tabelle di sistema devono essere aggiornate solo dal motore di database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rileva modifiche eseguite dall'utente sulle tabelle di sistema, viene generato l'errore 3859 nei due scenari seguenti:

- Scenario 1

    Un evento analogo al seguente viene registrato nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel registro applicazioni nel Visualizzatore eventi quando si avvia un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che contiene una tabella di sistema che è stata aggiornata manualmente:

    > Nome registro: Applicazione  
    Origine: ID evento MSSQLSERVER: 3859  
    Categoria attività: Server  
    Livello: Informazioni  
    Descrizione: Avviso: il catalogo di sistema è stato aggiornato direttamente nel database con ID \%d, l'ultima volta alle ore **date_time**  

- Scenario 2  

    Quando si esegue il comando `DBCC_CHECKDB` dopo l'aggiornamento manuale di una tabella di sistema, viene restituito il messaggio di avviso seguente:

    > Risultati DBCC per '**nome_database**'.  
    Messaggio 8992, livello 16, stato 1, riga 1  
    Messaggio di controllo del catalogo 3859, stato 1: Avviso: il catalogo di sistema è stato aggiornato direttamente nel database con ID \%d, l'ultima volta alle ore **date_time**.  
    CHECKDB ha trovato 0 errori di allocazione e 0 errori di consistenza nel database '**nome_db**'.  
    Esecuzione DBCC completata. Se sono stati visualizzati messaggi di errore DBCC, rivolgersi all'amministratore di sistema.

## <a name="user-action"></a>Azione utente

Per risolvere questo problema, usare uno dei metodi seguenti.

- Metodo 1

    Se è disponibile un backup pulito del database, ripristinare il database dal backup.  
    > [!NOTE]
    > Questo metodo funziona solo se il backup non presenta incoerenze nei metadati.  

- Metodo 2  

    Se non è possibile ripristinare il database da un backup, esportare i dati e gli oggetti in un nuovo database. Trasferire quindi il contenuto del database aggiornato manualmente nel nuovo database. Nota: non è possibile correggere le incoerenze nei cataloghi di sistema usando le opzioni REPAIR nei comandi DBCC CHECKDB. Per questo motivo, dato che il comando non può correggere il danneggiamento dei metadati, il comando non fornisce alcun livello di correzione consigliato.

    > [!NOTE]
    > È possibile visualizzare i dati nelle tabelle di sistema tramite le viste del catalogo di sistema.

## <a name="more-information"></a>Ulteriori informazioni

Per altre informazioni, vedere: [Tabelle di base di sistema](../system-tables/system-base-tables.md).