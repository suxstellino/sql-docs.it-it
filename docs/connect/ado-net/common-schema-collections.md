---
title: Raccolte di schemi comuni
description: Descrizione di tutte le raccolte di schemi comuni supportate da tutti i provider gestiti .NET.
ms.date: 11/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: f84cc2940e56116b9cef4600b21fe742f4ae2d07
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051324"
---
# <a name="common-schema-collections"></a>Raccolte di schemi comuni

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le raccolte di schemi comuni sono le raccolte di schemi implementate da ciascun provider gestito .NET. È possibile eseguire una query in un provider gestito .NET per determinare l'elenco delle raccolte di schemi supportate chiamando il metodo **GetSchema** senza argomenti oppure con il nome della raccolta di schemi "MetaDataCollections". In questo modo verrà restituito un oggetto <xref:System.Data.DataTable> con un elenco delle raccolte di schemi supportati, il numero delle restrizioni supportate da ciascuna raccolta e il numero di parti identificatore usate. Tutte le colonne richieste vengono descritte in queste raccolte. I provider hanno la possibilità di aggiungere colonne, se necessario. Ad esempio, il provider di dati Microsoft SqlClient per SQL Server aggiunge ParameterName alla raccolta delle restrizioni.  

Se un provider non è in grado di determinare il valore di una colonna richiesta, verrà restituito null.  
  
Per altre informazioni sull'uso dei metodi **GetSchema**, vedere [GetSchema e raccolte di schemi](getschema-and-schema-collections.md).  

## <a name="metadatacollections"></a>MetaDataCollections  

Questa raccolta di schemi espone informazioni su tutte le raccolte di schemi supportate dal provider di dati Microsoft SqlClient per SQL Server attualmente usate per la connessione al database.  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|CollectionName|string|Il nome della raccolta da passare al metodo **GetSchema** per restituire la raccolta.|  
|NumberOfRestrictions|INT|Il numero di restrizioni che è possibile specificare per la raccolta.|  
|NumberOfIdentifierParts|INT|Il numero di parti nel nome dell'oggetto di database/identificatore composito. Ad esempio, in SQL Server 3 corrisponde alle tabelle e 4 alle colonne.|  

## <a name="datasourceinformation"></a>DataSourceInformation  

