---
title: Gestire l'accesso al cluster Big Data in modalità Active Directory
description: Gestire l'accesso al cluster Big Data
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c40a136f748126bd7baa6892d5cdb5317363ad2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046331"
---
# <a name="manage-big-data-cluster-access-in-active-directory-mode"></a>Gestire l'accesso al cluster Big Data in modalità Active Directory

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive come aggiungere nuovi gruppi di Active Directory con i ruoli *bdcUser* in aggiunta a quelli forniti durante la distribuzione tramite l'impostazione di configurazione *clusterUsers*.

>[!IMPORTANT]
>Non usare questa procedura per aggiungere nuovi gruppi di Active Directory con il ruolo *bdcAdmin*. I componenti di Hadoop, ad esempio HDFS e Spark, consentono di aggiungere un solo gruppo di Active Directory come gruppo di utenti con privilegi avanzati, l'equivalente del ruolo *bdcAdmin* in BDC. Per concedere autorizzazioni *bdcAdmin* ad altri gruppi di Active Directory per il cluster Big Data dopo la distribuzione, è necessario aggiungere altri utenti e gruppi ai gruppi già nominati durante la distribuzione. È possibile seguire la stessa procedura per aggiornare le appartenenze ai gruppi con il ruolo *bdcUsers*.

## <a name="two-overarching-roles-in-the-big-data-cluster"></a>Due ruoli globali nel cluster Big Data

I gruppi di Active Directory possono essere specificati nella sezione sicurezza del profilo di distribuzione come parte di due ruoli globali per l'autorizzazione all'interno del cluster Big Data:

* `clusterAdmins`: Questo parametro accetta un gruppo di Active Directory. I membri di questo gruppo hanno il ruolo *bdcAdmin*, ovvero ottengono le autorizzazioni con privilegi di amministratore per l'intero cluster. Hanno autorizzazioni *sysadmin* in SQL Server, autorizzazioni *superuser* in Hadoop Distributed File System (HDFS) e Spark e diritti di *amministratore* nel controller.

* `clusterUsers`: questi gruppi di Active Directory sono mappati al ruolo *bdcUsers* in BDC. Si tratta di utenti normali senza autorizzazioni con privilegi di amministratore nel cluster. Sono autorizzati ad accedere all'istanza master di SQL Server ma, per impostazione predefinita, non hanno autorizzazioni per gli oggetti o i dati. Sono utenti normali per HDFS e Spark, senza le autorizzazioni di *utente con privilegi avanzati*. Quando ci si connette all'endpoint controller, questi utenti possono solo eseguire query sugli endpoint (usando l'*elenco di endpoint BDC azdata*).

Per concedere autorizzazioni *bdcAdmin* ad altri gruppi di Active Directory senza modificare le appartenenze ai gruppi all'interno di Active Directory, seguire le procedure descritte nelle sezioni successive.

## <a name="grant-bdcuser-permissions-to-additional-active-directory-groups"></a>Concedere le autorizzazioni *bdcUser* ad altri gruppi di Active Directory

### <a name="create-a-login-for-the-active-directory-user-or-group-in-the-sql-server-master-instance"></a>Creare un account di accesso per l'utente o il gruppo di Active Directory nell'istanza master di SQL Server

1. Connettersi all'endpoint SQL master usando il client SQL preferito. Usare qualsiasi account di accesso amministratore (ad esempio `AZDATA_USERNAME`, specificato durante la distribuzione). In alternativa, si può usare qualsiasi account Active Directory appartenente al gruppo di Active Directory indicato come `clusterAdmins` nella configurazione di sicurezza.

1. Per creare un account di accesso per l'utente o il gruppo di Active Directory, eseguire il comando TSQL seguente:

   ```sql
   CREATE LOGIN [<domain>\<principal>] FROM WINDOWS;
   ```

   Concedere le autorizzazioni desiderate nell'istanza di SQL Server:

   ```sql
   ALTER SERVER ROLE <server role> ADD MEMBER [<domain>\<principal>];
   GO
   ```

Per un elenco completo dei ruoli del server, vedere l'argomento corrispondente sulla sicurezza di SQL Server [qui](../relational-databases/security/authentication-access/server-level-roles.md).

### <a name="add-the-active-directory-user-or-group-to-the-roles-table-in-the-controller-database"></a>Aggiungere l'utente o il gruppo di Active Directory alla tabella dei ruoli nel database del controller

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

1. Usare la connessione precedente per inserire una nuova riga nelle tabelle *roles* e *active_directory_principals*. Digitare il valore *REALM* in lettere maiuscole.

   ```sql
   USE controller;
   GO

   INSERT INTO [controller].[auth].[roles] VALUES (N'<user or group name>@<REALM>', 'bdcUser')
   GO

   INSERT INTO [controller].[auth].[active_directory_principals] VALUES (N'<user or group name>@<REALM>', N'<SID>')
   GO
   ```

   Per trovare l'ID di sicurezza dell'utente o del gruppo da aggiungere, è possibile usare il comando [Get-ADUser](/powershell/module/addsadministration/get-aduser/) o [Get-ADGroup](/powershell/module/addsadministration/get-adgroup/) di PowerShell.

2. Verificare che i membri del gruppo aggiunto abbiano le autorizzazioni *bdcUser* previste eseguendo l'accesso all'endpoint controller o l'autenticazione all'istanza master di SQL Server. Ad esempio:

   ```bash
   azdata login
   azdata bdc endpoints list
   ```

## <a name="next-steps"></a>Passaggi successivi

- [Concetti relativi alla sicurezza per i cluster Big Data di SQL Server 2019](concept-security.md)
