---
description: MSSQLSERVER_3989
title: MSSQLSERVER_3989
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 3989 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 11862d9f21556dd5039024c225b58f5f0f05f9a4
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797764"
---
# <a name="mssqlserver_3989"></a>MSSQLSERVER_3989
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|3989|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|XACT_UNSUPPORT_PARALLEL_TRAN3|
|Testo del messaggio|Impossibile avviare la nuova richiesta perché deve disporre di un descrittore di transazione valido.|
||

## <a name="explanation"></a>Spiegazione

Questo errore si verifica quando si esegue una query distribuita che unisce più tabelle ospitate da istanze remote di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mentre l'impostazione della sessione di `XACT_ABORT` è **ON**. Viene visualizzato all'utente un messaggio di errore simile al seguente:

> Messaggio 3989, livello 16, stato 1, riga #  
Impossibile avviare la nuova richiesta perché deve disporre di un descrittore di transazione valido.

## <a name="cause"></a>Causa

Esistono alcune limitazioni di progettazione nel modo in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestisce le query distribuite quando sono soddisfatte le condizioni seguenti:

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unisce più tabelle di un'origine dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] remota.
- La sessione che esegue la query non è integrata in una transazione distribuita.

In questa situazione un tentativo di eseguire la query può generare uno dei due errori indicati nella sezione **Spiegazione**.

## <a name="user-action"></a>Azione utente

Per risolvere il problema, racchiudere la query distribuita in un'istruzione BEGIN DISTRIBUTED TRANSACTION:

```sql
BEGIN DISTRIBUTED TRANSACTION <Distributed Query> COMMIT TRANSACTION
```
