---
description: Gestire il rilevamento delle modifiche (SQL Server)
title: Gestire il rilevamento delle modifiche
ms.custom: seo-dt-2019
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- tracking data changes [SQL Server]
- change tracking [SQL Server], overhead
- change tracking [SQL Server]
- change tracking [SQL Server], managing
ms.assetid: 94a8d361-e897-4d6d-9a8f-1bb652e7a850
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1513249d7c071d35a11423d405ee4b7d528d829b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472572"
---
# <a name="manage-change-tracking-sql-server"></a>Gestire il rilevamento delle modifiche (SQL Server)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In questo argomento viene descritto come gestire il rilevamento delle modifiche. Nell'argomento viene descritto inoltre come configurare la sicurezza e determinare gli effetti sull'archiviazione e sulle prestazioni quando si utilizza il rilevamento delle modifiche.  
  
## <a name="managing-change-tracking"></a>Gestione del rilevamento delle modifiche  
 Nelle sezioni seguenti vengono elencate le viste del catalogo, le autorizzazioni e le impostazioni per la gestione del rilevamento delle modifiche.  
  
### <a name="catalog-views"></a>Viste del catalogo  
 Per determinare in quali tabelle e database è abilitato il rilevamento delle modifiche, è possibile utilizzare le seguenti viste del catalogo:  
  
-   [sys.change_tracking_databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)  
  
-   [sys.change_tracking_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)  
  
 Inoltre, la vista del catalogo [sys.internal_tables](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md) elenca le tabelle interne create quando il rilevamento delle modifiche è abilitato per una tabella utente.  
  
### <a name="security"></a>Sicurezza  
 Per accedere alle informazioni sul rilevamento delle modifiche utilizzando le [funzioni di rilevamento delle modifiche](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md), l'entità deve disporre delle autorizzazioni seguenti:  
  
-   Autorizzazione SELECT almeno nelle colonne chiave primaria nella tabella di cui sono state rilevate le modifiche per la tabella in cui si sta eseguendo la query.  
  
-   Autorizzazione VIEW CHANGE TRACKING nella tabella per la quale vengono ottenute le modifiche. L'autorizzazione VIEW CHANGE TRACKING è richiesta per i seguenti motivi:  
  
    -   I record sul rilevamento delle modifiche includono informazioni sulle righe eliminate, ovvero i valori della chiave primaria delle righe eliminate. A un'entità potrebbe essere stata concessa l'autorizzazione SELECT per una tabella con rilevamento delle modifiche dopo l'eliminazione di alcuni dati sensibili. In questo caso è necessario che tale entità non sia in grado di accedere alle informazioni eliminate utilizzando il rilevamento delle modifiche.  
  
    -   Le informazioni sul rilevamento delle modifiche possono archiviare informazioni sulle colonne modificate dalle operazioni di aggiornamento. A un'entità potrebbe essere stata negata l'autorizzazione relativa a una colonna che contiene informazioni riservate. Tuttavia, poiché le informazioni sul rilevamento delle modifiche sono disponibili, un'entità può determinare che un valore della colonna è stato aggiornato, ma non può determinare tale valore.  
  
## <a name="understanding-change-tracking-overhead"></a>Informazioni sull'overhead del rilevamento delle modifiche  
 Quando il rilevamento delle modifiche è abilitato per una tabella, sono interessate alcune operazioni di amministrazione. Nella tabella seguente sono elencate le operazioni e gli effetti da tenere in considerazione:  
  
