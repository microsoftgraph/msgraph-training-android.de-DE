<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung zur Unterstützung der Authentifizierung mit Azure AD. Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken für den Aufruf von Microsoft Graph abzurufen. In diesem Schritt integrieren Sie die [Microsoft-Authentifizierungsbibliothek (MSAL) für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) in die Anwendung.

Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Values** , und wählen Sie **neu**und dann **Ressourcendatei**aus. Benennen Sie die `oauth_strings` Datei, und klicken Sie auf **OK**. Fügen Sie dem- `resources` Element die folgenden Werte hinzu.

```xml
<string name="oauth_app_id">YOUR_APP_ID_HERE</string>
<string name="oauth_redirect_uri">msalYOUR_APP_ID_HERE</string>
<string-array name="oauth_scopes">
    <item>User.Read</item>
    <item>Calendars.Read</item>
</string-array>
```

> [!IMPORTANT]
> Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei `oauth_strings.xml` aus der Quellcodeverwaltung auszuschließen, um versehentlich Ihre APP-ID zu vernichten.

## <a name="implement-sign-in"></a>Implementieren der Anmeldung

Erweitern Sie den Ordner **App/Manifests** , und öffnen Sie **AndroidManifest. XML**. Fügen Sie die folgenden Elemente oberhalb `application` des-Elements hinzu.

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Diese Berechtigungen sind erforderlich, damit die MSAL-Bibliothek den Benutzer authentifizieren muss.

Fügen Sie nun das folgende Element innerhalb `application` des-Elements hinzu.

```xml
<activity android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />

        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <data
            android:host="auth"
            android:scheme="@string/oauth_redirect_uri" />
    </intent-filter>
</activity>
```

Dies ermöglicht es MSAL, einen Browser zur Authentifizierung des Benutzers zu verwenden und den Umleitungs-URI als von der APP behandelt zu registrieren.

### <a name="implement-an-authentication-helper"></a>Implementieren einer Authentifizierungs Hilfe

Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie **neu**und dann **Java Class**aus. Benennen Sie die `AuthenticationHelper` Klasse, und wählen Sie **OK**aus. Öffnen Sie die neue Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

```java
package com.example.graphtutorial;

import android.app.Activity;
import android.content.Context;
import android.content.Intent;

import com.microsoft.identity.client.AuthenticationCallback;
import com.microsoft.identity.client.IAccount;
import com.microsoft.identity.client.PublicClientApplication;

// Singleton class - the app only needs a single instance
// of PublicClientApplication
public class AuthenticationHelper {
    private static AuthenticationHelper INSTANCE = null;
    private PublicClientApplication mPCA = null;
    private String[] mScopes;

    private AuthenticationHelper(Context ctx) {
        String appId = ctx.getResources().getString(R.string.oauth_app_id);
        mScopes = ctx.getResources().getStringArray(R.array.oauth_scopes);
        mPCA = new PublicClientApplication(ctx, appId);
    }

    public static synchronized AuthenticationHelper getInstance(Context ctx) {
        if (INSTANCE == null) {
            INSTANCE = new AuthenticationHelper(ctx);
        }

        return INSTANCE;
    }

    // Version called from fragments. Does not create an
    // instance if one doesn't exist
    public static synchronized AuthenticationHelper getInstance() {
        if (INSTANCE == null) {
            throw new IllegalStateException(
                    "AuthenticationHelper has not been initialized from MainActivity");
        }

        return INSTANCE;
    }

    public boolean hasAccount() {
        return !mPCA.getAccounts().isEmpty();
    }

    public void handleRedirect(int requestCode, int resultCode, Intent data) {
        mPCA.handleInteractiveRequestRedirect(requestCode, resultCode, data);
    }

    public void acquireTokenInteractively(Activity activity, AuthenticationCallback callback) {
        mPCA.acquireToken(activity, mScopes, callback);
    }

    public void acquireTokenSilently(AuthenticationCallback callback) {
        mPCA.acquireTokenSilentAsync(mScopes, mPCA.getAccounts().get(0), callback);
    }

    public void signOut() {
        for (IAccount account : mPCA.getAccounts()) {
            mPCA.removeAccount(account);
        }
    }
}
```

Aktualisieren `MainActivity` Sie nun, um diese neue Klasse zu verwenden. Öffnen Sie **Main** -on, und `import` fügen Sie die folgenden Anweisungen hinzu.

```java
import android.content.Intent;
import android.support.annotation.Nullable;
import android.util.Log;

import com.microsoft.identity.client.AuthenticationCallback;
import com.microsoft.identity.client.AuthenticationResult;
import com.microsoft.identity.client.exception.MsalClientException;
import com.microsoft.identity.client.exception.MsalException;
import com.microsoft.identity.client.exception.MsalServiceException;
import com.microsoft.identity.client.exception.MsalUiRequiredException;
```

Als Nächstes fügen Sie der `MainActivity` Klasse die folgende Elementeigenschaft hinzu.

```java
private AuthenticationHelper mAuthHelper = null;
```

Update `onCreate` auf fest `mAuthHelper`gelegt. Fügen Sie Folgendes am Ende der `onCreate` Funktion hinzu.

