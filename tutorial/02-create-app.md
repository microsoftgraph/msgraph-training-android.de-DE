<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="59a1c-101">Erstellen Sie zunächst ein neues Android Studio-Projekt.</span><span class="sxs-lookup"><span data-stu-id="59a1c-101">Begin by creating a new Android Studio project.</span></span>

1. <span data-ttu-id="59a1c-102">Öffnen Sie Android Studio, und wählen **Sie auf der** Willkommensbildschirm die Option "Neues Android Studio-Projekt starten" aus.</span><span class="sxs-lookup"><span data-stu-id="59a1c-102">Open Android Studio, and select **Start a new Android Studio project** on the welcome screen.</span></span>

1. <span data-ttu-id="59a1c-103">Wählen Sie **im Dialogfeld "Neues Projekt** erstellen" die Option **"Leere Aktivität"** und dann **"Weiter" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-103">In the **Create New Project** dialog, select **Empty Activity**, then select **Next**.</span></span>

    ![Screenshot des Dialogfelds "Neues Projekt erstellen" in Android Studio](./images/choose-project.png)

1. <span data-ttu-id="59a1c-105">Legen Sie **im Dialogfeld** Projekt  konfigurieren den Namen auf , stellen Sie sicher, dass das Feld Sprache auf festgelegt ist, und stellen Sie sicher, dass die minimale `Graph Tutorial`  `Java` **API-Ebene** auf festgelegt `API 29: Android 10.0 (Q)` ist.</span><span class="sxs-lookup"><span data-stu-id="59a1c-105">In the **Configure your project** dialog, set the **Name** to `Graph Tutorial`, ensure the **Language** field is set to `Java`, and ensure the **Minimum API level** is set to `API 29: Android 10.0 (Q)`.</span></span> <span data-ttu-id="59a1c-106">Ändern Sie **den Paketnamen** und **den Speicherort** nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="59a1c-106">Modify the **Package name** and **Save location** as needed.</span></span> <span data-ttu-id="59a1c-107">Wählen Sie **Fertig stellen** aus.</span><span class="sxs-lookup"><span data-stu-id="59a1c-107">Select **Finish**.</span></span>

    ![Screenshot des Dialogfelds "Projekt konfigurieren"](./images/configure-project.png)

> [!IMPORTANT]
> <span data-ttu-id="59a1c-109">Der Code und die Anweisungen in diesem Lernprogramm verwenden den Paketnamen **"com.example.graphtutorial".**</span><span class="sxs-lookup"><span data-stu-id="59a1c-109">The code and instructions in this tutorial use the package name **com.example.graphtutorial**.</span></span> <span data-ttu-id="59a1c-110">Wenn Sie beim Erstellen des Projekts einen anderen Paketnamen verwenden, müssen Sie den Paketnamen überall dort verwenden, wo dieser Wert angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="59a1c-110">If you use a different package name when creating the project, be sure to use your package name wherever you see this value.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="59a1c-111">Installieren von Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="59a1c-111">Install dependencies</span></span>

<span data-ttu-id="59a1c-112">Installieren Sie vor dem Wechsel einige zusätzliche Abhängigkeiten, die Sie später verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="59a1c-112">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="59a1c-113">`com.google.android.material:material` , um die [Navigationsansicht](https://material.io/develop/android/components/navigation-view/) für die App verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="59a1c-113">`com.google.android.material:material` to make the [navigation view](https://material.io/develop/android/components/navigation-view/) available to the app.</span></span>
- <span data-ttu-id="59a1c-114">[Microsoft Authentication Library (MSAL) für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) zum Verarbeiten der Azure AD-Authentifizierung und Tokenverwaltung.</span><span class="sxs-lookup"><span data-stu-id="59a1c-114">[Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) to handle Azure AD authentication and token management.</span></span>
- <span data-ttu-id="59a1c-115">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) zum Aufrufen von Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="59a1c-115">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) for making calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="59a1c-116">Erweitern **Sie Gradle Scripts,** und öffnen Sie dann **"build.gradle" (Modul: Graph_Tutorial.app).**</span><span class="sxs-lookup"><span data-stu-id="59a1c-116">Expand **Gradle Scripts**, then open **build.gradle (Module: Graph_Tutorial.app)**.</span></span>

