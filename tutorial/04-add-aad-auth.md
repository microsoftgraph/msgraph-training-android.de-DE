<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ad6a8-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="ad6a8-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph abzurufen.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="ad6a8-103">Dazu integrieren Sie die [Microsoft Authentication Library (MSAL) für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-103">To do this, you will integrate the [Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) into the application.</span></span>

1. <span data-ttu-id="ad6a8-104">Klicken Sie mit der rechten Maustaste auf den Ordner **"res",** und wählen **Sie "Neu"** und dann **"Android-Ressourcenverzeichnis" aus.**</span><span class="sxs-lookup"><span data-stu-id="ad6a8-104">Right-click the **res** folder and select **New**, then **Android Resource Directory**.</span></span>

1. <span data-ttu-id="ad6a8-105">Ändern Sie den **Ressourcentyp,** `raw` und wählen Sie OK **aus.**</span><span class="sxs-lookup"><span data-stu-id="ad6a8-105">Change the **Resource type** to `raw` and select **OK**.</span></span>

1. <span data-ttu-id="ad6a8-106">Klicken Sie mit der rechten Maustaste auf den neuen **unformatierte** Ordner, und wählen **Sie "Neu"** und dann **"Datei" aus.**</span><span class="sxs-lookup"><span data-stu-id="ad6a8-106">Right-click the new **raw** folder and select **New**, then **File**.</span></span>

1. <span data-ttu-id="ad6a8-107">Benennen Sie die `msal_config.json` Datei, und wählen Sie **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="ad6a8-107">Name the file `msal_config.json` and select **OK**.</span></span>

1. <span data-ttu-id="ad6a8-108">Fügen Sie der Datei **msal_config.jsA0 Folgendes** hinzu.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-108">Add the following to the **msal_config.json** file.</span></span>

    :::code language="json" source="../demo/GraphTutorial/msal_config.json.example":::

1. <span data-ttu-id="ad6a8-109">Ersetzen Sie die App-ID aus Ihrer App-Registrierung, und ersetzen Sie sie durch den `YOUR_APP_ID_HERE` `com.example.graphtutorial` Paketnamen Ihres Projekts.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-109">Replace `YOUR_APP_ID_HERE` with the app ID from your app registration, and replace `com.example.graphtutorial` with your project's package name.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ad6a8-110">Wenn Sie Quellcodeverwaltung wie Git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei aus der Quellcodeverwaltung auszuschließen, um zu verhindern, dass versehentlich Ihre App-ID durch lecks `msal_config.json` geht.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-110">If you're using source control such as git, now would be a good time to exclude the `msal_config.json` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="ad6a8-111">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="ad6a8-111">Implement sign-in</span></span>

<span data-ttu-id="ad6a8-112">In diesem Abschnitt aktualisieren Sie das Manifest, um MSAL die Verwendung eines Browsers zur Authentifizierung des Benutzers zu ermöglichen, den Umleitungs-URI als von der App behandelt zu registrieren, eine Authentifizierungshilfsklasse zu erstellen und die App so zu aktualisieren, dass sie sich anmeldet und abmeldet.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-112">In this section you will update the manifest to allow MSAL to use a browser to authenticate the user, register your redirect URI as being handled by the app, create an authentication helper class, and update the app to sign in and sign out.</span></span>

1. <span data-ttu-id="ad6a8-113">Erweitern Sie **den Ordner "app/manifests",** und öffnen Sie **AndroidManifest.xml**.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-113">Expand the **app/manifests** folder and open **AndroidManifest.xml**.</span></span> <span data-ttu-id="ad6a8-114">Fügen Sie die folgenden Elemente über dem Element `application` hinzu.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-114">Add the following elements above the `application` element.</span></span>

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

    > [!NOTE]
    > <span data-ttu-id="ad6a8-115">Diese Berechtigungen sind erforderlich, damit die MSAL-Bibliothek den Benutzer authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-115">These permissions are required in order for the MSAL library to authenticate the user.</span></span>

