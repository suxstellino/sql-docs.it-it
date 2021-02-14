---
description: MSSQLSERVER_17659
title: MSSQLSERVER_17659
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17659 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 5a2670de8e501bb15c1a4792060722197cbff860
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352726"
---
# <a name="mssqlserver_17659"></a>MSSQLSERVER_17659
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|17659|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|DEMO_SYSCATUPDATE|
|Testo del messaggio|la tabella di sistema con ID \% è stata aggiornata direttamente nel database con ID \% ed è possibile che non sia stata mantenuta la coerenza a livello della cache. <br/> È necessario riavviare SQL Server.|
||

## <a name="explanation"></a>Spiegazione

Questo errore indica che un oggetto di sistema è stato aggiornato direttamente. L'aggiornamento manuale delle tabelle di sistema non è supportato. Le tabelle di sistema devono essere aggiornate solo dal motore di database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rileva modifiche eseguite dall'utente sulle tabelle di sistema, viene generato l'errore 17659. Un evento analogo al seguente viene registrato nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel registro applicazioni nel Visualizzatore eventi in questo scenario.

> Nome registro: Applicazione  
Origine: MSSQLServer  
ID evento: 17659  
Categoria attività: Server  
Livello: Informazioni  
Descrizione: Avviso: la tabella di sistema con ID \% è stata aggiornata direttamente nel database con ID %d ed è possibile che non sia stata mantenuta la coerenza a livello della cache. È necessario riavviare SQL Server.

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