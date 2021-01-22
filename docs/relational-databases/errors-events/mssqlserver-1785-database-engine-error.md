---
description: MSSQLSERVER_1785
title: MSSQLSERVER_1785
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 1785 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 7f300583da4c034da2963590c81e0aedbed86beb
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596266"
---
# <a name="mssqlserver_1785"></a>MSSQLSERVER_1785
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|1785|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|CRTFKINVTOPO|
|Testo del messaggio|L'introduzione del vincolo FOREIGN KEY '%.ls' nella tabella '%. ls' può determinare la creazione di cicli o più percorsi di propagazione. Specificare ON DELETE NO ACTION o ON UPDATE NO ACTION oppure modificare gli altri vincoli FOREIGN KEY.|
||

## <a name="explanation"></a>Spiegazione

Questo messaggio di errore viene generato perché in SQL Server una tabella non può essere visualizzata più di una volta in un elenco di tutte le azioni referenziali di propagazione avviate da un'istruzione `DELETE` o `UPDATE`. L'albero delle azioni referenziali di propagazione deve avere un solo percorso a una determinata tabella nell'albero delle azioni referenziali di propagazione.

Viene segnalato all'utente un messaggio di errore simile al seguente:

> Server:  Messaggio 1785, livello 16, stato 1, riga 1 - L'introduzione del vincolo FOREIGN KEY 'fk_two' nella tabella 'table2' può determinare la creazione di cicli o percorsi di propagazione multipli. Specificare ON DELETE NO ACTION o ON UPDATE NO ACTION oppure modificare gli altri vincoli FOREIGN KEY. Server:  Messaggio 1750, livello 16, stato 1, riga 1 - Impossibile creare il vincolo. Vedere gli errori precedenti

## <a name="user-action"></a>Azione utente

Per risolvere il problema, creare una chiave esterna con cui creare un singolo percorso di una tabella in un elenco di azioni referenziali di propagazione.

È possibile applicare l'integrità referenziale in diversi modi. L'integrità referenziale dichiarativa è la soluzione più semplice, ma è anche quella meno flessibile. Se è necessaria una maggiore flessibilità, ma si vuole comunque un livello elevato di integrità, è possibile usare i trigger.

## <a name="more-information"></a>Ulteriori informazioni

Il codice di esempio seguente è un esempio di tentativo di creazione di FOREIGN KEY che genera il messaggio di errore seguente:

```sql
USE tempdb
GO

CREATE TABLE table1 (user_ID INTEGER NOT NULL PRIMARY KEY, user_name
CHAR(50) NOT NULL)
GO

CREATE TABLE table2 (author_ID INTEGER NOT NULL PRIMARY KEY, author_name
CHAR(50) NOT NULL, lastModifiedBy INTEGER NOT NULL, addedby INTEGER NOT NULL)
GO

ALTER TABLE table2 ADD CONSTRAINT fk_one FOREIGN KEY (lastModifiedby)
REFERENCES table1 (user_ID) ON DELETE CASCADE ON UPDATE cascade
GO

ALTER TABLE table2 ADD CONSTRAINT fk_two FOREIGN KEY (addedby)
REFERENCES table1(user_ID) ON DELETE NO ACTION ON UPDATE cascade
GO
--this fails with the error because it provides a second cascading path to table2.

ALTER TABLE table2 ADD CONSTRAINT fk_two FOREIGN KEY (addedby)
REFERENCES table1 (user_ID) ON DELETE NO ACTION ON UPDATE NO ACTION
GO
-- this works.
```

### <a name="see-also"></a>Vedi anche

[Integrità referenziale di propagazione](../tables/primary-and-foreign-key-constraints.md#referential-integrity)