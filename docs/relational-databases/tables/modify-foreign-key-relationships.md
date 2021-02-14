---
description: Modifica di relazioni di chiave esterna
title: Modificare relazioni di chiave esterna | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- vdtsql.chm:65538
- vdt.ppg.relationships
helpviewer_keywords:
- foreign keys [SQL Server], modifying
- modifying foreign keys
ms.assetid: 0c9ca80d-d79b-44c4-a21e-0fce39c398ec
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 457f4db2126b314237ccc09678718e14ffc25069
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354125"
---
# <a name="modify-foreign-key-relationships"></a>Modifica di relazioni di chiave esterna

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  È possibile modificare il lato chiave esterna di una relazione in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Cambiando la chiave esterna di una tabella vengono modificate le colonne correlate alle colonne della tabella della chiave primaria.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per modificare una chiave esterna tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 La nuova colonna chiave esterna deve avere lo stesso tipo di dati e la stessa dimensione della colonna chiave primaria a cui è correlata, con le seguenti eccezioni:  
  
-   Una colonna **char** o **sysname** può essere correlata a una colonna **varchar** .  
  
-   Una colonna **binary** può essere correlata a una colonna **varbinary** .  
  
-   Un tipo di dati alias può essere correlato al corrispondente tipo di base.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER per la tabella.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-modify-a-foreign-key"></a>Per modificare una chiave esterna  
  
1.  In **Esplora oggetti** espandere la tabella contenente la chiave esterna, quindi espandere la cartella **Chiavi**.  
  
2.  Fare clic con il pulsante destro del mouse sulla chiave esterna da modificare, quindi scegliere **Modifica**.  
  
3.  Nella finestra di dialogo **Relazioni chiavi esterne** è possibile apportare le modifiche riportate di seguito.  
  
     **Relazione selezionata**  
     Vengono elencate le relazioni esistenti. Selezionarne una per visualizzarne le proprietà nella griglia a destra. Se l'elenco è vuoto, significa che per la tabella non sono state definite relazioni.  
  
     **Aggiungere**  
     Crea una nuova relazione. È necessario impostare l'opzione **Specifiche di tabelle e colonne** per rendere valida la relazione.  
  
     **Elimina**  
     Elimina la relazione selezionata dall'elenco **Relazione selezionata** . Per annullare l'aggiunta di una relazione, utilizzare questo pulsante per rimuovere la relazione.  
  
     **Categoria Generale**  
     Viene espansa per visualizzare **Verifica dati esistenti durante la creazione o la riabilitazione** e **Specifica tabelle e colonne**.  
  
     **Verifica dati esistenti durante la creazione o la riabilitazione**  
     Consente di verificare in base al vincolo tutti i dati esistenti nella tabella prima della creazione o della riabilitazione del vincolo.  
  
     **Categoria Specifica tabelle e colonne**  
     Viene espansa per visualizzare le colonne che fungono da chiave esterna e chiave primaria o univoca nella relazione, nonché le tabelle in cui sono contenute. Per modificare o definire questi valori, fare clic sul pulsante con puntini di sospensione ( **...** ) a destra del campo della proprietà.  
  
     **Tabella di base chiavi esterne**  
     Visualizza la tabella in cui è contenuta la colonna che funge da chiave esterna nella relazione selezionata.  
  
     **Colonne chiave esterne**  
     Visualizza la colonna che funge da chiave esterna nella relazione selezionata.  
  
     **Tabella di base chiavi primarie/univoche**  
     Visualizza la tabella in cui è contenuta la colonna che funge da chiave primaria o univoca nella relazione selezionata.  
  
     **Colonne chiave primarie/univoche**  
     Visualizza la colonna che funge da chiave primaria o univoca nella relazione selezionata.  
  
     **Categoria Identità**  
     Viene espansa per visualizzare i campi delle proprietà **Nome** e **Descrizione**.  
  
     **Nome**  
     Visualizza il nome della relazione. Quando viene creata una nuova relazione, le viene assegnato un nome predefinito sulla base della tabella presente nella finestra attiva di **Progettazione tabelle**. È possibile modificare il nome in qualsiasi momento.  
  
     **Descrizione**  
     Descrive la relazione. Per inserire una descrizione più dettagliata, fare clic su **Descrizione** , quindi sui puntini di sospensione ( **…** ) a destra del campo della proprietà. Viene così visualizzata un'area più grande in cui scrivere il testo.  
  
     **Categoria Progettazione tabelle**  
     Viene espansa per visualizzare le informazioni relative a **Verifica dati esistenti durante la creazione o la riabilitazione** e **Attiva per replica**.  
  
     **Applicare per replica**  
     Viene indicato se applicare il vincolo quando un agente di replica esegue un'inserimento, un aggiornamento o un'eliminazione in questa tabella.  
  
     **Attiva vincolo della chiave esterna**  
     Viene specificato se le modifiche ai dati delle colonne coinvolte nella relazione sono consentite quando rischiano di compromettere l'integrità della relazione di chiave esterna. Fare clic su **Sì** per non accettare tali modifiche e su **No** per accettarle.  
  
     **Categoria Specifica INSERT e UPDATE**  
     Viene espansa per visualizzare le informazioni relative a **Elimina regola** e **Aggiorna regola** per la relazione.  
  
     **Elimina regola**  
     Specifica che cosa accade se un utente tenta di eliminare una riga contenente dati coinvolti in una relazione di chiave esterna:  
  
    -   **Nessuna azione** Un messaggio di errore indica che l'eliminazione non è consentita e viene eseguito il rollback dell'operazione DELETE.  
  
    -   **Sovrapponi** Elimina tutte le righe che contengono dati coinvolti nella relazione di chiave esterna. Non specificare CASCADE se la tabella verrà inclusa in una pubblicazione di tipo merge che utilizza record logici.  
  
    -   **Set Null** Consente di impostare il valore su Null se in tutte le colonne chiave esterna della tabella sono accettati valori Null.  
  
    -   **Imposta predefinito** Imposta il valore predefinito per la colonna se per tutte le colonne chiave esterna della tabella sono stati impostati valori predefiniti.  
  
     **Aggiorna regola**  
     Specifica ciò che accade se un utente tenta di aggiornare una riga contenente dati coinvolti in una relazione di chiave esterna:  
  
    -   **Nessuna azione** Un messaggio di errore indica che l'aggiornamento non è consentito e viene eseguito il rollback dell'operazione UPDATE.  
  
    -   **Sovrapponi** Aggiorna tutte le righe che contengono dati coinvolti nella relazione di chiave esterna. Non specificare CASCADE se la tabella verrà inclusa in una pubblicazione di tipo merge che utilizza record logici.  
  
    -   **Set Null** Consente di impostare il valore su Null se in tutte le colonne chiave esterna della tabella sono accettati valori Null.  
  
    -   **Imposta predefinito** Viene impostato il valore predefinito stabilito per la colonna se per tutte le colonne chiave esterna della tabella sono stati impostati valori predefiniti.  
  
4.  Nel menu **File** fare clic su **Salva**_nome tabella_.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per modificare una chiave esterna**  
  
 Per modificare un vincolo FOREIGN KEY utilizzando Transact-SQL, è innanzitutto necessario eliminare il vincolo FOREIGN KEY esistente, quindi ricrearlo con la nuova definizione. Per ulteriori informazioni, vedere [Delete Foreign Key Relationships](../../relational-databases/tables/delete-foreign-key-relationships.md) e [Create Foreign Key Relationships](../../relational-databases/tables/create-foreign-key-relationships.md).  
  
###  <a name="TsqlExample"></a>  
