---
title: Parametri dei comandi (OLE DB Driver) | Microsoft Docs
description: Informazioni sui parametri dei comandi, inclusi i tipi supportati da OLE DB Driver per SQL Server per l'istruzione SQL e i comandi di chiamata di procedura.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- parameters [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, parameters
- OLE DB Driver for SQL Server, commands
- parameters [OLE DB Driver for SQL Server], OLE DB
- commands [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a36e123a61cc31e80eb667aab51082870f7970de
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737841"
---
# <a name="command-parameters"></a>Parametri dei comandi
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  I parametri sono contrassegnati nel testo dei comandi dal carattere del punto interrogativo. L'istruzione SQL seguente è ad esempio contrassegnata per un solo parametro di input:  
  
```  
{call SalesByCategory('Produce', ?)}  
```  
  
 Per migliorare le prestazioni riducendo il traffico di rete, il driver OLE DB per SQL Server non deduce automaticamente le informazioni sui parametri, a meno che non venga chiamato **ICommandWithParameters::GetParameterInfo** o **ICommandPrepare::Prepare** prima di eseguire un comando. Ciò significa che OLE DB Driver per SQL Server non esegue automaticamente le operazioni seguenti:  
  
-   Verificare la correttezza del tipo di dati specificato con **ICommandWithParameters::SetParameterInfo**.  
  
-   Eseguire il mapping tra il valore DBTYPE specificato nelle informazioni relative all'associazione della funzione di accesso e il tipo di dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] corretto per il parametro.  
  
 Se si specificano tipi di dati non compatibili con il tipo di dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] del parametro, è possibile che vengano ricevuti errori o che si verifichi una perdita della precisione con entrambi i metodi.  
  
 Per evitare che ciò si verifichi, è necessario:  
  
-   Assicurarsi che *pwszDataSourceType* corrisponda al tipo di dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per il parametro se si specifica **ICommandWithParameters::SetParameterInfo** a livello di codice.  
  
-   Assicurarsi che il valore DBTYPE associato al parametro sia dello stesso tipo del tipo di dati [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per il parametro in caso di specifica di una funzione di accesso a livello di codice.  
  
-   Codificare l'applicazione per chiamare **ICommandWithParameters::GetParameterInfo** in modo che il provider possa ottenere dinamicamente i tipi di dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dei parametri. Si noti che ciò determina un round trip aggiuntivo del server.  
  
> [!NOTE]  
>  Il provider non supporta la chiamata a **ICommandWithParameters::GetParameterInfo** per le istruzioni UPDATE o DELETE di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] contenenti una clausola FROM, per le istruzioni SQL dipendenti da una sottoquery contenente parametri, per le istruzioni SQL che contengono marcatori di parametro in entrambe le espressioni di un predicato di confronto, LIKE o quantificato oppure per le query in cui uno dei parametri è il parametro di una funzione. In caso di elaborazione di un batch di istruzioni SQL, il provider non supporta inoltre la chiamata a **ICommandWithParameters::GetParameterInfo** per i marcatori di parametro nelle istruzioni dopo la prima istruzione del batch. I commenti (/* \*/) non sono consentiti nel comando [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 OLE DB Driver per SQL Server supporta i parametri di input nei comandi delle istruzioni SQL. Nei comandi delle chiamate a procedure OLE DB Driver per SQL Server supporta parametri di input, di output e di input/output. I valori dei parametri di output vengono restituiti all'applicazione in fase di esecuzione (solo se non sono stati restituiti set di righe) o quando tutti i set di righe restituiti vengono esauriti dall'applicazione. Per assicurarsi che i valori restituiti siano validi, usare **IMultipleResults** per applicare l'utilizzo dei set di righe.  
  
 I nomi dei parametri delle stored procedure non devono essere specificati in una struttura DBPARAMBINDINFO. Usare NULL per il valore del membro *pwszName* per indicare che il driver OLE DB per SQL Server deve ignorare il nome del parametro e deve usare solo l'ordinale specificato nel membro *rgParamOrdinals* di **ICommandWithParameters::SetParameterInfo**. Se il testo del comando contiene parametri denominati e senza nome, tutti i parametri senza nome devono essere specificati prima di quelli denominati.  
  
 Se si specifica il nome del parametro di una stored procedure, il driver OLE DB per SQL Server ne verifica la validità. OLE DB Driver per SQL Server restituisce un errore quando riceve un nome di parametro errato dal consumer.  
  
> [!NOTE]  
>  Per esporre il supporto per i tipi definiti dall'utente e XML di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il driver OLE DB per SQL Server implementa una nuova interfaccia [ISSCommandWithParameters](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Comandi](../../oledb/ole-db-commands/commands.md)  
  
  
