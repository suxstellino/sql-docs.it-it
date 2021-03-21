---
description: Proprietà delle colonne delle tabelle (SQL Server Management Studio)
title: Proprietà delle colonne delle tabelle (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- vdtsql.chm:65558
- vdtsql.chm:69657
- vdt.ppg.columns
ms.assetid: 09830897-cc10-46b8-95f5-e0e9681b668c
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ea67d013f122ed4d539221cbe81a1955f30ac444
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738021"
---
# <a name="table-column-properties-sql-server-management-studio"></a>Proprietà delle colonne delle tabelle (SQL Server Management Studio)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Queste proprietà vengono visualizzate nel riquadro inferiore di Progettazione tabelle. Se non specificato diversamente, è possibile modificare tali proprietà nella finestra Proprietà, quando la colonna desiderata è selezionata. Le **Proprietà colonna** possono essere visualizzate in categorie o in ordine alfabetico. Molte proprietà sono visualizzate o possono essere modificate solo per determinati tipi di dati.  
  
> [!NOTE]  
>  Se la tabella viene pubblicata per la replica, è necessario apportare modifiche allo schema usando l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)][ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects (SMO). Quando si apportano modifiche allo schema utilizzando Progettazione tabelle o Progettazione diagrammi di database, viene effettuato il tentativo di rimuovere e rigenerare la tabella. La modifica allo schema non riuscirà, poiché non è consentita la rimozione di oggetti pubblicati.  
  
 **Generale**  
 Viene espansa per visualizzare le proprietà **Nome**, **Consenti valori Null**, **Tipo di dati**, **Valore predefinito dell'associazione**, **Lunghezza**, **Precisione** e **Scala**.  
  
 **Nome**  
 Visualizza il nome della colonna selezionata.  
  
 **Consenti valori NULL**  
 Indica se nella colonna sono consentiti valori Null. Per modificare questa proprietà, selezionare la casella di controllo Consenti valori NULL corrispondente alla colonna desiderata nel riquadro superiore di Progettazione tabelle.  
  
 **Tipo di dati**  
 Visualizza il tipo di dati della colonna selezionata. Per modificare questa proprietà, fare clic sul valore, espandere l'elenco a discesa e selezionare un nuovo valore.  
  
 **Valore predefinito o associazione**  
 Visualizza il valore predefinito utilizzato per la colonna quando non viene immesso alcun valore specifico. Il valore di questo campo può essere il valore di un vincolo predefinito di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure il nome di un vincolo globale cui è associata la colonna. Nell'elenco a discesa sono contenuti tutti i valori predefiniti globali impostati nel database. Per associare la colonna a un valore predefinito globale, selezionarlo dall'elenco a discesa. In alternativa, per creare un vincolo predefinito per la colonna, digitare direttamente il valore predefinito come testo.  
  
 **Lunghezza**  
 Indica il numero di caratteri consentiti per i tipi di dati basati su caratteri. Questa proprietà è disponibile solo per tipi di dati basati su caratteri.  
  
 **Ridimensionamento**  
 Visualizza il numero massimo di cifre consentito dopo la virgola decimale nei valori inclusi nella colonna. Questa proprietà corrisponde a **0** per i tipi di dati non numerici.  
  
 **Precisione**  
 Visualizza il numero massimo di cifre consentito per i valori inclusi nella colonna. Questa proprietà corrisponde a **0** per i tipi di dati non numerici.  
  
 **Progettazione tabelle**  
 Espande la sezione **Progettazione tabelle** .  
  
 **Regole di confronto**  
 Visualizza la sequenza di confronto applicata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per impostazione predefinita alla colonna quando i valori di colonna vengono utilizzati per ordinare le righe del risultato di una query. Per modificare le regole di confronto, selezionare la proprietà, fare clic sui puntini di sospensione (...) a destra del valore della proprietà per visualizzare la finestra di dialogo **Regole di confronto** .  
  
 **Specifica colonna calcolata**  
 visualizza informazioni su una colonna calcolata. Il valore visualizzato per la proprietà corrisponde al valore della proprietà figlio **Formula** . Viene anche visualizzata la formula relativa alla colonna calcolata.  
  
> [!NOTE]  
>  Per modificare il valore visualizzato per la proprietà **Specifica colonna calcolata** , è necessario espandere la proprietà e modificare la proprietà figlio **Formula** .  
  
-   **Formula** Visualizza la formula relativa alla colonna calcolata. Per modificare questa proprietà, digitare direttamente una nuova formula.  
  
-   **Persistente** Indica se i risultati della formula vengono archiviati. Se questa proprietà è impostata su **No** , viene archiviata solo la formula e i valori vengono calcolati ogni volta che si fa riferimento a questa colonna. Per modificare questa proprietà, fare clic sul valore, espandere l'elenco a discesa e selezionare un nuovo valore.  
  
 Per altre informazioni, vedere [Specificare le colonne calcolate in una tabella](../../relational-databases/tables/specify-computed-columns-in-a-table.md).  
  
 **Tipo di dati abbreviato**  
 Consente di visualizzare informazioni sul tipo di dati del campo, nello stesso formato dell'istruzione SQL CREATE TABLE. Ad esempio, un campo contenente una stringa di lunghezza variabile con un massimo di 20 caratteri viene rappresentato come "varchar(20)". Per modificare questa proprietà, digitare direttamente il valore desiderato.  
  
 **Descrizione**  
 visualizza il testo descrittivo relativo alla colonna selezionata. Per modificare la descrizione, selezionare la proprietà, fare clic sui puntini di sospensione (...) a destra del valore della proprietà e quindi modificare la descrizione nella finestra di dialogo **Proprietà Descrizione** .  
  
 **Deterministico**  
 Indica se il tipo di dati della colonna selezionata può essere determinato con certezza.  
  
 **Con pubblicazione di tipo DTS**  
 Indica se la pubblicazione della colonna è di tipo DTS. [La funzionalità Data Transformation Services è deprecata](/previous-versions/sql/sql-server-2008-r2/cc707786(v=sql.105)). 
  
 **Specifica testo completo**  
 Visualizza informazioni su un indice full-text. Il valore di questa proprietà corrisponde al valore della proprietà figlio **Indice full-text** e indica se a questa colonna è applicata l'indicizzazione full-text.  
  
> [!NOTE]  
>  Per modificare il valore visualizzato per la proprietà **Specifica full-text** , è necessario espandere la proprietà e modificare la proprietà figlio **Indice full-text** .  
  
-   **Indice full-text** Indica se a questa colonna è applicata l'indicizzazione full-text. Questa proprietà può essere impostata su **Sì** solo se il tipo di dati della colonna consente ricerche full-text e se per la tabella a cui la colonna appartiene è specificato un indice full-text. Per modificare questa proprietà, fare clic sul valore, espandere l'elenco a discesa e selezionare un valore.  
  
-   **Colonna di tipo full-text** Visualizza il nome della colonna in base alla quale viene applicata l'indicizzazione full-text a questa colonna. È necessario impostare questa proprietà se la proprietà **Tipo di dati** relativa a questa colonna è impostata su **image** o su **varbinary**. La colonna indicata in questa proprietà deve essere di tipo **[n]char, [n]varchar** o **xml** e l'elenco a discesa di questa proprietà include solo colonne con uno di questi tre tipi di dati. Nelle righe incluse nella colonna indicata da questa proprietà viene visualizzato il tipo di documento disponibile nelle righe corrispondenti della colonna in cui è possibile eseguire una ricerca full-text. Per modificare questa proprietà, fare clic sul valore, espandere l'elenco a discesa e selezionare un nuovo valore.  
  
-   **Lingua** Indica la lingua del word breaker usato per indicizzare la colonna. Il valore archiviato nella proprietà corrisponde effettivamente all'identificatore delle impostazioni locali relativo al word breaker. Per ulteriori informazioni sui word breaker e sugli identificatori delle impostazioni locali (LCID), vedere l'articolo relativo ai word breaker e agli stemmer. Per modificare questa proprietà, fare clic sul valore, espandere l'elenco a discesa e selezionare un nuovo valore.  
  
 **Semantica statistica**  
 Selezionare se abilitare l'indicizzazione semantica statistica per la colonna selezionata. Per altre informazioni, vedere [Ricerca semantica &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
 Se si seleziona una lingua in **Lingua** prima di selezionare **Semantica statistica** e alla lingua selezionata non è associato alcun modello di lingua semantico, l'opzione **Semantica statistica** viene impostata su **No** e non può essere modificata. Se si seleziona **Sì** per l'opzione **Semantica statistica** prima di selezionare una lingua in **Lingua**, le lingue disponibili nella colonna **Lingua** saranno limitate a quelle per cui è disponibile un modello di lingua semantico.  
  
 **Con Sottoscrittore non SQL Server**  
 Indica se la colonna viene replicata in un Sottoscrittore non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Specifica identità**  
 Visualizza informazioni su se e come l'univocità viene applicata da questa colonna ai valori inclusi nella colonna. Il valore di questa proprietà indica se si tratta di una colonna identity e se corrisponde al valore della proprietà figlio **Identity**.  
  
> [!NOTE]  
>  Per modificare il valore visualizzato per la proprietà **Specifica Identity** , è necessario espandere la proprietà e modificare la proprietà figlio **Identity** .  
  
-   **Identity** Indica se si tratta di una colonna identity. Per modificare questa proprietà, fare clic sul valore, espandere l'elenco a discesa e selezionare un nuovo valore.  
  
-   **Valore di inizializzazione Identity** Visualizza il valore di inizializzazione Identity specificato durante la creazione di questa colonna identity. Questo valore viene assegnato alla prima riga nella tabella. Se si lascia vuota questa cella, verrà assegnato un valore predefinito pari a 1. Per modificare questa proprietà, digitare direttamente il nuovo valore.  
  
-   **Incremento Identity** Visualizza il valore di incremento specificato durante la creazione della colonna identity. Tale valore è l'incremento che verrà aggiunto a **Valore di inizializzazione Identity** per ogni riga successiva. Se si lascia vuota questa cella, verrà assegnato un valore predefinito pari a 1. Per modificare questa proprietà, digitare direttamente il nuovo valore.  
  
 **Indicizzabile**  
 Indica se la colonna selezionata può essere indicizzata. Le colonne calcolate non deterministiche, ad esempio, non sono indicizzabili.  
  
 **Con pubblicazione di tipo merge**  
 Indica se la pubblicazione della colonna è di tipo merge.  
  
 **Non applicare in processi di replica**  
 Indica se durante la replica vengono mantenuti i valori di identità originari. Per ulteriori informazioni sulla replica, vedere CREATE TABLE. Per modificare questa proprietà, fare clic sul valore, espandere l'elenco a discesa e selezionare un nuovo valore.  
  
 **Tabella replicata**  
 Indica se questa colonna è replicata in un'altra posizione.  
  
 **RowGuid**  
 Indica se la colonna viene utilizzata da SQL Server come ROWGUID. È possibile impostare questo valore su **Sì** solo per una colonna identity univoca. Per modificare questa proprietà, fare clic sul valore, espandere l'elenco a discesa e selezionare un nuovo valore.  
  
 **Dimensione**  
 Indica la dimensione in byte consentita dal tipo di dati della colonna. Ad esempio, un tipo di dati nchar può avere una lunghezza di 10 (il numero di caratteri), ma avrebbe una dimensione di 20 per quanto riguarda i set di caratteri Unicode.  
  
> [!NOTE]  
>  La lunghezza di un tipo di dati **(max)** varia per ogni riga. **sp_help** restituisce (-1) come lunghezza delle colonne **(max)** . [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] visualizza -1 come dimensione della colonna.  
  
