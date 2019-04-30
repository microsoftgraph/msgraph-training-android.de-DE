<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="881ba-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung zur Unterstützung der Authentifizierung mit Azure AD.</span><span class="sxs-lookup"><span data-stu-id="881ba-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="881ba-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken für den Aufruf von Microsoft Graph abzurufen.</span><span class="sxs-lookup"><span data-stu-id="881ba-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="881ba-103">In diesem Schritt integrieren Sie die [Microsoft-Authentifizierungsbibliothek (MSAL) für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="881ba-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) into the application.</span></span>

<span data-ttu-id="881ba-104">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Values** , und wählen Sie **neu**und dann **Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="881ba-104">Right-click the **app/res/values** folder and choose **New**, then **Values resource file**.</span></span> <span data-ttu-id="881ba-105">Benennen Sie die `oauth_strings` Datei, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="881ba-105">Name the file `oauth_strings` and choose **OK**.</span></span> <span data-ttu-id="881ba-106">Fügen Sie dem- `resources` Element die folgenden Werte hinzu.</span><span class="sxs-lookup"><span data-stu-id="881ba-106">Add the following values to the `resources` element.</span></span>

```xml
<string name="oauth_app_id">YOUR_APP_ID_HERE</string>
<string name="oauth_redirect_uri">msalYOUR_APP_ID_HERE</string>
<string-array name="oauth_scopes">
    <item>User.Read</item>
    <item>Calendars.Read</item>
</string-array>
```

> [!IMPORTANT]
> <span data-ttu-id="881ba-107">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei `oauth_strings.xml` aus der Quellcodeverwaltung auszuschließen, um versehentlich Ihre APP-ID zu vernichten.</span><span class="sxs-lookup"><span data-stu-id="881ba-107">If you're using source control such as git, now would be a good time to exclude the `oauth_strings.xml` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="881ba-108">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="881ba-108">Implement sign-in</span></span>

<span data-ttu-id="881ba-109">Erweitern Sie den Ordner **App/Manifests** , und öffnen Sie **AndroidManifest. XML**.</span><span class="sxs-lookup"><span data-stu-id="881ba-109">Expand the **app/manifests** folder and open **AndroidManifest.xml**.</span></span> <span data-ttu-id="881ba-110">Fügen Sie die folgenden Elemente oberhalb `application` des-Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="881ba-110">Add the following elements above the `application` element.</span></span>

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

<span data-ttu-id="881ba-111">Diese Berechtigungen sind erforderlich, damit die MSAL-Bibliothek den Benutzer authentifizieren muss.</span><span class="sxs-lookup"><span data-stu-id="881ba-111">These permissions are required in order for the MSAL library to authenticate the user.</span></span>

<span data-ttu-id="881ba-112">Fügen Sie nun das folgende Element innerhalb `application` des-Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="881ba-112">Now add the following element inside the `application` element.</span></span>

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

<span data-ttu-id="881ba-113">Dies ermöglicht es MSAL, einen Browser zur Authentifizierung des Benutzers zu verwenden und den Umleitungs-URI als von der APP behandelt zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="881ba-113">This allows MSAL to use a browser to authenticate the user, and registers your redirect URI as being handled by the app.</span></span>

### <a name="implement-an-authentication-helper"></a><span data-ttu-id="881ba-114">Implementieren einer Authentifizierungs Hilfe</span><span class="sxs-lookup"><span data-stu-id="881ba-114">Implement an authentication helper</span></span>

<span data-ttu-id="881ba-115">Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie **neu**und dann **Java Class**aus.</span><span class="sxs-lookup"><span data-stu-id="881ba-115">Right-click the **app/java/com.example.graphtutorial** folder and choose **New**, then **Java Class**.</span></span> <span data-ttu-id="881ba-116">Benennen Sie die `AuthenticationHelper` Klasse, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="881ba-116">Name the class `AuthenticationHelper` and choose **OK**.</span></span> <span data-ttu-id="881ba-117">Öffnen Sie die neue Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="881ba-117">Open the new file and replace its contents with the following.</span></span>

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

<span data-ttu-id="881ba-118">Aktualisieren `MainActivity` Sie nun, um diese neue Klasse zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="881ba-118">Now update `MainActivity` to use this new class.</span></span> <span data-ttu-id="881ba-119">Öffnen Sie **Main** -on, und `import` fügen Sie die folgenden Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="881ba-119">Open **MainActivity** and add the following `import` statements.</span></span>

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

<span data-ttu-id="881ba-120">Als Nächstes fügen Sie der `MainActivity` Klasse die folgende Elementeigenschaft hinzu.</span><span class="sxs-lookup"><span data-stu-id="881ba-120">Next, add the following member property to the `MainActivity` class.</span></span>

```java
private AuthenticationHelper mAuthHelper = null;
```

<span data-ttu-id="881ba-121">Update `onCreate` auf fest `mAuthHelper`gelegt.</span><span class="sxs-lookup"><span data-stu-id="881ba-121">Update `onCreate` to set `mAuthHelper`.</span></span> <span data-ttu-id="881ba-122">Fügen Sie Folgendes am Ende der `onCreate` Funktion hinzu.</span><span class="sxs-lookup"><span data-stu-id="881ba-122">Add the following to the end of the `onCreate` function.</span></span>

```java
// Get the authentication helper
mAuthHelper = AuthenticationHelper.getInstance(getApplicationContext());
```

<span data-ttu-id="881ba-123">Fügen Sie eine außer `onActivityResult` Kraft setzung für die Behandlung von Authentifizierungs Antworten hinzu.</span><span class="sxs-lookup"><span data-stu-id="881ba-123">Add an override for `onActivityResult` to handle authentication responses.</span></span>

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    mAuthHelper.handleRedirect(requestCode, resultCode, data);
}
```

<span data-ttu-id="881ba-124">Fügen Sie nun die folgenden Funktionen zur `MainActivity` Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="881ba-124">Now, add the following functions to the `MainActivity` class.</span></span>

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

<span data-ttu-id="881ba-125">Ersetzen Sie schließlich die vorhandenen `signIn` und `signOut` -Funktionen durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="881ba-125">Finally, replace the existing `signIn` and `signOut` functions with the following.</span></span>

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

<span data-ttu-id="881ba-126">Beachten Sie, `signIn` dass die Methode zuerst überprüft, ob ein Benutzerkonto im MSAL-Cache vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="881ba-126">Notice that the `signIn` method first checks if there is a user account already in the MSAL cache.</span></span> <span data-ttu-id="881ba-127">Ist dies der Fall, wird versucht, die Token im Hintergrund zu aktualisieren, ohne dass der Benutzer jedes Mal aufgefordert werden muss, die APP zu starten.</span><span class="sxs-lookup"><span data-stu-id="881ba-127">If there is, it attempts to refresh its tokens silently, avoiding having to prompt the user every time they launch the app.</span></span>

<span data-ttu-id="881ba-128">Speichern Sie die Änderungen, und führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="881ba-128">Save your changes and run the app.</span></span> <span data-ttu-id="881ba-129">Wenn Sie auf das Menüelement **Anmelden** tippen, wird ein Browser zur Azure AD-Anmeldeseite geöffnet.</span><span class="sxs-lookup"><span data-stu-id="881ba-129">When you tap the **Sign in** menu item, a browser opens to the Azure AD login page.</span></span> <span data-ttu-id="881ba-130">Melden Sie sich mit Ihrem Konto an.</span><span class="sxs-lookup"><span data-stu-id="881ba-130">Sign in with your account.</span></span> <span data-ttu-id="881ba-131">Sobald die APP wieder aufgenommen wird, sollte ein Zugriffstoken im Debug-Protokoll in Android Studio gedruckt werden.</span><span class="sxs-lookup"><span data-stu-id="881ba-131">Once the app resumes, you should see an access token printed in the debug log in Android Studio.</span></span>

![Screenshot des Logcat-Fensters in Android Studio](./images/debugger-access-token.png)

## <a name="get-user-details"></a><span data-ttu-id="881ba-133">Benutzer Details abrufen</span><span class="sxs-lookup"><span data-stu-id="881ba-133">Get user details</span></span>

<span data-ttu-id="881ba-134">Erstellen Sie zunächst eine Hilfsklasse für alle Aufrufe von Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="881ba-134">Start by creating a helper class to hold all of the calls to Microsoft Graph.</span></span> <span data-ttu-id="881ba-135">Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie **neu**und dann **Java Class**aus.</span><span class="sxs-lookup"><span data-stu-id="881ba-135">Right-click the **app/java/com.example.graphtutorial** folder and choose **New**, then **Java Class**.</span></span> <span data-ttu-id="881ba-136">Benennen Sie die `GraphHelper` Klasse, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="881ba-136">Name the class `GraphHelper` and choose **OK**.</span></span> <span data-ttu-id="881ba-137">Öffnen Sie die neue Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="881ba-137">Open the new file and replace its contents with the following.</span></span>

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

<span data-ttu-id="881ba-138">Beachten Sie diesen Code.</span><span class="sxs-lookup"><span data-stu-id="881ba-138">Note what this code does.</span></span>

- <span data-ttu-id="881ba-139">Die `IAuthenticationProvider` Schnittstelle wird implementiert, um das Zugriffstoken in `Authorization` den Header für ausgehende HTTP-Anforderungen einzufügen.</span><span class="sxs-lookup"><span data-stu-id="881ba-139">It implements the `IAuthenticationProvider` interface to insert the access token in the `Authorization` header on outgoing HTTP requests.</span></span>
- <span data-ttu-id="881ba-140">Sie stellt eine `getUser` Funktion zum Abrufen der Informationen des angemeldeten Benutzers vom `/me` Graph-Endpunkt bereit.</span><span class="sxs-lookup"><span data-stu-id="881ba-140">It exposes a `getUser` function to get the logged-in user's information from the `/me` Graph endpoint.</span></span>

<span data-ttu-id="881ba-141">Aktualisieren Sie nun `MainActivity` die Klasse, um diese neue Klasse zum Abrufen des angemeldeten Benutzers zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="881ba-141">Now update the `MainActivity` class to use this new class to get the logged-in user.</span></span> <span data-ttu-id="881ba-142">Fügen Sie am `import` Anfang der **Haupt** Datei die folgenden Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="881ba-142">Add the following `import` statements to the top of the **MainActivity** file.</span></span>

```java
import com.microsoft.graph.concurrency.ICallback;
import com.microsoft.graph.core.ClientException;
import com.microsoft.graph.models.extensions.IGraphServiceClient;
import com.microsoft.graph.models.extensions.User;
```

<span data-ttu-id="881ba-143">Als Nächstes fügen Sie der `MainActivity` Klasse die folgende Funktion hinzu, um `ICallback` einen für den Graph-Aufruf zu generieren.</span><span class="sxs-lookup"><span data-stu-id="881ba-143">Next, add the following function to the `MainActivity` class to generate an `ICallback` for the Graph call.</span></span>

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

<span data-ttu-id="881ba-144">Entfernen Sie die folgenden Zeilen, die den Benutzernamen und die e-Mail-Adresse festlegen:</span><span class="sxs-lookup"><span data-stu-id="881ba-144">Remove the following lines that set the user name and email:</span></span>

```java
// For testing
mUserName = "Megan Bowen";
mUserEmail = "meganb@contoso.com";
```

<span data-ttu-id="881ba-145">Ersetzen Sie schließlich die `onSuccess` Außerkraftsetzung `AuthenticationCallback` im folgenden.</span><span class="sxs-lookup"><span data-stu-id="881ba-145">Finally, replace the `onSuccess` override in the `AuthenticationCallback` with the following.</span></span>

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

<span data-ttu-id="881ba-146">Wenn Sie Ihre Änderungen speichern und die APP jetzt ausführen, wird die benutzerOBERFLÄCHE nach der Anmeldung mit dem Anzeigenamen und der e-Mail-Adresse des Benutzers aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="881ba-146">If you save your changes and run the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
