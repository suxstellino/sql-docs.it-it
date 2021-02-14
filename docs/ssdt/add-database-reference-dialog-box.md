---
title: Finestra di dialogo Aggiungi riferimento al database
description: Informazioni sui riferimenti al database e su come usarli. Scoprire le procedure che è possibile eseguire nella finestra di dialogo Aggiungi riferimento al database.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.adddatabasereference.dialog
- sql.data.tools.newdatabase.dialog
- sql.data.tools.criticalerror.dialog
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: feb7df193ea54f4c7184066bb38cc5cc700f9bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079662"
---
# <a name="add-database-reference-dialog-box"></a>Finestra di dialogo Aggiungi riferimento al database

In questo argomento vengono descritte le procedure che è possibile eseguire nella finestra di dialogo **Aggiungi riferimento al database**.  
  
I riferimenti al database consentono di:  
  
Accedere a oggetti in altri database.  
Un progetto può fare riferimento a un altro database in qualsiasi server mediante la risoluzione dei nomi a tre o quattro parti. Se si utilizza un nome a tre o quattro parti per un riferimento, è possibile utilizzare le variabili SQLCMD per consentire il funzionamento dei riferimenti in più server e database.  
  
Creare una soluzione composita con più progetti di database.  
Con i riferimenti a database in un progetto composito è possibile partizionare un unico database di grandi dimensioni in diversi progetti distinti. A tale scopo, si crea un riferimento che non contiene variabili o valori relativi a database o server (solo con nomi a una e due parti).  
  
I riferimenti a database possono essere creati in un progetto di database nella soluzione corrente oppure in un file DACPAC. L'aggiunta di un riferimento a database in un progetto influisce sulle dipendenze del progetto e sull'ordine di compilazione.  
  
## <a name="selecting-the-database-to-reference"></a>Selezione del database a cui fare riferimento

È possibile fare riferimento a un altro progetto di database nella stessa soluzione, a un database di sistema o a un file DACPAC.  
  
Se nella soluzione sono contenuti più progetti di database, è abilitata l'opzione **Progetti di database nella soluzione corrente**. È possibile fare riferimento a un altro database della soluzione.  
  
Selezionare **Database di sistema** se si vuole selezionare uno dei database di sistema come riferimento a database.  
  
Selezionare **Applicazione livello dati (.dacpac)** per fare riferimento a un database in un file DACPAC, quindi passare alla directory contenente il file desiderato.  
  
## <a name="selecting-the-databases-relative-location"></a>Selezione del percorso relativo del database

Dopo aver selezionato il database a cui fare riferimento, è possibile specificare il percorso previsto di un oggetto di database, relativo al progetto cui si fa riferimento.  
  
I riferimenti possono essere risolti per gli oggetti in uno dei percorsi seguenti:  
  
- Nel database contenente il riferimento.  
  
- In un database diverso da quello contenente il riferimento, ma presente nello stesso server.  
  
- In un database diverso da quello contenente il riferimento su un altro server.  
  
Specificare un nome di database. Se si sceglie **Database di sistema**, non si deve modificare il relativo valore letterale. Se si sceglie **Progetti di database nella soluzione corrente**, il nome predefinito del database è basato sul nome del database nel progetto.  
  
Se si seleziona **Database e server diversi**, specificare un nome server.  
  
È possibile utilizzare una variabile di database (SQLCMD). Se si desidera fare riferimento al database con una variabile invece che con un nome letterale, accettare o modificare la variabile di database predefinita. Le variabili di database sono utili se si pubblica il progetto di database da più server e database. In questa circostanza, lo sviluppatore può passare a **Variabili SQLCMD** nelle pagine delle proprietà del progetto e specificare il nome locale del database. Se si lascia vuota l'opzione **Variabile database**, è possibile fare riferimento al database solo con il relativo nome letterale. Se si specifica un nome di variabile di database, non è possibile fare riferimento al database mediante il relativo nome letterale.  
  
