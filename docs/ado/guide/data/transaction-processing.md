---
description: Elaborazione delle transazioni
title: Elaborazione delle transazioni | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- transactions [ADO]
- data updates [ADO], transaction processing
- updating data [ADO], transaction processing
- nested transactions [ADO]
ms.assetid: 74ab6706-e2dc-42cb-af77-dbc58a9cf4ce
author: rothja
ms.author: jroth
ms.openlocfilehash: f534d38ea52e942a00e2e95f837b096ed6fef0f1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032383"
---
# <a name="transaction-processing"></a>Elaborazione delle transazioni
Una *transazione* delimita l'inizio e la fine di una serie di operazioni di accesso ai dati eseguite in una connessione. In conformità alle funzionalità transazionali dell'origine dati, l'oggetto **connessione** consente inoltre di creare e gestire le transazioni. Se ad esempio si utilizza il provider Microsoft OLE DB per SQL Server per accedere a un database in Microsoft SQL Server, è possibile creare più transazioni nidificate per i comandi eseguiti.  
  
 ADO garantisce che le modifiche apportate a un'origine dati risultanti da operazioni in una transazione vengano eseguite correttamente insieme.  
  
 Se si annulla la transazione o se una delle relative operazioni ha esito negativo, il risultato sarà come se nessuna delle operazioni nella transazione fosse stata eseguita. L'origine dati rimarrà come prima dell'inizio della transazione.  
  
 ADO fornisce i metodi seguenti per il controllo delle transazioni: **BeginTrans**, **CommitTrans** e **RollbackTrans**. Utilizzare questi metodi con un oggetto **Connection** quando si desidera salvare o annullare una serie di modifiche apportate ai dati di origine come singola unità. Per trasferire il denaro tra gli account, ad esempio, si sottrae un importo da uno e si aggiunge la stessa quantità all'altra. Se uno degli aggiornamenti ha esito negativo, gli account non sono più bilanciati. Apportare queste modifiche all'interno di una transazione aperta garantisce che tutte o nessuna delle modifiche venga attraversata.  
  
> [!NOTE]
>  Non tutti i provider supportano le transazioni. Verificare che la proprietà definita dal provider "**Transaction DDL**" sia presente nella raccolta [Properties](../../../ado/reference/ado-api/properties-collection-ado.md) dell'oggetto **Connection** , a indicare che il provider supporta le transazioni. Se il provider non supporta le transazioni, la chiamata a uno di questi metodi restituirà un errore.  
  
 Dopo aver chiamato il metodo **BeginTrans** , il provider non eseguirà più immediatamente il commit delle modifiche apportate fino a quando non si chiama **CommitTrans** o **RollbackTrans** per terminare la transazione.  
  
 La chiamata al metodo **CommitTrans** Salva le modifiche apportate all'interno di una transazione aperta sulla connessione e termina la transazione. La chiamata al metodo **RollbackTrans** inverte le modifiche apportate all'interno di una transazione aperta e termina la transazione. Se si chiama un metodo in assenza di una transazione aperta, viene generato un errore.  
  
 A seconda della proprietà [Attributes](../../../ado/reference/ado-api/attributes-property-ado.md) dell'oggetto **Connection** , la chiamata del metodo **CommitTrans** o **RollbackTrans** può avviare automaticamente una nuova transazione. Se la proprietà **Attributes** è impostata su **adXactCommitRetaining**, il provider avvia automaticamente una nuova transazione dopo una chiamata a **CommitTrans** . Se la proprietà **Attributes** è impostata su **adXactAbortRetaining**, il provider avvia automaticamente una nuova transazione dopo una chiamata a **RollbackTrans** .  
  
## <a name="transaction-isolation-level"></a>Livello di isolamento della transazione  
 Utilizzare la proprietà **IsolationLevel** per impostare il livello di isolamento di una transazione in un oggetto **Connection** . L'impostazione non ha effetto fino alla successiva chiamata del metodo [BeginTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md) . Se il livello di isolamento richiesto non è disponibile, il provider può restituire il livello di isolamento successivo. Per ulteriori informazioni sui valori validi, vedere la proprietà **IsolationLevel** nella Guida di riferimento per programmatori ADO.  
  
## <a name="nested-transactions"></a>Transazioni nidificate  
 Per i provider che supportano le transazioni nidificate, la chiamata al metodo **BeginTrans** all'interno di una transazione aperta avvia una nuova transazione nidificata. Il valore restituito indica il livello di annidamento: un valore restituito pari a "1" indica che è stata aperta una transazione di primo livello, ovvero che la transazione non è annidata all'interno di un'altra transazione, "2" indica che è stata aperta una transazione di secondo livello (una transazione nidificata all'interno di una transazione di primo livello) e così via. La chiamata a **CommitTrans** o **RollbackTrans** interessa solo la transazione aperta più di recente; prima di poter risolvere le transazioni di livello superiore, è necessario chiudere o eseguire il rollback della transazione corrente.
