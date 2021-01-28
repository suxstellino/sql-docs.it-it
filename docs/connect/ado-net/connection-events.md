---
title: Eventi di connessione
description: Eventi di connessione per recuperare messaggi informativi da un'origine dati e determinare se il relativo stato viene modificato.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 5a29de74-acfc-4134-8616-829dd7ce0710
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3fb13f3cc163bb157f418d3bda99e0173d81b842
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596008"
---
# <a name="connection-events"></a>Eventi di connessione

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Il provider di dati Microsoft SqlClient per SQL Server presenta oggetti relativi alla **Connessione** contenenti due eventi che è possibile usare per recuperare messaggi informativi da un'origine dati o per determinare se lo stato di una **Connessione** è stato modificato. Nella tabella seguente vengono illustrati gli eventi dell'oggetto **Connessione**.

|Event|Descrizione|  
|-----------|-----------------|  
|**InfoMessage**|Si verifica quando un messaggio informativo viene restituito da un'origine dati. I messaggi informativi sono i messaggi di un'origine dati per cui non viene generata un'eccezione.|  
|**StateChange**|Si verifica quando viene modificato lo stato della **Connessione**.|  

## <a name="work-with-the-infomessage-event"></a>Usare l'evento InfoMessage

È possibile recuperare avvisi e messaggi informativi da un'origine dati SQL Server usando l'evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> dell'oggetto <xref:Microsoft.Data.SqlClient.SqlConnection>. Gli errori restituiti da un'origine dati con un livello di gravità compreso tra 11 e 16 generano un'eccezione. Tuttavia, l'evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> consente di ottenere dall'origine dati i messaggi che non sono associati a un errore. Nel caso di Microsoft SQL Server i messaggi di errore con una gravità uguale o minore di 10 sono considerati informativi e vengono acquisiti usando l'evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>. Per altre informazioni, vedere l'articolo [Gravità degli errori del motore di database](../../relational-databases/errors-events/database-engine-error-severities.md).

L'evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> riceve un oggetto <xref:Microsoft.Data.SqlClient.SqlInfoMessageEventArgs> che contiene, nella proprietà **Errori**, una raccolta di messaggi dall'origine dati. È possibile eseguire una query negli oggetti **Errore** di questa raccolta per ottenere il numero dell'errore, il testo del messaggio e l'origine dell'errore. Il provider di dati Microsoft SqlClient per SQL Server include anche i dettagli sul database, sulla stored procedure e sul numero di riga da cui proviene il messaggio.

### <a name="example"></a>Esempio

Nell'esempio di codice seguente viene illustrato come aggiungere un gestore eventi per l'evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>.

[!code-csharp[SqlConnection_._InfoMessage#1](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#1)]

## <a name="handle-errors-as-infomessages"></a>Gestire gli errori come InfoMessages

In genere, l'evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> verrà generato solo per messaggi informativi e per messaggi di avviso inviati dal server. Tuttavia, quando si verifica un errore, l'esecuzione del metodo **ExecuteNonQuery** o **ExecuteReader** che ha avviato l'operazione del server viene interrotta e viene generata un'eccezione.

Per continuare a elaborare le restanti istruzioni di un comando indipendentemente da eventuali messaggi di errore generati dal server, impostare la proprietà <xref:Microsoft.Data.SqlClient.SqlConnection.FireInfoMessageEventOnUserErrors%2A> del tipo <xref:Microsoft.Data.SqlClient.SqlConnection> su `true`. In questo modo, invece di generare un'eccezione e di interrompere l'elaborazione, la connessione genera l'evento di errore <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>. L'applicazione client può quindi gestire questo evento e rispondere alle condizioni di errore.

> [!NOTE]
> Un errore con un livello di gravità pari a 17 o superiore che provoca l'interruzione dell'elaborazione del comando da parte del server deve essere gestito come un'eccezione. In questo caso viene generata un'eccezione indipendentemente da come viene gestito l'errore nell'evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>.

## <a name="work-with-the-statechange-event"></a>Usare l'evento StateChange

L'evento **StateChange** si verifica quando viene modificato lo stato di una **Connessione**. L'evento **StateChange** riceve il tipo <xref:System.Data.StateChangeEventArgs> che consente di determinare la modifica dello stato della **Connessione** usando le proprietà **OriginalState** e **CurrentState**. La proprietà **OriginalState** è un'enumerazione <xref:System.Data.ConnectionState> che indica lo stato della **Connessione** prima della modifica. **CurrentState** è un'enumerazione <xref:System.Data.ConnectionState> che indica lo stato della **Connessione** dopo la modifica.

Nell'esempio di codice seguente viene usato l'evento **StateChange** per scrivere un messaggio nella console quando viene modificato lo stato della **Connessione**.

[!code-csharp[SqlConnection_._StateChange#2](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#2)]

## <a name="see-also"></a>Vedere anche

- [Connessione a un'origine dati](connecting-to-data-source.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)