1. <span data-ttu-id="59a1c-117">Fügen Sie die folgenden Zeilen in den Wert `dependencies` ein.</span><span class="sxs-lookup"><span data-stu-id="59a1c-117">Add the following lines inside the `dependencies` value.</span></span>

    :::code language="gradle" source="../demo/GraphTutorial/app/build.gradle" id="DependenciesSnippet":::

1. <span data-ttu-id="59a1c-118">Fügen Sie `packagingOptions` einen Wert innerhalb des `android` Werts in **build.gradle (Module: Graph_Tutorial.app) hinzu.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-118">Add a `packagingOptions` value inside the `android` value in **build.gradle (Module: Graph_Tutorial.app)**.</span></span>

    ```Gradle
    packagingOptions {
        pickFirst 'META-INF/jersey-module-version'
    }
    ```

1. <span data-ttu-id="59a1c-119">Fügen Sie das Azure Maven-Repository für die MicrosoftDeviceSDK-Bibliothek, eine Abhängigkeit von MSAL, hinzu.</span><span class="sxs-lookup"><span data-stu-id="59a1c-119">Add the Azure Maven repository for the MicrosoftDeviceSDK library, a dependency of MSAL.</span></span> <span data-ttu-id="59a1c-120">Öffnen **Sie build.gradle (Project: Graph_Tutorial)**.</span><span class="sxs-lookup"><span data-stu-id="59a1c-120">Open **build.gradle (Project: Graph_Tutorial)**.</span></span> <span data-ttu-id="59a1c-121">Fügen Sie dem Wert innerhalb `repositories` des Werts Folgendes `allprojects` hinzu.</span><span class="sxs-lookup"><span data-stu-id="59a1c-121">Add the following to the `repositories` value inside the `allprojects` value.</span></span>

    ```Gradle
    maven {
        url 'https://pkgs.dev.azure.com/MicrosoftDeviceSDK/DuoSDK-Public/_packaging/Duo-SDK-Feed/maven/v1'
    }
    ```

1. <span data-ttu-id="59a1c-122">Speichern Sie Ihre Änderungen.</span><span class="sxs-lookup"><span data-stu-id="59a1c-122">Save your changes.</span></span> <span data-ttu-id="59a1c-123">Wählen Sie **im Menü "Datei"** die Option **"Projekt mit Gradledateien synchronisieren" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-123">On the **File** menu, select **Sync Project with Gradle Files**.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="59a1c-124">Entwerfen der App</span><span class="sxs-lookup"><span data-stu-id="59a1c-124">Design the app</span></span>

<span data-ttu-id="59a1c-125">Die Anwendung verwendet eine Navigationsschubschubte, um zwischen verschiedenen Ansichten zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="59a1c-125">The application will use a navigation drawer to navigate between different views.</span></span> <span data-ttu-id="59a1c-126">In diesem Schritt aktualisieren Sie die Aktivität so, dass ein Navigationsschubschub layout verwendet wird, und fügen Fragmente für die Ansichten hinzu.</span><span class="sxs-lookup"><span data-stu-id="59a1c-126">In this step you will update the activity to use a navigation drawer layout, and add fragments for the views.</span></span>

### <a name="create-a-navigation-drawer"></a><span data-ttu-id="59a1c-127">Erstellen einer Navigationsschubte</span><span class="sxs-lookup"><span data-stu-id="59a1c-127">Create a navigation drawer</span></span>

<span data-ttu-id="59a1c-128">In diesem Abschnitt erstellen Sie Symbole für das Navigationsmenü der App, erstellen ein Menü für die Anwendung und aktualisieren das Design und Layout der Anwendung so, dass sie mit einer Navigationsschubschubte kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="59a1c-128">In this section you will create icons for the app's navigation menu, create a menu for the application, and update the application's theme and layout to be compatible with a navigation drawer.</span></span>

#### <a name="create-icons"></a><span data-ttu-id="59a1c-129">Erstellen von Symbolen</span><span class="sxs-lookup"><span data-stu-id="59a1c-129">Create icons</span></span>

