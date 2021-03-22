---
description: Informazioni sull'utilizzo del mirroring del database con il driver JDBC per SQL Server e sugli aspetti da considerare quando si verifica il failover.
title: Uso del mirroring del database (JDBC)
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4ff59218-0d3b-4274-b647-9839c4955865
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 369c31309ab439bb3de5362c42c6563b99130b66
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793048"
---
# <a name="using-database-mirroring-jdbc"></a>Uso del mirroring del database (JDBC)

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Il mirroring del database è principalmente una soluzione software per l'aumento della disponibilità del database e della ridondanza dei dati. Poiché [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] offre supporto implicito per il mirroring del database, lo sviluppatore non deve scrivere alcun codice o eseguire altre azioni dopo aver configurato il mirroring per il database.

Il mirroring del database, implementato per ogni database, consente di conservare una copia di un database di produzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un server di standby. Tale server può essere warm standby o hot standby, a seconda della configurazione e dello stato della sessione di mirroring del database. Un server hot standby supporta il failover rapido senza perdita di transazioni di cui è stato eseguito il commit. Un server warm standby supporta la forzatura del servizio (con possibile perdita di dati).

Il database di produzione è denominato database _principale_ , mentre la copia di standby viene chiamata database _mirror_ . Il database principale e il database mirror devono trovarsi in istanze separate di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (istanze del server). Se possibile, devono risiedere in computer separati.

L'istanza del server di produzione, ovvero il server principale, comunica con l'istanza del server di standby (il server mirror). I server principale e mirror sono partner all'interno di una sessione di mirroring del database. Se si verifica un errore nel server principale, il server mirror può rendere il database principale il database principale tramite un processo denominato _failover_. Partner_A e Partner_B, ad esempio, sono due server partner, con il database principale inizialmente su Partner_A come server principale e il database mirror che risiede in Partner_B come server mirror. Se Partner_A passa alla modalità offline, il database su Partner_B può eseguire il failover per diventare il database principale corrente. Quando Partner_A si unisce alla sessione di mirroring, diventa il server mirror e il database diventa il database mirror.

Se Partner_A server è danneggiato in modo irreparabile, è possibile portare online un server di Partner_C in modo che funga da server mirror per Partner_B, che è ora il server principale. Tuttavia, in questo scenario l'applicazione client deve includere la logica di programmazione per assicurare che le proprietà della stringa di connessione siano aggiornate con i nuovi nomi server utilizzati nella configurazione di mirroring del database. In caso contrario, la connessione ai server non riesce.

Le configurazioni del mirroring del database alternative offrono livelli diversi di prestazioni e sicurezza dei dati e supportano forme diverse di failover. Per altre informazioni, vedere "Panoramica del mirroring del database" nella documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="programming-considerations"></a>Considerazioni sulla programmazione

Quando si verifica un errore nel server di database principale, l'applicazione client riceve errori in risposta a chiamate API che indicano che la connessione al database è stata persa. Quando si verificano questi errori, tutte le modifiche di cui non è stato eseguito il commit nel database vengono perse e viene eseguito il rollback della transazione corrente. In questo scenario, l'applicazione deve chiudere la connessione (o rilasciare l'oggetto origine dati) e provare a riaprirla. La nuova connessione viene reindirizzata in maniera trasparente al database mirror, che funge ora da server principale, senza che il client debba modificare la stringa di connessione o l'oggetto origine dati.

Quando viene inizialmente stabilita una connessione, il server principale invia l'identità del partner di failover al client che verrà utilizzando quando si verifica il failover. Se un'applicazione tenta di stabilire una connessione iniziale con un server principale con errori, il client non riconosce l'identità del partner di failover. Per consentire ai client di gestire questo scenario, la proprietà della stringa di connessione failoverPartner e facoltativamente il metodo di origine dati [setFailoverPartner](../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md) consentono al client di specificare autonomamente l'identità del partner di failover. La proprietà client viene usata solo in questo scenario. Se il server principale è disponibile, non viene usata.

> [!NOTE]
> Se la proprietà failoverPartner viene specificata nella stringa di connessione o nell'oggetto origine dati, è necessario impostare anche la proprietà databaseName. In caso contrario, verrà generata un'eccezione. Se le proprietà failoverPartner e databaseName non sono specificate in modo esplicito, l'applicazione non tenterà di eseguire il failover in caso di errore del server di database principale. In altri termini, il reindirizzamento trasparente funziona solo per connessioni che specificano in modo esplicito le proprietà failoverPartner e databaseName. Per altre informazioni su failoverPartner e altre proprietà della stringa di connessione, vedere [Impostazione delle proprietà di connessione](../../connect/jdbc/setting-the-connection-properties.md).

Se il server partner di failover fornito dal client non fa riferimento a un server che funge da partner di failover per il database specificato e se il server o il database a cui si fa riferimento è in una disposizione con mirroring, la connessione viene rifiutata dal server. Benché la classe [SQLServerDataSource](../../connect/jdbc/reference/sqlserverdatasource-class.md) fornisca il metodo [getFailoverPartner](../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md), questo metodo restituisce solo il nome del partner di failover specificato nella stringa di connessione o nel metodo setFailoverPartner. Per recuperare il nome dell'effettivo partner di failover usato, usare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:

```sql
SELECT m.mirroring_role_DESC, m.mirroring_state_DESC,
m.mirroring_partner_instance FROM sys.databases as db,
sys.database_mirroring AS m WHERE db.name = 'MirroringDBName'
AND db.database_id = m.database_id
```

> [!NOTE]
> Per utilizzare il nome del database di mirroring, è necessario modificare questa istruzione.

Si consiglia di memorizzare nella cache le informazioni del partner per aggiornare la stringa di connessione o definire una strategia di ripetizione dei tentativi nel caso in cui il primo tentativo di stabilire una connessione non riesca.

## <a name="example"></a>Esempio

Nel seguente esempio, viene eseguito un primo tentativo di connessione al server principale. Se il tentativo non riesce e viene generata un'eccezione, viene eseguito un tentativo di connessione al server mirror, che potrebbe essere stato promosso a nuovo server principale. Notare l'utilizzo della proprietà failoverPartner nella stringa di connessione.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class ClientFailover {
    public static void main(String[] args) {

        String connectionUrl = "jdbc:sqlserver://serverA:1433;"
                + "databaseName=AdventureWorks;integratedSecurity=true;"
                + "failoverPartner=serverB";

        // Establish the connection to the principal server.
        try (Connection con = DriverManager.getConnection(connectionUrl);
                Statement stmt = con.createStatement();) {
            System.out.println("Connected to the principal server.");

            // Note that if a failover of serverA occurs here, then an
            // exception will be thrown and the failover partner will
            // be used in the first catch block below.

            // Execute a SQL statement that inserts some data.

            // Note that the following statement assumes that the
            // TestTable table has been created in the AdventureWorks
            // sample database.
            stmt.executeUpdate("INSERT INTO TestTable (Col2, Col3) VALUES ('a', 10)");
        }
        catch (SQLException se) {
            System.out.println("Connection to principal server failed, " + "trying the mirror server.");
            // The connection to the principal server failed,
            // try the mirror server which may now be the new
            // principal server.
            try (Connection con = DriverManager.getConnection(connectionUrl);
                    Statement stmt = con.createStatement();) {
                System.out.println("Connected to the new principal server.");
                stmt.executeUpdate("INSERT INTO TestTable (Col2, Col3) VALUES ('a', 10)");
            }
            // Handle any errors that may have occurred.
            catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## <a name="see-also"></a>Vedere anche

[Connessione a SQL Server con il driver JDBC](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)
