---
title: Aggiornare AZDATA_PASSWORD
description: Aggiornare manualmente `AZDATA_PASSWORD`
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 12/19/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7fe856ae92c198d44ed0b11d1d2ce808e87fb4eb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039681"
---
# <a name="manually-update-azdata_password"></a>Aggiornare `AZDATA_PASSWORD` manualmente

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Indipendentemente dal fatto che il cluster stia funzionando con l'integrazione Active Directory, durante la distribuzione viene impostato `AZDATA_PASSWORD`. Offre un'autenticazione di base per il controller del cluster e l'istanza master. In questo documento viene descritto come aggiornare manualmente `AZDATA_PASSWORD`.

## <a name="change-azdata_password-for-controller"></a>Modificare `AZDATA_PASSWORD` per il controller

Se il cluster funziona in modalità non Active Directory, aggiornare la password del gateway Apache Knox seguendo questa procedura:

1. Ottenere le credenziali SQL Server del controller eseguendo questi comandi:

   a. Eseguire questo comando come amministratore di Kubernetes:

   ```bash
   kubectl get secret controller-sa-secret -n <cluster name> -o yaml | grep password
   ```

   b. Decodificare in Base64 il segreto:
   
   ```bash
   echo <password from kubectl command>  | base64 --decode && echo
   ```

1. In una finestra di comando separata esporre la porta del server di database del controller:

   ```bash
   kubectl port-forward controldb-0 1433:1433 --address 0.0.0.0 -n <cluster name>
   ```
 
1. Usare la password dell'amministratore di sistema appena ottenuta per connettersi al server di database del controller da uno strumento client SQL.

1. Generare una nuova password complessa per `AZDATA_USERNAME` per sostituire la `AZDATA_PASSWORD` esistente.

   Per semplificare l'esempio, i passaggi successivi usano "newPassword" perché la password generata è "newPassword". 

1. Ottenere `hexsalt` dalla tabella users:

   ```sql
   SELECT hexsalt FROM [auth].[users] WHERE username = '<username>'
   ```

   `hexsalt` restituisce una stringa esadecimale casuale, ad esempio `64FC59DF31244FFEE02F457BC0750226`.

1. Crittografare la nuova password complessa usando `hexsalt`:

   Per praticità, viene messo a disposizione uno strumento predefinito `pbkdf2` per crittografare la password. Scaricare l'app .NET Core appropriata per la piattaforma per [`pbkdf2`](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/security/password-hashing/pbkdf2/prebuilt-binaries).

   L'app è indipendente e non richiede prerequisiti, ad esempio i runtime .NET. Per crittografare la password, eseguire:

   ```bash
   pbkdf2 <password> <hexsalt>
   J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=
   ```

1. Aggiornare la password nella tabella users:

   ```SQL
   UPDATE [auth].[users] SET password = 'J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=' WHERE username = '<username>'
   ```

## <a name="change-azdata_password-in-the-sql-server-master-instance"></a>Modificare `AZDATA_PASSWORD` nell'istanza master di SQL Server

1. Connettersi all'endpoint SQL master con un utente amministratore.

1. Per modificare la password per le credenziali di accesso definite durante la distribuzione nel parametro `AZDATA_USERNAME`, eseguire il comando TSQL seguente:

   ```sql
   ALTER LOGIN <AZDATA_USERNAME> WITH PASSWORD = 'newPassword'
   ```
