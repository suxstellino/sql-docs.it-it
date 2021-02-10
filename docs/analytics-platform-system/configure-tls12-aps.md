---
title: Configurazione di TLS 1,2
description: Raccomandazione per la configurazione di TLS 1,2 in APS
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 561a0a4f1d45d78e0f2fd23d61aae67f6adb6ff7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052742"
---
# <a name="configure-tls-12-in-aps"></a>Configurare TLS 1,2 in APS

Per proteggere i punti di sicurezza per usare solo TLS 1,2, è necessario disabilitare in modo esplicito altri protocolli in tutti gli host fisici e virtuali. Per disabilitare i protocolli è necessario modificare le impostazioni del registro di sistema. Per le modifiche del registro di sistema è necessario riavviare gli host virtuali e fisici.

> [!WARNING]
> Questa sezione, metodo o attività contiene procedure che spiegano come modificare il Registro di sistema. Tuttavia, potrebbero verificarsi gravi problemi se si modifica in modo errato il registro di sistema che può causare la perdita di dati e richiedere la reinstallazione del sistema operativo. Si consiglia vivamente di eseguire il backup del registro di sistema prima di modificarlo. In caso di problemi, sarà quindi possibile ripristinare il Registro di sistema. Per ulteriori informazioni su come eseguire il backup e il ripristino del registro di sistema, fare clic sul seguente numero di articolo per visualizzarlo nella Microsoft Knowledge Base:<br>
[322756](https://support.microsoft.com/help/322756) come eseguire il backup e il ripristino del registro di sistema in Windows

**Disabilitare**
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client]
"Enabled"=dword:00000000
"DisabledByDefault"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]
"Enabled"=dword:00000000
"DisabledByDefault"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client]
"Enabled"=dword:00000000
"DisabledByDefault"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]
"Enabled"=dword:00000000
"DisabledByDefault"=dword:00000001
```

Impostare anche le chiavi seguenti nei computer client in cui sono installati strumenti come gli adattatori di destinazione SSIS di APS.
```
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001 
```



