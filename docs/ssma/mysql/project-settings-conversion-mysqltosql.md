---
description: Impostazioni del progetto (conversione) (MySQLToSQL)
title: Impostazioni progetto (conversione) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 7ad5fe44-6445-4ba8-a457-5af792631f11
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4a5086d6b193ac13d621c3831ebc10602cfdd16f
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054492"
---
# <a name="project-settings-conversion-mysqltosql"></a>Impostazioni del progetto (conversione) (MySQLToSQL)
La pagina conversione della finestra di dialogo **Impostazioni progetto** contiene impostazioni che consentono di personalizzare il modo in cui SSMA converte la sintassi di MySQL nella sintassi SQL Server o SQL Azure.  
  
Il riquadro conversione è disponibile nelle finestre di dialogo **Impostazioni progetto** e **Impostazioni progetto predefinite** .  
  
-   Utilizzare la finestra di dialogo **Impostazioni progetto predefinite** per impostare le opzioni di configurazione per tutti i progetti. Per accedere alle impostazioni di conversione, nel menu **strumenti** selezionare **Impostazioni progetto predefinite**, selezionare il tipo di progetto di migrazione per cui è necessario visualizzare le impostazioni/changed dall'elenco a discesa **versione destinazione migrazione** , fare clic su **generale** nella parte inferiore del riquadro a sinistra e quindi selezionare **conversione**.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , quindi fare clic su **generale** nella parte inferiore del riquadro a sinistra e quindi su **conversione**.  
  
## <a name="options"></a>Opzioni  
  
### <a name="collate-clause"></a>Clausola COLLATE  
  
|**Termine**|**Definizione**|  
|-|-|  
|**Conversione esplicita della clausola COLLATE**|L'opzione di conversione esplicita della clausola COLLATE specifica come convertire le clausole COLLATE esplicite nel codice MySQL. Opzioni possibili: ignora e contrassegna con un avviso/genera un errore<br /><br />**Modalità predefinita**: ignore e contrassegnare con un avviso<br /><br />**Modalità ottimistica**: ignorare e contrassegnare con un avviso<br /><br />**Modalità completa**: ignorare e contrassegnare con un avviso|  
  
### <a name="column-constraints"></a>Vincoli di colonna  
  
|**Termine**|**Definizione**|  
|-|-|  
|**Genera vincolo per le colonne del tipo di dati ENUM**|Genera un vincolo per le colonne del tipo di dati ENUM nella tabella SQL Server o SQL Azure, se non è presente nella tabella MySQL. In caso affermativo, tutte le colonne convertite del tipo di dati ENUM saranno accompagnate dal vincolo CHECK che controlla il valore.<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: Sì|  
|**Genera vincolo per le colonne con tipo di dati SET**|Genera un vincolo per le colonne con tipo di dati SET nella tabella SQL Server o SQL Azure, se non è presente nella tabella MySQL. In caso affermativo, tutte le colonne convertite del tipo di dati SET saranno accompagnate dal vincolo CHECK che controlla il valore.<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: Sì|  
|**Genera vincolo per colonne di colonne di tipi di dati numerici senza segno**|Aggiungere il controllo della presenza di un valore non negativo a colonne di tipi di dati numerici senza segno.<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: Sì|  
|**Genera vincolo per le colonne con tipo di dati YEAR**|Genera un vincolo per le colonne con tipo di dati YEAR nella tabella SQL Server o SQL Azure, se non è presente nella tabella MySQL. In caso affermativo, tutte le colonne convertite del tipo di dati YEAR saranno accompagnate dal vincolo CHECK che controlla il valore.<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: Sì|  
  
### <a name="data-types"></a>Tipi di dati  
  
|**Termine**|**Definizione**|  
|-|-|  
|**Conversione del tipo di dati ENUM**|Specifica la modalità di conversione del tipo di dati di MySQL ENUM come conversione in NVARCHAR o Convert in numeric<br /><br />**Modalità predefinita**: conversione in nvarchar<br /><br />**Modalità ottimistica**: conversione in nvarchar<br /><br />**Modalità completa**: conversione in nvarchar|  
|**IMPOSTA conversione del tipo di dati**|Specifica il modo in cui convertire il tipo di dati di MySQL SET, convertirlo in NVARCHAR (L)/Convert in BINARY (L)<br /><br />**Modalità predefinita**: conversione in nvarchar (L)<br /><br />**Modalità ottimistica**: conversione in nvarchar (L)<br /><br />**Modalità completa**: conversione in nvarchar (L)|  
  
