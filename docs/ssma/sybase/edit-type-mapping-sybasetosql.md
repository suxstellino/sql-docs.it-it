---
description: Modificare il mapping dei tipi (SybaseToSQL)
title: Modificare il mapping dei tipi (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 513f071a-d5e6-4ed5-acca-269bf76323c5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7fc5d4a2e2e907feb867d5cd9e44d2b7d8e77bd4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064226"
---
# <a name="edit-type-mapping-sybasetosql"></a>Modificare il mapping dei tipi (SybaseToSQL)
La finestra di dialogo **Modifica mapping tipi** consente di specificare la modalità di mapping dei tipi tra gli oggetti di database di origine e di destinazione.  
  
È possibile accedere a questa finestra di dialogo in diverse posizioni:  
  
-   Quando si seleziona un database di origine o un oggetto di database, la scheda **mapping dei tipi** viene visualizzata a destra di Esplora metadati. Fare clic su **Aggiungi** per aggiungere un nuovo mapping dei tipi oppure fare clic su **modifica** per modificare un mapping dei tipi esistente.  
  
-   Scegliere **Impostazioni progetto** o **Impostazioni progetto predefinite** dal menu **strumenti** . Nella finestra di dialogo risultante selezionare **mapping dei tipi**. Fare clic su **Aggiungi** per aggiungere un nuovo mapping dei tipi oppure fare clic su **modifica** per modificare un mapping dei tipi esistente.  
  
I mapping dei tipi specifici della tabella sostituiscono i mapping dei tipi di progetto e di database. I mapping specifici del database sostituiscono i mapping del progetto.  
  
## <a name="options"></a>Opzioni  
**Tipo di origine**  
Consente di selezionare il tipo di dati di origine per il mapping a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo di dati.  
  
Se il tipo di dati è di lunghezza variabile, i campi seguenti verranno visualizzati in **tipo di origine**:  
  
**From**  
Consente di specificare la lunghezza minima per questo mapping. Per il tipo di dati **nchar** , ad esempio, è possibile immettere 10 per specificare che il mapping è relativo a un intervallo a partire da **nchar (10)**.  
  
**To**  
Consente di specificare la lunghezza massima consentita per questo mapping. Per il tipo di dati **nchar** , ad esempio, è possibile immettere 20 per specificare che il mapping è per un intervallo che termina con **nchar (20)**.  
  
**Tipo di destinazione**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Consente di selezionare il tipo di dati a cui viene eseguito il mapping del tipo di dati di origine. Quando SSMA crea la tabella o stored procedure in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il tipo di dati di origine verrà modificato in questo tipo di dati.  
  
Se il tipo di dati è di lunghezza variabile, il campo seguente verrà visualizzato in **tipo di destinazione**:  
  
**Replace with**  
Specificare la lunghezza di destinazione per questo mapping. Per il tipo di dati **nvarchar** , ad esempio, è possibile immettere 20 per specificare che il tipo di dati di origine specificato deve essere mappato a **nvarchar (20)**.  
  
