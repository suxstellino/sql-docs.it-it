---
description: Autenticazione del certificato client per scenari di loopback
title: Autenticazione del certificato client per scenari di loopback | Microsoft Docs
ms.custom: ''
ms.date: 07/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: peterbae
ms.author: v-hyba
ms.openlocfilehash: 168034034e5f2abe53bd47029b99fa78c250336b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836968"
---
# <a name="client-certificate-authentication-for-loopback-scenarios"></a>Autenticazione del certificato client per scenari di loopback

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

In SQL Server 2016 è stata aggiunta una nuova stored procedure denominata sp_execute_external_script (SPEES). Questa stored procedure consente a SQL Server di avviare ed eseguire uno script esterno al di fuori di SQL Server, come parte di un'operazione di estendibilità. Con tale stored procedure è stato fornito il supporto degli script R e Python, entrambi dotati di librerie che possono usare un driver JDBC per connettersi a SQL Server. Sebbene gli SQL Server nella casella Windows possano usare l'autenticazione integrata di Windows per autenticare queste connessioni loopback con le stesse credenziali dell'utente che ha avviato la query, Linux SQL Server non può eseguire la stessa operazione. Pertanto, viene aggiunta l'autenticazione del certificato client per consentire agli utenti di eseguire l'autenticazione con un certificato e una chiave.

## <a name="connecting-using-client-certificate-authentication"></a>Eseguire la connessione usando l'autenticazione con certificato client

Il driver JDBC aggiunge tre proprietà di connessione per questa funzionalità:

* clientCertificate: specifica il certificato da usare per l'autenticazione. Il driver JDBC supporterà le estensioni di file PFX, PEM, DER e CER.

Formato
```
clientCertificate=<file_location>
``` 
Il driver usa un file di certificato. Per i certificati nei formati PEM, DER e CER, è obbligatorio l'attributo clientKey. Il percorso del file può essere relativo o assoluto.
 
* clientKey: specifica il percorso di un file della chiave privata per i certificati PEM, DER e CER specificati dall'attributo clientCertificate.

Formato
```
clientKey=<file_location>
```
Specifica il percorso del file di chiave privata. Se il file di chiave privata è protetto da password, è necessario specificare la parola chiave della password. Il percorso del file può essere relativo o assoluto.

* clientKeyPassword: stringa di password facoltativa fornita per accedere alla chiave privata del file clientkey vuoto.

Questa funzionalità è supportata ufficialmente solo per gli scenari di autenticazione del loopback per Linux SQL Server 2019 e versioni successive.

## <a name="see-also"></a>Vedere anche

[Connessione a SQL Server con il driver JDBC](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)  
[sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