1. <span data-ttu-id="59a1c-130">Klicken Sie mit der rechten Maustaste auf den **Ordner "app/res/drawable",** und wählen **Sie "Neu"** und dann **"Vector Asset" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-130">Right-click the **app/res/drawable** folder and select **New**, then **Vector Asset**.</span></span>

1. <span data-ttu-id="59a1c-131">Klicken Sie auf die Symbolschaltfläche neben **ClipArt.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-131">Click the icon button next to **Clip Art**.</span></span>

1. <span data-ttu-id="59a1c-132">Geben Sie **im Fenster "Symbol** auswählen" die Suchleiste ein, wählen Sie dann das Startsymbol `home` aus, und wählen Sie OK **aus.** </span><span class="sxs-lookup"><span data-stu-id="59a1c-132">In the **Select Icon** window, type `home` in the search bar, then select the **Home** icon and select **OK**.</span></span>

1. <span data-ttu-id="59a1c-133">Ändern Sie **den Namen** in `ic_menu_home` .</span><span class="sxs-lookup"><span data-stu-id="59a1c-133">Change the **Name** to `ic_menu_home`.</span></span>

    ![Screenshot des Fensters "Vector Asset konfigurieren"](./images/create-icon.png)

1. <span data-ttu-id="59a1c-135">Wählen Sie **"Weiter"** und dann **"Fertig stellen" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-135">Select **Next**, then **Finish**.</span></span>

1. <span data-ttu-id="59a1c-136">Wiederholen Sie den vorherigen Schritt, um vier weitere Symbole zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="59a1c-136">Repeat the previous step to create four more icons.</span></span>

    - <span data-ttu-id="59a1c-137">Name: `ic_menu_calendar` , Symbol: `event`</span><span class="sxs-lookup"><span data-stu-id="59a1c-137">Name: `ic_menu_calendar`, Icon: `event`</span></span>
    - <span data-ttu-id="59a1c-138">Name: `ic_menu_add_event` , Symbol: `add box`</span><span class="sxs-lookup"><span data-stu-id="59a1c-138">Name: `ic_menu_add_event`, Icon: `add box`</span></span>
    - <span data-ttu-id="59a1c-139">Name: `ic_menu_signout` , Symbol: `exit to app`</span><span class="sxs-lookup"><span data-stu-id="59a1c-139">Name: `ic_menu_signout`, Icon: `exit to app`</span></span>
    - <span data-ttu-id="59a1c-140">Name: `ic_menu_signin` , Symbol: `person add`</span><span class="sxs-lookup"><span data-stu-id="59a1c-140">Name: `ic_menu_signin`, Icon: `person add`</span></span>

#### <a name="create-the-menu"></a><span data-ttu-id="59a1c-141">Erstellen des Menüs</span><span class="sxs-lookup"><span data-stu-id="59a1c-141">Create the menu</span></span>

1. <span data-ttu-id="59a1c-142">Klicken Sie mit der rechten Maustaste auf den Ordner **"res",** und wählen **Sie "Neu"** und dann **"Android-Ressourcenverzeichnis" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-142">Right-click the **res** folder and select **New**, then **Android Resource Directory**.</span></span>

1. <span data-ttu-id="59a1c-143">Ändern Sie den **Ressourcentyp,** `menu` und wählen Sie OK **aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-143">Change the **Resource type** to `menu` and select **OK**.</span></span>

1. <span data-ttu-id="59a1c-144">Klicken Sie mit der rechten Maustaste auf den neuen **Menüordner,** und wählen **Sie "Neu"** und dann **"Menüressourcendatei" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-144">Right-click the new **menu** folder and select **New**, then **Menu resource file**.</span></span>

1. <span data-ttu-id="59a1c-145">Benennen Sie die `drawer_menu` Datei, und wählen Sie **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-145">Name the file `drawer_menu` and select **OK**.</span></span>

1. <span data-ttu-id="59a1c-146">Wenn die Datei geöffnet wird, wählen Sie die Registerkarte **"Code"** aus, um den XML-Code anzuzeigen, und ersetzen Sie dann den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="59a1c-146">When the file opens, select the **Code** tab to view the XML, then replace the entire contents with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/menu/drawer_menu.xml":::

