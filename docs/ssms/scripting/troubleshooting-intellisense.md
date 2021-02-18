---
title: Identificare i problemi di IntelliSense (SSMS)
description: Informazioni sui casi in cui IntelliSense in SQL Server Management Studio (SSMS) non funziona come previsto.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- unavailable options [IntelliSense]
- IntelliSense [SQL Server], troubleshooting
- IntelliSense [SQL Server], unavailable options
- troubleshooting [IntelliSense]
ms.assetid: 4b72ffc6-aea2-4e11-ab36-fa2de4d7bcc5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a054f2807bb4ae0c8ea61a81aae56548df75834e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351131"
---
# <a name="identify-issues-with-intellisense---sql-server-management-studio-ssms"></a>Identificare i problemi di IntelliSense - SQL Server Management Studio (SSMS)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  In alcuni casi le opzioni di IntelliSense potrebbero non funzionare nel modo previsto.  
  
## <a name="conditions-that-affect-intellisense"></a>Condizioni che influiscono su IntelliSense  
 Le condizioni seguenti potrebbero influire sul comportamento di IntelliSense:  
  
-   È presente un errore del codice sopra il cursore.  
  
     Se esiste un'istruzione incompleta o un altro errore del codice sopra la posizione del punto di inserimento, IntelliSense potrebbe non essere in grado di analizzare gli elementi del codice e quindi non funzionare. Per attivarlo di nuovo, è possibile impostare come commento il codice pertinente.  
  
-   Il punto di inserimento si trova all'interno di un commento di codice.  
  
     Le opzioni di IntelliSense non sono disponibili quando il punto di inserimento si trova all'interno di un commento nel file di origine.  
  
-   Il punto di inserimento si trova all'interno di un valore letterale stringa.  
  
     Le opzioni di IntelliSense non sono disponibili quando il punto di inserimento si trova all'interno delle virgolette che racchiudono un valore letterale stringa, come illustrato nell'esempio seguente:  
  
     `WHERE FirstName LIKE 'Patri%|'`  
  
-   Le opzioni automatiche sono disattivate.  
  
     Per impostazione predefinita, molte caratteristiche di IntelliSense operano automaticamente ma possono essere disabilitate.  
  
     È possibile utilizzarle anche se il completamento automatico delle istruzioni è disabilitato. Per altre informazioni, vedere [Configurare IntelliSense &#40;SQL Server Management Studio&#41;](./configure-intellisense-sql-server-management-studio.md).  
  
## <a name="database-engine-query-intellisense"></a>Funzionalità IntelliSense in query del Motore di database  
 I problemi descritti di seguito sono relativi all'editor di query del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] :  
  
-   La funzionalità IntelliSense dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] non supporta tutti gli elementi della sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] . La Guida relativa ai parametri non supporta i parametri in alcuni oggetti, ad esempio nelle stored procedure estese. Per altre informazioni, vedere [Sintassi Transact-SQL supportata da IntelliSense](./transact-sql-syntax-supported-by-intellisense.md).  
  
-   IntelliSense è disponibile solo quando l'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] è connesso a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versione successiva. IntelliSense non è disponibile quando l'editor di query è connesso a versioni precedenti del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   IntelliSense è disattivato nell'editor di query di [!INCLUDE[ssDE](../../includes/ssde-md.md)] quando è impostata la modalità SQLCMD.  
  
-   La funzionalità IntelliSense non può essere utilizzata per oggetti di database creati da un'altra connessione dopo che la finestra dell'editor si è connessa al database. Se nelle caratteristiche di IntelliSense mancano oggetti, ad esempio gli elenchi di completamento, è possibile scegliere uno di questi tre meccanismi per aggiornare la cache degli oggetti per la finestra dell'editor:  
  
    -   Scegliere **IntelliSense** dal menu **Modifica**, quindi fare clic su **Aggiorna cache locale**.  
  
    -   Utilizzare i tasti di scelta rapida CTRL+MAIUSC+R.  
  
    -   Disconnettere la finestra dell'editor dall'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] ed effettuare una nuova connessione.  
  
-   Gli elenchi di completamento non includono oggetti di database per cui non si dispone di autorizzazioni. Tramite IntelliSense vengono contrassegnati i riferimenti a oggetti per i quali si dispone di autorizzazioni. Se, ad esempio, se si apre uno script scritto da un altro utente, qualsiasi riferimento a oggetti per i quali l'autore dispone di autorizzazioni e l'utente che apre lo script no viene contrassegnato come non corretto.  
  
-   Gli elenchi di completamento potrebbero smettere di funzionare se si perde la connessione all'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Connettersi di nuovo all'istanza.  
  
