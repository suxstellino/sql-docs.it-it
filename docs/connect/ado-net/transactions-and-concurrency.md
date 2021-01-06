---
title: Transazioni e concorrenza
description: Descrizione di come usare il provider di dati Microsoft SqlClient per SQL Server con transazioni e concorrenza.
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 2a00ef1ec1f2f5d8ee892289021f42cb139b5a12
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771322"
---
# <a name="transactions-and-concurrency"></a>Transazioni e concorrenza

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Una transazione è costituita da un singolo comando o da un gruppo di comandi che vengono eseguiti come un pacchetto. Le transazioni consentono di combinare più operazioni in un'unica unità di lavoro. Se si verifica un problema in un determinato punto della transazione, sarà possibile annullare tutti gli aggiornamenti ripristinando la condizione antecedente all'inizio della transazione.

Per poter garantire la coerenza dei dati, una transazione deve essere conforme alle proprietà ACID, ovvero atomicità, coerenza, isolamento e durabilità. La maggior parte dei sistemi di database relazionale, ad esempio Microsoft SQL Server, supporta le transazioni fornendo funzionalità di blocco, di registrazione e di gestione delle transazioni ogni volta che un'applicazione client esegue un'operazione di aggiornamento, inserimento o eliminazione.

> [!NOTE]
> Le transazioni che implicano l'uso di più risorse possono abbassare la concorrenza se i blocchi vengono mantenuti troppo a lungo. Mantenere pertanto le transazioni per il minor tempo possibile.  

Se una transazione implica l'uso di più tabelle nello stesso database o server, è spesso preferibile usare transazioni esplicite nelle stored procedure. È possibile creare transazioni in stored procedure SQL Server usando le istruzioni Transact-SQL `BEGIN TRANSACTION`, `COMMIT TRANSACTION` e `ROLLBACK TRANSACTION`. Per ulteriori informazioni, vedere la documentazione online di SQL Server.

Per le transazioni che implicano l'uso di gestori di risorse diversi, ad esempio una transazione tra SQL Server e Oracle, è necessaria una transazione distribuita.

## <a name="in-this-section"></a>Contenuto della sezione

[Transazioni locali](local-transactions.md)  
Viene illustrato come eseguire transazioni su un database.  
  
[Transazioni distribuite](distributed-transactions.md)  
Viene descritto come eseguire transazioni distribuite in ADO.NET.  
  
[Integrazione di System.Transactions con SQL Server](system-transactions-integration-with-sql-server.md)  
Viene descritta l'integrazione di <xref:System.Transactions> con SQL Server per l'utilizzo di transazioni distribuite.  
  
[Concorrenza ottimistica](optimistic-concurrency.md) Descrizione della concorrenza ottimistica e pessimistica e di come è possibile verificare le violazioni della concorrenza.  

## <a name="see-also"></a>Vedere anche

- [Nozioni fondamentali sulle transazioni](/dotnet/framework/data/transactions/transaction-fundamentals)
- [Connessione a un'origine dati](connecting-to-data-source.md)
- [Comandi e parametri](commands-parameters.md)
- [DataAdapter e DataReader](dataadapters-datareaders.md)
- [Oggetti DbProviderFactory](dbproviderfactories.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
