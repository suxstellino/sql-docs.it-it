---
title: Recuperare i dati binari
description: Viene descritto come recuperare dati binari o strutture di dati di grandi dimensioni usando `CommandBehavior`.`SequentialAccess` per modificare il comportamento predefinito di un oggetto `DataReader`.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 56c5a9e3-31f1-482f-bce0-ff1c41a658d0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1033a6b5394d92a45cd19d70f4dd6c250ec37b62
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744434"
---
# <a name="retrieve-binary-data"></a>Recuperare i dati binari

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Per impostazione predefinita, l'oggetto **DataReader** carica i dati in arrivo come riga non appena è disponibile una riga intera di dati. Tuttavia, è necessario gestire gli oggetti BLOB (Binary Large Object, oggetto binario di grandi dimensioni) in modo diverso, poiché è possibile che contengano gigabyte di dati che non possono risiedere in una sola riga. Il metodo **Command.ExecuteReader** dispone di un overload che accetta un argomento <xref:System.Data.CommandBehavior> per modificare il comportamento predefinito dell'oggetto **DataReader**. È possibile passare <xref:System.Data.CommandBehavior.SequentialAccess> al metodo **ExecuteReader** per modificare il comportamento predefinito di **DataReader** in modo che, invece di caricare righe di dati, carichi i dati in modo sequenziale man mano che li riceve. Si consiglia di usare questa procedura per caricare BLOB o altre strutture di dati di grandi dimensioni.

> [!NOTE]
> Quando si imposta **DataReader** per usare **SequentialAccess**, è importante prestare attenzione alla sequenza con cui si accede ai campi restituiti. Il comportamento predefinito di **DataReader**, in base al quale una riga intera viene caricata non appena è disponibile, consente di accedere ai campi restituiti in qualsiasi ordine fino a quando non viene letta la riga successiva. Quando si usa **SequentialAccess**, invece, è necessario accedere ai campi restituiti da **DataReader** in ordine. Ad esempio, se la query restituisce tre colonne, di cui la terza è un BLOB, è necessario restituire i valori del primo e del secondo campo prima di accedere ai dati BLOB nel terzo campo. Se si accede al terzo campo prima del primo o del secondo, i valori del primo o del secondo campo non saranno più disponibili. Questa situazione si verifica perché **SequentialAccess** ha modificato **DataReader** in modo da restituire i dati in sequenza e, dopo che sono stati letti da **DataReader**, i dati non sono più disponibili.

Quando si accede ai dati contenuti nel campo BLOB, usare le funzioni di accesso tipizzate **GetBytes** o **GetChars** di **DataReader**, che inseriscono dati in una matrice. Per i dati di tipo carattere, è anche possibile utilizzare **GetString**. Tuttavia, è probabile che per risparmiare risorse di sistema si preferisca non caricare un intero valore BLOB in un'unica variabile di stringa. È possibile invece specificare una determinata dimensione del buffer da restituire e una posizione iniziale per il primo byte o carattere da leggere dai dati restituiti. **GetBytes** e **GetChars** restituiranno un valore `long` che rappresenta il numero di byte o di caratteri restituiti. Se si passa una matrice null a **GetBytes** o a **GetChars**, il valore long restituito corrisponderà al numero totale di byte o di caratteri del BLOB. Facoltativamente, è possibile specificare un indice nella matrice come posizione iniziale per i dati che vengono letti.

## <a name="example"></a>Esempio

Nell'esempio seguente vengono restituiti l'identificatore e il logo dell'editore dal [server di esempio **pubs**](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs). L'identificatore dell'editore (`pub_id`) è un campo di testo è il logo è un'immagine, quindi un BLOB. Poiché il campo **logo** è un'immagine bitmap, nell'esempio i dati binari vengono restituiti tramite **GetBytes**. Notare che all'identificatore dell'editore nella riga corrente di dati si accede prima del logo, in quanto i campi devono essere letti in modo sequenziale.

[!code-csharp[SqlCommand_ExecuteReader_SequentialAccess#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteReader_SequentialAccess.cs#1)]

## <a name="see-also"></a>Vedi anche

- [Dati binari e con valori di grandi dimensioni di SQL Server](./sql/sql-server-binary-large-value-data.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
