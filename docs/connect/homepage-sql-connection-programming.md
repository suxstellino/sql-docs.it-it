---
title: Home page per la programmazione client SQL | Microsoft Docs
description: Pagina con collegamenti annotati a download e documentazione per diversi linguaggi e sistemi operativi, per la connessione a SQL Server o al database SQL di Azure.
author: David-Engel
ms.date: 11/07/2018
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-daveng
ms.author: v-daenge
ms.openlocfilehash: 10d04306621722abd9c9a170028ba632a8d95c9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045501"
---
# <a name="homepage-for-client-programming-to-microsoft-sql-server"></a>Home page per la programmazione client per Microsoft SQL Server


Questa è la home page sulla programmazione client per l'interazione con Microsoft SQL Server e con il database SQL di Azure nel cloud. Questo articolo contiene le informazioni seguenti:

- Elenca e descrive le combinazioni di linguaggio e driver disponibili.
  - Le informazioni disponibili riguardano i sistemi operativi Linux (Ubuntu e altri), macOS e Windows.
- Mette a disposizione collegamenti a documentazione dettagliata per ogni combinazione.
- Visualizza le aree e le sottoaree della documentazione gerarchica per linguaggi specifici, ove appropriato.


#### <a name="azure-sql-database"></a>database SQL di Azure

In tutti i linguaggi specifici, il codice che consente la connessione a SQL Server è quasi identico al codice per la connessione al database SQL di Azure.

Per i dettagli sulle stringhe di connessione per la connessione al database SQL di Azure, vedere:

