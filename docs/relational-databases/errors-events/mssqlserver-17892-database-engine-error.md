---
description: MSSQLSERVER_17892
title: MSSQLSERVER_17892
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17892 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 052c8c5670ba079cac980151f123cd8c35fd53b1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345559"
---
# <a name="mssqlserver_17892"></a>MSSQLSERVER_17892
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|17892|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|SRV_LOGON_FAILED_BY_TRIGGER|
|Testo del messaggio|Accesso non riuscito per l'account di accesso \<Login Name> a causa dell'esecuzione di un trigger.|
||

## <a name="explanation"></a>Spiegazione

L'errore 17892 viene generato quando non è possibile eseguire correttamente il codice di un trigger di accesso. I [trigger di accesso](../triggers/logon-triggers.md) attivano stored procedure in risposta a un evento LOGON generato quando viene stabilita una sessione utente a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Viene segnalato all'utente un messaggio di errore simile al seguente:

> Messaggio 17892, livello 14, stato 1, server \<Server Name>, riga 1  
Accesso non riuscito per l'account di accesso \<Login Name> a causa dell'esecuzione di un trigger.

## <a name="possible-causes"></a>Possibili cause

Il problema può verificarsi se si verifica un errore durante l'esecuzione del codice del trigger per l'account utente specifico. Alcuni degli scenari includono:

- Il trigger tenta di inserire dati in una tabella che non esiste.
- L'account di accesso non ha le autorizzazioni per l'oggetto a cui fa riferimento il trigger di accesso.

## <a name="user-action"></a>Azione utente

È possibile usare una delle soluzioni riportate di seguito in base allo scenario.

- **Scenario 1:** attualmente si ha accesso a una sessione aperta per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con un account amministratore

  In questo caso, è possibile eseguire l'azione correttiva necessaria per correggere il codice del trigger.

  - Esempio 1: se un oggetto a cui fa riferimento il codice del trigger non esiste, creare l'oggetto in modo che il trigger di accesso possa essere eseguito correttamente.

  - Esempio 2: se un oggetto a cui fa riferimento il codice del trigger esiste, ma gli utenti non hanno le autorizzazioni, concedere loro i privilegi necessari per accedere all'oggetto.  
  
  In alternativa, è possibile semplicemente eliminare o disabilitare il trigger di accesso in modo che gli utenti possano continuare ad accedere a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

- **Scenario 2**: non sono presenti sessioni correnti aperte con privilegi amministrativi, ma la connessione amministrativa dedicata (DAC) è abilitata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

    In questo caso, è possibile usare la connessione DAC per eseguire gli stessi passaggi illustrati nel caso 1 perché le connessioni DAC non sono interessate dai trigger di accesso. Per altre informazioni sulla connessione DAC, vedere: [Connessione di diagnostica per gli amministratori di database](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md).

    Per verificare se la connessione DAC è abilitata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile controllare se nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è presente un messaggio simile al seguente:

    > 2020-02-09 16:17:44.150 Stabilito il supporto per le connessioni amministrative dedicate per l'attesa locale sulla porta 1434.  

- **Scenario 3**: la connessione DAC non è stata abilitata nel server e non è disponibile una sessione di amministrazione esistente per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

    In questo scenario, l'unico modo per risolvere il problema è eseguire i passaggi seguenti:
  
    1. Arrestare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e servizi correlati.
    2. Avviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dal [prompt dei comandi](/previous-versions/sql/sql-server-2008-r2/ms180965(v=sql.105)) usando i parametri di avvio `-c`, `-m` e `-f`. Viene così disabilitato il trigger di accesso ed è possibile applicare le stesse misure correttive descritte nel **caso 1** in precedenza.
  
        > [!NOTE]
        > Per la procedura precedente è necessario un account *SA* o un account amministratore equivalente.
  
         Per altre informazioni su queste e altre opzioni di avvio, vedere: [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md).

## <a name="more-information"></a>Ulteriori informazioni

Un'altra situazione in cui i trigger di accesso causano errori è quando si usa la funzione `EVENTDATA`. Questa funzione restituisce codice XML e fa distinzione tra maiuscole e minuscole.  Quindi, se si crea il trigger di accesso seguente con l'intento di bloccare l'accesso in base all'indirizzo IP, è possibile che si riscontri il problema:

``` sql
 CREATE TRIGGER tr_logon_CheckIP  
 ON ALL SERVER  
 FOR LOGON  
 AS
 BEGIN
  IF IS_SRVROLEMEMBER ( 'sysadmin' ) = 1  
     BEGIN
         DECLARE @IP NVARCHAR ( 15 );  
         SET @IP = ( SELECT EVENTDATA ().value ( '(/EVENT_INSTANCE/ClientHost)[1]' , 'NVARCHAR(15)' ));  
         IF NOT EXISTS( SELECT IP FROM DBAWork.dbo.ValidIP WHERE IP = @IP )  
         ROLLBACK ;  
     END ;  
 END ;  
 GO
```

L'utente non ha rispettato la combinazione di maiuscole/minuscole durante la copia di questo script da Internet in questa parte del trigger:

```sql
 SELECT EVENTDATA ().value ( '(/event_instance/clienthost)[1]' , 'NVARCHAR(15)' ));  
```

Di conseguenza, `EVENTDATA` ha restituito sempre **NULL** e l'accesso è stato negato a tutti gli account di accesso di SA equivalenti. In questo caso, la connessione DAC non era stata abilitata, quindi l'unica scelta era riavviare il server con i parametri di avvio elencati sopra per eliminare il trigger.