1. <span data-ttu-id="ad6a8-116">Fügen Sie das folgende Element innerhalb des Elements `application` hinzu, und ersetzen Sie die `YOUR_PACKAGE_NAME_HERE` Zeichenfolge durch den Paketnamen.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-116">Add the following element inside the `application` element, replacing the `YOUR_PACKAGE_NAME_HERE` string with your package name.</span></span>

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

1. <span data-ttu-id="ad6a8-117">Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.**</span><span class="sxs-lookup"><span data-stu-id="ad6a8-117">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="ad6a8-118">Ändern Der **Art in** **Schnittstelle**.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-118">Change the **Kind** to **Interface**.</span></span> <span data-ttu-id="ad6a8-119">Benennen Sie die `IAuthenticationHelperCreatedListener` Schnittstelle, und wählen Sie **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="ad6a8-119">Name the interface `IAuthenticationHelperCreatedListener` and select **OK**.</span></span>

1. <span data-ttu-id="ad6a8-120">Öffnen Sie die neue Datei, und ersetzen Sie ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-120">Open the new file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/IAuthenticationHelperCreatedListener.java" id="ListenerSnippet":::

1. <span data-ttu-id="ad6a8-121">Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.**</span><span class="sxs-lookup"><span data-stu-id="ad6a8-121">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="ad6a8-122">Benennen Sie die `AuthenticationHelper` Klasse, und wählen Sie **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="ad6a8-122">Name the class `AuthenticationHelper` and select **OK**.</span></span>

1. <span data-ttu-id="ad6a8-123">Öffnen Sie die neue Datei, und ersetzen Sie ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-123">Open the new file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/AuthenticationHelper.java" id="AuthHelperSnippet":::

1. <span data-ttu-id="ad6a8-124">Öffnen **Sie MainActivity,** und fügen Sie die folgenden Anweisungen `import` hinzu.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-124">Open **MainActivity** and add the following `import` statements.</span></span>

    ```java
    import android.util.Log;

    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalClientException;
    import com.microsoft.identity.client.exception.MsalException;
    import com.microsoft.identity.client.exception.MsalServiceException;
    import com.microsoft.identity.client.exception.MsalUiRequiredException;
    ```

1. <span data-ttu-id="ad6a8-125">Fügen Sie der Klasse die folgenden Membereigenschaften `MainActivity` hinzu.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-125">Add the following member properties to the `MainActivity` class.</span></span>

    ```java
    private AuthenticationHelper mAuthHelper = null;
    private boolean mAttemptInteractiveSignIn = false;
    ```

1. <span data-ttu-id="ad6a8-126">Fügen Sie am Ende der `onCreate`-Funktion den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-126">Add the following code to the end of the `onCreate` function.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="InitialLoginSnippet":::

1. <span data-ttu-id="ad6a8-127">Fügen Sie der Klasse die folgenden Funktionen `MainActivity` hinzu.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-127">Add the following functions to the `MainActivity` class.</span></span>

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

1. <span data-ttu-id="ad6a8-128">Ersetzen Sie vorhandene `signIn` und `signOut` Funktionen durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-128">Replace the existing `signIn` and `signOut` functions with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="SignInAndOutSnippet":::

    > [!NOTE]
    > <span data-ttu-id="ad6a8-129">Beachten Sie, `signIn` dass die Methode eine automatische Anmeldung (über) vor sich `doSilentSignIn` führt.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-129">Notice that the `signIn` method does a silent sign-in (via `doSilentSignIn`).</span></span> <span data-ttu-id="ad6a8-130">Beim Rückruf für diese Methode wird eine interaktive Anmeldung ausgeführt, wenn die automatische Anmeldung fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-130">The callback for this method will do an interactive sign-in if the silent one fails.</span></span> <span data-ttu-id="ad6a8-131">Dadurch wird vermieden, dass der Benutzer bei jedem Start der App eine Eingabeaufforderung einfordern muss.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-131">This avoids having to prompt the user every time they launch the app.</span></span>

1. <span data-ttu-id="ad6a8-132">Speichern Sie die Änderungen, und führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-132">Save your changes and run the app.</span></span>