```java
// Get the authentication helper
mAuthHelper = AuthenticationHelper.getInstance(getApplicationContext());
```

Fügen Sie eine außer `onActivityResult` Kraft setzung für die Behandlung von Authentifizierungs Antworten hinzu.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    mAuthHelper.handleRedirect(requestCode, resultCode, data);
}
```

Fügen Sie nun die folgenden Funktionen zur `MainActivity` Klasse hinzu.

```java
// Silently sign in - used if there is already a
// user account in the MSAL cache
private void doSilentSignIn() {
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
        public void onSuccess(AuthenticationResult authenticationResult) {
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
                doInteractiveSignIn();

            } else if (exception instanceof MsalClientException) {
                // Exception inside MSAL, more info inside MsalError.java
                Log.e("AUTH", "Client error authenticating", exception);
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

Ersetzen Sie schließlich die vorhandenen `signIn` und `signOut` -Funktionen durch Folgendes.

```java
private void signIn() {
    showProgressBar();
    if (mAuthHelper.hasAccount()) {
        doSilentSignIn();
    } else {
        doInteractiveSignIn();
    }
}

private void signOut() {
    mAuthHelper.signOut();

    setSignedInState(false);
    openHomeFragment(mUserName);
}
```

Beachten Sie, `signIn` dass die Methode zuerst überprüft, ob ein Benutzerkonto im MSAL-Cache vorhanden ist. Ist dies der Fall, wird versucht, die Token im Hintergrund zu aktualisieren, ohne dass der Benutzer jedes Mal aufgefordert werden muss, die APP zu starten.

Speichern Sie die Änderungen, und führen Sie die App aus. Wenn Sie auf das Menüelement **Anmelden** tippen, wird ein Browser zur Azure AD-Anmeldeseite geöffnet. Melden Sie sich mit Ihrem Konto an. Sobald die APP wieder aufgenommen wird, sollte ein Zugriffstoken im Debug-Protokoll in Android Studio gedruckt werden.

![Screenshot des Logcat-Fensters in Android Studio](./images/debugger-access-token.png)

## <a name="get-user-details"></a>Benutzer Details abrufen

Erstellen Sie zunächst eine Hilfsklasse für alle Aufrufe von Microsoft Graph. Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie **neu**und dann **Java Class**aus. Benennen Sie die `GraphHelper` Klasse, und wählen Sie **OK**aus. Öffnen Sie die neue Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

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
        mClient.me().buildRequest().get(callback);
    }
}
```

Beachten Sie diesen Code.

- Die `IAuthenticationProvider` Schnittstelle wird implementiert, um das Zugriffstoken in `Authorization` den Header für ausgehende HTTP-Anforderungen einzufügen.
- Sie stellt eine `getUser` Funktion zum Abrufen der Informationen des angemeldeten Benutzers vom `/me` Graph-Endpunkt bereit.

Aktualisieren Sie nun `MainActivity` die Klasse, um diese neue Klasse zum Abrufen des angemeldeten Benutzers zu verwenden. Fügen Sie am `import` Anfang der **Haupt** Datei die folgenden Anweisungen hinzu.

```java
import com.microsoft.graph.concurrency.ICallback;
import com.microsoft.graph.core.ClientException;
import com.microsoft.graph.models.extensions.IGraphServiceClient;
import com.microsoft.graph.models.extensions.User;
```

Als Nächstes fügen Sie der `MainActivity` Klasse die folgende Funktion hinzu, um `ICallback` einen für den Graph-Aufruf zu generieren.

```java
private ICallback<User> getUserCallback() {
    return new ICallback<User>() {
        @Override
        public void success(User user) {
            Log.d("AUTH", "User: " + user.displayName);

            mUserName = user.displayName;
            mUserEmail = user.mail == null ? user.userPrincipalName : user.mail;

            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    hideProgressBar();

                    setSignedInState(true);
                    openHomeFragment(mUserName);
                }
            });

        }

        @Override
        public void failure(ClientException ex) {
            Log.e("AUTH", "Error getting /me", ex);
            mUserName = "ERROR";
            mUserEmail = "ERROR";

            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    hideProgressBar();

                    setSignedInState(true);
                    openHomeFragment(mUserName);
                }
            });
        }
    };
}
```

Entfernen Sie die folgenden Zeilen, die den Benutzernamen und die e-Mail-Adresse festlegen:

```java
// For testing
mUserName = "Megan Bowen";
mUserEmail = "meganb@contoso.com";
```

Ersetzen Sie schließlich die `onSuccess` Außerkraftsetzung `AuthenticationCallback` im folgenden.

```java
@Override
public void onSuccess(AuthenticationResult authenticationResult) {
    // Log the token for debug purposes
    String accessToken = authenticationResult.getAccessToken();
    Log.d("AUTH", String.format("Access token: %s", accessToken));

    // Get Graph client and get user
    GraphHelper graphHelper = GraphHelper.getInstance();
    graphHelper.getUser(accessToken, getUserCallback());
}
```

Wenn Sie Ihre Änderungen speichern und die APP jetzt ausführen, wird die benutzerOBERFLÄCHE nach der Anmeldung mit dem Anzeigenamen und der e-Mail-Adresse des Benutzers aktualisiert.
