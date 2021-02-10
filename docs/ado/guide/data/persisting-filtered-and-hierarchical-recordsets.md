---
description: Persistenza di recordset filtrati e gerarchici
title: Salvataggio permanente di recordset filtrati e gerarchici | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- filtered Recordset persistence [ADO]
- persisting data [ADO]
- data updates [ADO], persisting data
- data persistence [ADO]
- updating data [ADO], persisting data
ms.assetid: d01aeb4d-4e43-450b-b3f2-0c27eaaf9f86
author: rothja
ms.author: jroth
ms.openlocfilehash: 774670bf109dfa28c3b9253daaae0e801167ce1d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032633"
---
# <a name="persisting-filtered-and-hierarchical-recordsets"></a>Persistenza di recordset filtrati e gerarchici
Se la proprietà [Filter](../../../ado/reference/ado-api/filter-property.md) è attiva per il **Recordset**, verranno salvate solo le righe accessibili nel filtro. Se il **Recordset** è gerarchico, vengono salvati il **Recordset** figlio corrente e i relativi elementi figlio, incluso il **Recordset** padre. Se viene chiamato il metodo **Save** di un **Recordset** figlio, l'elemento figlio e tutti i relativi elementi figlio vengono salvati, ma l'elemento padre non lo è. Per ulteriori informazioni sui **Recordset** gerarchici, vedere [data shaping](../../../ado/guide/data/data-shaping.md).  
  
> [!NOTE]
>  Alcune limitazioni si applicano quando si salvano **Recordset** gerarchici (forme dati) in formato XML. Per ulteriori informazioni, vedere la pagina relativa [alla permanenza dei record in formato XML](../../../ado/guide/data/persisting-records-in-xml-format.md).
