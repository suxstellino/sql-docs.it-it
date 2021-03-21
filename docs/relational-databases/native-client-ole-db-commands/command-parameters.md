---
description: Parametri di comando (provider OLE DB Native Client)
title: Parametri di comando (provider OLE DB Native Client) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- parameters [SQL Server Native Client]
- SQL Server Native Client OLE DB provider, parameters
- SQL Server Native Client OLE DB provider, commands
- parameters [SQL Server Native Client], OLE DB
- commands [OLE DB]
ms.assetid: 072ead49-ebaf-41eb-9a0f-613e9d990f26
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4ff31ddd371504192e844aa8b597839a63b6ceb5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749491"
---
# <a name="sql-server-native-client-command-parameters"></a>Parametri del comando SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  I parametri sono contrassegnati nel testo dei comandi dal carattere del punto interrogativo. L'istruzione SQL seguente è ad esempio contrassegnata per un solo parametro di input:  
  
```  
{call SalesByCategory('Produce', ?)}  
```  
  
 Per migliorare le prestazioni riducendo il traffico di rete, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client non deriva automaticamente le informazioni sui parametri, a meno che non venga chiamato **ICommandWithParameters:: GetParameterInfo** o **ICommandPrepare::P repare** prima di eseguire un comando. Ciò significa che il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client non esegue automaticamente le operazioni seguenti:  
  
-   Verificare la correttezza del tipo di dati specificato con **ICommandWithParameters::SetParameterInfo**.  
  
-   Eseguire il mapping tra il valore DBTYPE specificato nelle informazioni relative all'associazione della funzione di accesso e il tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corretto per il parametro.  
  
 Se si specificano tipi di dati non compatibili con il tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del parametro, è possibile che vengano ricevuti errori o che si verifichi una perdita della precisione con entrambi i metodi.  
  
 Per evitare che ciò si verifichi, è necessario:  
  
-   Assicurarsi che *pwszDataSourceType* corrisponda al tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per il parametro se si specifica **ICommandWithParameters::SetParameterInfo** a livello di codice.  
  
-   Assicurarsi che il valore DBTYPE associato al parametro sia dello stesso tipo del tipo di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per il parametro in caso di specifica di una funzione di accesso a livello di codice.  
  
-   Codificare l'applicazione per chiamare **ICommandWithParameters::GetParameterInfo** in modo che il provider possa ottenere dinamicamente i tipi di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dei parametri. Si noti che ciò determina un round trip aggiuntivo del server.  
  
> [!NOTE]  
>  Il provider non supporta la chiamata a **ICommandWithParameters::GetParameterInfo** per le istruzioni UPDATE o DELETE di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenenti una clausola FROM, per le istruzioni SQL dipendenti da una sottoquery contenente parametri, per le istruzioni SQL che contengono marcatori di parametro in entrambe le espressioni di un predicato di confronto, LIKE o quantificato oppure per le query in cui uno dei parametri è il parametro di una funzione. In caso di elaborazione di un batch di istruzioni SQL, il provider non supporta inoltre la chiamata a **ICommandWithParameters::GetParameterInfo** per i marcatori di parametro nelle istruzioni dopo la prima istruzione del batch. I commenti (/* \*/) non sono consentiti nel comando [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client supporta i parametri di input nei comandi dell'istruzione SQL. Nei comandi di chiamata di routine il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client supporta i parametri di input, di output e di input/output. I valori dei parametri di output vengono restituiti all'applicazione in fase di esecuzione (solo se non sono stati restituiti set di righe) o quando tutti i set di righe restituiti vengono esauriti dall'applicazione. Per assicurarsi che i valori restituiti siano validi, usare **IMultipleResults** per applicare l'utilizzo dei set di righe.  
  
 I nomi dei parametri delle stored procedure non devono essere specificati in una struttura DBPARAMBINDINFO. Utilizzare NULL per il valore del membro *pwszName* per indicare che il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client deve ignorare il nome del parametro e utilizzare solo l'ordinale specificato nel membro *rgParamOrdinals* di **ICommandWithParameters::** SetValue. Se il testo del comando contiene parametri denominati e senza nome, tutti i parametri senza nome devono essere specificati prima di quelli denominati.  
  
 Se viene specificato il nome di un parametro di stored procedure, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider OLE DB di Native Client verifica il nome per verificare che sia valido. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client restituisce un errore quando riceve un nome di parametro errato dal consumer.  
  
> [!NOTE]  
>  Per esporre il supporto per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i tipi XML e definiti dall'utente (UDT), il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client implementa una nuova interfaccia [ISSCommandWithParameters](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md) .  
  
## <a name="see-also"></a>Vedere anche  
 [Comandi](../../relational-databases/native-client-ole-db-commands/commands.md)  
  
  