Questa raccolta di schemi espone informazioni sull'origine dati a cui è attualmente connesso il provider di dati Microsoft SqlClient per SQL Server.  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|CompositeIdentifierSeparatorPattern|string|L'espressione regolare che corrisponde ai separatori compositi in un identificatore composito. Ad esempio, "\\". (per SQL Server).<br /><br /> Un identificatore composito viene generalmente usato come nome di oggetto di database, ad esempio: pubs.dbo.authors o pubs\@dbo.authors.<br /><br /> Per SQL Server, usare l'espressione regolare "\\.". |  
|DataSourceProductName|string|Nome del prodotto a cui accede il provider, ad esempio "SQLServer".|  
|DataSourceProductVersion|string|Indica la versione del prodotto a cui ha avuto accesso il provider, nel formato nativo delle origini dati e non in formato Microsoft.<br /><br /> In alcuni casi DataSourceProductVersion e DataSourceProductVersionNormalized corrisponderanno allo stesso valore. |  
|DataSourceProductVersionNormalized|string|Una versione normalizzata per l'origine dati, che è possibile confrontare con `String.Compare()`. Il formato è lo stesso in tutte le versioni del provider per evitare che la versione 10 venga elencata tra la versione 1 e la versione 2.<br /><br /> Ad esempio, SQL Server usa il formato "nn.nn.nnnn" tipico di Microsoft.<br /><br /> In alcuni casi DataSourceProductVersion e DataSourceProductVersionNormalized corrisponderanno allo stesso valore. |  
|GroupByBehavior|<xref:System.Data.Common.GroupByBehavior>|Specifica il rapporto tra le colonne nella clausola GROUP BY e le colonne non aggregate nell'elenco di selezione.|  
|IdentifierPattern|string|Un'espressione regolare che corrisponde a un identificatore e dispone di un valore di corrispondenza dell'identificatore. Ad esempio "[A-Za-z0-9_#$]".|  
|IdentifierCase|<xref:System.Data.Common.IdentifierCase>|Indica se per gli identificatori non delimitati viene eseguita la distinzione tra maiuscole e minuscole.|  
|OrderByColumnsInSelect|bool|Specifica se le colonne nella clausola ORDER BY devono essere presenti nell'elenco di selezione. Il valore true indica che le colonne devono risultare nell'elenco di selezione, mentre il valore false indica che non è necessario.|  
|ParameterMarkerFormat|string|Una stringa di formato che rappresenta la modalità di formattazione di un parametro.<br /><br /> Se i parametri denominati sono supportati dall'origine dati, il primo segnalibro di questa stringa deve trovarsi nella posizione in cui verrà formattato il nome del parametro.<br /><br /> Ad esempio, se l'origine dati prevede che i parametri vengano denominati e includano il prefisso ':', il risultato sarà ":{0}". Quando si esegue la formattazione con il nome di parametro "p1" la stringa risultante sarà ":p1".<br /><br /> Se l'origine dati prevede che i parametri presentino il prefisso '\@' che tuttavia è già incluso nel nome, il risultato sarà '{0}', mentre il risultato della formattazione di un parametro denominato "\@p1" sarà semplicemente "\@p1".<br /><br /> Per le origini dati che non prevedono parametri denominati, bensì l'uso del carattere '?', la stringa di formato può essere specificata semplicemente come '?', in modo da ignorare il nome del parametro. |  
|ParameterMarkerPattern|string|Un'espressione regolare che corrisponde al marcatore di parametro. Avrà un valore corrispondente per il nome del parametro, se disponibile.<br /><br /> Ad esempio, se i parametri denominati sono supportati con un carattere '\@' iniziale incluso nel nome del parametro, il risultato sarà: "(\@[A-Za-z0-9_$#]*)".<br /><br /> Se tuttavia i parametri denominati sono supportati con un carattere ':' iniziale non incluso nel nome del parametro, il risultato sarà: ":([A-Za-z0-9_$#]\*)".<br /><br /> Naturalmente, se l'origine dati non supporta i parametri denominati, il risultato sarà semplicemente "?".|  
|ParameterNameMaxLength|INT|La lunghezza massima del nome del parametro in caratteri. In Visual Studio si presuppone che se i nomi di parametri sono supportati, il valore minimo per la lunghezza massima corrisponderà a 30 caratteri.<br /><br /> Se l'origine dati non supporta i parametri denominati, questa proprietà restituisce zero.|  
|ParameterNamePattern|string|Un'espressione regolare che corrisponde ai nomi di parametro validi. Origini dati diverse hanno regole diverse per i caratteri che è possibile usare con i nomi di parametro.<br /><br /> In Visual Studio si presuppone che se sono supportati i nomi di parametro, i caratteri "\p{Lu}\p{Ll}\p{Lt}\p{Lm}\p{Lo}\p{Nl}\p{Nd}" rappresentano il set di caratteri minimo supportato, valido per i nomi di parametro.|  
|QuotedIdentifierPattern|string|Un'espressione regolare che corrisponde a un identificatore delimitato e dispone di un valore di corrispondenza dell'identificatore senza virgolette. Ad esempio, se l'origine dati ha usato le virgolette doppie per identificare gli identificatori delimitati, il risultato sarà: "(([^\\"]&#124;\\"\\")*)".|  
|QuotedIdentifierCase|<xref:System.Data.Common.IdentifierCase>|Indica se per gli identificatori delimitati viene eseguita la distinzione tra maiuscole e minuscole.|  
|StatementSeparatorPattern|string|Un'espressione regolare che corrisponde al separatore di istruzione.|  
|StringLiteralPattern|string|Un'espressione regolare che corrisponde a una stringa letterale e dispone di un valore di corrispondenza del valore letterale. Ad esempio, se l'origine dati ha usato le virgolette singole per identificare le stringhe, il risultato sarà: "('([^']&#124;'')*')"'|  
|SupportedJoinOperators|<xref:System.Data.Common.SupportedJoinOperators>|Specifica i tipi di istruzioni join di SQL supportati dall'origine dati.|  

