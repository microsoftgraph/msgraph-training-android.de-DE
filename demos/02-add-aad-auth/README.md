# <a name="completed-module-add-azure-ad-authentication"></a>Abgeschlossenes Modul: Azure AD Authentifizierung hinzufügen

Die Version des Projekts in diesem Verzeichnis spiegelt das Abschließen des Lernprogramms bis zum [Add Azure AD-Authentifizierung](https://docs.microsoft.com/graph/tutorials/android?tutorial-step=3)wider. Wenn Sie diese Version des Projekts verwenden, müssen Sie den Rest des Lernprogramms ab dem [Abrufen von Kalenderdaten](https://docs.microsoft.com/graph/tutorials/android?tutorial-step=4)ausführen.

> **Hinweis:** Es wird davon ausgegangen, dass Sie bereits eine Anwendung im Azure-Portal registriert haben, wie unter [Registrieren der APP im Portal](https://docs.microsoft.com/graph/tutorials/android?tutorial-step=2)angegeben. Sie müssen diese Version des Beispiels wie folgt konfigurieren:
>
> 1. Benennen Sie `./GraphTutorial/msal_config.json.example` die Datei `msal_config.json`in.
> 1. Verschiebt die `msal_config.json` Datei in das `./GraphTutorial/app/src/main/res/raw` Verzeichnis.
> 1. Bearbeiten Sie `msal_config.json` die Datei, und nehmen Sie die folgenden Änderungen vor.
>     1. Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie aus dem Azure-Portal erhalten haben.