#### <a name="update-application-theme-and-layout"></a><span data-ttu-id="59a1c-147">Aktualisieren des Anwendungsdesigns und -layouts</span><span class="sxs-lookup"><span data-stu-id="59a1c-147">Update application theme and layout</span></span>

1. <span data-ttu-id="59a1c-148">Öffnen Sie **die Datei "app/res/values/styles.xml"** und ersetzen Sie `Theme.AppCompat.Light.DarkActionBar` sie durch `Theme.AppCompat.Light.NoActionBar` .</span><span class="sxs-lookup"><span data-stu-id="59a1c-148">Open the **app/res/values/styles.xml** file and replace `Theme.AppCompat.Light.DarkActionBar` with `Theme.AppCompat.Light.NoActionBar`.</span></span>

1. <span data-ttu-id="59a1c-149">Fügen Sie die folgenden Zeilen innerhalb des Elements `style` hinzu.</span><span class="sxs-lookup"><span data-stu-id="59a1c-149">Add the following lines inside the `style` element.</span></span>

    ```xml
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    ```

1. <span data-ttu-id="59a1c-150">Klicken Sie mit der rechten Maustaste **auf den Ordner "app/res/layout".**</span><span class="sxs-lookup"><span data-stu-id="59a1c-150">Right-click the **app/res/layout** folder.</span></span>