Se si seleziona l'opzione **Database e server diversi** è obbligatorio specificare una variabile server (SQLCMD). Le variabili server sono utili se si pubblica il progetto di database da più server e database. In questa circostanza, lo sviluppatore può passare a **Variabili SQLCMD** nelle pagine delle proprietà del progetto e specificare il nome locale del server. È possibile fare riferimento al server solo con la variabile e non con il nome server.  
  
> [!IMPORTANT]  
> In alcune situazioni, è possibile creare un riferimento a database con lo stesso nome di un riferimento a database esistente. La presenza di due riferimenti a database con lo stesso nome può produrre risultati imprevisti. In questo caso, eliminare entrambi i riferimenti a database.  
  
## <a name="common-procedures"></a>Procedure comuni

Di seguito sono indicate le procedure comuni:  
  
### <a name="to-create-a-reference-to-a-database-on-the-same-server"></a>Per creare un riferimento a un database nello stesso server  
  
1.  In Esplora soluzioni fare clic con il pulsante destro del mouse su **Riferimenti** e scegliere **Aggiungi riferimento al database**.  
  
2.  Nella finestra di dialogo **Aggiungi riferimento al database** specificare il **Percorso database** come **Database diverso, stesso server**.  
  
3.  Specificare il nome del database.  
  
4.  Decidere se usare una variabile di database.  
  
5.  Copiare la sintassi di esempio e incollarla nel file con estensione sql. Nella sintassi di esempio, sostituire [Schema1] con il nome dello schema (ad esempio, [dbo]). Inoltre, modificare il nome dell'oggetto di database come appropriato per il progetto in uso.  
  
6.  Compilare la soluzione.  
  
### <a name="to-create-a-reference-to-a-database-on-another-server"></a>Per creare un riferimento a un database in un altro server  
  
1.  In Esplora soluzioni fare clic con il pulsante destro del mouse su **Riferimenti** e scegliere **Aggiungi riferimento al database**.  
  
2.  Nella finestra di dialogo **Aggiungi riferimento al database** specificare il **Percorso database** come **Database e server diversi**.  
  
3.  Verificare che il nome del database sia corretto.  
  
4.  Decidere se usare una variabile di database.  
  
5.  Specificare il nome del server e la relativa variabile.  
  
6.  Copiare la sintassi di esempio e incollarla nel file con estensione sql. Nella sintassi di esempio, sostituire [Schema1] con il nome dello schema (ad esempio, [dbo]). Inoltre, modificare il nome dell'oggetto di database come appropriato per il progetto in uso.  
  
7.  Compilare la soluzione.  
  
### <a name="to-create-a-composite-project"></a>Per creare un progetto composito  
  
1.  In Esplora soluzioni fare clic con il pulsante destro del mouse su **Riferimenti** e scegliere **Aggiungi riferimento al database**.  
  
2.  Selezionare l'origine del database a cui si fa riferimento, vale a dire un progetto nella soluzione o un file DACPAC.  
  
3.  Nella finestra di dialogo **Aggiungi riferimento al database** specificare il **Percorso database** come **Stesso database**.  
  
4.  Copiare la sintassi di esempio e incollarla nel file con estensione sql. Nella sintassi di esempio sostituire [Schema1] con il nome dello schema. Inoltre, modificare il nome dell'oggetto di database come appropriato per il progetto in uso.  
  
5.  Compilare la soluzione.  
  
Quando si pubblica il progetto, è possibile distribuire progetti compositi nella stessa soluzione in una singola destinazione:  
  
1.  Fare clic con il pulsante destro del mouse sul nome del progetto in **Esplora soluzioni** e selezionare **Pubblica** per visualizzare la finestra di dialogo **Database di pubblicazione**.  
  
2.  Nella finestra di dialogo **Database di pubblicazione** fare clic su **Avanzate**.  
  
3.  Nella finestra di dialogo **Impostazioni di pubblicazione avanzate** assicurarsi che sia selezionato **Includi oggetti compositi** nell'elenco **Opzioni di distribuzione avanzate**.  
  
## <a name="see-also"></a>Vedere anche

[Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md)