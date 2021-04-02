---
title: Protezione delle applicazioni del driver JDBC
description: Questi articoli descrivono alcune problematiche di sicurezza comuni, incluse le stringhe di connessione, la convalida dell'input utente e la sicurezza generale dell'applicazione.
ms.custom: ''
ms.date: 03/31/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 90724ec6-a9cb-43ef-903e-793f89410bc0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 29bb22bccceab099c648cb07a2ca3199f5846f58
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177096"
---
# <a name="securing-jdbc-driver-applications"></a>Protezione delle applicazioni del driver JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Migliorare la sicurezza di un' [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] applicazione è fondamentale. La sicurezza prevede più di evitare trappole comuni per il codice. Un'applicazione che accede ai dati ha molti potenziali punti di errore che possono essere sfruttati da un utente malintenzionato. Gli errori di sicurezza possono consentire agli utenti malintenzionati di recuperare, modificare o eliminare definitivamente i dati sensibili. È importante comprendere tutti gli aspetti della sicurezza dell'applicazione. Dal processo di modellazione delle minacce durante la fase di progettazione alla distribuzione finale e continuando con la manutenzione continuativa.

Negli articoli di questa sezione vengono descritte alcune problematiche di sicurezza comuni, incluse le stringhe di connessione, la convalida dell'input dell'utente e la sicurezza generale dell'applicazione.

## <a name="in-this-section"></a>Contenuto della sezione

| Articolo | Descrizione |
| ------- | ----------- |
| [Protezione delle stringhe di connessione](securing-connection-strings.md) | Vengono descritte le tecniche per la protezione delle informazioni utilizzate per la connessione a un'origine dati. |
| [Convalida dell'input utente](validating-user-input.md) | Vengono descritte le tecniche per la convalida dell'input utente. |
| [Sicurezza delle applicazioni](application-security.md) | Viene descritto come utilizzare le autorizzazioni relative ai criteri Java per proteggere un'applicazione del driver JDBC. |
| [Uso della crittografia](using-ssl-encryption.md) | Viene descritto come stabilire un canale di comunicazione sicuro con un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite Transport Layer Security (TLS), noto in precedenza come Secure Sockets Layer (SSL). |
| [Modalità FIPS](fips-mode.md) | Viene descritto come utilizzare il driver JDBC in modalità conforme a FIPS. |
  
## <a name="see-also"></a>Vedere anche

[Panoramica del driver JDBC](overview-of-the-jdbc-driver.md)