1. <span data-ttu-id="59a1c-151">Wählen **Sie "Neu"** und dann **"Layoutressourcendatei" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-151">Select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="59a1c-152">Benennen Sie die `nav_header` Datei, und ändern Sie das **Stammelement** in `LinearLayout` , und wählen Sie dann OK **aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-152">Name the file `nav_header` and change the **Root element** to `LinearLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="59a1c-153">Öffnen Sie **dienav_header.xml,** und wählen Sie die Registerkarte **"Text"** aus. Ersetzen Sie den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="59a1c-153">Open the **nav_header.xml** file and select the **Text** tab. Replace the entire contents with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/nav_header.xml":::

1. <span data-ttu-id="59a1c-154">Öffnen Sie **die Datei "app/res/layout/activity_main.xml",** und aktualisieren Sie das Layout auf a, indem Sie die vorhandene XML durch `DrawerLayout` Folgendes ersetzen.</span><span class="sxs-lookup"><span data-stu-id="59a1c-154">Open the **app/res/layout/activity_main.xml** file and update the layout to a `DrawerLayout` by replacing the existing XML with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/activity_main.xml":::

1. <span data-ttu-id="59a1c-155">Öffnen **Sie "app/res/values/strings.xml" und** fügen Sie die folgenden Elemente innerhalb des Elements `resources` hinzu.</span><span class="sxs-lookup"><span data-stu-id="59a1c-155">Open **app/res/values/strings.xml** and add the following elements inside the `resources` element.</span></span>

    ```xml
    <string name="navigation_drawer_open">Open navigation drawer</string>
    <string name="navigation_drawer_close">Close navigation drawer</string>
    ```

1. <span data-ttu-id="59a1c-156">Öffnen Sie **die Datei "app/java/com.example/graphtutorial/MainActivity",** und ersetzen Sie den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="59a1c-156">Open the **app/java/com.example/graphtutorial/MainActivity** file and replace the entire contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.os.Bundle;
    import android.view.Menu;
    import android.view.MenuItem;
    import android.view.View;
    import android.widget.FrameLayout;
    import android.widget.ProgressBar;
    import android.widget.TextView;
    import androidx.annotation.NonNull;
    import androidx.appcompat.app.ActionBarDrawerToggle;
    import androidx.appcompat.app.AppCompatActivity;
    import androidx.appcompat.widget.Toolbar;
    import androidx.core.view.GravityCompat;
    import androidx.drawerlayout.widget.DrawerLayout;
    import com.google.android.material.navigation.NavigationView;

    public class MainActivity extends AppCompatActivity implements NavigationView.OnNavigationItemSelectedListener {
        private static final String SAVED_IS_SIGNED_IN = "isSignedIn";
        private static final String SAVED_USER_NAME = "userName";
        private static final String SAVED_USER_EMAIL = "userEmail";
        private static final String SAVED_USER_TIMEZONE = "userTimeZone";

        private DrawerLayout mDrawer;
        private NavigationView mNavigationView;
        private View mHeaderView;
        private boolean mIsSignedIn = false;
        private String mUserName = null;
        private String mUserEmail = null;
        private String mUserTimeZone = null;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            // Set the toolbar
            Toolbar toolbar = findViewById(R.id.toolbar);
            setSupportActionBar(toolbar);

            mDrawer = findViewById(R.id.drawer_layout);

            // Add the hamburger menu icon
            ActionBarDrawerToggle toggle = new ActionBarDrawerToggle(this, mDrawer, toolbar,
                    R.string.navigation_drawer_open, R.string.navigation_drawer_close);
            mDrawer.addDrawerListener(toggle);
            toggle.syncState();

            mNavigationView = findViewById(R.id.nav_view);

            // Set user name and email
            mHeaderView = mNavigationView.getHeaderView(0);
            setSignedInState(mIsSignedIn);

            // Listen for item select events on menu
            mNavigationView.setNavigationItemSelectedListener(this);

            if (savedInstanceState == null) {
                // Load the home fragment by default on startup
                openHomeFragment(mUserName);
            } else {
                // Restore state
                mIsSignedIn = savedInstanceState.getBoolean(SAVED_IS_SIGNED_IN);
                mUserName = savedInstanceState.getString(SAVED_USER_NAME);
                mUserEmail = savedInstanceState.getString(SAVED_USER_EMAIL);
                mUserTimeZone = savedInstanceState.getString(SAVED_USER_TIMEZONE);
                setSignedInState(mIsSignedIn);
            }
        }

        @Override
        protected void onSaveInstanceState(@NonNull Bundle outState) {
            super.onSaveInstanceState(outState);
            outState.putBoolean(SAVED_IS_SIGNED_IN, mIsSignedIn);
            outState.putString(SAVED_USER_NAME, mUserName);
            outState.putString(SAVED_USER_EMAIL, mUserEmail);
            outState.putString(SAVED_USER_TIMEZONE, mUserTimeZone);
        }

        @Override
        public boolean onNavigationItemSelected(@NonNull MenuItem menuItem) {
            // TEMPORARY
            return false;
        }

        @Override
        public void onBackPressed() {
            if (mDrawer.isDrawerOpen(GravityCompat.START)) {
                mDrawer.closeDrawer(GravityCompat.START);
            } else {
                super.onBackPressed();
            }
        }

        public void showProgressBar()
        {
            FrameLayout container = findViewById(R.id.fragment_container);
            ProgressBar progressBar = findViewById(R.id.progressbar);
            container.setVisibility(View.GONE);
            progressBar.setVisibility(View.VISIBLE);
        }

        public void hideProgressBar()
        {
            FrameLayout container = findViewById(R.id.fragment_container);
            ProgressBar progressBar = findViewById(R.id.progressbar);
            progressBar.setVisibility(View.GONE);
            container.setVisibility(View.VISIBLE);
        }

        // Update the menu and get the user's name and email
        private void setSignedInState(boolean isSignedIn) {
            mIsSignedIn = isSignedIn;

            mNavigationView.getMenu().clear();
            mNavigationView.inflateMenu(R.menu.drawer_menu);

            Menu menu = mNavigationView.getMenu();

            // Hide/show the Sign in, Calendar, and Sign Out buttons
            if (isSignedIn) {
                menu.removeItem(R.id.nav_signin);
            } else {
                menu.removeItem(R.id.nav_home);
                menu.removeItem(R.id.nav_calendar);
                menu.removeItem(R.id.nav_create_event);
                menu.removeItem(R.id.nav_signout);
            }

            // Set the user name and email in the nav drawer
            TextView userName = mHeaderView.findViewById(R.id.user_name);
            TextView userEmail = mHeaderView.findViewById(R.id.user_email);

            if (isSignedIn) {
                // For testing
                mUserName = "Lynne Robbins";
                mUserEmail = "lynner@contoso.com";
                mUserTimeZone = "Pacific Standard Time";

                userName.setText(mUserName);
                userEmail.setText(mUserEmail);
            } else {
                mUserName = null;
                mUserEmail = null;
                mUserTimeZone = null;

                userName.setText("Please sign in");
                userEmail.setText("");
            }
        }
    }
    ```

