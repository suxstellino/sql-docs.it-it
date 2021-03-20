---
description: Informazioni sulle proprietà di SQL Server Native Client OLE DB
title: Informazioni sulle proprietà di OLE DB | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, properties
- SQL Server Native Client OLE DB provider, properties
- properties [OLE DB]
- property values [SQL Server Native Client]
ms.assetid: 0b36a61e-b542-400d-a3d2-e6f643caf2c6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c4037f4fdc78594c5a9ae9ebb0a23dd4de9cc4b5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750381"
---
# <a name="about-sql-server-native-client-ole-db-properties"></a>Informazioni sulle proprietà di SQL Server Native Client OLE DB
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  I consumer impostano i valori delle proprietà per richiedere specifici comportamenti degli oggetti. Utilizzano, ad esempio, proprietà per specificare le interfacce che devono essere esposte da un set di righe. I consumer recuperano i valori delle proprietà per determinare le funzionalità di un oggetto, ad esempio un set di righe, una sessione o un oggetto origine dati.  
  
 Ogni proprietà presenta un valore, un tipo, una descrizione e un attributo di lettura/scrittura e, per le proprietà del set di righe, un indicatore che specifica se la proprietà può essere applicata alle singole colonne.  
  
 Una proprietà viene identificata da un GUID e da un numero intero che rappresenta l'ID della proprietà. Un set di proprietà è un set di tutte le proprietà che condividono lo stesso GUID. Oltre ai set di proprietà predefiniti OLE DB, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider OLE DB di Native Client implementa set di proprietà e proprietà specifiche del provider. Ogni proprietà appartiene a uno o più gruppi di proprietà. Un gruppo di proprietà è il gruppo di tutte le proprietà che si applicano a un particolare oggetto. Alcuni gruppi di proprietà includono i seguenti gruppi di proprietà: di inizializzazione, dell'origine dati, di sessione, del set di righe, di tabella e di colonna. In ognuno di questi gruppi sono presenti proprietà.  
  
 L'impostazione dei valori delle proprietà comporta:  
  
1.  Determinazione delle proprietà per le quali impostare i valori.  
  
2.  Determinazione dei set di proprietà che contengono le proprietà identificate.  
  
3.  Allocazione di una matrice di strutture DBPROPSET, una per ogni set di proprietà identificato.  
  
4.  Allocazione di una matrice di strutture DBPROP, una per ogni set di proprietà. Il numero di elementi di ogni matrice è il numero di proprietà (identificate nel passaggio 1) che appartengono al set di proprietà.  
  
5.  Inserimento di dati nella struttura DBPROP per ogni proprietà.  
  
6.  Inserimento di informazioni (GUID del set di proprietà, conteggio del numero di elementi e un puntatore alla matrice DBPROP corrispondente) nella struttura DBPROPSET per ogni set di proprietà.  
  
7.  Chiamata a un metodo per impostare proprietà e passaggio del conteggio e della matrice delle strutture DBPROPSET.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di un'applicazione provider OLE DB SQL Server Native Client](../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)   
 [Proprietà (OLE DB)](/previous-versions/windows/desktop/ms722734(v=vs.85))  
  