### <a name="generic"></a>Generico  
  
|**Termine**|**Definizione**|  
|-|-|  
|**Colonne senza valore predefinito in INSERT e REPLACE**|Se è impostata su' Yes ', tutte le istruzioni che fanno riferimento a tabelle che utilizzano motori archiviati diversi da ISAM e InnoDb devono essere contrassegnate con messaggi di conversione di avviso.<br /><br />**Modalità predefinita**: Aggiungi a elenco colonne<br /><br />**Modalità ottimistica**: Aggiungi a elenco colonne<br /><br />**Modalità completa**: aggiunta all'elenco di colonne|  
|**La conversione di divisione per zero produce**|Specifica se emulare MySQL senza ERROR_FOR_DIVISION_BY_ZERO comportamento.<br /><br />**Modalità predefinita**: errore<br /><br />**Modalità ottimistica**: errore<br /><br />**Modalità completa**: null|  
|**Operatore IN**|Specifica come convertire MySQL nell'operatore.<br /><br />**Modalità predefinita**: Converti sempre in in<br /><br />**Modalità ottimistica**: Converti sempre in in<br /><br />**Modalità completa**: espandere se necessario|  
|**Conversione di funzioni MySQL**|Specifica come convertire le funzioni standard di MySQL.<br /><br />**Modalità predefinita**: ottimistica<br /><br />**Modalità ottimistica**: ottimistica<br /><br />**Modalità completa**: precisa|  
|**Motori di archiviazione non supportati**|Se è impostata su' Yes ', tutte le istruzioni che fanno riferimento a tabelle che utilizzano motori archiviati diversi da ISAM e InnoDb devono essere contrassegnate con messaggi di conversione di avviso.<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: Sì|  
|**Non visualizzare la generazione della colonna ausiliaria ROWID**|In caso affermativo, impedisce la creazione di una colonna ausiliaria ROWD per le tabelle di destinazione. Può influire sulla migrazione di alcune strutture.<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: No|  
|**TRONCAmento conversione istruzione**|Specifica la modalità di conversione delle istruzioni TRUNCATE.<br /><br />**Modalità predefinita**: troncamento<br /><br />**Modalità ottimistica**: troncamento<br /><br />**Modalità completa**: troncamento|  
  
### <a name="misc"></a>Varie  
  
|**Termine**|**Definizione**|  
|-|-|  
|**Mapping dello schema predefinito**|Specifica come eseguire il mapping dei database MySQL in SQL Server schemi.<br /><br />**Modalità predefinita**: database to database<br /><br />**Modalità ottimistica**: database to database<br /><br />**Modalità completa**: da database a database|  
  
### <a name="procedures-and-functions"></a>Procedure e funzioni  
  
|**Termine**|**Definizione**|  
|-|-|  
|**Conversione di funzione predefinita**|Specifica se per impostazione predefinita le funzioni devono essere convertite in funzioni T-SQL o stored procedure.<br /><br />**Modalità predefinita**: conversione in funzione<br /><br />**Modalità ottimistica**: conversione in funzione<br /><br />**Modalità completa**: conversione in funzione|  
|**Genera SET XACT_ABORT ON**|Specifica se è necessario aggiungere XACT_ABORT ON all'inizio della procedura o del trigger convertito.<br /><br />**Modalità predefinita**: Sì<br /><br />**Modalità ottimistica**: Sì<br /><br />**Modalità completa**: Sì|  
|**Genera SET NOCOUNT ON**|Specifica se l'oggetto SET NOCOUNT ON deve essere aggiunto all'inizio della procedura o del trigger convertito.<br /><br />**Modalità predefinita**: Sì<br /><br />**Modalità ottimistica**: Sì<br /><br />**Modalità completa**: Sì|  
  
### <a name="spatial-data-types"></a>Tipi di dati spaziali  
  
