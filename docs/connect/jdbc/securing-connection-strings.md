---
title: Protezione delle stringhe di connessione
description: Informazioni su come proteggere le informazioni della stringa di connessione quando si usa JDBC Driver per SQL Server.
ms.custom: ''
ms.date: 03/31/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 69ce8557-5260-4ea4-81b8-d0c5481f0868
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c9d2744eb4845855d6cb33a15e6a3fd440545d22
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177072"
---
# <a name="securing-connection-strings"></a>Protezione delle stringhe di connessione

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

La protezione dell'accesso all'origine dati è uno degli obiettivi più importanti di un'applicazione. Per limitare l'accesso all'origine dati, è necessario adottare alcune precauzioni per garantire la protezione delle informazioni di connessione quali ID utente, password e nome dell'origine dei dati. L'archiviazione di un ID utente e della password in testo normale, ad esempio nel codice sorgente, comporta un problema serio di protezione. Anche se si fornisce una versione compilata del codice contenente le informazioni su ID utente e password in un'origine esterna, il codice compilato potrebbe essere disassemblato e l'ID utente e la password risultare quindi esposti. Di conseguenza, è fondamentale che le informazioni critiche, ad esempio un ID utente e una password, non esistano nel codice.

## <a name="recommendations"></a>Consigli

Si consiglia di non archiviare la password insieme all'URL di connessione nel codice sorgente dell'applicazione. ma piuttosto in un file distinto con accesso limitato. L'accesso al file può essere concesso in base al contesto in cui viene eseguita l'applicazione.

Un altro metodo è l'archiviazione della password crittografata in un file. Assicurarsi di usare un'API di crittografia che non richiede che la chiave venga archiviata in un punto e non derivi dalla password di un utente. Ad esempio, è consigliabile valutare l'opportunità di utilizzare coppie di chiavi pubbliche/private basate sui certificati oppure un metodo in cui le due parti utilizzano un protocollo di chiave concordata (algoritmo di Diffie-Hellman) per generare le stesse chiavi private per la crittografia senza mai dover trasmettere la chiave privata.

Se si accettano le informazioni sulla stringa di connessione da un'origine esterna, ad esempio un utente che fornisce un ID utente e una password, è necessario convalidare qualsiasi input dall'origine per assicurarsi che segua il formato corretto e non contenga parametri aggiuntivi che influiscono sulla connessione.

## <a name="see-also"></a>Vedere anche

[Protezione delle applicazioni del driver JDBC](securing-jdbc-driver-applications.md)
