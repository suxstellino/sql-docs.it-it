---
title: Sicurezza di SQL Server
description: Viene fornita una panoramica delle funzionalità di sicurezza di SQL Server e degli scenari per la creazione di applicazioni ADO.NET sicure da usare con SQL Server.
ms.date: 09/26/2019
ms.assetid: 9053724d-a1fb-4f0f-b9dc-7f6dd893e8ff
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-kaywon
ms.openlocfilehash: e70f6da7755b28c400cc80a5ee6de79a004e4e42
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2020
ms.locfileid: "97038919"
---
# <a name="sql-server-security"></a>Sicurezza di SQL Server

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

In SQL Server sono disponibili molte funzionalità che supportano la creazione di applicazioni di database sicure.  
  
Indipendentemente dalla versione di SQL Server in uso, esistono problemi di sicurezza comuni, ad esempio furto dei dati o vandalismo, che è opportuno considerare. L'integrità dei dati deve essere considerata anche un problema di sicurezza. Se i dati non sono protetti, è possibile che possano diventare inutili se è consentita la manipolazione dei dati ad hoc e se i dati vengono modificati inavvertitamente o intenzionalmente con valori non corretti o eliminati completamente. Inoltre, esistono spesso requisiti legali che devono essere rispettati, ad esempio l'archiviazione corretta delle informazioni riservate. L'archiviazione di alcuni tipi di dati personali è interamente proscritta, a seconda delle leggi che si applicano a una particolare giurisdizione.  
  
Ogni versione di SQL Server, così come ogni versione di Windows, include funzionalità di sicurezza diverse e ogni nuova versione presenta funzionalità più avanzate rispetto alle versioni precedenti. È importante comprendere che le funzionalità di sicurezza non possono garantire da sole la sicurezza di un'applicazione di database. Ogni applicazione di database è univoca relativamente a requisiti, ambiente di esecuzione, modello di distribuzione, posizione fisica e popolamento degli utenti. Per alcune applicazioni locali nell'ambito può essere necessaria solo una protezione minima, mentre altre applicazioni locali o distribuite su Internet possono richiedere misure di sicurezza rigorose e monitoraggio e valutazione continui.  
  
I requisiti di sicurezza di un'applicazione di database di SQL Server devono essere considerati già in fase di progettazione e non in un secondo momento. La valutazione delle minacce nelle prime fasi del ciclo di sviluppo offre la possibilità di attenuare i potenziali danni laddove viene rilevata una vulnerabilità.  
  
Anche se la progettazione iniziale di un'applicazione è valida, è possibile che emergano nuove minacce man mano che il sistema evolve. Creando più linee di difesa per il database, è possibile ridurre al minimo i danni causati da una violazione della sicurezza. La prima linea di difesa consiste nel ridurre la superficie di attacco senza concedere mai più autorizzazioni rispetto a quelle assolutamente necessarie.  
  
Negli argomenti di questa sezione viene fornita una breve descrizione delle funzionalità di sicurezza di SQL Server pertinenti per gli sviluppatori, con collegamenti ai relativi argomenti della documentazione online di SQL Server e ad altre risorse di approfondimento.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
[Autenticazione in SQL Server](authentication-sql-server.md)  
Descrive gli account di accesso e l'autenticazione in SQL Server e offre collegamenti a risorse aggiuntive. 

[Autenticazione di Azure Active Directory](azure-active-directory-authentication.md)  
Descrizione di come usare le modalità di autenticazione di Azure Active Directory supportate per connettersi alle origini dati di Azure SQL con SqlClient.
  
[Scenari di sicurezza delle applicazioni in SQL Server](application-security-scenarios-sql-server.md)  
Sono contenuti argomenti che illustrano diversi scenari di sicurezza per applicazioni ADO.NET e SQL Server.  
  
[Sicurezza di SQL Server Express](sql-server-express-security.md)  
Vengono illustrate le considerazioni sulla sicurezza relative a SQL Server Express.  
  
## <a name="related-sections"></a>Sezioni correlate  
[Centro sicurezza per il motore di Database di SQL Server e il Database SQL di Azure](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
Offre considerazioni sulla sicurezza per SQL Server e il database SQL di Azure.

[Considerazioni sulla sicurezza per un'installazione di SQL Server](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
Descrive i problemi di sicurezza da prendere in considerazione prima di installare SQL Server.

## <a name="next-steps"></a>Passaggi successivi
- [SQL Server e ADO.NET](index.md)
