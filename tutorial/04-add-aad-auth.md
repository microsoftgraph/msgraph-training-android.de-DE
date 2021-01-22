<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen. Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph abzurufen. Dazu integrieren Sie die [Microsoft Authentication Library (MSAL) für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) in die Anwendung.

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"res",** und wählen **Sie "Neu"** und dann **"Android-Ressourcenverzeichnis" aus.**

1. Ändern Sie den **Ressourcentyp,** `raw` und wählen Sie OK **aus.**

1. Klicken Sie mit der rechten Maustaste auf den neuen **unformatierte** Ordner, und wählen **Sie "Neu"** und dann **"Datei" aus.**

1. Benennen Sie die `msal_config.json` Datei, und wählen Sie **OK aus.**

1. Fügen Sie der Datei **msal_config.jsA0 Folgendes** hinzu.

    :::code language="json" source="../demo/GraphTutorial/msal_config.json.example":::

1. Ersetzen Sie die App-ID aus Ihrer App-Registrierung, und ersetzen Sie sie durch den `YOUR_APP_ID_HERE` `com.example.graphtutorial` Paketnamen Ihres Projekts.

    > [!IMPORTANT]
    > Wenn Sie Quellcodeverwaltung wie Git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei aus der Quellcodeverwaltung auszuschließen, um zu verhindern, dass versehentlich Ihre App-ID durch lecks `msal_config.json` geht.

## <a name="implement-sign-in"></a>Implementieren der Anmeldung

In diesem Abschnitt aktualisieren Sie das Manifest, um MSAL die Verwendung eines Browsers zur Authentifizierung des Benutzers zu ermöglichen, den Umleitungs-URI als von der App behandelt zu registrieren, eine Authentifizierungshilfsklasse zu erstellen und die App so zu aktualisieren, dass sie sich anmeldet und abmeldet.

1. Erweitern Sie **den Ordner "app/manifests",** und öffnen Sie **AndroidManifest.xml**. Fügen Sie die folgenden Elemente über dem Element `application` hinzu.

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

    > [!NOTE]
    > Diese Berechtigungen sind erforderlich, damit die MSAL-Bibliothek den Benutzer authentifiziert.

1. Fügen Sie das folgende Element innerhalb des Elements `application` hinzu, und ersetzen Sie die `YOUR_PACKAGE_NAME_HERE` Zeichenfolge durch den Paketnamen.

    ```xml
    <!--Intent filter to capture authorization code response from the default browser on the
        device calling back to the app after interactive sign in -->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data
                android:scheme="msauth"
                android:host="YOUR_PACKAGE_NAME_HERE"
                android:path="/callback" />
        </intent-filter>
    </activity>
    ```

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.** Ändern Der **Art in** **Schnittstelle**. Benennen Sie die `IAuthenticationHelperCreatedListener` Schnittstelle, und wählen Sie **OK aus.**

1. Öffnen Sie die neue Datei, und ersetzen Sie ihren Inhalt durch Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/IAuthenticationHelperCreatedListener.java" id="ListenerSnippet":::

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.** Benennen Sie die `AuthenticationHelper` Klasse, und wählen Sie **OK aus.**

1. Öffnen Sie die neue Datei, und ersetzen Sie ihren Inhalt durch Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/AuthenticationHelper.java" id="AuthHelperSnippet":::

1. Öffnen **Sie MainActivity,** und fügen Sie die folgenden Anweisungen `import` hinzu.

    ```java
    import android.util.Log;

    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalClientException;
    import com.microsoft.identity.client.exception.MsalException;
    import com.microsoft.identity.client.exception.MsalServiceException;
    import com.microsoft.identity.client.exception.MsalUiRequiredException;
    ```

1. Fügen Sie der Klasse die folgenden Membereigenschaften `MainActivity` hinzu.

    ```java
    private AuthenticationHelper mAuthHelper = null;
    private boolean mAttemptInteractiveSignIn = false;
    ```

1. Fügen Sie am Ende der `onCreate`-Funktion den folgenden Code hinzu.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="InitialLoginSnippet":::

1. Fügen Sie der Klasse die folgenden Funktionen `MainActivity` hinzu.

    ```java
    // Silently sign in - used if there is already a
    // user account in the MSAL cache
    private void doSilentSignIn(boolean shouldAttemptInteractive) {
        mAttemptInteractiveSignIn = shouldAttemptInteractive;
        mAuthHelper.acquireTokenSilently(getAuthCallback());
    }

    // Prompt the user to sign in
    private void doInteractiveSignIn() {
        mAuthHelper.acquireTokenInteractively(this, getAuthCallback());
    }

    // Handles the authentication result
    public AuthenticationCallback getAuthCallback() {
        return new AuthenticationCallback() {

            @Override
            public void onSuccess(IAuthenticationResult authenticationResult) {
                // Log the token for debug purposes
                String accessToken = authenticationResult.getAccessToken();
                Log.d("AUTH", String.format("Access token: %s", accessToken));

                hideProgressBar();

                setSignedInState(true);
                openHomeFragment(mUserName);
            }

            @Override
            public void onError(MsalException exception) {
                // Check the type of exception and handle appropriately
                if (exception instanceof MsalUiRequiredException) {
                    Log.d("AUTH", "Interactive login required");
                    if (mAttemptInteractiveSignIn) {
                        doInteractiveSignIn();
                    }
                } else if (exception instanceof MsalClientException) {
                    if (exception.getErrorCode() == "no_current_account" ||
                        exception.getErrorCode() == "no_account_found") {
                        Log.d("AUTH", "No current account, interactive login required");
                        if (mAttemptInteractiveSignIn) {
                            doInteractiveSignIn();
                        }
                    } else {
                        // Exception inside MSAL, more info inside MsalError.java
                        Log.e("AUTH", "Client error authenticating", exception);
                    }
                } else if (exception instanceof MsalServiceException) {
                    // Exception when communicating with the auth server, likely config issue
                    Log.e("AUTH", "Service error authenticating", exception);
                }
                hideProgressBar();
            }

            @Override
            public void onCancel() {
                // User canceled the authentication
                Log.d("AUTH", "Authentication canceled");
                hideProgressBar();
            }
        };
    }
    ```

