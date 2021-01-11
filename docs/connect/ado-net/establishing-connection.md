---
title: Istituzione di una connessione
description: Linee guida per la connessione a un SQL Server dal provider SqlClient.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 3af512f3-87d9-4005-9e2f-abb1060ff43f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: b3d92f49fb4eae6184a673ffc25ff6bbdf521626
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771593"
---
# <a name="establishing-connection"></a>Istituzione di una connessione

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Per eseguire la connessione a Microsoft SQL Server, utilizzare l'oggetto <xref:Microsoft.Data.SqlClient.SqlConnection> del provider di dati Microsoft SqlClient per SQL Server. Per informazioni sulla sicurezza dell'archiviazione e del recupero delle stringhe di connessione, vedere [Protezione delle informazioni di connessione](protecting-connection-information.md).

## <a name="closing-connections"></a>Chiusura di connessioni

Al termine dell'utilizzo, chiudere sempre la connessione in modo che possa essere restituita al pool. Il blocco `Using` in Visual Basic o C# elimina automaticamente la connessione quando il codice esce dal blocco, anche in caso di eccezione non gestita. Per altre informazioni vedere [Istruzione using](/dotnet/csharp/language-reference/keywords/using-statement) e [Istruzione Using](/dotnet/visual-basic/language-reference/statements/using-statement).

È anche possibile usare i metodi `Close` o `Dispose` dell'oggetto Connessione. Le connessioni che non vengono chiuse in modo esplicito potrebbero non essere aggiunte o restituite al pool. Ad esempio, una connessione che esce dall'ambito ma non viene chiusa in modo esplicito verrà restituita al pool di connessioni solo se è stata raggiunta la dimensione massima del pool e la connessione è ancora valida.

> [!NOTE]
> Non chiamare `Close` o `Dispose` su una **Connessione**, un **DataReader** o qualsiasi altro oggetto gestito nel metodo `Finalize` della classe. Nei finalizzatori rilasciare solo le risorse non gestite che la classe controlla direttamente. Se nella classe non sono presenti risorse non gestite, non includere un metodo `Finalize` nella relativa definizione della classe. Per altre informazioni, vedere [Garbage Collection](/dotnet/standard/garbage-collection/index).

> [!NOTE]
> Nel server non vengono generati eventi di accesso e di disconnessione quando una connessione viene recuperata dal o restituita al pool di connessioni, in quanto la connessione non viene effettivamente chiusa quando viene restituita al pool di connessioni. Per altre informazioni, vedere [Pool di connessioni SQL Server (ADO.NET)](sql-server-connection-pooling.md).

## <a name="connecting-to-sql-server"></a>Connessione a SQL Server

Per informazioni sui nomi e sui valori validi del formato della stringa, vedere la proprietà <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> dell'oggetto <xref:Microsoft.Data.SqlClient.SqlConnection>. È anche possibile usare la classe <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> per creare stringhe di connessione sintatticamente valide in fase di esecuzione. Per altre informazioni, vedere [Compilatori di stringhe di connessione](connection-string-builders.md).

Nel codice di esempio seguente viene descritta la procedura di creazione e di apertura di una connessione a un database SQL Server.

[!code-csharp[SqlConnection.Open#1](~/../sqlclient/doc/samples/SqlConnection_Open.cs#1)]

### <a name="integrated-security-and-aspnet"></a>Sicurezza integrata e ASP.NET

La sicurezza integrata di SQL Server, nota anche come connessioni attendibili, consente di garantire la protezione quando ci si connette a SQL Server perché non espone un ID utente e una password nella stringa di connessione ed è il metodo consigliato per autenticare una connessione. La sicurezza integrata usa l'identità di sicurezza corrente, o token, del processo in esecuzione. Per le applicazioni desktop, questa identità è in genere l'identità dell'utente attualmente connesso.

L'identità di sicurezza delle applicazioni ASP.NET può essere impostata su diverse opzioni. Per altre informazioni sull'identità di sicurezza usata da un'applicazione ASP.NET al momento della connessione a SQL Server, vedere [ASP.NET Impersonation](/previous-versions/aspnet/xh507fc5(v=vs.100)), [ASP.NET Authentication](/previous-versions/aspnet/eeyk640h(v=vs.100)) e [How to: Access SQL Server Using Windows Integrated Security](/previous-versions/aspnet/bsz5788z(v=vs.100)).

## <a name="see-also"></a>Vedere anche

- [Connessione a un'origine dati](connecting-to-data-source.md)
- [Stringhe di connessione](connection-strings.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