## <a name="datatypes"></a>DataTypes  

Questa raccolta di schemi espone informazioni sui tipi di dati supportati dal database a cui è attualmente connesso il provider di dati Microsoft SqlClient per SQL Server.  

|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|TypeName|string|Il nome del tipo di dati specifico del provider.|  
|ProviderDbType|INT|Valore di tipo specifico del provider da usare quando si specifica un tipo di parametro. Ad esempio, SqlDbType.Money.|  
|ColumnSize|long|La lunghezza di una colonna o di un parametro non numerico fa riferimento alla lunghezza massima o definita per questo tipo dal provider.<br /><br /> Per i dati di tipo carattere, rappresenta la lunghezza massima o definita in unità, definita dall'origine dati. <br /><br /> Per i tipi di dati data-ora, rappresenta la lunghezza della rappresentazione stringa (se si suppone la massima precisione consentita del componente in frazioni di secondo).<br /><br /> Se il tipo di dati è numerico, rappresenta il limite superiore sulla massima precisione del tipo di dati.|  
|CreateFormat|string|Stringa di formato che indica come aggiungere la colonna a un'istruzione di definizione dei dati, come CREATE TABLE. Ciascun elemento nella matrice CreateParameter deve essere rappresentato da un "marcatore di parametro" nella stringa di formato.<br /><br /> Ad esempio, per il tipo di dati SQL DECIMAL sono necessarie una precisione e una scala. In questo caso, la stringa di formato sarà "DECIMAL({0},{1})".|  
|CreateParameters|string|I parametri di creazione da specificare durante la creazione di una colonna di questo tipo di dati. Ciascun parametro di creazione viene elencato nella stringa, separato da una virgola nell'ordine in cui deve essere fornito.<br /><br /> Ad esempio, per il tipo di dati SQL DECIMAL sono necessarie una precisione e una scala. In questo caso, i parametri di creazione devono contenere la stringa "precision, scale".<br /><br /> In un comando di testo per creare una colonna DECIMAL con una precisione di 10 e una scala di 2, il valore della colonna CreateFormat può essere DECIMAL({0},{1})" e la specifica completa del tipo sarà DECIMAL(10,2).|  
|DataType|string|Nome del tipo .NET del tipo di dati.|  
|IsAutoincrementable|bool|true—I valori di questo tipo di dati possono essere a incremento automatico.<br /><br /> false—I valori di questo tipo di dati possono non essere a incremento automatico.<br /><br /> Notare che anche se una colonna di questo tipo di dati può essere a incremento automatico, non significa che tutte le colonne di questo tipo lo siano.|  
|IsBestMatch|bool|true - Il tipo di dati è la corrispondenza più appropriata tra tutti i tipi di dati nell'archivio dati e il tipo di dati .NET indicato dal valore nella colonna DataType.<br /><br /> false—Il tipo di dati non rappresenta la corrispondenza più appropriata.<br /><br /> Per ciascun set di righe in cui il valore della colonna DataType è lo stesso, la colonna IsBestMatch è impostata su true in una sola riga.|  
|IsCaseSensitive|bool|true—Il tipo di dati è di tipo carattere e viene fatta distinzione tra maiuscole e minuscole.<br /><br /> false—Il tipo di dati è di tipo carattere e viene fatta distinzione tra maiuscole e minuscole.|  
|IsFixedLength|bool|true—Le colonne di questo tipo di dati create dal DDL (Data Definition Language) saranno di lunghezza fissa.<br /><br /> false—Le colonne di questo tipo di dati create dal DDL saranno di lunghezza variabile.<br /><br /> DBNull.Value—Non è noto se il provider eseguirà il mapping del campo con una colonna di lunghezza fissa o di lunghezza variabile.|  
|IsFixedPrecisionScale|bool|true—Il tipo di dati dispone di una precisione e una scala fisse.<br /><br /> false—Il tipo di dati non dispone di una precisione e una scala fisse.|  
|IsLong|bool|true—Il tipo di dati contiene dati molto lunghi. La definizione dei dati molto lunghi è specifica del provider.<br /><br /> false—Il tipo di dati non contiene dati molto lunghi.|  
|IsNullable|bool|true—Il tipo di dati ammette valori null.<br /><br /> false—Il tipo di dati non ammette valori null.<br /><br /> DBNull.Value—Non è noto se il tipo di dati ammette valori null.|  
|IsSearchable|bool|true—Il tipo di dati può essere usato in una clausola WHERE con qualsiasi operatore ad eccezione del predicato LIKE.<br /><br /> false—Il tipo di dati non può essere usato in una clausola WHERE con qualsiasi operatore ad eccezione del predicato LIKE.|  
|IsSearchableWithLike|bool|true—Il tipo di dati può essere usato con il predicato LIKE<br /><br /> false—Il tipo di dati non può essere usato con il predicato LIKE.|  
|IsUnsigned|bool|true—Il tipo di dati è unsigned.<br /><br /> false—Il tipo di dati è signed.<br /><br /> DBNull.Value—Non applicabile al tipo di dati.|  
|MaximumScale|short|Se l'indicatore di tipo è numerico, corrisponde al numero massimo di cifre consentito a destra del separatore decimale. Altrimenti sarà DBNull.Value.|  
|MinimumScale|short|Se l'indicatore di tipo è numerico, corrisponde al numero minimo di cifre consentito a destra del separatore decimale. Altrimenti sarà DBNull.Value.|  
|IsConcurrencyType|bool|true – Il tipo di dati viene aggiornato dal database ogni volta che la riga viene modificata e il valore della colonna è diverso da tutti i valori precedenti<br /><br /> false – Il tipo di dati non viene aggiornato dal database ogni volta che viene modificata la riga<br /><br /> DBNull.Value – il database non supporta questo tipo di dati|  
|IsLiteralSupported|bool|true – Il tipo di dati può essere espresso come valore letterale<br /><br /> false – Il tipo di dati non può essere espresso come valore letterale|  
|LiteralPrefix|string|Il prefisso applicato a un dato valore letterale.|  
|LiteralSuffix|string|Il suffisso applicato a un dato valore letterale.|  

## <a name="restrictions"></a>Restrizioni 

Questa raccolta di schemi espone informazioni sulle restrizioni supportate dal provider di dati Microsoft SqlClient per SQL Server attualmente usato per la connessione al database.  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|CollectionName|string|Il nome della raccolta a cui sono applicate queste restrizioni.|  
|RestrictionName|string|Il nome della restrizione nella raccolta.|  
|RestrictionDefault|string|Ignorato.|  
|RestrictionNumber|INT|La posizione effettiva nelle restrizioni delle raccolte in cui rientra questa particolare restrizione.|  

## <a name="reservedwords"></a>ReservedWords  

Questa raccolta di schemi espone informazioni sulle parole riservate dal database a cui è attualmente connesso il provider di dati Microsoft SqlClient per SQL Server.  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|ReservedWord|string|Parola riservata specifica del provider.|  

## <a name="see-also"></a>Vedere anche

- [Recupero di informazioni dello schema del database](retrieving-database-schema-information.md)
- [GetSchema e raccolte di schemi](getschema-and-schema-collections.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
