---
title: Memoria esaurita con raccolte di XML Schema di grandi dimensioni | Microsoft Docs
description: Informazioni sulle soluzioni per la gestione delle condizioni di memoria insufficiente durante il caricamento o l'eliminazione di una raccolta di XML Schema di grandi dimensioni.
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- out-of-memory conditions
- XML schema collections [SQL Server], large
ms.assetid: 29b9d839-aaaf-48fb-be17-840c751f36f1
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 3e9a422c35dc2e535472af0aa29e465167af556d
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490779"
---
# <a name="large-xml-schema-collections-and-out-of-memory-conditions"></a>Raccolte di XML Schema di grandi dimensioni e condizioni di memoria insufficiente
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Durante una chiamata alla funzione incorporata XML_SCHEMA_NAMESPACE() in una raccolta XML Schema di grandi dimensioni o durante il tentativo di eliminare queste ultime, potrebbe verificarsi una condizione di memoria insufficiente. Per risolvere questo problema, è possibile utilizzare le soluzioni seguenti:  
  
-   Quando il carico di lavoro del sistema è ridotto, è consigliabile utilizzare il comando DROP_XML_SCHEMA_COLLECTION. Se tale tentativo ha esito negativo, configurare il database in modalità utente singolo mediante l'istruzione ALTER DATABASE e provare ad eseguire di nuovo il comando DROP XML SCHEMA COLLECTION. Se la raccolta XML Schema è contenuta nel database **master**, **model** o **tempdb**, per attivare la modalità utente singolo è necessario riavviare il server.  
  
-   Quando si chiama la funzione XML_SCHEMA_NAMESPACE, è possibile provare a recuperare un singolo spazio dei nomi XML Schema ; provare a eseguire la chiamata quando diminuisce il carico di lavoro del sistema oppure provare ad eseguirla in modalità utente singolo.  
  
## <a name="see-also"></a>Vedere anche  
 [Requisiti e limitazioni per le raccolte di XML Schema nel server](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
