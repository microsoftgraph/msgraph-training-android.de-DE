# <a name="how-to-run-the-completed-project"></a>So führen Sie das abgeschlossene Projekt aus

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen des abgeschlossenen Projekts in diesem Ordner benötigen Sie Folgendes:

- [Android Studio](https://developer.android.com/studio/) ist auf Ihrem Entwicklungscomputer installiert. (**Hinweis:** Dieses Lernprogramm wurde mit Android Studio, Version 3.6.2, und dem Android 10.0 SDK geschrieben. Die Schritte in diesem Handbuch funktionieren möglicherweise mit anderen Versionen, wurden jedoch noch nicht getestet.)
- Entweder ein persönliches Microsoft-Konto mit einem Postfach auf Outlook.com oder ein Microsoft-Arbeits- oder Schulkonto.

Wenn Sie kein Microsoft-Konto haben, gibt es mehrere Optionen, um ein kostenloses Konto zu erhalten:

- Sie können [sich für ein neues persönliches Microsoft-Konto registrieren.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)
- Sie können [sich für das Office 365-Entwicklerprogramm](https://developer.microsoft.com/office/dev-program) registrieren, um ein kostenloses Office 365-Abonnement zu erhalten.

## <a name="register-an-application-with-the-azure-portal"></a>Registrieren einer Anwendung im Azure Portal

1. Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com). Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.

1. Wählen Sie in der linken Navigationsleiste **Azure Active Directory** aus, und wählen Sie dann **App-Registrierungen** unter **Verwalten** aus.

    ![Screenshot der APP-Registrierungen ](../../tutorial/images/aad-portal-app-registrations.png)

1. Wählen Sie **Neue Registrierung** aus. Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.

    - Legen Sie **Name** auf `Android Graph Tutorial` fest.
    - Legen Sie **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** fest.
    - Legen **Sie unter "Umleitungs-URI"** die Dropdownliste auf "Öffentlicher Client/systemeigener Client" **(mobiler &-Desktop)** und den Wert auf "Ersetzen" durch den Paketnamen Ihres `msauth://YOUR_PACKAGE_NAME/callback` `YOUR_PACKAGE_NAME` Projekts.

    ![Screenshot der Seite "Anwendung registrieren"](../../tutorial/images/aad-register-an-app.png)

1. Wählen Sie **Registrieren** aus. Kopieren Sie auf der **Android Graph-Lernprogrammseite** den Wert der **Anwendungs-ID (Client-ID),** und speichern Sie ihn. Sie benötigen ihn im nächsten Schritt.

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](../../tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a>Konfigurieren des Beispiels

1. Benennen Sie `msal_config.json.example` die Datei `msal_config.json` um, und verschieben Sie sie in das `GraphTutorial/app/src/main/res/raw` Verzeichnis.
1. Bearbeiten Sie `msal_config.json` die Datei, und nehmen Sie die folgenden Änderungen vor.
    1. Ersetzen `YOUR_APP_ID_HERE` Sie diese durch die **Anwendungs-ID,** die Sie aus dem Azure-Portal erhalten haben.
    1. Ersetzen `com.example.graphtutorial` Sie dies durch den Paketnamen.

## <a name="run-the-sample"></a>Ausführen des Beispiels

Wählen Sie in Android Studio im Menü "Ausführen" die Option **"App** **ausführen"** aus.
