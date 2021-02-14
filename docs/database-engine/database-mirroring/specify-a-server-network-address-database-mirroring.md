---
title: Specificare un indirizzo di rete del server (mirroring del database)
description: Informazioni su come specificare un indirizzo di rete del server per un endpoint del mirroring del database. Per una sessione di mirroring del database è necessario un indirizzo per ogni istanza del server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- database mirroring [SQL Server], endpoint
- endpoints [SQL Server], database mirroring
- server network addresses [SQL Server]
ms.assetid: a64d4b6b-9016-4f1e-a310-b1df181dd0c6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f8e778699add5318ca7c42f36ae9cb2c5243d2cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352232"
---
# <a name="specify-a-server-network-address-database-mirroring"></a>Specificare un indirizzo di rete del server (Mirroring del database)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Per impostare una sessione di mirroring del database, è necessario un indirizzo di rete del server per ogni istanza del server. Tale indirizzo deve identificare in maniera univoca l'istanza includendo un indirizzo di sistema e il numero di porta su cui l'istanza è in attesa.  
  
 Per poter specificare una porta in un indirizzo di rete del server, è necessario che l'endpoint del mirroring del database sia disponibile sull'istanza del server. Per altre informazioni, vedere [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
  
##  <a name="syntax-for-a-server-network-address"></a><a name="Syntax"></a> Sintassi per un indirizzo di rete del server  
 La sintassi per un indirizzo di rete del server presenta la struttura seguente:  
  
 TCP <strong>://</strong> *\<system-address>* <strong>:</strong> *\<port>*  
  
 dove  
  
-   *\<system-address>* è una stringa che identifica in modo univoco il computer di destinazione. In genere, l'indirizzo del server è un nome di sistema, se i sistemi si trovano nello stesso dominio, un nome di dominio completo o un indirizzo IP.  
  
    -   Se i sistemi si trovano nello stesso dominio, è possibile utilizzare il nome del computer, ad esempio `SYSTEM46`.  
  
    -   Per utilizzare un indirizzo IP, è necessario che esso sia univoco nell'ambiente. È consigliabile utilizzare un indirizzo IP solo se è statico. L'indirizzo IP può essere IP versione 4 (IPv4) o IP versione 6 (IPv6). Un indirizzo IPv6 deve essere racchiuso tra parentesi quadre, ad esempio **[** _<indirizzo_IPv6>_ **]** .  
  
         Per individuare l'indirizzo IP di un sistema, al prompt dei comandi di Windows immettere il comando **ipconfig** .  
  
    -   Il funzionamento del nome di dominio completo è garantito. Il nome di dominio completo è costituito da una stringa di indirizzo definita a livello locale con forme diverse a seconda della sua posizione. Spesso ma non sempre, un nome di dominio completo è un nome composto che include un nome computer e una serie di segmenti di dominio separati da virgole, ad esempio:  
  
         _nome_computer_ **.** _segmento_dominio_[... **.** _segmento_dominio_]  
  
         dove *nome_computer* è il nome di rete del computer che esegue l'istanza del server e *segmento_dominio*[... **.** _segmento_dominio_] è la parte rimanente delle informazioni sul dominio del server, ad esempio: `localinfo.corp.Adventure-Works.com`.  
  
         Il contenuto e il numero dei segmenti di dominio sono determinati all'interno della società o dell'organizzazione. Se non si conosce il nome di dominio completo del server, consultare l'amministratore di sistema.  
  
        > [!NOTE]  
        >  Per informazioni sull'individuazione di un nome di dominio completo, vedere "Individuazione del nome di dominio completo" di seguito in questo argomento.  
  
-   *\<port>* è il numero di porta usato dall'endpoint del mirroring dell'istanza del server partner. Per informazioni su come specificare un endpoint, vedere [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
     Un endpoint del mirroring del database può utilizzare qualsiasi porta disponibile nel computer. Ogni numero di porta su un sistema di computer deve essere associato a un solo endpoint e ogni endpoint a una singola istanza del server. In questo modo, istanze del server diverse sullo stesso server restano in attesa su endpoint diversi con porte diverse. Pertanto, la parte specificata nell'indirizzo di rete del server quando si imposta una sessione di mirroring del database dirigerà la sessione sempre all'istanza del server il cui endpoint è associato a tale porta.  
  
     Nell'indirizzo di rete del server di un'istanza del server, solo il numero della porta associata al relativo endpoint del mirroring distingue l'istanza dalle altre sul computer. Nella figura seguente vengono illustrati gli indirizzi di rete del server di due istanze del server su un unico computer. L'istanza predefinita utilizza la porta `7022` , mentre l'istanza denominata utilizza la porta `7033`. Gli indirizzi di rete di queste due istanze del server sono rispettivamente `TCP://MYSYSTEM.Adventure-works.MyDomain.com:7022` e `TCP://MYSYSTEM.Adventure-works.MyDomain.com:7033`. Si noti che l'indirizzo non include il nome dell'istanza del server.  
  
     ![Indirizzi di rete del server per un'istanza predefinita](../../database-engine/availability-groups/windows/media/dbm-2-instances-ports-1-system.gif "Indirizzi di rete del server per un'istanza predefinita")  
  
     Per individuare la porta attualmente associata all'endpoint di mirroring del database per un'istanza del server, utilizzare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente:  
  
    ```  
    SELECT type_desc, port FROM sys.tcp_endpoints  
    ```  
  
     Individuare la riga il cui valore **type_desc** è "DATABASE_MIRRORING" e usare il numero di porta corrispondente.  
  
### <a name="examples"></a>Esempi  
  
#### <a name="a-using-a-system-name"></a>R. Utilizzo di un nome di sistema  
 L'indirizzo di rete del server seguente specifica un nome di sistema, `SYSTEM46`, e la porta `7022`.  
  
```  
ALTER DATABASE AdventureWorks SET PARTNER ='tcp://SYSTEM46:7022';  
```  
  
#### <a name="b-using-a-fully-qualified-domain-name"></a>B. Utilizzo di un nome di dominio completo  
 L'indirizzo di rete del server seguente specifica un nome di dominio completo, `DBSERVER8.manufacturing.Adventure-Works.com`, e la porta `7024`.  
  
```  
ALTER DATABASE AdventureWorks SET PARTNER ='tcp://DBSERVER8.manufacturing.Adventure-Works.com:7024';  
```  
  
#### <a name="c-using-ipv4"></a>C. Utilizzo di IPv4  
 L'indirizzo di rete del server seguente specifica un indirizzo IPv4, `10.193.9.134`, e la porta `7023`.  
  
```  
ALTER DATABASE AdventureWorks SET PARTNER ='tcp://10.193.9.134:7023';  
```  
  
#### <a name="d-using-ipv6"></a>D. Utilizzo di IPv6  
 L'indirizzo di rete del server seguente include un indirizzo IPv6, `2001:4898:23:1002:20f:1fff:feff:b3a3`, e la porta `7022`.  
  
```  
ALTER DATABASE AdventureWorks SET PARTNER ='tcp://[2001:4898:23:1002:20f:1fff:feff:b3a3]:7022';  
```  
  
## <a name="finding-the-fully-qualified-domain-name"></a>Individuazione del nome di dominio completo  
 Per individuare il nome di dominio completo di un sistema, al prompt dei comandi di Windows immettere:  
  
 **IPCONFIG /ALL**  
  
 Per ottenere il nome di dominio completo, concatenare i valori di *<host_name>* e *<Primary_Dns_Suffix>* come riportato di seguito:  
  
 _<host_name>_ **.** _<Primary_Dns_Suffix>_  
  
 Ad esempio, la configurazione IP  
  
 `Host Name  .  .  .  .  .  .  : MYSERVER`  
  
 `Primary Dns Suffix  .  .  .  : mydomain.Adventure-Works.com`  
  
 corrisponde al nome di dominio completo seguente:  
  
 `MYSERVER.mydomain.Adventure-Works.com`  
  
##  <a name="examples"></a><a name="Examples"></a> Esempi  
 Nell'esempio seguente viene illustrato l'indirizzo di rete del server per un'istanza del server in un computer denominato `REMOTESYSTEM3` in un altro dominio. Il nome di dominio è `NORTHWEST.ADVENTURE-WORKS.COM`e la porta dell'endpoint del mirroring del database è `7025`. Considerati questi componenti di esempio, l'indirizzo di rete del server è:  
  
 `TCP://REMOTESYSTEM3.NORTHWEST.ADVENTURE-WORKS.COM:7025`  
  
 Nell'esempio seguente viene indicato l'indirizzo di rete del server per un'istanza del server in un computer denominato `DBSERVER1`. Questo sistema si trova nel dominio locale ed è identificato in maniera univoca mediante il nome del sistema. La porta dell'endpoint del mirroring del database è `7022`.  
  
 `TCP://DBSERVER1:7022`  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  
