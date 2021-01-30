---
description: MSSQLSERVER_5009
title: MSSQLSERVER_5009
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5009 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 70269ab1c31eb5b66c6853688243c872359aaddf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154280"
---
# <a name="mssqlserver_5009"></a>MSSQLSERVER_5009
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|5009|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|ALT_BADDISKS|
|Testo del messaggio|Impossibile trovare o inizializzare uno o più file elencati nell'istruzione|
||

## <a name="explanation"></a>Spiegazione

Questo errore indica che è stato specificato un nome di file o fileID nel comando ALTER DATABASE o DBCC SHRINK* che non è stato possibile risolvere.

Considerare lo scenario seguente:

- Si dispone di un database di Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che usa un modello di recupero con registrazione completa o con registrazione minima delle operazioni bulk.
- Viene aggiunto un nuovo file di dati denominato *db_file1* al database.
- Si imposta il tipo di file per il file `db_file1` come dati.
- Ci si rende conto che il tipo di file è stato specificato in modo errato.
- Si rimuove il file `db_file1` e quindi si esegue il backup del log delle transazioni per questo database.
- Si aggiunge un nuovo file di log denominato *db_file1* allo stesso database.
- Si tenta di rimuovere il file di log denominato *db_file1* usando l'istruzione ALTER DATABASE o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio.

In questo scenario si riceve un messaggio di errore simile al seguente:

> Messaggio 5009, livello 16, stato 9, riga 1 Impossibile trovare o inizializzare uno o più file elencati nell'istruzione.

## <a name="possible-causes"></a>Possibili cause

Questo problema si verifica se il nome logico del file che si tenta di rimuovere non è univoco nelle tabelle del catalogo di sistema. Questo problema si verifica, ad esempio, se il file esisteva in precedenza nel database e poi è stato rimosso.

Quando si tenta di rimuovere un file con lo stesso nome logico, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenta di rimuovere il file logico eliminato. Questa condizione genera il messaggio di errore.

## <a name="user-action"></a>Azione utente

Per risolvere il problema, seguire questa procedura.

> [!NOTE]
> Questa procedura causa il riutilizzo dei valori di ID dei file.

1. Usare l'istruzione ALTER DATABASE per creare un nuovo file logico con un nome diverso e lo stesso tipo di dati. Ad esempio, denominare il file logico `different_remove_file_name` anziché `db_file1`, come nell'esempio seguente:

    ```sql
    ALTER DATABASE [DBNAME] ADD FILE ( NAME = N'different_remove_file_name',
    FILENAME = N'D:\MSSQL.1\MSSQL\DATA\db_file1.ndf', SIZE = 1MB, MAXSIZE = 1MB)
    ```

    > [!NOTE]
    > È possibile usare qualsiasi nome o percorso di file.

1. Usare l'istruzione ALTER DATABASE per rimuovere il file logico creato nel passaggio 1, come nell'esempio seguente:

    ```sql
    ALTER DATABASE [DBNAME] REMOVE FILE [different_remove_file_name]
    ```

1. Eseguire un backup del log delle transazioni del database.
1. Provare a rimuovere di nuovo il file logico denominato *db_file1*.
