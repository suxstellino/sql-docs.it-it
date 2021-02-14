---
title: Opzione di configurazione del server clr enabled | Microsoft Docs
description: Informazioni su come usare l'opzione clr enabled per specificare se SQL Server può eseguire assembly utente. Scoprire quando non è supportata l'esecuzione di Common Language Runtime.
ms.custom: ''
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- assemblies [CLR integration], verifying can run
- clr enabled option
ms.assetid: 0722d382-8fd3-4fac-b4a8-cd2b7a7e0293
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 600b70e751fe4fcd3d80c7e86bd494ecc8cf1758
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "100347032"
---
# <a name="clr-enabled-server-configuration-option"></a>Opzione di configurazione del server clr enabled
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Utilizzare l'opzione clr enabled per specificare se gli assembly utente possono essere eseguiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'opzione clr enabled restituisce i valori riportati di seguito: 
  
|valore|Descrizione|  
|-----------|-----------------|  
|0|Esecuzione degli assembly non consentita in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|1|Esecuzione degli assembly consentita in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
Solo WOW64. Riavviare i server WOW64 per rendere effettive le modifiche alle impostazioni. Per altri tipi di server il riavvio non è necessario.  

Quando si esegue RECONFIGURE e il valore dell'opzione clr enabled viene modificato da 1 a 0, vengono scaricati immediatamente tutti i domini dell'applicazione contenenti assembly utente.  
  
>  **Esecuzione CLR (Common Language Runtime) non supportata quando è attivo il lightweight pooling.** Disabilitare una delle due opzioni "clr enabled" o "lightweight pooling". Le caratteristiche che si basano su CLR e che non funzionano correttamente in modalità fiber includono il tipo di dati **hierarchyid**, la funzione `FORMAT`, la replica e la gestione basata su criteri.  
> 
> [!WARNING]
>  CLR usa la Sicurezza dall'accesso di codice (CAS, Code Access Security) in .NET Framework, non più supportata come limite di sicurezza. Un assembly CLR creato con `PERMISSION_SET = SAFE` potrebbe essere in grado di accedere alle risorse di sistema esterne, chiamare codice non gestito e acquisire privilegi sysadmin. A partire da [!INCLUDE[sssql17](../../includes/sssql17-md.md)], viene introdotta un'opzione `sp_configure` denominata `clr strict security` per migliorare la sicurezza degli assembly CLR. `clr strict security` è abilitata per impostazione predefinita e considera gli assembly CLR `SAFE` e `UNSAFE` come se fossero contrassegnati `EXTERNAL_ACCESS`. È possibile disabilitare l'opzione `clr strict security` per la compatibilità con le versioni precedenti, ma questa operazione è sconsigliata. Microsoft consiglia che tutti gli assembly siano firmati con un certificato o una chiave asimmetrica con un account di accesso corrispondente che disponga dell'autorizzazione `UNSAFE ASSEMBLY` nel database master. Gli amministratori [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono anche aggiungere assembly a un elenco di assembly, considerato attendibile dal motore di database. Per altre, vedere [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md).
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene prima visualizzata l'impostazione corrente dell'opzione clr enabled, quindi abilitata l'opzione impostandone il valore su 1. Per disabilitare l'opzione, impostare il valore su 0.  
  
```sql  
EXEC sp_configure 'clr enabled';  
EXEC sp_configure 'clr enabled' , '1';  
RECONFIGURE;    
```  
  
## <a name="see-also"></a>Vedere anche  
 [Opzione di configurazione del server lightweight pooling](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Opzione di configurazione del server lightweight pooling](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)  
  
  
