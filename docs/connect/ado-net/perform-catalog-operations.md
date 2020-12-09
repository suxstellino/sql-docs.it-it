---
title: Esecuzione di operazioni di catalogo
description: Viene descritto come eseguire i comandi per la modifica dello schema di database.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: e60f542f-6271-495b-a9e4-48553481c2a3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 85e68bd77b11fd70e4071d7ae67ebf26c643b540
ms.sourcegitcommit: debaff72dbfae91b303f0acd42dd6d99e03135a2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2020
ms.locfileid: "96428243"
---
# <a name="performing-catalog-operations"></a>Esecuzione di operazioni di catalogo

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Per eseguire un comando che modifica un database o un catalogo, ad esempio l'istruzione CREATE TABLE o CREATE PROCEDURE, creare un oggetto **Command** usando le istruzioni SQL appropriate e un oggetto **Connection**. Eseguire il comando con il metodo <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> dell'oggetto <xref:Microsoft.Data.SqlClient.SqlCommand>.

## <a name="example"></a>Esempio

Nell'esempio di codice seguente viene creata una stored procedure in un database Microsoft SQL Server.

[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#3](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#3)]

## <a name="see-also"></a>Vedere anche

- [Uso di comandi per modificare i dati](use-commands-to-modify-data.md)
- [Comandi e parametri](commands-parameters.md)
