---
title: Failover avanzato per un gruppo di disponibilità
description: Procedura per l'abilitazione del failover avanzato del database, che attiva un failover, se un database in un gruppo di disponibilità Always On non è più in grado di scrivere transazioni.
ms.custom: seodec18
ms.date: 06/03/2020
ms.prod: sql
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], enhanced database failover
- Availability Groups [SQL Server], failover
ms.assetid: ''
author: cawrites
ms.reviewer: mikeray
ms.author: chadam
ms.openlocfilehash: d26d584c4a644d9ff712f7bf1ac5a82e87302f83
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643561"
---
# <a name="enable-enhanced-database-failover-to-a-database-in-an-always-on-availability-group"></a>Abilitare il failover avanzato del database per un database in un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

In SQL Server 2012 e 2014, se un database che fa parte di un gruppo di disponibilità nella replica primaria perde la possibilità di scrivere le transazioni, non attiva un failover anche se le repliche sono sincronizzate e configurate per il failover automatico.

SQL Server 2016 introduce un nuovo comportamento facoltativo denominato *failover avanzato del database* che può essere impostato dalla procedura guidata o mediante Transact-SQL. Se questa opzione è abilitata ed è configurato il failover automatico, quando un database che fa parte di un gruppo di disponibilità non è più in grado di scrivere le transazioni, verrà attivato un failover in una replica secondaria sincronizzata.

**Scenario 1**

Viene configurato un gruppo di disponibilità tra l'istanza A e l'istanza B che contiene un singolo database denominato DB1. Il file di dati di DB1 si trova nell'unità E e il relativo file di log delle transazioni nell'unità F. La modalità di disponibilità è impostata su commit sincrono con failover automatico. Nel gruppo di disponibilità è configurata la nuova opzione di failover avanzato del database. Le due repliche si trovano attualmente in uno stato sincronizzato. Un problema causa un errore nell'unità E. Questo scenario non causa un failover avanzato del database perché l'unità E non contiene il log delle transazioni.  

**Scenario 2**

Questo scenario prevede la stessa configurazione del gruppo di disponibilità dello Scenario 1. Però questa volta l'errore anziché verificarsi nell'unità E si verifica nell'unità F che contiene il log delle transazioni. In questo caso verrà attivato un failover perché vengono soddisfatte le condizioni coperte dal failover avanzato del database: il log delle transazioni non è raggiungibile, per cui il database non può scrivere le transazioni.

**Scenario 3**

Viene configurato un gruppo di disponibilità tra l'istanza A e l'istanza B che contiene due database: DB1e DB2. La modalità di disponibilità è impostata su commit sincrono con failover automatico ed è abilitato il failover avanzato del database. Viene perso l'accesso al disco contenente i dati di DB2 e i file di log delle transazioni. Quando viene rilevato il problema, il gruppo di disponibilità effettuerà automaticamente il failover nell'istanza B.

## <a name="configure-enhanced-failover"></a>Configurare il failover avanzato

Il failover avanzato del database può essere configurato con SQL Server Management Studio o Transact-SQL. I cmdlet di PowerShell non prevedono attualmente questa possibilità. Per impostazione predefinita, il failover avanzato del database è disabilitato.

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

È possibile abilitare il failover avanzato del database durante la creazione di un gruppo di disponibilità mediante SQL Server Management Studio. L'unico modo per disabilitarlo o abilitarlo dopo la creazione del gruppo di disponibilità consiste nell'usare Transact-SQL.

*Creazione manuale del gruppo di disponibilità*

Usare le istruzioni disponibili nell'articolo [Usare la finestra di dialogo Nuovo gruppo di disponibilità (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md) per creare il gruppo di disponibilità. Per abilitare il failover avanzato del database, selezionare la casella di controllo accanto a *Rilevamento dell'integrità a livello di database*.

*Uso della Creazione guidata gruppo di disponibilità*

Usare le istruzioni disponibili nell'articolo [Usare la Creazione guidata Gruppo di disponibilità (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md). L'opzione per abilitare il failover avanzato del database si trova nella finestra di dialogo Specifica nome del gruppo di disponibilità. Per abilitarlo, selezionare la casella accanto a *Rilevamento dell'integrità a livello di database*.

### <a name="transact-sql"></a>Transact-SQL

Per configurare il comportamento del failover avanzato del database durante la creazione di un gruppo di disponibilità, DB_FAILOVER deve essere impostato su ON nel modo seguente:

```SQL
CREATE AVAILABILITY GROUP [AGNAME]
WITH ( DB_FAILOVER = ON)
...
```
Per aggiungere questo comportamento dopo la configurazione di un gruppo di disponibilità, usare il comando ALTER AVAILABILITY GROUP:
```SQL
ALTER AVAILABILITY GROUP [AGNAME] SET (DB_FAILOVER = ON)
```
Per disabilitare questo comportamento, eseguire il comando ALTER AVAILABILITY GROUP seguente:
```SQL
ALTER AVAILABILITY GROUP [AGNAME] SET (DB_FAILOVER = OFF)
```
### <a name="dynamic-management-view"></a>Vista a gestione dinamica
Per vedere se in un gruppo di disponibilità è abilitato il failover avanzato del database, eseguire una query sulla vista a gestione dinamica `sys.availability_groups`. La colonna `db_failover` conterrà zero se è disabilitato o 1 se è abilitato. 

## <a name="next-steps"></a>Passaggi successivi 

- [Configurare il rilevamento dell'integrità del database](sql-server-always-on-database-health-detection-failover-option.md)

- [Usare la Creazione guidata Gruppo di disponibilità (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

- [Usare la finestra di dialogo Nuovo gruppo di disponibilità (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
- [Creare un gruppo di disponibilità con Transact-SQL](create-an-availability-group-transact-sql.md)

