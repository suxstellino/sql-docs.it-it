---
description: Modificare il mapping dei tipi (MySQLToSQL)
title: Modificare il mapping dei tipi (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 184f7ab2-725f-491e-a15b-b889f2fb6a68
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b1ddf264835cf0a9a008335787353d7a1051431f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016601"
---
# <a name="edit-type-mapping-mysqltosql"></a>Modificare il mapping dei tipi (MySQLToSQL)
La finestra di dialogo **Modifica mapping tipi** consente di specificare la modalità di mapping dei tipi tra gli oggetti di database di origine e di destinazione.  
  
È possibile accedere a questa finestra di dialogo in diverse posizioni:  
  
-   Quando si seleziona un database di origine o un oggetto di database, la scheda **mapping dei tipi** viene visualizzata a destra di Esplora metadati. Fare clic su **Aggiungi** per aggiungere un nuovo mapping dei tipi oppure fare clic su **modifica** per modificare un mapping dei tipi esistente.  
  
-   Scegliere **Impostazioni progetto** o **Impostazioni progetto predefinite** dal menu **strumenti** . Nella finestra di dialogo risultante selezionare **mapping dei tipi**. Fare clic su **Aggiungi** per aggiungere un nuovo mapping dei tipi oppure fare clic su **modifica** per modificare un mapping dei tipi esistente.  
  
-   I mapping dei tipi specifici della tabella sostituiscono i mapping dei tipi di progetto e di database. I mapping specifici del database sostituiscono i mapping del progetto.  
  
## <a name="options"></a>Opzioni  
  
##### <a name="source-type"></a>Tipo di origine  
Consente di selezionare il tipo di dati di origine di cui eseguire il mapping a un tipo di dati SQL Server.  
  
Se il tipo di dati è di lunghezza variabile, i campi seguenti verranno visualizzati in **sourceType**:  
  
##### <a name="from"></a>Da  
Consente di specificare la lunghezza minima per questo mapping. Per il tipo di dati **nchar** , ad esempio, è possibile immettere 10 per specificare che il mapping è relativo a un intervallo a partire da **nchar (10).**  
  
##### <a name="to"></a>A  
Consente di specificare la lunghezza massima consentita per questo mapping. Per il tipo di dati **nchar** , ad esempio, è possibile immettere 20 per specificare che il mapping è per un intervallo che termina con **nchar (20).**  
  
##### <a name="target-type"></a>Tipo di destinazione  
Consente di selezionare il tipo di dati SQL Server a cui viene eseguito il mapping del tipo di dati di origine. Quando SSMA crea la tabella o nel SQL Server, il tipo di dati di origine verrà modificato in questo tipo di dati.  
  
Se il tipo di dati è di lunghezza variabile, il campo seguente verrà visualizzato in **tipo di destinazione**:  
  
##### <a name="replace-with"></a>Sostituire con  
Specificare la lunghezza di destinazione per questo mapping. Per il tipo di dati **nvarchar** , ad esempio, è possibile immettere 20 per specificare che il tipo di dati di origine specificato deve essere mappato a **nvarchar (20).**  
  
