---
description: MSSQLSERVER_6602
title: MSSQLSERVER_6602
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 6602 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: dc40432af6994b184678414efba4dcd098dc400b
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797762"
---
# <a name="mssqlserver_6602"></a>MSSQLSERVER_6602
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|6602|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|XMLERR_PARSEERR2|
|Testo del messaggio|Descrizione dell'errore: '%.*ls'.|
||

## <a name="explanation"></a>Spiegazione

Questo errore si verifica quando si tenta di eseguire una stored procedure `sp_xml_preparedocument` in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui il contenuto del parametro `xmltext` è un documento XML complesso. Viene visualizzato all'utente un messaggio di errore simile al seguente

> Si è verificato l'errore di analisi 0x80004005 nella riga numero 1, accanto al testo XML "\<XML document sample>"  
Messaggio 6602, livello 16, stato 2, procedure sp_xml_preparedocument, riga 1  
La descrizione dell'errore è 'Errore non specificato'.

## <a name="cause"></a>Causa

Questo problema si verifica a causa di una limitazione di progettazione del parser MSXML (Msxmlsql. dll) usato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Il problema non è strettamente correlato alle dimensioni del documento XML ma alla sua struttura complessa. Una combinazione della profondità della struttura dell'elemento XML, il numero e le dimensioni degli attributi e il numero di entità all'interno degli attributi possono causare questo problema. Tuttavia, il livello di complessità necessario per raggiungere questo limite è presente in documenti XML di molti megabyte.

## <a name="user-action"></a>Azione utente

Per risolvere il problema, provare a ridurre la complessità del documento XML.

> [!NOTE]
> Prestare attenzione agli attributi di stringa singola di grandi dimensioni che contengono molte entità XML \.
