---
title: Proprietà - Protocolli per MSSQLSERVER (scheda Avanzate)
description: Informazioni su vantaggi e requisiti della protezione estesa per l'autenticazione per il motore di database di SQL Server e su come abilitarla e configurarla.
ms.custom: seo-lt-2019
ms.date: 01/24/2019
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: abd5ca68-825f-4c07-b27c-3b3a79d03d74
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: eabb3e524e052823d4ea7b04348a58ccaf24fb64
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478412"
---
# <a name="protocols-for-mssqlserver-properties-advanced-tab"></a>Proprietà - Protocolli per MSSQLSERVER (scheda Avanzate)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Usare la scheda **Avanzate** della finestra di dialogo **Proprietà - Protocolli per MSSQLSERVER** per configurare la **protezione estesa per l'autenticazione** per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. La **protezione estesa** è una caratteristica dei componenti della rete implementati dal sistema operativo. La **protezione estesa** è disponibile in Windows 7 e Windows Server 2008 R2 ed è inclusa nei Service Pack per i sistemi operativi precedenti. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è più sicuro quando le connessioni vengono effettuate tramite **protezione estesa**. Per sfruttare alcuni vantaggi della **protezione estesa** è necessario selezionare **Forza crittografia** nella scheda **Flag** .

> [!IMPORTANT]  
> Per impostazione predefinita, in Windows la **protezione estesa** non è abilitata. Per informazioni su come abilitare la **Protezione estesa**, vedere:
> - [Protezione estesa di Windows \<extendedProtection\>](/iis/configuration/system.webserver/security/authentication/windowsauthentication/extendedprotection/)
> - [Panoramica sulla protezione estesa per l'autenticazione](/dotnet/framework/wcf/feature-details/extended-protection-for-authentication-overview)

Per ulteriori informazioni sulla configurazione di altri servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e una descrizione completa della **protezione estesa**, vedere le informazioni più recenti su [Microsoft.com](https://go.microsoft.com/fwlink/?LinkId=177752).

La **protezione estesa** è supportata in modo completo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client a partire da [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Al momento il supporto per **protezione estesa** non è previsto per altri provider client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="options"></a>Opzioni

### <a name="extended-protection"></a>protezione estesa

I tre valori possibili sono:  

- **Off**: indica che la **protezione estesa** è disabilitata. L'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] accetterà connessioni da qualsiasi client, protetto o non protetto. Il valore **Disattivata** è compatibile con i sistemi operativi precedenti e senza patch installate, sebbene sia meno sicuro. Utilizzare questa impostazione solo quando si è sicuri che i sistemi operativi dei client non supportano la protezione estesa.

- Se impostata su **Consentita**, la **protezione estesa** è obbligatoria per le connessioni da sistemi operativi che supportano la **protezione estesa**. Le connessioni da applicazioni client non protette in esecuzione su sistemi operativi client protetti vengono rifiutate. La **protezione estesa** viene ignorata per le connessioni da sistemi operativi non protetti. Sebbene sia più sicura di **Disattivata**, questa impostazione non garantisce il livello più elevato di sicurezza. È consigliabile utilizzarla negli ambienti misti, dove solo alcuni sistemi operativi o applicazioni supportano la **protezione estesa** .

- **Obbligatoria**: indica che una connessione, per essere accettata, deve provenire da un'applicazione protetta in un sistema operativo protetto. Questa impostazione è la più sicura delle tre opzioni. Tuttavia le connessioni da sistemi operativi che non supportano la **protezione estesa** non riusciranno a connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

### <a name="accepted-ntlm-spns"></a>SPN NTLM accettati

Un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può essere identificata da più di un nome dell'entità servizio (SPN) NTLM. I nomi SPN vengono elencati come una serie di stringhe separate da punti e virgola. Il valore **MSSQLSvc/HostName1.Contoso.com;MSSQLSvc/HostName2.Contoso.com**, ad esempio, indica che i client che tentano di connettersi ai nomi SPN denominati **MSSQLSvc/HOST1.Contoso.com** o **MSSQLSvc/HOST2.Contoso.com** sono consentiti. La lunghezza massima della variabile è di 2048 caratteri.

## <a name="see-also"></a>Vedere anche

[Protezione estesa per l'autenticazione con Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)