|**Termine**|**Definizione**|  
|-|-|  
|**Rettangolo di delimitazione predefinito {XMAX&#124;XMIN&#124;YMAX&#124;YMIN} per gli indici spaziali**|Definisce il valore predefinito per il parametro {XMAX&#124;XMIN&#124;YMAX&#124;YMIN} del rettangolo di delimitazione utilizzato negli indici spaziali.<br /><br />**Modalità predefinita**<br /><br />XMAX: 100<br /><br />XMIN: 0<br /><br />YMAX: 100<br /><br />YMIN: 0<br /><br />**Modalità ottimistica**<br /><br />XMAX: 100<br /><br />XMIN: 0<br /><br />YMAX: 100<br /><br />YMIN: 0<br /><br />**Modalità completa**<br /><br />XMAX: 100<br /><br />XMIN: 0<br /><br />YMAX: 100<br /><br />YMIN: 0|  
|**Densità della griglia predefinita per gli indici spaziali**|Definisce il valore predefinito per LEVEL_1, LEVEL_2, LEVEL_3 e LEVEL_4 della densità della griglia utilizzata negli indici spaziali.<br /><br />**Modalità predefinita**<br /><br />LEVEL_1: predefinito<br /><br />LEVEL_2: predefinito<br /><br />LEVEL_3: predefinito<br /><br />LEVEL_4: predefinito<br /><br />**Modalità ottimistica**<br /><br />LEVEL_1: predefinito<br /><br />LEVEL_2: predefinito<br /><br />LEVEL_3: predefinito<br /><br />LEVEL_4: predefinito<br /><br />**Modalità completa**<br /><br />LEVEL_1: predefinito<br /><br />LEVEL_2: predefinito<br /><br />LEVEL_3: predefinito<br /><br />LEVEL_4: predefinito|  
  
### <a name="transactions"></a>Transazioni  
  
|**Termine**|**Definizione**|  
|-|-|  
|**Tabelle non transazionali**|Specifica se tutti i riferimenti alla tabella che non supportano le transazioni devono essere contrassegnati con messaggi di conversione di avviso.<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: Sì|  
|**Livello di isolamento delle transazioni**|Specifica il livello di isolamento delle transazioni da utilizzare per le nuove transazioni.<br /><br />**Modalità predefinita**: predefinita<br /><br />**Modalità ottimistica**: predefinita<br /><br />**Modalità completa**: lettura ripetibile|  
  
### <a name="value-control"></a>Controllo valore  
  
|**Termine**|**Definizione**|  
|-|-|  
|**Conversione da carattere a numerico**|Specifica come gestire la conversione implicita ed esplicita dal tipo di dati character ai tipi di dati numerici.<br /><br />**Modalità predefinita**: ottimistica<br /><br />**Modalità ottimistica**: ottimistica<br /><br />**Modalità completa**: precisa|  
|**Controllare i valori numerici senza segno**|Controllare l'assegnazione di valori a variabili e parametri numerici senza segno.<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: Sì|  
|**Controllare la sottrazione senza segno**|Modificare i valori negativi inseriti nelle colonne della tabella del tipo di dati senza segno.<br /><br />**Modalità predefinita**: Convert ' as-is '<br /><br />**Modalità ottimistica**: convertire ' as-is '<br /><br />**Modalità completa**: contrassegno con un avviso|  
|**Conversione da e verso il tipo di dati binary**|Specifica come gestire la conversione implicita ed esplicita dal tipo di dati binario.<br /><br />**Modalità predefinita**: ottimistica<br /><br />**Modalità ottimistica**: ottimistica<br /><br />**Modalità completa**: precisa|  
|**Conversione in tipo di dati data/ora**|Specifica come gestire la conversione implicita ed esplicita nel tipo di dati data/ora.<br /><br />**Modalità predefinita**: emulare il formato MySQL<br /><br />**Modalità ottimistica**: usare il formato SQL Server<br /><br />**Modalità completa**: emulare il formato MySQL|  
|**Valori letterali numerici con precisione superiore a 38**|Specifica la modalità di conversione dei valori letterali numerici con precisione superiore a 38.<br /><br />**Modalità predefinita**: Arrotonda se possibile<br /><br />**Modalità ottimistica**: arrotondare se possibile<br /><br />**Modalità completa**: arrotondare se possibile|  
|**Zero-date in colonne NOT NULL**|Specifica come gestire l'assegnazione a colonne NOT NULL con valori di data/ora zero, data zero o non validi.<br /><br />**Modalità predefinita**: GETDATE ()<br /><br />**Modalità ottimistica**: GETDATE ()<br /><br />**Modalità completa**: GETDATE ()|  
  
## <a name="see-also"></a>Vedere anche  
[Informazioni di riferimento sull'interfaccia utente &#40;MySQLToSQL&#41;](../../ssma/mysql/user-interface-reference-mysqltosql.md)  
  
