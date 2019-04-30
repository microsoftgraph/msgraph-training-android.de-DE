# <a name="completed-module-add-azure-ad-authentication"></a>Abgeschlossenes Modul: Hinzufügen der Azure AD-Authentifizierung

Die Version des Projekts in diesem Verzeichnis spiegelt das Abschließen des Lernprogramms durch [Hinzufügen der Azure AD-Authentifizierung](https://docs.microsoft.com/graph/tutorials/android?tutorial-step=3)wider. Wenn Sie diese Version des Projekts verwenden, müssen Sie den Rest des Lernprogramms beginnend beim [Abrufen von Kalenderdaten](https://docs.microsoft.com/graph/tutorials/android?tutorial-step=4)abschließen.

> **Hinweis:** Es wird davon ausgegangen, dass Sie bereits eine Anwendung im Azure-Portal registriert haben, wie in [Registrieren der APP im Portal](https://docs.microsoft.com/graph/tutorials/android?tutorial-step=2)angegeben. Sie müssen diese Version des Beispiels wie folgt konfigurieren:
>
> 1. Benennen Sie `./GraphTutorial/oauth_strings.xml.example` die Datei `oauth_strings.xml`in um.
> 1. Verschieben Sie `oauth_strings.xml` die Datei in `./GraphTutorial/app/src/main/res/values` das Verzeichnis.
> 1. Bearbeiten Sie `oauth_strings.xml` die Datei, und nehmen Sie die folgenden Änderungen vor.
>     1. Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie vom Azure-Portal erhalten haben.