1. Ersetzen Sie vorhandene `signIn` und `signOut` Funktionen durch Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="SignInAndOutSnippet":::

    > [!NOTE]
    > Beachten Sie, `signIn` dass die Methode eine automatische Anmeldung (über) vor sich `doSilentSignIn` führt. Beim Rückruf für diese Methode wird eine interaktive Anmeldung ausgeführt, wenn die automatische Anmeldung fehlschlägt. Dadurch wird vermieden, dass der Benutzer bei jedem Start der App eine Eingabeaufforderung einfordern muss.

1. Speichern Sie die Änderungen, und führen Sie die App aus.

1. Wenn Sie auf das **Menüelement "Anmelden"** tippen, wird ein Browser zur Azure AD-Anmeldeseite geöffnet. Melden Sie sich mit Ihrem Konto an.

Sobald die App fortgesetzt wird, sollte ein Zugriffstoken im Debugprotokoll in Android Studio angezeigt werden.

![Screenshot des Logcat-Fensters in Android Studio](./images/debugger-access-token.png)

## <a name="get-user-details"></a>Benutzerdetails abrufen

In diesem Abschnitt erstellen Sie eine Hilfsklasse für alle Aufrufe von Microsoft Graph und aktualisieren die Klasse so, dass sie diese neue Klasse verwendet, um den angemeldeten `MainActivity` Benutzer zu erhalten.

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.** Benennen Sie die `GraphHelper` Klasse, und wählen Sie **OK aus.**

1. Öffnen Sie die neue Datei, und ersetzen Sie ihren Inhalt durch Folgendes.

    ```java
    package com.example.graphtutorial;

    import com.microsoft.graph.authentication.IAuthenticationProvider;
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.http.IHttpRequest;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;

    // Singleton class - the app only needs a single instance
    // of the Graph client
    public class GraphHelper implements IAuthenticationProvider {
        private static GraphHelper INSTANCE = null;
        private IGraphServiceClient mClient = null;
        private String mAccessToken = null;

        private GraphHelper() {
            mClient = GraphServiceClient.builder()
                    .authenticationProvider(this).buildClient();
        }

        public static synchronized GraphHelper getInstance() {
            if (INSTANCE == null) {
                INSTANCE = new GraphHelper();
            }

            return INSTANCE;
        }

        // Part of the Graph IAuthenticationProvider interface
        // This method is called before sending the HTTP request
        @Override
        public void authenticateRequest(IHttpRequest request) {
            // Add the access token in the Authorization header
            request.addHeader("Authorization", "Bearer " + mAccessToken);
        }

        public void getUser(String accessToken, ICallback<User> callback) {
            mAccessToken = accessToken;

            // GET /me (logged in user)
            mClient.me().buildRequest()
                    .select("displayName,mail,mailboxSettings,userPrincipalName")
                    .get(callback);
        }
    }
    ```

    > [!NOTE]
    > Überlegen Sie, was dieser Code macht.
    >
    > - Es implementiert die Schnittstelle `IAuthenticationProvider` zum Einfügen des Zugriffstokens in `Authorization` den Header für ausgehende HTTP-Anforderungen.
    > - Es macht eine Funktion verfügbar, um die Informationen des angemeldeten Benutzers `getUser` vom Graph-Endpunkt `/me` zu erhalten.

1. Fügen Sie die `import` folgenden Anweisungen am oberen Rand der Datei **"MainActivity"** hinzu.

    ```java
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.User;
    ```

1. Fügen Sie der Klasse die folgende Funktion `MainActivity` hinzu, um eine für `ICallback` den Graph-Aufruf zu generieren.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="GetUserCallbackSnippet":::

1. Entfernen Sie die folgenden Zeilen, die den Benutzernamen und die E-Mail festlegen:

    ```java
    // For testing
    mUserName = "Megan Bowen";
    mUserEmail = "meganb@contoso.com";
    ```

1. Ersetzen Sie `onSuccess` die Außerkraftsetzung im `AuthenticationCallback` Folgenden durch Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="OnSuccessSnippet":::

1. Speichern Sie die Änderungen, und führen Sie die App aus. Nach der Anmeldung wird die Benutzeroberfläche mit dem Anzeigenamen und der E-Mail-Adresse des Benutzers aktualisiert.
