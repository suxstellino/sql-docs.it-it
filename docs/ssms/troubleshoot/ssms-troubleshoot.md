---
description: Ottenere dati di diagnostica dopo un arresto anomalo di SQL Server Management Studio (SSMS)
title: Risoluzione dei problemi relativi a un sistema che non risponde o ad arresti anomali di SSMS
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, sstein
ms.custom: seo-lt-2019
ms.date: 09/18/2019
ms.openlocfilehash: 5e4b7e4926265a1d6358b95bba4fae7b63ae016f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066056"
---
# <a name="get-diagnostic-data-after-a-sql-server-management-studio-ssms-crash"></a>Ottenere dati di diagnostica dopo un arresto anomalo di SQL Server Management Studio (SSMS)

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

## <a name="get-full-memory-dump-after-an-unresponsive-system-or-crash"></a>Ottenere il dump completo della memoria in caso di un sistema che non risponde o di un arresto anomalo

Ottenere un dump completo della memoria di SQL Server Management Studio (SSMS) quando smette di rispondere o in seguito a un arresto anomalo.

Per acquisire le informazioni di diagnostica per risolvere i problemi di un arresto anomalo di SSMS o quando SSMS non risponde, seguire questa procedura.

1. Scaricare [ProcDump](/sysinternals/downloads/procdump).

2. Decomprimere il download in una cartella.

3. Aprire il prompt dei comandi ed eseguire il comando seguente.

    ```console
    <PathToProcDumpFolder>\procdump.exe -e -h -ma -w ssms.exe
    ```

    Se viene richiesto di accettare un contratto di licenza, selezionare *Accetto*.

4. Avviare SSMS, se non è ancora stato avviato.

5. Riprodurre il problema.

6. Il testo verrà visualizzato nel prompt dei comandi relativo alla scrittura del file di dump. Attendere il completamento dell'operazione.

7. Creare una nuova cartella e copiare il file *.dmp di cui è stata eseguita la scrittura in tale cartella.

8. Copiare i file seguenti nella stessa cartella.

    "C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll"

9. Comprimere la cartella

## <a name="get-full-memory-dump-for-an-outofmemoryexception"></a>Ottenere il dump completo della memoria per un'eccezione OutOfMemoryException

Ottenere un dump completo della memoria di SSMS quando genera un'eccezione OutOfMemoryException.

È possibile ottenere un dump completo della memoria con qualsiasi eccezione gestita.

Per acquisire le informazioni di diagnostica per la risoluzione dei problemi relativi a un'eccezione di tipo OutOfMemoryException generata da SSMS, seguire questa procedura.

1. Scaricare [ProcDump](/sysinternals/downloads/procdump).

2. Decomprimere il download in una cartella.

3. Aprire il prompt dei comandi ed eseguire il comando seguente.

    ```console
    <PathToProcDumpFolder>\procdump.exe -e 1 -f System.OutOfMemoryException -ma -w ssms.exe
    ```

    Se viene richiesto di accettare un contratto di licenza, selezionare *Accetto*.

4. Avviare SQL Server Management Studio, se non è ancora stato avviato.

5. Riprodurre il problema.

6. Il testo verrà visualizzato nel prompt dei comandi relativo alla scrittura del file di dump. Attendere il completamento dell'operazione.

7. Creare una nuova cartella e copiare il file *.dmp di cui è stata eseguita la scrittura in tale cartella.

8. Copiare i file seguenti nella stessa cartella.

    "C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll"

9. Comprimere la cartella.