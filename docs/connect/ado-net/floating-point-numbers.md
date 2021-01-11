---
title: Numeri a virgola mobile
description: Informazioni su alcuni dei problemi che si verificano quando si usano i numeri a virgola mobile nel provider di dati Microsoft SqlClient per SQL Server.
ms.date: 11/13/2020
ms.assetid: 73c218c6-1c44-4402-a167-4f6262629a91
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1ca251c6de5ca6157ace8ee481bae02afa9fb2fb
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771569"
---
# <a name="floating-point-numbers"></a>Numeri a virgola mobile

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

In questo argomento vengono descritti alcuni dei problemi che gli sviluppatori incontrano spesso quando usano i numeri a virgola mobile nel provider di dati Microsoft SqlClient per SQL Server. Questi problemi sono dovuti al modo in cui i computer archiviano i numeri a virgola mobile e non sono specifici di un determinato provider, ad esempio <xref:Microsoft.Data.SqlClient>.

## <a name="floating-point-pitfall"></a>Pitfall a virgola mobile

I numeri a virgola mobile in genere non hanno una rappresentazione binaria esatta Al contrario, il computer archivia un'approssimazione del numero. In momenti differenti è possibile che vengano usati numeri diversi di cifre binarie per rappresentare il numero. Quando un numero a virgola mobile viene convertito da una rappresentazione a un'altra, le cifre meno significative di tale numero possono variare leggermente. La conversione si verifica un genere quando viene eseguito il cast di un numero da un tipo a un altro. La variazione si verifica se la conversione avviene all'interno di un database, tra tipi che rappresentano valori del database oppure tra tipi. A causa di queste modifiche, i numeri che logicamente sarebbero uguali potrebbero presentare modifiche nelle cifre meno significative, generando in questo modo valori diversi. Il numero di cifre di precisione nel numero può essere maggiore o minore del previsto. Quando viene formattato come stringa, il numero potrebbe non presentare il valore previsto.

## <a name="suggested-workarounds"></a>Soluzioni alternative suggerite

Per minimizzare questi effetti, è necessario usare la corrispondenza più vicina disponibile tra tipi numerici. Se ad esempio si usa SQL Server, il valore numerico esatto può variare se si converte un valore Transact-SQL di tipo reale in un valore di tipo float. In .NET anche la conversione di <xref:System.Single> in <xref:System.Double> può produrre risultati imprevisti. In entrambi questi casi, una strategia efficace consiste nel fare in modo che tutti i valori dell'applicazione utilizzino lo stesso tipo numerico. È anche possibile usare un tipo decimale a precisione fissa o eseguire il cast dei numeri a virgola mobile in un tipo decimale a precisione fissa prima di usarli.

Per risolvere i problemi con il confronto di uguaglianza, è consigliabile scrivere il codice dell'applicazione in modo che le variazioni delle cifre meno significative vengano ignorate. Ad esempio, anziché eseguire un confronto per verificare l'uguaglianza tra due numeri, sottrarre un numero dall'altro. Se la differenza rientra in un margine accettabile di arrotondamento, l'applicazione può considerare i numeri come se fossero identici.

## <a name="see-also"></a>Vedere anche

- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
