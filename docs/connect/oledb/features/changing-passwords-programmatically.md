---
title: Modifica delle password a livello di programmazione | Microsoft Docs
description: OLE DB Driver per SQL Server supporta la gestione della scadenza delle password a livello di programmazione tramite OLE DB Driver e le finestre di dialogo di accesso di SQL Server.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], password expiration
- OLE DB Driver for SQL Server, passwords
- passwords [SQL Server], expiration
- MSOLEDBSQL, password expiration
- expiration [OLE DB Driver for SQL Server]
- passwords [SQL Server], modifying
- expired passwords [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, password expiration
- modifying passwords
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ca8bfec9c9404f2912d288cb434aa064200fea45
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742851"
---
# <a name="changing-passwords-programmatically"></a>Modifica delle password a livello di programmazione
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Nelle versioni precedenti a [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] una password di un utente scaduta può essere reimpostata solo da un amministratore. A partire da [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], OLE DB Driver per SQL Server supporta la gestione della scadenza delle password a livello di programmazione tramite OLE DB Driver e tramite modifiche alle finestre di dialogo **Account di accesso di SQL Server**.  
  
> [!NOTE]  
>  Quando possibile, richiedere agli utenti di immettere le credenziali in fase di esecuzione ed evitare di archiviarle in un formato persistente. Se è necessario rendere persistenti le credenziali, è consigliabile crittografarle usando l'[API di crittografia Win32](/windows/win32/seccrypto/cryptography-reference). Per altre informazioni sull'uso delle password, vedere [Password complesse](../../../relational-databases/security/strong-passwords.md).  
  
## <a name="sql-server-login-error-codes"></a>Codici di errore degli account di accesso di SQL Server  
 Quando non è possibile stabilire una connessione a causa di problemi di autenticazione, sarà disponibile uno dei codici di errore di SQL Server seguenti per consentire la diagnosi e il recupero.  
  
|Codice di errore di SQL Server|Messaggio di errore|  
|---------------------------|-------------------|  
|15113|Accesso non riuscito per l'utente '%.*ls' Motivo: impossibile convalidare la password. L'account è bloccato.|  
|18463|Accesso non riuscito per l'utente '%.*ls'. Motivo: impossibile modificare la password. Impossibile utilizzare la password in questo momento.|  
|18464|Accesso non riuscito per l'utente '%.*ls'. Motivo: impossibile modificare la password. La password non soddisfa i criteri di Windows in quanto è troppo breve.|  
|18465|Accesso non riuscito per l'utente '%.*ls'. Motivo: impossibile modificare la password. La password non soddisfa i criteri di Windows in quanto è troppo lunga.|  
|18466|Accesso non riuscito per l'utente '%.*ls'. Motivo: impossibile modificare la password. La password non soddisfa i criteri di Windows in quanto non è sufficientemente complessa.|  
|18467|Accesso non riuscito per l'utente '%.*ls'. Motivo: impossibile modificare la password. La password non soddisfa i requisiti della DLL per il filtro delle password.|  
|18468|Accesso non riuscito per l'utente '%.*ls'. Motivo: impossibile modificare la password. Si è verificato un errore imprevisto durante la convalida della password.|  
|18487|Accesso non riuscito per l'utente '%.*ls'. Motivo: la password dell'account è scaduta.|  
|18488|Accesso non riuscito per l'utente '%.*ls'. Motivo: è necessario modificare la password dell'account.|  
  
## <a name="ole-db-driver-for-sql-server"></a>Driver OLE DB per SQL Server  
 OLE DB Driver per SQL Server supporta la scadenza della password tramite un'interfaccia utente e a livello di programmazione.  
  
### <a name="ole-db-user-interface-password-expiration"></a>Scadenza password dell'interfaccia utente OLE DB  
 Il driver OLE DB per SQL Server supporta la scadenza password mediante le modifiche apportate alle finestre di dialogo **Account di accesso di SQL Server**. Se il valore di DBPROP_INIT_PROMPT è impostato su DBPROMPT_NOPROMPT, il tentativo di connessione iniziale non riesce se la password è scaduta.  
  
 Se DBPROP_INIT_PROMPT è stato impostato su qualsiasi altro valore, viene visualizzata la finestra di dialogo **Account di accesso di SQL Server** indipendentemente dalla scadenza della password. L'utente può cambiare la password facendo clic sul pulsante **Opzioni** e selezionando **Cambia password**.  
  
 Se l'utente fa clic su OK e la password è scaduta, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] richiede all'utente di immettere e confermare una nuova password usando la finestra di dialogo **Modifica password SQL Server**.  
  
#### <a name="ole-db-prompt-behavior-and-locked-accounts"></a>Comportamento del prompt di OLE DB e account bloccati  
 È possibile che non si riesca a stabilire una connessione perché l'account è bloccato. Se questo problema si verifica in seguito alla visualizzazione della finestra di dialogo **Account di accesso di SQL Server**, viene visualizzato il messaggio di errore del server e il tentativo di connessione viene interrotto. Il problema può verificarsi anche se, in seguito alla visualizzazione della finestra di dialogo **Modifica password SQL Server**, l'utente immette un valore non corretto per la vecchia password. In questo caso viene visualizzato lo stesso messaggio di errore e il tentativo di connessione viene interrotto.  
  
#### <a name="ole-db-connection-pooling-password-expiration-and-locked-accounts"></a>Pool di connessioni OLE DB, scadenza password e account bloccati  
 Un account può essere bloccato o la rispettiva password può scadere mentre la connessione è ancora attiva in un pool di connessioni. Il server controlla le password scadute e gli account bloccati in due circostanze. Quando una connessione viene creata per la prima volta e quando la connessione viene reimpostata, ovvero quando viene estratta dal pool.  
  
 Quando il tentativo di ripristino non riesce, la connessione viene rimossa dal pool e viene restituito un errore.  
  
### <a name="ole-db-programmatic-password-expiration"></a>Scadenza password OLE DB a livello di programmazione  
 Il driver OLE DB per SQL Server supporta la scadenza password mediante l'aggiunta della proprietà SSPROP_AUTH_OLD_PASSWORD (tipo VT_BSTR) che è stata aggiunta al set di proprietà DBPROPSET_SQLSERVERDBINIT.  
  
 La proprietà "Password" esistente si riferisce a DBPROP_AUTH_PASSWORD e viene utilizzata per archiviare la nuova password.  
  
> [!NOTE]  
>  Nella stringa di connessione la proprietà "Vecchia password" imposta SSPROP_AUTH_OLD_PASSWORD, ovvero la password corrente (probabilmente scaduta) che non è disponibile tramite una proprietà della stringa del provider.  
  
 Il provider non rende persistente il valore di questa proprietà. Quando questa proprietà è impostata, il provider non utilizza il pool di connessioni per la prima connessione, dal momento che verrà stabilita una nuova connessione. Dopo aver cambiato la password, non sarà più possibile utilizzare la connessione corrente in quanto contiene ancora la vecchia password che non è più valida. Se inoltre si riesce ad accedere, il provider cancella questa proprietà. I tentativi successivi di recuperare la vecchia password restituiscono VT_EMPTY.  
  
> [!NOTE]  
>  SSPROP_AUTH_OLD_PASSWORD non deve essere resa persistente in quanto viene utilizzata solo quando una password è scaduta.  
  
 Ogni volta che la proprietà "Vecchia password" viene impostata, il provider presuppone che sia in atto un tentativo di modificare la password, a meno che non venga specificata anche l'autenticazione di Windows. In questo caso, questa avrebbe comunque ha la precedenza.  
  
 Se si usa l'autenticazione di Windows, l'immissione della vecchia password restituisce DB_E_ERRORSOCCURRED o DB_S_ERRORSOCCURRED, a seconda se la vecchia password è stata specificata rispettivamente come REQUIRED o OPTIONAL e il valore dello stato di DBPROPSTATUS_CONFLICTINGBADVALUE viene restituito in *dwStatus*. Questo errore viene rilevato quando viene chiamato **IDBInitialize::Initialize**.  
  
 Se un tentativo di modificare la password non riesce in modo imprevisto, il server restituisce il codice di errore 18468. Dal tentativo di connessione viene restituito un errore OLE DB standard.  
  
 Per altre informazioni sul set di proprietà DBPROPSET_SQLSERVERDBINIT, vedere [Proprietà di inizializzazione e di autorizzazione](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md).  

  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
