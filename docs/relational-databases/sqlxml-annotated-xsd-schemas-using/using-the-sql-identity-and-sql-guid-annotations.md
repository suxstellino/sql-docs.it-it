---
title: Utilizzo delle annotazioni sql:identity e sql:guid
description: Informazioni su come usare le annotazioni sql:identity e sql:guid in uno schema XSD per definire il comportamento di un updategram XML.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sql:guid
- annotated XSD schemas, IDENTITY-type columns
- annotated XSD schemas, GUID values
- DiffGrams [SQLXML], annotations
- identity annotation
- XSD schemas [SQLXML], GUID values
- GUIDs [SQLXML]
- guid annotation
- sql:identity
- IDENTITY-type column
- XSD schemas [SQLXML], IDENTITY-type columns
- updategrams [SQLXML], GUID values
ms.assetid: 7661dfd0-6573-4692-a8f1-3597adcd33c4
author: rothja
ms.author: jroth
ms.reviewer: ''
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 29236ca0dfdce10cf4a957a2972c7f65e1f82c38
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490756"
---
# <a name="using-the-sqlidentity-and-sqlguid-annotations"></a>Utilizzo delle annotazioni sql:identity e sql:guid
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile specificare le **annotazioni sql:identity** e **sql:guid** in uno schema XSD in qualsiasi nodo mappato a una colonna di database in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Mentre il formato updategram supporta gli **attributi updg:at-identity** e **updg:guid,** il formato DiffGram non lo fa. **L'attributo updg:at-identity** definisce il comportamento nell'aggiornamento di una colonna di tipo IDENTITY. **L'attributo updg:guid** consente di ottenere un valore GUID da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e usarlo nell'updategram. Per altre informazioni ed esempi di lavoro, vedere Inserimento di dati tramite [updategram XML &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md).  
  
 Le **annotazioni sql:identity** **e sql:guid** estendono questa funzionalità ai DiffGram.  
  
 Per eseguire un DiffGram, è necessario prima convertirlo in updategram. Specificando le **annotazioni sql:identity** e **sql:guid** nello schema XSD, si definisce infatti il comportamento di un updategram. Tutte le annotazioni vengono pertanto descritte nel contesto di un updategram. Le annotazioni possono essere utilizzate sia per i DiffGram che per gli updategram. Per gli updategram è tuttavia già disponibile una modalità di gestione dell'identità e dei valori GUID più potente.  
  
 Le **annotazioni sql:identity** **e sql:guid** possono essere definite in un elemento di contenuto complesso.  
  
## <a name="sqlidentity-annotation"></a>Annotazione sql:identity  
 È possibile specificare **l'annotazione sql:identity** nello schema XSD in qualsiasi nodo mappato a una colonna di database di tipo IDENTITY. Il valore specificato per questa annotazione definisce la modalità di aggiornamento della colonna di tipo IDENTITY (usando il valore fornito nell'updategram per modificare la colonna o ignorando il valore, nel qual caso per questa colonna viene usato un valore generato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ).  
  
 **All'annotazione sql:identity** è possibile assegnare due valori:  
  
 ignore  
 Indica all'updategram di ignorare qualsiasi valore fornito nell'updategram per la colonna in oggetto e di generare il valore Identity in base a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 useValue  
 Indica all'updategram di utilizzare il valore fornito nell'updategram per aggiornare la colonna di tipo IDENTITY. Un updategram non controlla se la colonna è un valore Identity.  
  
 Se l'updategram specifica un valore per la colonna di tipo IDENTITY, è necessario specificare **sql:identity="useValue"** nello schema.  
  
## <a name="sqlguid-annotation"></a>Annotazione sql:guid  
 Un valore GUID può essere generato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e quindi utilizzato nell'updategram. Nel contesto di DiffGrams è possibile usare l'annotazione **sql:guid** per specificare se usare un valore GUID generato da SQL Server o usare il valore fornito nell'updategram per tale colonna.  
  
 **All'annotazione sql:guid** è possibile assegnare due valori:  
  
 generate  
 Specifica che il valore GUID generato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve essere utilizzato per la colonna specifica nell'operazione di aggiornamento.  
  
 useValue  
 Specifica che per la colonna deve essere utilizzato il valore specificato nell'updategram. Rappresenta il valore predefinito.  
  
  
