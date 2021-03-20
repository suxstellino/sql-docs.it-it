---
description: Viene illustrato come utilizzare il blocco di riga per controllare il modo in cui gli utenti simultanei accedono ai dati contemporaneamente da diverse connessioni.
title: Informazioni sul blocco delle righe
ms.custom: ''
ms.date: 12/08/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 63c76a2f-f2b9-461f-8904-acbda0169ac3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 589700b7d3f4a08958e306f59e87249263f86675
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673145"
---
# <a name="understanding-row-locking"></a>Informazioni sul blocco delle righe

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] usa blocchi di riga di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questi blocchi consentono di implementare i controlli della concorrenza tra più utenti che eseguono contemporaneamente modifiche in un database. Per impostazione predefinita, le transazioni e i blocchi sono gestiti a livello di singola connessione. Se, ad esempio, tramite un'applicazione vengono aperte due connessioni JDBC, i blocchi acquisiti da una connessione non possono essere condivisi con l'altra connessione. Nessuna delle due connessioni può acquisire blocchi che entrerebbero in conflitto con i blocchi mantenuti attivi dall'altra connessione.

> [!NOTE]  
> Se viene utilizzato il blocco di riga, tutte le righe nel buffer di recupero sono bloccate, pertanto un'impostazione molto grande per la dimensione di recupero può influire sulla concorrenza.

Il blocco viene utilizzato per garantire l'integrità delle transazioni e la coerenza dei database. Il blocco impedisce agli utenti di leggere dati di cui è in corso la modifica da parte di altri utenti e impedisce che gli stessi dati vengano modificati contemporaneamente da più utenti. Se il blocco non viene utilizzato, i dati inclusi in un database possono diventare incorretti a livello logico e le query eseguite su tali dati possono produrre risultati imprevisti.

> [!NOTE]  
> Per ulteriori informazioni sul blocco di riga in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [blocco in [!INCLUDE[ssDE](../../includes/ssde_md.md)] ](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine).

## <a name="see-also"></a>Vedere anche

[Gestione dei set di risultati con il driver JDBC](managing-result-sets-with-the-jdbc-driver.md)