### <a name="add-fragments"></a><span data-ttu-id="59a1c-157">Hinzufügen von Fragmenten</span><span class="sxs-lookup"><span data-stu-id="59a1c-157">Add fragments</span></span>

<span data-ttu-id="59a1c-158">In diesem Abschnitt erstellen Sie Fragmente für die Homepage- und Kalenderansichten.</span><span class="sxs-lookup"><span data-stu-id="59a1c-158">In this section you will create fragments for the home and calendar views.</span></span>

1. <span data-ttu-id="59a1c-159">Klicken Sie mit der rechten Maustaste auf den **Ordner "app/res/layout",** und wählen **Sie "Neu"** und dann **"Layoutressourcendatei" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-159">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="59a1c-160">Benennen Sie die `fragment_home` Datei, und ändern Sie das **Stammelement** in `RelativeLayout` , und wählen Sie dann OK **aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-160">Name the file `fragment_home` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="59a1c-161">Öffnen Sie **fragment_home.xml** Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="59a1c-161">Open the **fragment_home.xml** file and replace its contents with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_home.xml":::

1. <span data-ttu-id="59a1c-162">Klicken Sie mit der rechten Maustaste auf den **Ordner "app/res/layout",** und wählen **Sie "Neu"** und dann **"Layoutressourcendatei" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-162">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="59a1c-163">Benennen Sie die `fragment_calendar` Datei, und ändern Sie das **Stammelement** in `RelativeLayout` , und wählen Sie dann OK **aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-163">Name the file `fragment_calendar` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="59a1c-164">Öffnen Sie **fragment_calendar.xml** Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="59a1c-164">Open the **fragment_calendar.xml** file and replace its contents with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:text="Calendar"
            android:textSize="30sp" />

    </RelativeLayout>
    ```

1. <span data-ttu-id="59a1c-165">Klicken Sie mit der rechten Maustaste auf den **Ordner "app/res/layout",** und wählen **Sie "Neu"** und dann **"Layoutressourcendatei" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-165">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="59a1c-166">Benennen Sie die `fragment_new_event` Datei, und ändern Sie das **Stammelement** in `RelativeLayout` , und wählen Sie dann OK **aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-166">Name the file `fragment_new_event` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="59a1c-167">Öffnen Sie **fragment_new_event.xml** Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="59a1c-167">Open the **fragment_new_event.xml** file and replace its contents with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:text="New Event"
            android:textSize="30sp" />

    </RelativeLayout>
    ```

1. <span data-ttu-id="59a1c-168">Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** aus, und klicken Sie **dann Java Klasse**.</span><span class="sxs-lookup"><span data-stu-id="59a1c-168">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="59a1c-169">Benennen Sie die `HomeFragment` Klasse, und wählen Sie dann **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-169">Name the class `HomeFragment`, then select **OK**.</span></span>

1. <span data-ttu-id="59a1c-170">Öffnen Sie **die Datei HomeFragment,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="59a1c-170">Open the **HomeFragment** file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/HomeFragment.java" id="HomeSnippet":::

1. <span data-ttu-id="59a1c-171">Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-171">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="59a1c-172">Benennen Sie die `CalendarFragment` Klasse, und wählen Sie dann **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-172">Name the class `CalendarFragment`, then select **OK**.</span></span>

