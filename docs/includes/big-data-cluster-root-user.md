---
author: MikeRayMSFT
ms.prod: sql
ms.technology: big-data-cluster
ms.topic: include
ms.date: 06/22/2020
ms.author: mikeray
ms.openlocfilehash: 599b4072c5d03c8a4753c7c00bc7edc14e0f3b05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038937"
---
A partire da SQL Server 2019 CU5, quando si distribuisce un nuovo cluster con l'autenticazione di base, tutti gli endpoint, incluso il gateway, usano `AZDATA_USERNAME` e `AZDATA_PASSWORD`. Gli endpoint nei cluster aggiornati a CU5 continuano a usare `root` come nome utente per la connessione all'endpoint del gateway. Questa modifica non si applica alle distribuzioni che usano l'autenticazione Active Directory. Vedere [Credenziali per l'accesso ai servizi tramite l'endpoint del gateway](../big-data-cluster/release-notes-big-data-cluster.md#credentials-for-accessing-services-through-gateway-endpoint) nelle note sulla versione.