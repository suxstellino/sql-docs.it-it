---
title: Comandi e parametri
description: Informazioni su come usare gli oggetti Command perché il provider di dati Microsoft SqlClient per SQL Server esegua i comandi e restituisca i risultati da un'origine dati.
ms.date: 11/25/2020
ms.assetid: b623f810-d871-49a5-b0f5-078cc3c34db6
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 07f23aa6545636fd8cbbf1717f6c8ee81ebe9294
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771473"
---
# <a name="commands-and-parameters"></a>Comandi e parametri

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Una volta stabilita una connessione a un'origine dati, è possibile eseguire i comandi e restituire i risultati dell'origine dati usando un oggetto <xref:System.Data.Common.DbCommand>. È possibile creare un comando usando uno dei costruttori di comando per il provider di dati Microsoft SqlClient per SQL Server. I costruttori possono accettare argomenti facoltativi, ad esempio un'istruzione SQL da eseguire nell'origine dati, un oggetto <xref:System.Data.Common.DbConnection> o un oggetto <xref:System.Data.Common.DbTransaction>.

È inoltre possibile configurare tali oggetti come proprietà del comando, nonché creare un comando per una particolare connessione usando il metodo <xref:System.Data.Common.DbConnection.CreateCommand%2A> di un oggetto `DbConnection`. È possibile configurare l'istruzione SQL eseguita dal comando tramite la proprietà <xref:System.Data.Common.DbCommand.CommandText%2A>. Il provider di dati Microsoft SqlClient per SQL Server include l'oggetto <xref:Microsoft.Data.SqlClient.SqlCommand>.

## <a name="in-this-section"></a>Contenuto della sezione

[Esecuzione di un comando](execute-command.md)  
Viene descritto l'oggetto `Command` di ADO.NET e viene illustrato come usarlo per eseguire query e comandi su un'origine dati.

[Configurazione dei parametri](configure-parameters.md)  
Viene descritto l'uso dei parametri di `Command`, inclusa la direzione, i tipi di dati e la sintassi.

[Generazione dei comandi con CommandBuilders](generate-commands-with-commandbuilders.md)  
Viene descritto come usare compilatori di comandi per generare automaticamente i comandi INSERT, UPDATE e DELETE per un `DataAdapter` in cui è presente il comando SELECT di una singola tabella.

[Recupero di un valore singolo da un database](obtain-single-value-from-database.md)  
Viene descritto come usare il metodo `ExecuteScalar` di un oggetto `Command` per restituire un singolo valore in una query sul database.

[Uso di comandi per modificare i dati](use-commands-to-modify-data.md)  
Descrive come usare il provider di dati Microsoft SqlClient per SQL Server per eseguire stored procedure o istruzioni Data Definition Language (DDL).

## <a name="see-also"></a>Vedere anche

- [DataAdapter e DataReader](dataadapters-datareaders.md)
- [Connessione a un'origine dati](connecting-to-data-source.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
