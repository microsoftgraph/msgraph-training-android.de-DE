<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="78596-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="78596-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="78596-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="78596-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="78596-103">Hierzu integrieren Sie die [Microsoft-Authentifizierungsbibliothek (MSAL) für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="78596-103">To do this, you will integrate the [Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) into the application.</span></span>

1. <span data-ttu-id="78596-104">Klicken Sie mit der rechten Maustaste auf den Ordner **Res** , und wählen Sie **neu**und dann **Android-Ressourcenverzeichnis**aus.</span><span class="sxs-lookup"><span data-stu-id="78596-104">Right-click the **res** folder and select **New**, then **Android Resource Directory**.</span></span>

1. <span data-ttu-id="78596-105">Ändern Sie den **Ressourcentyp** in, `raw` und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="78596-105">Change the **Resource type** to `raw` and select **OK**.</span></span>

1. <span data-ttu-id="78596-106">Klicken Sie mit der rechten Maustaste auf den neuen **RAW** -Ordner, und wählen Sie **neu**und dann **Datei**aus.</span><span class="sxs-lookup"><span data-stu-id="78596-106">Right-click the new **raw** folder and select **New**, then **File**.</span></span>

1. <span data-ttu-id="78596-107">Nennen Sie die `msal_config.json` Datei, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="78596-107">Name the file `msal_config.json` and select **OK**.</span></span>

1. <span data-ttu-id="78596-108">Fügen Sie der Datei **msal_config. JSON** Folgendes hinzu.</span><span class="sxs-lookup"><span data-stu-id="78596-108">Add the following to the **msal_config.json** file.</span></span>

    ```json
    {
      "client_id" : "YOUR_APP_ID_HERE",
      "redirect_uri" : "msauth://YOUR_PACKAGE_NAME_HERE/callback",
      "broker_redirect_uri_registered": false,
      "account_mode": "SINGLE",
      "authorities" : [
        {
          "type": "AAD",
          "audience": {
            "type": "AzureADandPersonalMicrosoftAccount"
          },
          "default": true
        }
      ]
    }
    ```

    <span data-ttu-id="78596-109">Ersetzen `YOUR_APP_ID_HERE` Sie durch die APP-ID aus Ihrer APP-Registrierung `YOUR_PACKAGE_NAME_HERE` , und ersetzen Sie durch den Paketnamen Ihres Projekts.</span><span class="sxs-lookup"><span data-stu-id="78596-109">Replace `YOUR_APP_ID_HERE` with the app ID from your app registration, and replace `YOUR_PACKAGE_NAME_HERE` with your project's package name.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="78596-110">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei `msal_config.json` aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="78596-110">If you're using source control such as git, now would be a good time to exclude the `msal_config.json` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="78596-111">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="78596-111">Implement sign-in</span></span>

<span data-ttu-id="78596-112">In diesem Abschnitt aktualisieren Sie das Manifest so, dass MSAL einen Browser zur Authentifizierung des Benutzers verwenden, den Umleitungs-URI so registrieren kann, dass er von der APP verarbeitet wird, eine Authentifizierungs Hilfsklasse erstellt und die APP zur Anmeldung und Abmeldung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="78596-112">In this section you will update the manifest to allow MSAL to use a browser to authenticate the user, register your redirect URI as being handled by the app, create an authentication helper class, and update the app to sign in and sign out.</span></span>

1. <span data-ttu-id="78596-113">Erweitern Sie den Ordner **App/Manifests** , und öffnen Sie **AndroidManifest. XML**.</span><span class="sxs-lookup"><span data-stu-id="78596-113">Expand the **app/manifests** folder and open **AndroidManifest.xml**.</span></span> <span data-ttu-id="78596-114">Fügen Sie die folgenden Elemente oberhalb `application` des-Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="78596-114">Add the following elements above the `application` element.</span></span>

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

    > [!NOTE]
    > <span data-ttu-id="78596-115">Diese Berechtigungen sind erforderlich, damit die MSAL-Bibliothek den Benutzer authentifizieren muss.</span><span class="sxs-lookup"><span data-stu-id="78596-115">These permissions are required in order for the MSAL library to authenticate the user.</span></span>

1. <span data-ttu-id="78596-116">Fügen Sie das folgende Element innerhalb `application` des-Elements hinzu `YOUR_PACKAGE_NAME_HERE` , und ersetzen Sie die Zeichenfolge durch ihren Paketnamen.</span><span class="sxs-lookup"><span data-stu-id="78596-116">Add the following element inside the `application` element, replacing the `YOUR_PACKAGE_NAME_HERE` string with your package name.</span></span>

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

1. <span data-ttu-id="78596-117">Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie dann **neu**und dann **Java-Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="78596-117">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="78596-118">Nennen Sie die `AuthenticationHelper` Klasse, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="78596-118">Name the class `AuthenticationHelper` and select **OK**.</span></span>

1. <span data-ttu-id="78596-119">Öffnen Sie die neue Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="78596-119">Open the new file and replace its contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.app.Activity;
    import android.content.Context;
    import android.util.Log;
    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IPublicClientApplication;
    import com.microsoft.identity.client.ISingleAccountPublicClientApplication;
    import com.microsoft.identity.client.PublicClientApplication;
    import com.microsoft.identity.client.exception.MsalException;

    // Singleton class - the app only needs a single instance
    // of PublicClientApplication
    public class AuthenticationHelper {
        private static AuthenticationHelper INSTANCE = null;
        private ISingleAccountPublicClientApplication mPCA = null;
        private String[] mScopes = { "User.Read", "Calendars.Read" };

        private AuthenticationHelper(Context ctx) {
            PublicClientApplication.createSingleAccountPublicClientApplication(ctx, R.raw.msal_config,
                new IPublicClientApplication.ISingleAccountApplicationCreatedListener() {
                    @Override
                    public void onCreated(ISingleAccountPublicClientApplication application) {
                        mPCA = application;
                    }

                    @Override
                    public void onError(MsalException exception) {
                        Log.e("AUTHHELPER", "Error creating MSAL application", exception);
                    }
                });
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

        public void acquireTokenInteractively(Activity activity, AuthenticationCallback callback) {
            mPCA.signIn(activity, null, mScopes, callback);
        }

        public void acquireTokenSilently(AuthenticationCallback callback) {
            // Get the authority from MSAL config
            String authority = mPCA.getConfiguration().getDefaultAuthority().getAuthorityURL().toString();
            mPCA.acquireTokenSilentAsync(mScopes, authority, callback);
        }

        public void signOut() {
            mPCA.signOut(new ISingleAccountPublicClientApplication.SignOutCallback() {
                @Override
                public void onSignOut() {
                    Log.d("AUTHHELPER", "Signed out");
                }

                @Override
                public void onError(@NonNull MsalException exception) {
                    Log.d("AUTHHELPER", "MSAL error signing out", exception);
                }
            });
        }
    }
    ```

1. <span data-ttu-id="78596-120">Öffnen Sie **mainrate** , und fügen `import` Sie die folgenden Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="78596-120">Open **MainActivity** and add the following `import` statements.</span></span>

    ```java
    import android.util.Log;

    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalClientException;
    import com.microsoft.identity.client.exception.MsalException;
    import com.microsoft.identity.client.exception.MsalServiceException;
    import com.microsoft.identity.client.exception.MsalUiRequiredException;
    ```

1. <span data-ttu-id="78596-121">Fügen Sie der `MainActivity` -Klasse die folgende Elementeigenschaft hinzu.</span><span class="sxs-lookup"><span data-stu-id="78596-121">Add the following member property to the `MainActivity` class.</span></span>

    ```java
    private AuthenticationHelper mAuthHelper = null;
    ```

1. <span data-ttu-id="78596-122">Fügen Sie am Ende der `onCreate` Funktion Folgendes hinzu.</span><span class="sxs-lookup"><span data-stu-id="78596-122">Add the following to the end of the `onCreate` function.</span></span>

    ```java
    // Get the authentication helper
    mAuthHelper = AuthenticationHelper.getInstance(getApplicationContext());
    ```

1. <span data-ttu-id="78596-123">Fügen Sie der `MainActivity` -Klasse die folgenden Funktionen hinzu.</span><span class="sxs-lookup"><span data-stu-id="78596-123">Add the following functions to the `MainActivity` class.</span></span>

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
                    doInteractiveSignIn();

                } else if (exception instanceof MsalClientException) {
                    if (exception.getErrorCode() == "no_current_account") {
                        Log.d("AUTH", "No current account, interactive login required");
                        doInteractiveSignIn();
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

1. <span data-ttu-id="78596-124">Ersetzen Sie die `signIn` vorhandenen `signOut` und-Funktionen durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="78596-124">Replace the existing `signIn` and `signOut` functions with the following.</span></span>

    ```java
    private void signIn() {
        showProgressBar();
        // Attempt silent sign in first
        // if this fails, the callback will handle doing
        // interactive sign in
        doSilentSignIn();
    }

    private void signOut() {
        mAuthHelper.signOut();

        setSignedInState(false);
        openHomeFragment(mUserName);
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="78596-125">Beachten Sie, `signIn` dass die Methode zuerst überprüft, ob sich bereits ein Benutzerkonto im MSAL-Cache befindet.</span><span class="sxs-lookup"><span data-stu-id="78596-125">Notice that the `signIn` method first checks if there is a user account already in the MSAL cache.</span></span> <span data-ttu-id="78596-126">Wenn dies der Fall ist, wird versucht, die Token im Hintergrund zu aktualisieren, wodurch vermieden werden muss, dass der Benutzer bei jedem Start der APP auffordern kann.</span><span class="sxs-lookup"><span data-stu-id="78596-126">If there is, it attempts to refresh its tokens silently, avoiding having to prompt the user every time they launch the app.</span></span>

1. <span data-ttu-id="78596-127">Speichern Sie die Änderungen, und führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="78596-127">Save your changes and run the app.</span></span>

1. <span data-ttu-id="78596-128">Wenn Sie auf das Menüelement " **Anmelden** " tippen, wird ein Browser mit der Azure AD Anmeldeseite geöffnet.</span><span class="sxs-lookup"><span data-stu-id="78596-128">When you tap the **Sign in** menu item, a browser opens to the Azure AD login page.</span></span> <span data-ttu-id="78596-129">Melden Sie sich mit Ihrem Konto an.</span><span class="sxs-lookup"><span data-stu-id="78596-129">Sign in with your account.</span></span>

<span data-ttu-id="78596-130">Sobald die APP wieder aufgenommen wurde, sollte ein Zugriffstoken im Debug-Protokoll in Android Studio angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="78596-130">Once the app resumes, you should see an access token printed in the debug log in Android Studio.</span></span>

![Screenshot des Logcat-Fensters in Android Studio](./images/debugger-access-token.png)

## <a name="get-user-details"></a><span data-ttu-id="78596-132">Abrufen von Benutzer Details</span><span class="sxs-lookup"><span data-stu-id="78596-132">Get user details</span></span>

<span data-ttu-id="78596-133">In diesem Abschnitt erstellen Sie eine Hilfsklasse für alle Aufrufe von Microsoft Graph und aktualisieren die `MainActivity` Klasse so, dass die neue Klasse verwendet wird, um den angemeldeten Benutzer abzurufen.</span><span class="sxs-lookup"><span data-stu-id="78596-133">In this section you will create a helper class to hold all of the calls to Microsoft Graph and update the `MainActivity` class to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="78596-134">Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie dann **neu**und dann **Java-Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="78596-134">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="78596-135">Nennen Sie die `GraphHelper` Klasse, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="78596-135">Name the class `GraphHelper` and select **OK**.</span></span>

1. <span data-ttu-id="78596-136">Öffnen Sie die neue Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="78596-136">Open the new file and replace its contents with the following.</span></span>

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

    > [!NOTE]
    > <span data-ttu-id="78596-137">Überprüfen Sie die Funktionsweise dieses Codes.</span><span class="sxs-lookup"><span data-stu-id="78596-137">Consider what this code does.</span></span>
    >
    > - <span data-ttu-id="78596-138">Sie implementiert die `IAuthenticationProvider` Schnittstelle, um das Zugriffstoken in `Authorization` den Header in ausgehenden HTTP-Anforderungen einzufügen.</span><span class="sxs-lookup"><span data-stu-id="78596-138">It implements the `IAuthenticationProvider` interface to insert the access token in the `Authorization` header on outgoing HTTP requests.</span></span>
    > - <span data-ttu-id="78596-139">Es macht eine `getUser` Funktion verfügbar, um die Informationen des angemeldeten Benutzers vom `/me` Graph-Endpunkt abzurufen.</span><span class="sxs-lookup"><span data-stu-id="78596-139">It exposes a `getUser` function to get the logged-in user's information from the `/me` Graph endpoint.</span></span>

1. <span data-ttu-id="78596-140">Fügen Sie die `import` folgenden Anweisungen am Anfang der **Main** -Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="78596-140">Add the following `import` statements to the top of the **MainActivity** file.</span></span>

    ```java
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="78596-141">Fügen Sie der `MainActivity` Klasse die folgende Funktion hinzu, um `ICallback` einen für den Graph-Aufruf zu generieren.</span><span class="sxs-lookup"><span data-stu-id="78596-141">Add the following function to the `MainActivity` class to generate an `ICallback` for the Graph call.</span></span>

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

1. <span data-ttu-id="78596-142">Entfernen Sie die folgenden Zeilen, die den Benutzernamen und die e-Mail-Adresse festlegen:</span><span class="sxs-lookup"><span data-stu-id="78596-142">Remove the following lines that set the user name and email:</span></span>

    ```java
    // For testing
    mUserName = "Megan Bowen";
    mUserEmail = "meganb@contoso.com";
    ```

1. <span data-ttu-id="78596-143">Ersetzen Sie `onSuccess` die außer Kraft `AuthenticationCallback` Setzung in der durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="78596-143">Replace the `onSuccess` override in the `AuthenticationCallback` with the following.</span></span>

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

<span data-ttu-id="78596-144">Wenn Sie Ihre Änderungen speichern und die APP jetzt ausführen, wird die Benutzeroberfläche nach der Anmeldung mit dem Anzeigenamen und der e-Mail-Adresse des Benutzers aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="78596-144">If you save your changes and run the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