1. <span data-ttu-id="ad6a8-133">Wenn Sie auf das **Menüelement "Anmelden"** tippen, wird ein Browser zur Azure AD-Anmeldeseite geöffnet.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-133">When you tap the **Sign in** menu item, a browser opens to the Azure AD login page.</span></span> <span data-ttu-id="ad6a8-134">Melden Sie sich mit Ihrem Konto an.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-134">Sign in with your account.</span></span>

<span data-ttu-id="ad6a8-135">Sobald die App fortgesetzt wird, sollte ein Zugriffstoken im Debugprotokoll in Android Studio angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-135">Once the app resumes, you should see an access token printed in the debug log in Android Studio.</span></span>

![Screenshot des Logcat-Fensters in Android Studio](./images/debugger-access-token.png)

## <a name="get-user-details"></a><span data-ttu-id="ad6a8-137">Benutzerdetails abrufen</span><span class="sxs-lookup"><span data-stu-id="ad6a8-137">Get user details</span></span>

<span data-ttu-id="ad6a8-138">In diesem Abschnitt erstellen Sie eine Hilfsklasse für alle Aufrufe von Microsoft Graph und aktualisieren die Klasse so, dass sie diese neue Klasse verwendet, um den angemeldeten `MainActivity` Benutzer zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-138">In this section you will create a helper class to hold all of the calls to Microsoft Graph and update the `MainActivity` class to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="ad6a8-139">Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.**</span><span class="sxs-lookup"><span data-stu-id="ad6a8-139">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="ad6a8-140">Benennen Sie die `GraphHelper` Klasse, und wählen Sie **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="ad6a8-140">Name the class `GraphHelper` and select **OK**.</span></span>

1. <span data-ttu-id="ad6a8-141">Öffnen Sie die neue Datei, und ersetzen Sie ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-141">Open the new file and replace its contents with the following.</span></span>

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
    > <span data-ttu-id="ad6a8-142">Überlegen Sie, was dieser Code macht.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-142">Consider what this code does.</span></span>
    >
    > - <span data-ttu-id="ad6a8-143">Es implementiert die Schnittstelle `IAuthenticationProvider` zum Einfügen des Zugriffstokens in `Authorization` den Header für ausgehende HTTP-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-143">It implements the `IAuthenticationProvider` interface to insert the access token in the `Authorization` header on outgoing HTTP requests.</span></span>
    > - <span data-ttu-id="ad6a8-144">Es macht eine Funktion verfügbar, um die Informationen des angemeldeten Benutzers `getUser` vom Graph-Endpunkt `/me` zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-144">It exposes a `getUser` function to get the logged-in user's information from the `/me` Graph endpoint.</span></span>

1. <span data-ttu-id="ad6a8-145">Fügen Sie die `import` folgenden Anweisungen am oberen Rand der Datei **"MainActivity"** hinzu.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-145">Add the following `import` statements to the top of the **MainActivity** file.</span></span>

    ```java
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="ad6a8-146">Fügen Sie der Klasse die folgende Funktion `MainActivity` hinzu, um eine für `ICallback` den Graph-Aufruf zu generieren.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-146">Add the following function to the `MainActivity` class to generate an `ICallback` for the Graph call.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="GetUserCallbackSnippet":::

1. <span data-ttu-id="ad6a8-147">Entfernen Sie die folgenden Zeilen, die den Benutzernamen und die E-Mail festlegen:</span><span class="sxs-lookup"><span data-stu-id="ad6a8-147">Remove the following lines that set the user name and email:</span></span>

    ```java
    // For testing
    mUserName = "Megan Bowen";
    mUserEmail = "meganb@contoso.com";
    ```

1. <span data-ttu-id="ad6a8-148">Ersetzen Sie `onSuccess` die Außerkraftsetzung im `AuthenticationCallback` Folgenden durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-148">Replace the `onSuccess` override in the `AuthenticationCallback` with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="OnSuccessSnippet":::

1. <span data-ttu-id="ad6a8-149">Speichern Sie die Änderungen, und führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-149">Save your changes and run the app.</span></span> <span data-ttu-id="ad6a8-150">Nach der Anmeldung wird die Benutzeroberfläche mit dem Anzeigenamen und der E-Mail-Adresse des Benutzers aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-150">After sign-in the UI is updated with the user's display name and email address.</span></span>
