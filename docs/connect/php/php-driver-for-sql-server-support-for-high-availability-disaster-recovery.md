---
description: Supporto del ripristino di emergenza a disponibilità elevata per i driver Microsoft per PHP per SQL Server
title: Supporto del ripristino di emergenza a disponibilità elevata per i driver Microsoft per PHP per SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 07/31/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 73a80821-d345-4fea-b076-f4aabeb4af3e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4a5c5747dad97cc54859cb9b40b966cec66970fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058402"
---
# <a name="support-for-high-availability-disaster-recovery"></a>Supporto per il ripristino di emergenza a disponibilità elevata
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Questo argomento descrive il supporto di [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] (aggiunto nella versione 3.0) per il ripristino di emergenza a disponibilità elevata.

A partire dalla versione 3.0 dei driver Microsoft per PHP per SQL Server, è possibile specificare il listener del gruppo di disponibilità di un gruppo di disponibilità di ripristino di emergenza a disponibilità elevata o di un'istanza del cluster di failover come server nella stringa di connessione.

La proprietà di connessione **MultiSubnetFailover** indica che l'applicazione viene distribuita in un gruppo di disponibilità o nell'istanza del cluster di failover e che il driver tenta di connettersi al database nell'istanza di SQL Server primaria tramite la connessione a tutti gli indirizzi IP. Specificare sempre **MultiSubnetFailover=true** in caso di connessione a un listener del gruppo di disponibilità di SQL Serve o a un'istanza del cluster di failover di SQL Server. Se l'applicazione è connessa a un database Always On che esegue il failover, dopo il failover la connessione originale viene interrotta e l'applicazione deve aprire una nuova connessione per proseguire con l'esecuzione.

Per informazioni dettagliate sui Gruppi di disponibilità Always On, vedere la pagina della documentazione sul [ripristino di emergenza a disponibilità elevata](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).

## <a name="transparent-network-ip-resolution-tnir"></a>Risoluzione dell'IP di rete trasparente (TNIR)

La risoluzione dell'IP di rete trasparente (TNIR) è una revisione della funzionalità **MultiSubnetFailover** esistente. Influisce sulla sequenza di connessione del driver quando il primo IP risolto del nome host non risponde e sono presenti più IP associati al nome host. L'opzione di connessione corrispondente è **TransparentNetworkIPResolution**. Insieme a **MultiSubnetFailover** offre le quattro sequenze di connessione seguenti: 

- TNIR abilitata e **MultiSubnetFailover** disabilitata: viene eseguito un tentativo per un indirizzo IP, seguito da tutti gli indirizzi IP in parallelo
- TNIR abilitata e **MultiSubnetFailover** abilitata: viene eseguito un tentativo per tutti gli indirizzi IP in parallelo
- TNIR disabilitata e **MultiSubnetFailover** disabilitata: viene eseguito un tentativo per tutti gli indirizzi IP uno dopo l'altro
- TNIR disabilitata e **MultiSubnetFailover** abilitata: viene eseguito un tentativo per tutti gli indirizzi IP in parallelo

Per impostazione predefinita, TNIR è abilitata e **MultiSubnetFailover** è disabilitata.

Di seguito è riportato un esempio di abilitazione di TNIR e **MultiSubnetFailover** con il driver PDO_SQLSRV:

```
<?php
$serverName = "yourservername";
$username = "yourusername";
$password = "yourpassword";
$connectionString = "sqlsrv:Server=$serverName; TransparentNetworkIPResolution=Enabled; MultiSubnetFailover=yes";
try {
    $conn = new PDO($connectionString, $username, $password, array(PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION));
    // your code 
    // more of your code
    // when done, close the connection
    unset($conn);
} catch(PDOException $e) {
    print_r($e->errorInfo);
}
?>
```

## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Aggiornamento per l'utilizzo di cluster su più subnet dal mirroring del database  
Si verificherà un errore di connessione se nella stringa di connessione sono presenti le parole chiave di connessione **MultiSubnetFailover** e **Failover_Partner**. Si verificherà un errore anche nel caso in cui venga usata **MultiSubnetFailover** e SQL Server restituisca una risposta del partner di failover che indica che è parte di una coppia di mirroring del database.  
  
Quando si aggiorna un'applicazione PHP che usa il mirroring del database in uno scenario con più subnet, rimuovere la proprietà di connessione **Failover_Partner** e sostituirla con **MultiSubnetFailover** impostata su **True** e sostituire il nome del server nella stringa di connessione con un listener del gruppo di disponibilità. Se in una stringa di connessione vengono usati **Failover_Partner** e **MultiSubnetFailover=true** il driver genera un errore. Se tuttavia in una stringa di connessione vengono usati **Failover_Partner** e **MultiSubnetFailover=false** (o **ApplicationIntent=ReadWrite**) l'applicazione usa il mirroring del database.  
  
Il driver restituisce un errore se il mirroring del database viene usato nel database primario nel gruppo di disponibilità e se **MultiSubnetFailover=true** viene usato nella stringa di connessione a un database primario anziché a un listener del gruppo di disponibilità.  

[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="see-also"></a>Vedere anche  
[Connessione al server](../../connect/php/connecting-to-the-server.md)  
