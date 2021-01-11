---
title: Generatori di stringhe di connessione
description: Informazioni sulle classi del generatore di stringhe di connessione usate per diversi provider in ADO.NET, che ereditano tutte da DbConnectionStringBuilder.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 8434b608-c4d3-43d3-8ae3-6d8c6b726759
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 38c1ad34fb4819460c08f00da4c1dbd31749b7cb
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771434"
---
# <a name="connection-string-builders"></a>Generatori di stringhe di connessione

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Nelle versioni precedenti di ADO.NET il controllo in fase di compilazione delle stringhe di connessione con valori di stringhe concatenate non veniva eseguito, quindi in fase di esecuzione una parola chiave non corretta avrebbe generato una <xref:System.ArgumentException>. Il provider di dati Microsoft SqlClient per SQL Server comprende la classe del generatore di stringhe di connessione fortemente tipizzata <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=nameWithType> che eredita da <xref:System.Data.Common.DbConnectionStringBuilder>.

## <a name="connection-string-injection-attacks"></a>Attacchi injection alle stringhe di connessione

Un attacco injection alle stringhe di connessione può verificarsi quando si usa la concatenazione dinamica di stringhe per compilare stringhe di connessione basate sull'input dell'utente. Se la stringa non viene convalidata e il testo o i caratteri dannosi non vengono convertiti in caratteri di escape, un utente non autorizzato potrebbe accedere a dati sensibili o ad altre risorse del server. Ad esempio, un utente non autorizzato potrebbe eseguire un attacco specificando un punto e virgola e aggiungendo un altro valore. La stringa di connessione viene analizzata usando un algoritmo "**priorità all'ultimo**" e l'input ostile viene sostituito con un valore legittimo.

Le classi di generatori di stringhe di connessione sono progettate per eliminare la necessità di basarsi su congetture e proteggersi da errori di sintassi e vulnerabilità della sicurezza. Forniscono metodi e proprietà che corrispondono alle coppie chiave/valore note consentite dal provider di dati. Ogni classe mantiene una raccolta fissa di sinonimi e può essere convertita da un sinonimo al nome di chiave noto. Vengono eseguiti controlli per rilevare coppie chiave/valore valide e le coppie non valide generano un'eccezione. Inoltre, i valori inseriti vengono gestiti in modo sicuro.

Nell'esempio seguente viene illustrata la modalità con cui <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> gestisce un valore aggiuntivo inserito per l'impostazione `Initial Catalog`.

[!code-csharp[SqlConnectionStringBuilder_InjectionAttack#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_InjectionAttack.cs#1)]

L'output mostra che <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> ha gestito correttamente tale inserimento eseguendo l'escape del valore aggiuntivo racchiuso tra virgolette doppie anziché aggiungerlo alla stringa di connessione quale nuova coppia chiave/valore.

```output
data source=(local);Integrated Security=True;
initial catalog="AdventureWorks;NewValue=Bad"
```

## <a name="build-connection-strings-from-configuration-files"></a>Creare stringhe di connessione da file di configurazione

Se determinati elementi di una stringa di connessione sono noti in anticipo, possono essere archiviati in un file di configurazione e recuperati in fase di esecuzione per costruire una stringa di connessione completa. Ad esempio, è possibile che il nome del database sia noto in anticipo, ma non il nome del server. Oppure è possibile che si desideri che un utente specifichi un nome utente e una password in fase di esecuzione senza avere la possibilità di inserire altri valori nella stringa di connessione.

Uno dei costruttori di overload per un generatore di stringhe di connessione accetta <xref:System.String> come argomento, consentendo di specificare una stringa di connessione parziale che può essere quindi completata dall'input dell'utente. La stringa di connessione parziale può essere archiviata in un file di configurazione e recuperata in fase di esecuzione.

> [!NOTE]
> Lo spazio dei nomi <xref:System.Configuration> consente l'accesso a livello di codice ai file di configurazione che usano <xref:System.Web.Configuration.WebConfigurationManager> per le applicazioni Web e <xref:System.Configuration.ConfigurationManager> per le applicazioni Windows. Per altre informazioni sull'utilizzo delle stringhe di connessione e dei file di configurazione, vedere [Connection Strings and Configuration Files](connection-strings-and-configuration-files.md) (Stringhe di connessione e file di configurazione).

### <a name="example"></a>Esempio

In questo esempio vengono illustrati il recupero di una stringa di connessione da un file di configurazione e il relativo completamento tramite l'impostazione delle proprietà <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A>, <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.UserID%2A> e <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> di <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>. Il file di configurazione viene definito come segue.

```xml
<connectionStrings>
  <clear/>
  <add name="partialConnectString"
    connectionString="Initial Catalog=Northwind;"
    providerName="Microsoft.Data.SqlClient" />
</connectionStrings>
```

> [!NOTE]
> È necessario impostare un riferimento a `System.Configuration.dll` nel progetto affinché il codice venga eseguito.

[!code-csharp[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_UserNamePwd.cs#1)]
  
## <a name="see-also"></a>Vedere anche

- [Stringhe di connessione](connection-strings.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
