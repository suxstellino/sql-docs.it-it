---
description: Proprietà ConnectionString (ADO)
title: Proprietà ConnectionString (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::ConnectionString
helpviewer_keywords:
- ConnectionString property [ADO]
ms.assetid: 3be75b75-4d36-4479-ab64-9a456869252a
author: rothja
ms.author: jroth
ms.openlocfilehash: 3a5e9bc25728de2859074136adb1a28349b2806d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167717"
---
# <a name="connectionstring-property-ado"></a>Proprietà ConnectionString (ADO)
Indica le informazioni utilizzate per stabilire una connessione a un'origine dati.  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti  
 Imposta o restituisce un valore **stringa** .  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **ConnectionString** per specificare un'origine dati passando una stringa di connessione dettagliata contenente una serie di istruzioni *argument* *= value* separate da punti e virgola.  
  
 ADO supporta cinque argomenti per la proprietà **ConnectionString** . tutti gli altri argomenti passano direttamente al provider senza alcuna elaborazione da parte di ADO. Gli argomenti supportati da ADO sono i seguenti.  
  
|Argomento|Descrizione|  
|--------------|-----------------|  
|*Provider =*|Specifica il nome di un provider da utilizzare per la connessione.|  
|*Nome file =*|Specifica il nome di un file specifico del provider (ad esempio, un oggetto origine dati permanente) contenente le informazioni di connessione predefinite.|  
|*Provider remoto =*|Specifica il nome di un provider da utilizzare per l'apertura di una connessione sul lato client. (Solo Remote Data Service).|  
|*Server remoto =*|Specifica il nome del percorso del server da utilizzare per l'apertura di una connessione sul lato client. (Solo Remote Data Service).|  
|*URL =*|Specifica la stringa di connessione come URL assoluto che identifica una risorsa, ad esempio un file o una directory.|  
  
 Dopo aver impostato la proprietà **ConnectionString** e aperto l'oggetto [Connection](./connection-object-ado.md) , il provider può modificare il contenuto della proprietà, ad esempio eseguendo il mapping dei nomi degli argomenti definiti da ADO ai rispettivi equivalenti per il provider specifico.  
  
 La proprietà **ConnectionString** eredita automaticamente il valore usato per l'argomento *ConnectionString* del metodo [Open](./open-method-ado-connection.md) , pertanto è possibile eseguire l'override della proprietà **ConnectionString** corrente durante la chiamata al metodo **Open** .  
  
 Poiché l'argomento del *nome file* fa in modo che ADO carichi il provider associato, non è possibile passare gli argomenti *provider* e *nome file* .  
  
 La proprietà **ConnectionString** è in lettura/scrittura quando la connessione è chiusa e in sola lettura quando è aperta.  
  
 I duplicati di un argomento nella proprietà **ConnectionString** vengono ignorati. Viene utilizzata l'ultima istanza di qualsiasi argomento.  
  
> [!NOTE]
>  **Utilizzo servizio dati remoto** Se utilizzata su un oggetto **connessione** lato client, la proprietà **ConnectionString** può includere solo i parametri del *provider remoto* e del *server remoto* .  
  
 Nella tabella seguente sono elencati i provider ADO predefiniti per ogni sistema operativo Windows:  
  
|Provider ADO predefinito|Sistema operativo Windows|  
|--------------------------|------------------------------|  
|MSDASQL<br /><br /> Per migliorare la leggibilità del codice sorgente, specificare in modo esplicito il nome del provider nella stringa di connessione.|Windows 2000 (32 bit)<br /><br /> Windows XP (32 bit)<br /><br /> Windows 2003 Server (32 bit)<br /><br /> Windows Vista (32 bit)<br /><br /> Windows Vista Service Pack 1 o versione successiva (32 bit e 64 bit)<br /><br /> Versioni di Windows successive a Windows Vista (a 32 bit e a 64 bit)|  
|Nessuna impostazione predefinita.<br /><br /> Quando un'applicazione ADO viene eseguita nei sistemi operativi seguenti e non specifica il provider in modo esplicito, ADO restituisce l'errore seguente: "ADODB. Connessione: il provider non è specificato e non è presente alcun provider predefinito designato "|Windows 2000 (64 bit)<br /><br /> Windows XP (64 bit)<br /><br /> Windows 2003 Server (64 bit)<br /><br /> Windows Vista (64 bit)|  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Connection (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà ConnectionString, ConnectionTimeout e state (VB)](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [Esempio di proprietà ConnectionString, ConnectionTimeout e state (VC + +)](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)   
 [Appendice A: Provider](../../guide/appendixes/appendix-a-providers.md)