- [Usare .NET Core (C#) per eseguire query su un database SQL di Azure](/azure/sql-database/sql-database-connect-query-dotnet-core).
- Altri articoli relativi ad altri linguaggi e al database SQL di Azure che si trovano vicini all'articolo precedente nel sommario. Vedere, ad esempio, [Usare PHP per eseguire query su un database SQL di Azure](/azure/sql-database/sql-database-connect-query-php).


#### <a name="build-an-app-webpages"></a>Pagine Web di compilazione app

Le pagine Web di *compilazione app* presentano esempi di codice e informazioni di configurazione in un formato alternativo. Per altre informazioni, vedere la [sezione *Sito web di compilazione app*](#an-204-aka-ms-sqldev) più avanti in questo articolo.



<a name="an-050-languages-clients" />

## <a name="languages-and-drivers-for-client-programs"></a>Linguaggi e driver per programmi client


Nella tabella seguente, l'immagine di ogni linguaggio è un collegamento che consente di visualizzare dettagli sull'uso del linguaggio corrispondente con SQL Server. Ogni collegamento passa a una sezione successiva di questo articolo.

:::row:::
    :::column:::
        [![Logo di C#][image-ref-320-csharp]](#an-110-ado-net-docu)  

        [![Logo di Node.js][image-ref-340-node]](#an-140-node-js-docu)  

        [![Logo di Python][image-ref-370-python]](#an-180-python-docu)  
    :::column-end:::
    :::column:::
        [![ORM Entity Framework, di .NET Framework][image-ref-333-ef]](#an-116-csharp-ef-orm)  

        [**`ODBC for C++`**](#an-160-odbc-cpp-docu)<br/>[![cpp-big-plus][image-ref-322-cpp]](#an-160-odbc-cpp-docu)  

        [![Logo di Ruby][image-ref-380-ruby]](#an-190-ruby-docu)  
    :::column-end:::
    :::column:::
        [![Logo di Java][image-ref-330-java]](#an-130-jdbc-docu)  

        [![Logo di PHP][image-ref-360-php]](#an-170-php-docu)  
    :::column-end:::
:::row-end:::

#### <a name="downloads-and-installs"></a>Download e installazioni

L'articolo seguente è dedicato al download e all'installazione di diversi driver di connessione SQL per l'uso tramite i linguaggi di programmazione:

- [Driver di SQL Server](./sql-connection-libraries.md)



<a name="an-110-ado-net-docu" />

## <a name="c-logoimage-ref-320-csharp-c-using-adonet"></a>![Logo di C#][image-ref-320-csharp] C# con ADO.NET

I linguaggi gestiti .NET, ad esempio C# e Visual Basic, sono i linguaggi più comuni che usano ADO.NET. *ADO.NET* è il nome colloquiale di un subset di classi di .NET Framework.

#### <a name="code-examples"></a>Esempi di codice

| Esempio | Descrizione |
| :-- | :-- |
| [Modello di verifica per la connessione a SQL tramite ADO.NET](./ado-net/step-3-connect-sql-ado-net.md) | Piccolo esempio di codice concentrato sulla connessione a SQL Server e sull'esecuzione di query su questo tipo di database. |
| [Connettersi in modo resiliente a SQL con ADO.NET](./ado-net/step-4-connect-resiliently-sql-ado-net.md) | Logica di ripetizione dei tentativi in un esempio di codice, poiché per le connessioni possono occasionalmente verificarsi momenti di perdita della connettività.<br /><br />La logica di ripetizione dei tentativi è appropriata per le connessioni gestite tramite Internet a qualsiasi database cloud, ad esempio il database SQL di Azure. |
| [Database SQL di Azure: dimostrazione dell'uso di .NET Core in Windows/Linux/macOS per creare un programma C# per la connessione e l'esecuzione di query](/azure/sql-database/sql-database-connect-query-dotnet-core) | Esempio per il database SQL di Azure. |
| [Compilazione app: C#, ADO.NET, Windows](https://www.microsoft.com/sql-server/developer-get-started/csharp/win/) | Informazioni di configurazione ed esempi di codice. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Documentazione

| Area | Descrizione |
| :-- | :-- |
| [C# con ADO.NET](./ado-net/microsoft-ado-net-sql-server.md)| Radice della documentazione Microsoft. |
| [Spazio dei nomi: System.Data](/dotnet/api/system.data) | Set di classi usato per ADO.NET. |
| [Spazio dei nomi: Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.SqlClient) | Set di classi usato per il provider di dati Microsoft .NET per SQL Server |
| &nbsp; | <br /> |



<a name="an-116-csharp-ef-orm" />

## <a name="entity-framework-logoimage-ref-333-ef-entity-framework-ef-with-cx23"></a>![Logo di Entity Framework][image-ref-333-ef] Entity Framework (EF) con C&#x23;

Entity Framework (EF) è dotato della funzionalità ORM (Object-Relational Mapping). Questa funzionalità rende più semplice per il codice sorgente di programmazione orientata a oggetti la modifica dei dati recuperati da un database SQL relazionale.

EF presenta relazioni dirette o indirette con le tecnologie seguenti:

- .NET Framework
- [LINQ to SQL](/dotnet/framework/data/adonet/sql/linq/) o [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities)
- Miglioramenti della sintassi del linguaggio, ad esempio l'operatore **=>** in C#.
- Programmi molto utili, che generano codice sorgente per le classi di cui viene eseguito il mapping alle tabelle del database SQL. Ad esempio, [EdmGen.exe](/dotnet/framework/data/adonet/ef/edm-generator-edmgen-exe).


#### <a name="original-ef-and-new-ef"></a>EF originale e nuovo EF

La [pagina iniziale di Entity Framework](/ef/) presenta Entity Framework con una descrizione simile alla seguente:

- Entity Framework è un mapper relazionale a oggetti (O/RM, Object-Relational Mapper) che consente agli sviluppatori .NET di usare un database tramite oggetti .NET. Elimina la necessità della maggior parte del codice sorgente di accesso ai dati che in genere gli sviluppatori devono scrivere.

*Entity Framework* è un nome condiviso da due rami di codice sorgente distinti. Un ramo di EF è meno recente e il relativo codice sorgente può ora essere gestito dal pubblico. L'altro ramo di EF è nuovo. I due rami di EF sono descritti di seguito:

| Versione | Descrizione |
| :-- | :-- |
| [EF 6.x](/ef/ef6/) | Microsoft ha rilasciato EF per la prima volta nel mese di agosto 2008. Nel marzo 2015 Microsoft ha annunciato che Entity Framework 6.x sarebbe stata la versione finale sviluppata dall'azienda e ne ha rilasciato il codice sorgente al pubblico dominio.<br /><br />Inizialmente EF faceva parte di .NET Framework, ma con la versione 6.x EF ne è stato rimosso.<br /><br />[Codice sorgente di Entity Framework 6.x in GitHub, nel repository *aspnet/EntityFramework6*](https://github.com/aspnet/EntityFramework6) |
| [EF Core](/ef/core/) | Microsoft ha rilasciato EF Core, sviluppato più di recente, nel mese di giugno 2016. Progettato per offrire maggiore flessibilità e portabilità, EF Core può essere eseguito all'interno di altri sistemi operativi, oltre a Microsoft Windows. EF Core può interagire con altri database, oltre a Microsoft SQL Server e agli altri database relazionali.<br /><br />**Esempi di codice C&#x23;:**<br />[Introduzione a Entity Framework Core](/ef/core/get-started/index)<br />[Introduzione a EF Core in .NET Framework con un database esistente](/ef/core/get-started/full-dotnet/existing-db) |
| &nbsp; | <br /> |

EF e le tecnologie correlate sono molto avanzate e rappresentano una grande quantità di informazioni per gli sviluppatori che vogliono padroneggiare l'intera area.

&nbsp;



<a name="an-130-jdbc-docu" />

## <a name="java-logoimage-ref-330-java-java-and-jdbc"></a>![Logo di Java][image-ref-330-java] Java e JDBC

Microsoft offre un driver Java Database Connectivity (JDBC) da usare con SQL Server o con il database SQL di Azure. Si tratta di un driver JDBC di tipo 4 che offre connettività di database tramite le interfacce API (Application Program Interface) JDBC standard.

#### <a name="code-examples"></a>Esempi di codice

| Esempio | Descrizione |
| :-- | :-- |
| [Esempi di codice](./jdbc/sample-jdbc-driver-applications.md) | Esempi di codice che consentono di apprendere tipi di dati, set di risultati e dati di grandi dimensioni. |
| [Esempio di URL di connessione](./jdbc/connection-url-sample.md) | Descrive come usare un URL di connessione per connettersi a SQL Server e quindi come usarlo per applicare un'istruzione SQL per il recupero di dati. |
| [Esempio di origine dati](./jdbc/data-source-sample.md) | Descrive come usare un'origine dati per connettersi a SQL Server e quindi come usare una stored procedure per recuperare dati. |
| [Usare Java per eseguire query su un database SQL di Azure](/azure/sql-database/sql-database-connect-query-java) | Esempio per il database SQL di Azure. |
| [Creare app Java usando SQL Server in Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/) | Informazioni di configurazione ed esempi di codice. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Documentazione

La documentazione di JDBC include le aree principali seguenti:

| Area | Descrizione |
| :-- | :-- |
| [Java Database Connectivity (JDBC)](./jdbc/microsoft-jdbc-driver-for-sql-server.md) | Radice della documentazione Microsoft su JDBC. |
| [Riferimento](./jdbc/reference/jdbc-driver-api-reference.md) | Interfacce, classi e membri. |
| [Guida di programmazione per il driver PHP per SQL](./jdbc/programming-guide-for-jdbc-sql-driver.md) | Informazioni di configurazione ed esempi di codice. |
| &nbsp; | <br /> |



<a name="an-140-node-js-docu" />

## <a name="nodejs-logoimage-ref-340-node-nodejs"></a>![Logo di Node.js][image-ref-340-node] Node.js

Con Node.js è possibile connettersi a SQL Server da Windows, Linux o macOS. La radice della documentazione di Node.js è disponibile [qui](./node-js/node-js-driver-for-sql-server.md).

Il driver di connessione Node.js per SQL Server è implementato in JavaScript. Il driver usa il protocollo TDS, supportato da tutte le versioni moderne di SQL Server. Il driver è un progetto open source, [disponibile in GitHub](https://tediousjs.github.io/tedious/).

#### <a name="code-examples"></a>Esempi di codice

| Esempio | Descrizione |
| :-- | :-- |
| [Modello di verifica per la connessione a SQL tramite Node.js](./node-js/step-3-proof-of-concept-connecting-to-sql-using-node-js.md) | Codice sorgente essenziale per la connessione a SQL Server e l'esecuzione di una query. |
| [Database SQL di Azure: usare Node.js per eseguire query](/azure/sql-database/sql-database-connect-query-nodejs) | Esempio per il database SQL di Azure nel cloud. |
| [Creare app Node.js per usare SQL Server in macOS](https://www.microsoft.com/sql-server/developer-get-started/node/mac/) | Informazioni di configurazione ed esempi di codice. |
| &nbsp; | <br /> |



<a name="an-160-odbc-cpp-docu" />

## <a name="odbc-for-c"></a>ODBC per C++

![Logo di ODBC][image-ref-350-odbc] ![cpp-big-plus][image-ref-322-cpp]

ODBC (Open Database Connectivity) è stato sviluppato negli anni '90 e precede .NET Framework. ODBC è progettato per essere indipendente da qualsiasi sistema di database specifico e da qualsiasi sistema operativo.

Nel corso degli anni sono stati creati e rilasciati numerosi driver ODBC da gruppi all'interno e all'esterno di Microsoft. La gamma dei driver interessa diversi linguaggi di programmazione client. L'elenco delle destinazioni dei dati va oltre SQL Server.

Altri driver di connettività usano ODBC internamente.

#### <a name="code-example"></a>Esempio di codice

- [Esempio di codice C++ che usa ODBC](../odbc/reference/sample-odbc-program.md)

#### <a name="documentation-outline"></a>Struttura della documentazione

Il contenuto ODBC in questa sezione si concentra sull'accesso a SQL Server o al database SQL di Azure da C++. La tabella seguente delinea una struttura approssimativa della documentazione principale per ODBC.


| Area | Area secondaria | Descrizione |
| :--- | :------ | :---------- |
| [ODBC per C++](./odbc/microsoft-odbc-driver-for-sql-server.md) | Radice della documentazione Microsoft. |
| [Linux-macOS](./odbc/linux-mac/system-requirements.md) | &nbsp; | Informazioni sull'uso di ODBC nei sistemi operativi Linux o macOS. |
| [Windows](./odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)     | &nbsp; | Informazioni sull'uso di ODBC nel sistema operativo Windows. |
| [Amministrazione](../odbc/admin/odbc-data-source-administrator.md) | &nbsp; | Strumento di amministrazione per la gestione delle origini dati ODBC. |
| [Microsoft](../odbc/microsoft/microsoft-supplied-odbc-drivers.md)  | &nbsp; | Diversi driver ODBC creati e offerti da Microsoft. |
| [Informazioni concettuali e di riferimento](../odbc/reference/introduction-to-odbc.md) | &nbsp; | Informazioni concettuali sull'interfaccia ODBC, oltre alle informazioni di riferimento tradizionali. |
| &nbsp; " | [Appendici](../odbc/reference/appendixes/odbc-appendixes.md)    | Tabelle di transizione di stato, libreria di cursori ODBC e altro ancora. |
| &nbsp; " | [Sviluppare app](../odbc/reference/develop-app/checking-feature-support-and-variability.md)  | Funzioni, handle e molto altro ancora. |
| &nbsp; " | [Sviluppare driver](../odbc/reference/develop-driver/developing-an-odbc-driver.md) | Come sviluppare un driver ODBC personalizzato, nel caso in cui si abbia un'origine dati specializzata. |
| &nbsp; " | [Installazione](../odbc/reference/install/odbc-subkey.md) | Installazione di ODBC, sottochiavi e altro ancora. |
| &nbsp; " | [Sintassi](../odbc/reference/syntax/odbc-reference.md)   | API per l'installazione, programma di installazione, conversione e accesso ai dati. |
| &nbsp; | &nbsp; | <br /> |



<a name="an-170-php-docu" />

## <a name="php-logoimage-ref-360-php-php"></a>![Logo di PHP][image-ref-360-php] PHP

È possibile usare PHP per interagire con SQL Server. La radice della documentazione di PHP è disponibile [qui](./php/microsoft-php-driver-for-sql-server.md).

#### <a name="code-examples"></a>Esempi di codice

| Esempio | Descrizione |
| :-- | :-- |
| [Modello di verifica per la connessione a SQL tramite PHP](./php/step-3-proof-of-concept-connecting-to-sql-using-php.md) | Piccolo esempio di codice concentrato sulla connessione a SQL Server e sull'esecuzione di query su questo tipo di database. |
| [Connettere in modo resiliente a SQL con PHP](./php/step-4-connect-resiliently-to-sql-with-php.md) | Logica di ripetizione dei tentativi in un esempio di codice, poiché per le connessioni via Internet e il cloud possono occasionalmente verificarsi momenti di perdita della connettività. |
| [Database SQL di Azure: usare PHP per eseguire query](/azure/sql-database/sql-database-connect-query-php) | Esempio per il database SQL di Azure. |
| [Creare app PHP per usare SQL Server in RHEL](https://www.microsoft.com/sql-server/developer-get-started/php/rhel/) | Informazioni di configurazione ed esempi di codice. |
| &nbsp; | <br /> |



<a name="an-180-python-docu" />

## <a name="python-logoimage-ref-370-python-python"></a>![Logo di Python][image-ref-370-python] Python


È possibile usare Python per interagire con SQL Server.

#### <a name="code-examples"></a>Esempi di codice

| Esempio | Descrizione |
| :-- | :-- |
| [Modello di verifica per la connessione a SQL con Python tramite pyodbc](./python/pyodbc/step-3-proof-of-concept-connecting-to-sql-using-pyodbc.md) | Piccolo esempio di codice concentrato sulla connessione a SQL Server e sull'esecuzione di query su questo tipo di database. |
| [Database SQL di Azure: usare Python per eseguire query](/azure/sql-database/sql-database-connect-query-python) | Esempio per il database SQL di Azure. |
| [Creare app PHP per usare SQL Server in SLES](https://www.microsoft.com/sql-server/developer-get-started/python/sles/) | Informazioni di configurazione ed esempi di codice. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Documentazione

| Area | Descrizione |
| :--- | :---------- |
| [Da Python a SQL Server](./python/python-driver-for-sql-server.md) | Radice della documentazione Microsoft. |
| [driver pymssql](./python/pymssql/python-sql-driver-pymssql.md) | Microsoft non gestisce né sottopone a test il driver pymssql.<br /><br />Il driver di connessione pymssql è un'interfaccia semplice per database SQL da usare nei programmi Python. Pymssql si basa su FreeTDS per fornire un'interfaccia Python DB-API (PEP-249) a Microsoft SQL Server. |
| [driver pyodbc](./python/pyodbc/python-sql-driver-pyodbc.md)   | Il driver di connessione pyodbc è un modulo Python open source che semplifica l'accesso ai database ODBC. Implementa la specifica DB API 2.0, ma è dotato di un numero di funzioni pratiche di Python ancora maggiore. |
| &nbsp; | <br /> |


<a name="an-190-ruby-docu" />

## <a name="ruby-logoimage-ref-380-ruby-ruby"></a>![Logo di Ruby][image-ref-380-ruby] Ruby

È possibile usare Ruby per interagire con SQL Server. La radice della documentazione di Ruby è disponibile [qui](./ruby/ruby-driver-for-sql-server.md).

#### <a name="code-examples"></a>Esempi di codice

| Esempio | Descrizione |
| :-- | :-- |
| [Modello di verifica per la connessione a SQL tramite Ruby](./ruby/step-3-proof-of-concept-connecting-to-sql-using-ruby.md) | Piccolo esempio di codice concentrato sulla connessione a SQL Server e sull'esecuzione di query su questo tipo di database. |
| [Database SQL di Azure: Usare Ruby per eseguire query](/azure/sql-database/sql-database-connect-query-ruby) | Esempio per il database SQL di Azure. |
| [Creare app Ruby per usare SQL Server in macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/) | Informazioni di configurazione ed esempi di codice. |
| &nbsp; | <br /> |



<a name="an-204-aka-ms-sqldev" />

## <a name="build-an-app-website-for-sql-client-development"></a>[Sito web di compilazione app per lo sviluppo di client SQL](https://www.microsoft.com/sql-server/developer-get-started/)


Nelle pagine Web di [*compilazione app*](https://www.microsoft.com/sql-server/developer-get-started/) è possibile scegliere da un lungo elenco di linguaggi di programmazione per la connessione a SQL Server. E il programma client può eseguire un'ampia gamma di sistemi operativi.

Il concetto di *compilazione app* enfatizza la semplicità e la completezza offerta agli sviluppatori agli inizi. I passaggi descrivono le attività seguenti:

1. Come installare Microsoft SQL Server
2. Come scaricare e installare strumenti e driver.
3. Come eseguire le configurazioni necessarie, a seconda del sistema operativo scelto.
4. Come compilare il codice sorgente fornito.
5. Come eseguire il programma.

Di seguito sono riportate alcune indicazioni approssimative dei dettagli disponibili nel sito Web:

#### <a name="java-on-ubuntu"></a>Java in Ubuntu

1. Configurare l'ambiente
    - Passaggio 1.1 installare SQL Server
    - Passaggio 1.2 Installare Java
    - Passaggio 1.3 Installare Java Development Kit (JDK)
    - Passaggio 1.4 Installare Maven
2. Creare un'applicazione Java con SQL Server
    - Passaggio 2.1 Creare un'app Java che si connette a SQL Server ed esegue query
    - Passaggio 2.2 Creare un'app Java che si connette a SQL Server usando il framework di ampia diffusione Hibernate
3. Rendere l'app Java fino a 100 volte più veloce
    - Passaggio 3.1 Creare un'app Java per illustrare indici columnstore

#### <a name="python-on-windows"></a>Python in Windows

1. Configurare l'ambiente
    - Passaggio 1.1 installare SQL Server
    - Passaggio 1.2 Installare Python
    - Passaggio 1.3 Installare il driver ODBC e l'utilità da riga di comando SQL per SQL Server
2. Creare un'applicazione Python con SQL Server
    - Passaggio 2.1 Installare il driver Python per SQL Server
    - Passaggio 2.2 Creare un database per l'applicazione
    - Passaggio 2.3 Creare un'app Python che si connette a SQL Server ed esegue query
3. Rendere l'app Python fino a 100 volte più veloce
    - Passaggio 3.1 Creare una nuova tabella con 5 milioni usando sqlcmd
    - Passaggio 3.2 Creare un'app Python che esegue una query su questa tabella e misura il tempo impiegato
    - Passaggio 3.3 Misurare il tempo impiegato per l'esecuzione della query
    - Passaggio 3.4 Aggiungere un indice columnstore alla tabella
    - Passaggio 3.5 Misurare il tempo impiegato per l'esecuzione della query con un indice columnstore

Gli screenshot seguenti offrono un'idea dell'aspetto del sito Web della documentazione per lo sviluppo SQL.

#### <a name="choose-a-language"></a>Scegliere una lingua

![Sito Web di sviluppo SQL, per iniziare][image-ref-390-aka-ms-sqldev-choose-language]

&nbsp;

#### <a name="choose-an-operating-system"></a>Scegliere un sistema operativo

![Sito Web di sviluppo SQL, Java Ubuntu][image-ref-400-aka-ms-sqldev-java-ubuntu]

&nbsp;



## <a name="other-development"></a>Altri tipi di sviluppo


Questa sezione mette a disposizione collegamenti per altre opzioni di sviluppo. Questi prevedono l'uso degli stessi linguaggi usati in genere per lo sviluppo di Azure. Le informazioni vanno oltre il database SQL di Azure e Microsoft SQL Server.

#### <a name="developer-hub-for-azure"></a>Hub per sviluppatori di Azure

- [Hub per sviluppatori di Azure](/azure/)
- [Azure for .NET developers](/dotnet/azure/) (Azure per sviluppatori .NET)
- [Azure for Java developers](/java/azure/) (Azure per sviluppatori Java)
- [Azure for Node.js developers](/nodejs/azure/) (Azure per sviluppatori Node.js)
- [Azure for Python developers](/python/azure/) (Azure per sviluppatori Python)
- [Creare un'app Web PHP in Azure](/azure/app-service-web/app-service-web-get-started-php)

#### <a name="other-languages"></a>Altri linguaggi

- [Creare app Go usando SQL Server in Windows](https://www.microsoft.com/sql-server/developer-get-started/go/windows/)



<!-- Image references. -->

[image-ref-322-cpp]: ./media/homepage-sql-connection-drivers/gm-cpp-4point-p61f.png
[image-ref-320-csharp]: ./media/homepage-sql-connection-drivers/gm-csharp-c10c.png
[image-ref-333-ef]: ./media/homepage-sql-connection-drivers/gm-entity-framework-ef20d.png
[image-ref-330-java]: ./media/homepage-sql-connection-drivers/gm-java-j18c.png
[image-ref-340-node]: ./media/homepage-sql-connection-drivers/gm-node-n30.png
[image-ref-350-odbc]: ./media/homepage-sql-connection-drivers/gm-odbc-ic55826-o35.png
[image-ref-360-php]: ./media/homepage-sql-connection-drivers/gm-php-php60.png
[image-ref-370-python]: ./media/homepage-sql-connection-drivers/gm-python-py72.png
[image-ref-380-ruby]: ./media/homepage-sql-connection-drivers/gm-ruby-un-r82.png
[image-ref-390-aka-ms-sqldev-choose-language]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png
[image-ref-400-aka-ms-sqldev-java-ubuntu]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png