|Operazione|Se il rilevamento delle modifiche è abilitato|  
|---------------|-------------------------------------|  
|DROP TABLE|Tutte le informazioni sul rilevamento delle modifiche per la tabella eliminata vengono rimosse.|  
|ALTER TABLE DROP CONSTRAINT|Il tentativo di eliminare il vincolo PRIMARY KEY avrà esito negativo. Il rilevamento delle modifiche deve essere disabilitato prima che un vincolo PRIMARY KEY possa essere eliminato.|  
|ALTER TABLE DROP COLUMN|Se una colonna in fase di eliminazione fa parte della chiave primaria, l'eliminazione della colonna non è consentita, indipendentemente dal rilevamento delle modifiche.<br /><br /> Se la colonna in fase di eliminazione non fa parte della chiave primaria, l'eliminazione della colonna viene eseguita. Tuttavia, è necessario conoscere prima l'effetto su qualsiasi applicazione che sta sincronizzando questi dati. Se il rilevamento delle modifiche a livello della colonna è abilitato per la tabella, la colonna eliminata potrebbe ancora essere restituita come parte delle informazioni sul rilevamento delle modifiche. È compito dell'applicazione gestire la colonna eliminata.|  
|ALTER TABLE ADD COLUMN|Se una nuova colonna viene aggiunta alla tabella con rilevamento delle modifiche, l'aggiunta della colonna non viene rilevata, ma vengono rilevati solo gli aggiornamenti e le modifiche apportati alla nuova colonna.|  
|ALTER TABLE ALTER COLUMN|Le modifiche ai tipi di dati di colonne chiave non primaria non vengono rilevate.|  
|ALTER TABLE SWITCH|Lo spostamento di una partizione non riesce se in una o in entrambe le tabelle è abilitato il rilevamento delle modifiche.|  
|DROP INDEX o ALTER INDEX DISABLE|L'indice che applica la chiave primaria non può essere eliminato o disabilitato.|  
|TRUNCATE TABLE|Il troncamento di una tabella può essere eseguito su una tabella in cui è abilitato il rilevamento delle modifiche. Tuttavia, le righe eliminate dall'operazione non vengono rilevate e viene aggiornata la versione minima valida. Quando un'applicazione verifica la versione, viene indicato che la versione è obsoleta ed è necessario eseguire la reinizializzazione. Questa situazione è la stessa che si verifica quando il rilevamento delle modifiche per la tabella viene disabilitato e successivamente riabilitato.|  
  
 L'utilizzo del rilevamento delle modifiche provoca l'aumento di overhead delle operazioni DML a causa delle informazioni sul rilevamento delle modifiche archiviate come parte dell'operazione.  
  
### <a name="effects-on-dml"></a>Effetti sulle operazioni DML  
 Il rilevamento delle modifiche è stato ottimizzato per minimizzare l'overhead delle prestazioni nelle operazioni DML. L'overhead incrementale delle prestazioni associato all'utilizzo del rilevamento delle modifiche in una tabella è analogo all'overhead causato dalla creazione di un indice per una tabella e dalla relativa necessità di gestirlo.  
  
 Per ogni riga modificata da un'operazione DML, viene aggiunta una riga alla tabella del rilevamento delle modifiche interna. Il suo effetto in relazione all'operazione DML dipende da vari fattori, quali i seguenti:  
  
-   Numero di colonne chiave primaria  
  
-   Quantità di dati di cui è in corso la modifica nella riga della tabella utente  
  
-   Numero di operazioni in esecuzione in una transazione  
  
 L'isolamento dello snapshot, se utilizzato, influisce anche sulle prestazioni per tutte le operazioni DML, indipendentemente dall'abilitazione o meno del rilevamento delle modifiche.  
  
### <a name="effects-on-storage"></a>Effetti sull'archiviazione  
 I dati relativi al rilevamento delle modifiche vengono archiviati nei seguenti tipi di tabelle interne:  
  
-   Tabella delle modifiche interna  
  
     È disponibile una tabella delle modifiche interna per ciascuna tabella utente per cui è abilitato il rilevamento delle modifiche.  
  
-   Tabella delle transazioni interna  
  
     È disponibile una tabella delle transazioni interna per il database.  
  
 Tali tabelle interne influiscono sui requisiti di archiviazione nei modi descritti di seguito:  
  
-   Per ciascuna modifica a ciascuna riga della tabella utente, viene aggiunta una riga alla tabella delle modifiche interna. Questa riga ha un basso overhead fisso più un overhead variabile uguale alle dimensioni delle colonne chiave primaria. La riga può contenere informazioni facoltative sul contesto impostate da un'applicazione. Inoltre, se il rilevamento a livello della colonna è abilitato, ciascuna colonna modificata richiede 4 byte nella tabella di rilevamento.  
  
-   Per ogni transazione di cui è stato eseguito il commit, viene aggiunta una riga a una tabella delle transazioni interna.  
  
 Come per le altre tabelle interne, è possibile determinare lo spazio usato per le tabelle di rilevamento delle modifiche usando la stored procedure [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) . I nomi delle tabelle interne possono essere ottenuti usando la vista del catalogo [sys.internal_tables](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md) , come illustrato nell'esempio seguente.  
  
```sql  
sp_spaceused 'sys.change_tracking_309576141'  
sp_spaceused 'sys.syscommittab'  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Tenere traccia delle modifiche ai dati &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [Proprietà database &#40;pagina Rilevamento delle modifiche&#41;](../../relational-databases/databases/database-properties-changetracking-page.md)   
 [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.change_tracking_databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)   
 [sys.change_tracking_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)   
 [Tenere traccia delle modifiche ai dati &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [Informazioni sul rilevamento delle modifiche &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-tracking-sql-server.md)   
 [Usare i dati delle modifiche &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)  
  
  
