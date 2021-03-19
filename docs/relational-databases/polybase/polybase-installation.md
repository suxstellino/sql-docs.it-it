---
title: Installare PolyBase in Windows
description: Informazioni su come installare PolyBase come singolo nodo o come gruppo con scalabilità orizzontale di PolyBase. È possibile usare un'installazione guidata o un prompt dei comandi. Abilitare infine PolyBase.
ms.date: 02/05/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
helpviewer_keywords:
- PolyBase, installation
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 6a8f7bffd47e5159c0a048a3783f3ea6647bc097
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610923"
---
# <a name="install-polybase-on-windows"></a>Installare PolyBase in Windows

[!INCLUDE [SQL Server Windows Only ](../../includes/applies-to-version/sql-windows-only.md)]

Per installare una versione di valutazione di SQL Server, visitare [SQL Server Valutazioni](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016). 
   
## <a name="prerequisites"></a>Prerequisiti  
   
- Copia di valutazione di SQL Server a 64 bit.  
   
- Microsoft .NET Framework 4.5.  

- Memoria minima: 4 GB. 
   
- Spazio su disco rigido minimo: 2 GB.
  
- Consigliato: Almeno 16 GB di RAM.
   
- Polybase funziona correttamente se è abilitato il protocollo TCP/IP. TCP/IP è abilitato per impostazione predefinita in tutte le edizioni di SQL Server tranne le edizioni Developer e SQL Server Express. Perché PolyBase funzioni correttamente nelle edizioni Developer ed Express è necessario abilitare la connettività TCP/IP. Vedere [Abilitare o disabilitare un protocollo di rete del server](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).


>[!NOTE] 
> È possibile installare PolyBase in una sola istanza di SQL Server per computer.


>[!NOTE]
>Per usare PolyBase, sono necessarie le autorizzazioni a livello di CONTROLLO SERVER o sysadmin per il database.

>[!IMPORTANT]
>Per usare la funzionalità di distribuzione di calcolo su Hadoop, il cluster Hadoop di destinazione deve disporre dei componenti principali di HDFS, YARN e MapReduce con il server della cronologia processo abilitato. PolyBase invia la query di distribuzione tramite MapReduce e recupera lo stato dal server della cronologia processo. Senza uno dei due componenti la query ha esito negativo.
  
## <a name="single-node-or-polybase-scale-out-group"></a>Singolo nodo o gruppo di scalabilità orizzontale di PolyBase

Prima di installare PolyBase nelle istanze di SQL Server, decidere se si vuole eseguire un'installazione in un singolo nodo o in un [gruppo con scalabilità orizzontale PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).

Per un gruppo con scalabilità orizzontale PolyBase, verificare le condizioni seguenti:

- Tutti i computer si trovano nello stesso dominio.
- Usare lo stesso account del servizio e la stessa password durante l'installazione di PolyBase.
- Le istanze di SQL Server possono comunicare tra loro in rete.
- Le istanze di SQL Server sono tutte della stessa versione di SQL Server.

Dopo aver installato PolyBase autonomo o in un gruppo di scalabilità orizzontale, non è possibile apportare modifiche. Per modificare questa impostazione, è necessario disinstallare e reinstallare la funzionalità.

## <a name="use-the-installation-wizard"></a>Usare l'installazione guidata
   
1. Eseguire il file di installazione setup.exe di SQL Server.   
   
2. Selezionare **Installazione** e quindi scegliere **Nuova installazione di SQL Server autonomo o aggiunta di funzionalità**.  
   
3. Nella pagina Selezione funzionalità scegliere **Servizio query PolyBase per i dati esterni**.  

   ![Servizi PolyBase](../../relational-databases/polybase/media/install-wizard.png "Servizi PolyBase")  
   
   >[!NOTE]
   >PolyBase in SQL Server 2019 ora include un'opzione aggiuntiva **Connettore Java per origini dati HDFS**. Per altre informazioni su questa funzionalità, vedere [Funzionalità di anteprima di SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/04/24/sql-server-2019-community-technology-preview-2-5-is-now-available/).
   