1. <span data-ttu-id="59a1c-173">Öffnen Sie **die CalendarFragment-Datei,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="59a1c-173">Open the **CalendarFragment** file and replace its contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.os.Bundle;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import androidx.annotation.NonNull;
    import androidx.annotation.Nullable;
    import androidx.fragment.app.Fragment;

    public class CalendarFragment extends Fragment {
        private static final String TIME_ZONE = "timeZone";

        private String mTimeZone;

        public CalendarFragment() {}

        public static CalendarFragment createInstance(String timeZone) {
            CalendarFragment fragment = new CalendarFragment();

            // Add the provided time zone to the fragment's arguments
            Bundle args = new Bundle();
            args.putString(TIME_ZONE, timeZone);
            fragment.setArguments(args);
            return fragment;
        }

        @Override
        public void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            if (getArguments() != null) {
                mTimeZone = getArguments().getString(TIME_ZONE);
            }
        }

        @Nullable
        @Override
        public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
            return inflater.inflate(R.layout.fragment_calendar, container, false);
        }
    }
    ```

1. <span data-ttu-id="59a1c-174">Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-174">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="59a1c-175">Benennen Sie die `NewEventFragment` Klasse, und wählen Sie dann **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-175">Name the class `NewEventFragment`, then select **OK**.</span></span>

1. <span data-ttu-id="59a1c-176">Öffnen Sie **die Datei NewEventFragment,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="59a1c-176">Open the **NewEventFragment** file and replace its contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.os.Bundle;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import androidx.annotation.NonNull;
    import androidx.annotation.Nullable;
    import androidx.fragment.app.Fragment;

    public class NewEventFragment extends Fragment {
        private static final String TIME_ZONE = "timeZone";

        private String mTimeZone;

        public NewEventFragment() {}

        public static NewEventFragment createInstance(String timeZone) {
            NewEventFragment fragment = new NewEventFragment();

            // Add the provided time zone to the fragment's arguments
            Bundle args = new Bundle();
            args.putString(TIME_ZONE, timeZone);
            fragment.setArguments(args);
            return fragment;
        }

        @Override
        public void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            if (getArguments() != null) {
                mTimeZone = getArguments().getString(TIME_ZONE);
            }
        }

        @Nullable
        @Override
        public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
            return inflater.inflate(R.layout.fragment_new_event, container, false);
        }
    }
    ```

1. <span data-ttu-id="59a1c-177">Öffnen Sie **die Datei "MainActivity.java",** und fügen Sie der Klasse die folgenden Funktionen hinzu.</span><span class="sxs-lookup"><span data-stu-id="59a1c-177">Open the **MainActivity.java** file and add the the following functions to the class.</span></span>

    ```java
    // Load the "Home" fragment
    public void openHomeFragment(String userName) {
        HomeFragment fragment = HomeFragment.createInstance(userName);
        getSupportFragmentManager().beginTransaction()
                .replace(R.id.fragment_container, fragment)
                .commit();
        mNavigationView.setCheckedItem(R.id.nav_home);
    }

    // Load the "Calendar" fragment
    private void openCalendarFragment(String timeZone) {
        CalendarFragment fragment = CalendarFragment.createInstance(timeZone);
        getSupportFragmentManager().beginTransaction()
                .replace(R.id.fragment_container, fragment)
                .commit();
        mNavigationView.setCheckedItem(R.id.nav_calendar);
    }

    // Load the "New Event" fragment
    private void openNewEventFragment(String timeZone) {
        NewEventFragment fragment = NewEventFragment.createInstance(timeZone);
        getSupportFragmentManager().beginTransaction()
                .replace(R.id.fragment_container, fragment)
                .commit();
        mNavigationView.setCheckedItem(R.id.nav_create_event);
    }

    private void signIn() {
        setSignedInState(true);
        openHomeFragment(mUserName);
    }

    private void signOut() {
        setSignedInState(false);
        openHomeFragment(mUserName);
    }
    ```

1. <span data-ttu-id="59a1c-178">Ersetzen Sie die vorhandene `onNavigationItemSelected`-Funktion durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="59a1c-178">Replace the existing `onNavigationItemSelected` function with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="OnNavItemSelectedSnippet":::

1. <span data-ttu-id="59a1c-179">Speichern Sie alle Änderungen.</span><span class="sxs-lookup"><span data-stu-id="59a1c-179">Save all of your changes.</span></span>

1. <span data-ttu-id="59a1c-180">Wählen Sie **im Menü "Ausführen"** die Option **"App ausführen" aus.**</span><span class="sxs-lookup"><span data-stu-id="59a1c-180">On the **Run** menu, select **Run 'app'**.</span></span>

<span data-ttu-id="59a1c-181">Das Menü der App sollte funktionieren, um zwischen den beiden  Fragmenten zu navigieren und zu ändern, wenn Sie auf die Schaltflächen "Anmelden" oder **"Abmelden"** tippen.</span><span class="sxs-lookup"><span data-stu-id="59a1c-181">The app's menu should work to navigate between the two fragments and change when you tap the **Sign in** or **Sign out** buttons.</span></span>

![Screenshot der Anwendung](./images/app-screens.png)
