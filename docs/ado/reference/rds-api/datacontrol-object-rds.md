---
description: Oggetto DataControl (Servizi Desktop remoto)
title: Oggetto DataControl (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DataControl
- RDS.DataControl
helpviewer_keywords:
- DataControl object [ADO]
ms.assetid: d85ea4fc-451c-436e-97b8-58f92b149dd0
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c65d8f639ec1747e7085f8d88c266ed6b6299d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169003"
---
# <a name="datacontrol-object-rds"></a>Oggetto DataControl (Servizi Desktop remoto)
Associa un [Recordset](../ado-api/recordset-object-ado.md) di query di dati a uno o più controlli, ad esempio una casella di testo, un controllo griglia o una casella combinata, per visualizzare i dati del **Recordset** in una pagina Web.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DataControl"  
   <PARAM NAME="Connect" VALUE="DSN=DSNName;UID=MyUserID;PWD=MyPassword;">  
   <PARAM NAME="Server" VALUE="https://awebsrvr">  
   <PARAM NAME="SQL" VALUE="QueryText">  
</OBJECT>  
```  
  
## <a name="remarks"></a>Osservazioni  
 ID di classe per **RDS. L'oggetto DataControl** è BD96C556-65A3-11D0-983A-00C04FC29E33.  
  
> [!NOTE]
>  Se viene ricevuto un errore, il Servizi Desktop remoto [. DataSpace](./dataspace-object-rds.md) o Servizi Desktop remoto **. L'oggetto DataControl** non viene caricato, assicurarsi di usare l'ID classe corretto. Gli ID di classe per questi oggetti sono stati modificati rispetto alla versione 1,0 e 1,1. Tenere inoltre presente che quando si utilizza l'oggetto di **controllo** Servizi Desktop remoto è necessario impostare anche le colonne che ammettono valori null.  
  
 Per uno scenario di base, è necessario impostare solo le proprietà **SQL**, **Connect** e **server** del Servizi Desktop remoto **. Oggetto DataControl** , che chiamerà automaticamente l'oggetto business predefinito, [RDSServer. DataFactory](./datafactory-object-rdsserver.md).  
  
 Tutte le proprietà in Servizi Desktop remoto **. DataControl** è facoltativo perché gli oggetti business personalizzati possono sostituire le proprie funzionalità.  
  
> [!NOTE]
>  Se si eseguono query per più risultati, viene restituito solo il primo [Recordset](../ado-api/recordset-object-ado.md) . Se sono necessari più set di risultati, assegnare ognuno a un proprio **DataControl**. Di seguito è riportato un esempio di query per più risultati: `"Select * from Authors, Select * from Topics"`  
  
 Aggiunta di "DFMode = 20;" alla stringa di connessione quando si utilizza **RDS. L'oggetto DataControl** può migliorare le prestazioni del server quando si aggiornano i dati. Con questa impostazione, l'oggetto **RDSServer. DataFactory** sul server usa una modalità con utilizzo intensivo di risorse minore. Tuttavia, le funzionalità seguenti non sono disponibili in questa configurazione:  
  
-   Utilizzo di query con parametri.  
  
-   Recupero delle informazioni sul parametro o sulla colonna prima della chiamata al metodo **Execute** .  
  
-   Impostazione degli **aggiornamenti Transact** su **true**.  
  
-   Recupero dello stato delle righe.  
  
-   Chiamata al metodo [Resync](../ado-api/resync-method.md) .  
  
-   Aggiornamento (in modo esplicito o automatico) tramite la proprietà [Update Resync](../ado-api/update-resync-property-dynamic-ado.md) .  
  
-   Impostazione delle proprietà del **comando** o del [Recordset](./recordset-sourcerecordset-properties-rds.md) .  
  
-   Uso di **adCmdTableDirect**.  
  
 **RDS.** Per impostazione predefinita, l'oggetto DataControl viene eseguito in modalità asincrona. Se è necessaria l'esecuzione sincrona per l'applicazione, impostare il parametro [ExecuteOptions](./executeoptions-property-rds.md) uguale a **adcExecSync** e il parametro [FetchOptions](./fetchoptions-property-rds.md) uguale a **adcFetchUpFront**, come illustrato nell'esempio seguente.  
  
```  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"   
    ID="DataControl"  
   <PARAM NAME="Connect" VALUE="DSN=DSNName;UID=MyUserID;PWD=MyPassword;">  
   <PARAM NAME="Server" VALUE="https://awebsrvr">  
   <PARAM NAME="SQL" VALUE="QueryText">  
   <PARAM NAME="ExecuteOptions" VALUE="1">   <PARAM NAME="FetchOptions" VALUE="1">  
</OBJECT>  
```  
  
 Utilizzare un Servizi Desktop remoto **. Oggetto DataControl** per collegare i risultati di una singola query a uno o più controlli visivi. Si supponga, ad esempio, di codificare una query che richiede i dati del cliente, ad esempio nome, residenza, luogo di nascita, età e stato del cliente prioritario. È possibile utilizzare un singolo **RDS. Oggetto DataControl** per visualizzare il nome, l'età e l'area geografica di un cliente in tre caselle di testo separate; Stato del cliente prioritario in una casella di controllo; e tutti i dati in un controllo Grid.  
  
 Usare Servizi Desktop remoto diversi **. Oggetti DataControl** per collegare i risultati di più query a controlli visivi diversi. Si supponga, ad esempio, di usare una query per ottenere informazioni su un cliente e una seconda query per ottenere informazioni sulle merci acquistate dal cliente. Si desidera visualizzare i risultati della prima query in tre caselle di testo e una casella di controllo e i risultati della seconda query in un controllo griglia. Se si usa l'oggetto business predefinito (**RDSServer. DataFactory**), è necessario eseguire le operazioni seguenti:  
  
-   Aggiungere due servizi desktop remoto **. Oggetti DataControl** per la pagina Web.  
  
-   Scrivere due query, una per ogni proprietà **SQL** dei due servizi desktop remoto **. Oggetti DataControl** . Un **RDS. L'oggetto DataControl** conterrà una query SQL che richiede informazioni sui clienti; il secondo conterrà una query che richiede un elenco di merci acquistate dal cliente.  
  
-   Nei tag OBJECT di ogni controllo associato specificare il valore DATAFLD per impostare i valori per i dati che si desidera visualizzare in ogni controllo visivo.  
  
 Non esiste alcuna restrizione del conteggio per il numero di Servizi Desktop remoto **. Oggetti DataControl** che è possibile incorporare utilizzando tag oggetto in una singola pagina Web.  
  
 Quando si definisce il Servizi Desktop remoto **. Oggetto DataControl** in una pagina Web, usare valori di **altezza** e **larghezza** diversi da zero, ad esempio 1 (per evitare l'inclusione di spazio aggiuntivo).  
  
 I componenti client di Remote Data Service sono già inclusi come parte di Internet Explorer 4,0; Pertanto, non è necessario includere un parametro codebase in Servizi Desktop remoto **. Tag oggetto DataControl** .  
  
 Con Internet Explorer 4,0 o versione successiva, è possibile eseguire l'associazione ai dati usando i controlli HTML e i controlli ActiveX® solo se sono contrassegnati come controlli del modello di Apartment.  
  
> [!NOTE]
>  **Utenti di Microsoft Visual Basic** **RDS. DataControl** è sicuro per gli script e viene utilizzato solo nelle applicazioni basate sul Web. Non è necessario un Visual Basic applicazione client.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di oggetto DataControl (VBScript)](./datacontrol-object-example-vbscript.md)