4. Nella pagina Configurazione server configurare il **servizio motore PolyBase di SQL Server** e **SQL Server PolyBase Data Movement Service** per l'esecuzione con lo stesso account di dominio.  

   >[!IMPORTANT]
   >In un gruppo con scalabilità orizzontale di PolyBase, il motore PolyBase e il servizio di spostamento dati di PolyBase su tutti i nodi devono essere eseguiti con lo stesso account di dominio. Vedere [Gruppi con scalabilità orizzontale di PolyBase](#enable).

5. Nella pagina Configurazione di PolyBase, selezionare una delle due opzioni. Per altre informazioni, vedere [Gruppi con scalabilità orizzontale di PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
   
   - Usare un'istanza di SQL Server come istanza abilitata di PolyBase autonoma.  
   
     Scegliere questa opzione per usare l'istanza di SQL Server come nodo head autonomo.  
   
   - Usare l'istanza di SQL Server come parte di un gruppo con scalabilità orizzontale di PolyBase. Questa opzione apre il firewall per consentire le connessioni in ingresso. Le connessioni sono consentite per il motore di database di SQL Server, il motore PolyBase di SQL Server, SQL Server PolyBase Data Movement Service e SQL Browser. Il firewall consente anche le connessioni in ingresso da altri nodi in un gruppo con scalabilità orizzontale di PolyBase.  
   
     Questa opzione abilita anche le connessioni al firewall di Microsoft Distributed Transaction Coordinator (MSDTC) e modifica le impostazioni del registro di MSDTC.  
   
6. Nella pagina Configurazione di PolyBase, specificare un intervallo di porte che comprenda almeno sei porte. L'installazione di SQL Server alloca le prime sei porte disponibili dell'intervallo.  

   >[!IMPORTANT]
   > Dopo l'installazione, è necessario [abilitare la funzionalità PolyBase](#enable).


##  <a name="use-a-command-prompt"></a><a name="installing"></a> Usare un prompt dei comandi

Usare i valori in questa tabella per creare gli script di installazione. Il servizio motore PolyBase di SQL Server e SQL Server PolyBase Data Movement Service devono essere eseguiti con lo stesso account. In un gruppo con scalabilità orizzontale di PolyBase, i servizi di PolyBase su tutti i nodi devono essere eseguiti con lo stesso account di dominio.  
   
<!--SQL Server 2016/2017-->
::: moniker range="= sql-server-2016 || = sql-server-2017"

|componente di SQL Server|Parametro e valori|Descrizione|  
|--------------------------|--------------------------|-----------------|  
|Controllo dell'installazione di SQL Server|**Obbligatorio**<br /><br /> /FEATURES=PolyBase|Viene selezionata la funzionalità PolyBase.|  
|Motore di PolyBase per SQL Server|**Facoltativo**<br /><br /> /PBENGSVCACCOUNT|Viene specificato l'account per il servizio motore. L'impostazione predefinita è **NT Authority\NETWORK SERVICE**.|  
|Motore di PolyBase per SQL Server|**Facoltativo**<br /><br /> /PBENGSVCPASSWORD|Viene specificata la password per l'account del servizio motore.|  
|Motore di PolyBase per SQL Server|**Facoltativo**<br /><br /> /PBENGSVCSTARTUPTYPE|Viene specificata la modalità di avvio per il motore PolyBase: Automatico (predefinito), disabilitato e manuale.|  
|SQL Server PolyBase Data Movement |**Facoltativo**<br /><br /> /PBDMSSVCACCOUNT|Viene specificato l'account per il servizio di spostamento dati. L'impostazione predefinita è **NT Authority\NETWORK SERVICE**.|  
|SQL Server PolyBase Data Movement |**Facoltativo**<br /><br /> /PBDMSSVCPASSWORD|Viene specificata la password per l'account di spostamento dati.|  
|SQL Server PolyBase Data Movement |**Facoltativo**<br /><br /> /PBDMSSVCSTARTUPTYPE|Viene specificata la modalità di avvio per il servizio di spostamento dati: Automatico (predefinito), disabilitato e manuale.|  
|PolyBase|**Facoltativo**<br /><br /> /PBSCALEOUT|Viene indicato se l'istanza di SQL Server viene usata come parte di un gruppo di calcolo con scalabilità orizzontale di PolyBase. <br />Valori supportati: True, False.|  
|PolyBase|**Facoltativo**<br /><br /> /PBPORTRANGE|Viene specificato un intervallo di porte con almeno sei porte per i servizi PolyBase. Esempio:<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end
<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

|componente di SQL Server|Parametro e valori|Descrizione|  
|--------------------------|--------------------------|-----------------|  
|Controllo dell'installazione di SQL Server|**Obbligatorio**<br /><br /> /FEATURES=PolyBaseCore, PolyBaseJava, PolyBase | PolyBaseCore installa il supporto per tutte le funzionalità di PolyBase, ad eccezione della connettività Hadoop. PolyBaseJava abilita la connettività Hadoop. PolyBase installa entrambe le funzionalità. |  
|Motore di PolyBase per SQL Server|**Facoltativo**<br /><br /> /PBENGSVCACCOUNT|Viene specificato l'account per il servizio motore. L'impostazione predefinita è **NT Authority\NETWORK SERVICE**.|  
|Motore di PolyBase per SQL Server|**Facoltativo**<br /><br /> /PBENGSVCPASSWORD|Viene specificata la password per l'account del servizio motore.|  
|Motore di PolyBase per SQL Server|**Facoltativo**<br /><br /> /PBENGSVCSTARTUPTYPE|Viene specificata la modalità di avvio per il motore PolyBase: Automatico (predefinito), disabilitato e manuale.|  
|SQL Server PolyBase Data Movement |**Facoltativo**<br /><br /> /PBDMSSVCACCOUNT|Viene specificato l'account per il servizio di spostamento dati. L'impostazione predefinita è **NT Authority\NETWORK SERVICE**.|  
|SQL Server PolyBase Data Movement |**Facoltativo**<br /><br /> /PBDMSSVCPASSWORD|Viene specificata la password per l'account di spostamento dati.|  
|SQL Server PolyBase Data Movement |**Facoltativo**<br /><br /> /PBDMSSVCSTARTUPTYPE|Viene specificata la modalità di avvio per il servizio di spostamento dati: Automatico (predefinito), disabilitato e manuale.|  
|PolyBase|**Facoltativo**<br /><br /> /PBSCALEOUT|Viene indicato se l'istanza di SQL Server viene usata come parte di un gruppo di calcolo con scalabilità orizzontale di PolyBase. <br />Valori supportati: True, False.|  
|PolyBase|**Facoltativo**<br /><br /> /PBPORTRANGE|Viene specificato un intervallo di porte con almeno sei porte per i servizi PolyBase. Esempio:<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end

Dopo l'installazione, è necessario [abilitare la funzionalità PolyBase](#enable).



**Esempio**

Questo esempio mostra un esempio di script di installazione.  

```cmd
   
Setup.exe /Q /ACTION=INSTALL /IACCEPTSQLSERVERLICENSETERMS /FEATURES=SQLEngine,PolyBase   
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="\<fabric-domain>\Administrator"   
/INSTANCEDIR="C:\Program Files\Microsoft SQL Server" /PBSCALEOUT=TRUE   
/PBPORTRANGE=16450-16460 /SECURITYMODE=SQL /SAPWD="<StrongPassword>"   
/PBENGSVCACCOUNT="<DomainName>\<UserName>" /PBENGSVCPASSWORD="<StrongPassword>"   
/PBDMSSVCACCOUNT="<DomainName>\<UserName>" /PBDMSSVCPASSWORD="<StrongPassword>"  
   
```  

## <a name="enable-polybase"></a><a id="enable"></a> Abilitare PolyBase

Dopo l'installazione, è necessario abilitare PolyBase per accedere alle relative funzionalità. Usare il comando Transact-SQL seguente. Questa impostazione è abilitata per impostazione predefinita nelle istanze di SQL 2019 distribuite durante l'installazione del cluster Big Data.


```sql
exec sp_configure @configname = 'polybase enabled', @configvalue = 1;
RECONFIGURE;
```
## <a name="post-installation-notes"></a>Note sulle operazioni successive all'installazione  

PolyBase installa tre database utente, DWConfiguration, DWDiagnostics e DWQueue. Questi database sono per l'uso con PolyBase. Non modificarli o eliminarli.  

> [!CAUTION]
> L'aggiunta di polibase a un'installazione esistente di SQL Server installerà la funzionalità a livello di versione del supporto di installazione di, che può trovarsi dietro le altre funzionalità di SQL Server del livello di versione. Questo può causare un comportamento imprevisto o errori. Completare sempre l'installazione della funzionalità di polibase, portando la nuova funzionalità allo stesso livello di versione. Installare i Service Pack (SPs), gli aggiornamenti cumulativi (CUs) e/o le versioni di distribuzione generale (GDR) in base alle esigenze. Per determinare la versione di polibase, vedere [determinare la versione, l'edizione e il livello di aggiornamento delle SQL Server e dei relativi componenti](/troubleshoot/sql/general/determine-version-edition-update-level#polybase).
   
### <a name="how-to-confirm-installation"></a><a id="confirminstall"></a> Come confermare l'installazione  

Eseguire il comando seguente. Se installato, PolyBase restituisce 1; in caso contrario, restituisce 0.  

```sql  
SELECT SERVERPROPERTY ('IsPolyBaseInstalled') AS IsPolyBaseInstalled;  
```  

### <a name="firewall-rules"></a>Regole del firewall  

L'installazione di PolyBase di SQL Server crea sul computer le regole del firewall seguenti:  
   
- SQL Server PolyBase - Motore di database - \<SQLServerInstanceName> (TCP-In)  
   
- SQL Server PolyBase - Servizi PolyBase - \<SQLServerInstanceName> (TCP-In)  

- PolyBase di SQL Server - SQL Browser - (UDP-In)  
   
Al momento dell'installazione, se si usa l'istanza di SQL Server come parte di un gruppo di scalabilità orizzontale di PolyBase, queste regole sono attivate. Il firewall viene aperto per consentire le connessioni in ingresso. Tali connessioni sono consentite per il motore di database di SQL Server, il motore PolyBase di SQL Server, SQL Server PolyBase Data Movement Service e SQL Browser. Se il servizio firewall nel computer non è in esecuzione durante l'installazione, l'installazione di SQL Server non è in grado di attivare queste regole. In tal caso, avviare il servizio firewall sul computer e abilitare queste regole dopo l'installazione.  
   
#### <a name="to-enable-the-firewall-rules"></a>Per abilitare le regole del firewall  

1. Aprire il **Pannello di controllo**.  

2. Selezionare **Sistema e sicurezza** e selezionare **Windows Firewall**.  
   
3. Selezionare **Impostazioni avanzate** e selezionare **Regole connessioni in entrata**.  
   
4. Fare clic con il pulsante destro del mouse sulla regola disattivata e selezionare **Abilita regola**.  
   
### <a name="polybase-service-accounts"></a>Account del servizio PolyBase

Per modificare gli account del servizio per il motore PolyBase e PolyBase Data Movement Service, disinstallare e reinstallare la funzionalità PolyBase.

## <a name="next-steps"></a>Passaggi successivi  

Vedere [PolyBase configuration](../../relational-databases/polybase/polybase-configuration.md).