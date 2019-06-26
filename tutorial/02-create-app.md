<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d2ff1-101">Erstellen Sie zunächst ein neues Android Studio-Projekt.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-101">Begin by creating a new Android Studio project.</span></span>

1. <span data-ttu-id="d2ff1-102">Öffnen Sie Android Studio, und wählen Sie auf der Willkommensseite **ein neues Android Studio-Projekt starten** aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-102">Open Android Studio, and select **Start a new Android Studio project** on the welcome screen.</span></span>

1. <span data-ttu-id="d2ff1-103">Wählen Sie im Dialogfeld **Neues Projekt erstellen** die Option **leere Aktivität**aus, und wählen Sie dann **weiter**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-103">In the **Create New Project** dialog, select **Empty Activity**, then select **Next**.</span></span>

    ![Screenshot des Dialogfelds "Neues Projekt erstellen" in Android Studio](./images/choose-project.png)

1. <span data-ttu-id="d2ff1-105">Legen Sie im Dialogfeld **Projekt konfigurieren** den **Namen** fest `Graph Tutorial`, stellen Sie sicher \*\*\*\* , dass das Feld Sprache `Java`auf festgelegt ist, und stellen Sie sicher, `API 27: Android 8.1 (Oreo)`dass die **minimale API-Ebene** auf festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-105">In the **Configure your project** dialog, set the **Name** to `Graph Tutorial`, ensure the **Language** field is set to `Java`, and ensure the **Minimum API level** is set to `API 27: Android 8.1 (Oreo)`.</span></span> <span data-ttu-id="d2ff1-106">Ändern Sie den **Paketnamen** , und speichern Sie den **Speicherort** bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-106">Modify the **Package name** and **Save location** as needed.</span></span> <span data-ttu-id="d2ff1-107">Wählen Sie **Fertig stellen** aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-107">Select **Finish**.</span></span>

    ![Screenshot des Dialogfelds "Projekt konfigurieren"](./images/configure-project.png)

> [!IMPORTANT]
> <span data-ttu-id="d2ff1-109">Stellen Sie sicher, dass Sie den exakt gleichen Namen für das Projekt eingeben, das in diesen Übungsanweisungen angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-109">Ensure that you enter the exact same name for the project that is specified in these lab instructions.</span></span> <span data-ttu-id="d2ff1-110">Der Projektname wird Teil des Namespaces im Code.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-110">The project name becomes part of the namespace in the code.</span></span> <span data-ttu-id="d2ff1-111">Der Code in diesen Anweisungen hängt vom Namespace ab, der dem in diesen Anweisungen angegebenen Projektnamen entspricht.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-111">The code inside these instructions depends on the namespace matching the project name specified in these instructions.</span></span> <span data-ttu-id="d2ff1-112">Wenn Sie einen anderen Projektnamen verwenden, wird der Code nur dann kompiliert, wenn Sie alle Namespaces so anpassen, dass Sie dem Projektnamen entsprechen, den Sie beim Erstellen des Projekts eingeben.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-112">If you use a different project name the code will not compile unless you adjust all the namespaces to match the project name you enter when you create the project.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="d2ff1-113">Installieren von Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="d2ff1-113">Install dependencies</span></span>

<span data-ttu-id="d2ff1-114">Installieren Sie vor dem Verschieben einige zusätzliche Abhängigkeiten, die Sie später verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-114">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="d2ff1-115">`com.android.support:design`, um die Navigations Schublade-Layouts für die APP zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-115">`com.android.support:design` to make the navigation drawer layouts available to the app.</span></span>
- <span data-ttu-id="d2ff1-116">[Microsoft-Authentifizierungsbibliothek (MSAL) für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) , um die Azure AD Authentifizierung und Tokenverwaltung zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-116">[Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) to handle Azure AD authentication and token management.</span></span>
- <span data-ttu-id="d2ff1-117">[Microsoft Graph SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) zum tätigen von Anrufen an Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-117">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) for making calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="d2ff1-118">Erweitern Sie **Gradle-Skripts**, und öffnen Sie dann die Datei **Build. Gradle (Module: app)** .</span><span class="sxs-lookup"><span data-stu-id="d2ff1-118">Expand **Gradle Scripts**, then open the **build.gradle (Module: app)** file.</span></span>

1. <span data-ttu-id="d2ff1-119">Fügen Sie die folgenden Zeilen in `dependencies` den Wert ein.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-119">Add the following lines inside the `dependencies` value.</span></span>

    ```Gradle
    implementation 'com.android.support:design:28.0.0'
    implementation 'com.microsoft.graph:microsoft-graph:1.4.0'
    implementation 'com.microsoft.identity.client:msal:0.2.2'
    ```

    > [!NOTE]
    > <span data-ttu-id="d2ff1-120">Wenn Sie eine andere SDK-Version verwenden, stellen Sie sicher, dass `28.0.0` Sie die so ändern, dass `com.android.support:appcompat-v7` Sie mit der Version der Abhängigkeit übereinstimmt, die bereits in **Build. gradle**vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-120">If you are using a different SDK version, make sure to change the `28.0.0` to match the version of the `com.android.support:appcompat-v7` dependency already present in **build.gradle**.</span></span>

1. <span data-ttu-id="d2ff1-121">Fügen Sie `packagingOptions` in der `android` Datei **Build. gradle (Module: app)** einen in den Wert ein.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-121">Add a `packagingOptions` inside the `android` value in the **build.gradle (Module: app)** file.</span></span>

    ```Gradle
    packagingOptions {
        pickFirst 'META-INF/jersey-module-version'
    }
    ```

1. <span data-ttu-id="d2ff1-122">Speichern Sie Ihre Änderungen.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-122">Save your changes.</span></span> <span data-ttu-id="d2ff1-123">Wählen Sie im Menü **Datei** die Option **Projekt mit Gradle-Dateien synchronisieren**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-123">On the **File** menu, select **Sync Project with Gradle Files**.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="d2ff1-124">Entwerfen der APP</span><span class="sxs-lookup"><span data-stu-id="d2ff1-124">Design the app</span></span>

<span data-ttu-id="d2ff1-125">Die Anwendung wird eine [Navigations Schublade](https://developer.android.com/training/implementing-navigation/nav-drawer) verwenden, um zwischen verschiedenen Ansichten zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-125">The application will use a [navigation drawer](https://developer.android.com/training/implementing-navigation/nav-drawer) to navigate between different views.</span></span> <span data-ttu-id="d2ff1-126">In diesem Schritt aktualisieren Sie die Aktivität so, dass ein Navigations Fach Layout verwendet wird, und fügen Fragmente für die Ansichten hinzu.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-126">In this step you will update the activity to use a navigation drawer layout, and add fragments for the views.</span></span>

### <a name="create-a-navigation-drawer"></a><span data-ttu-id="d2ff1-127">Erstellen einer Navigations Schublade</span><span class="sxs-lookup"><span data-stu-id="d2ff1-127">Create a navigation drawer</span></span>

<span data-ttu-id="d2ff1-128">In diesem Abschnitt erstellen Sie Symbole für das Navigationsmenü der APP, erstellen ein Menü für die Anwendung und aktualisieren das Design und das Layout der Anwendung so, dass Sie mit einer Navigations Schublade kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-128">In this section you will create icons for the app's navigation menu, create a menu for the application, and update the application's theme and layout to be compatible with a navigation drawer.</span></span>

#### <a name="create-icons"></a><span data-ttu-id="d2ff1-129">Erstellen von Symbolen</span><span class="sxs-lookup"><span data-stu-id="d2ff1-129">Create icons</span></span>

1. <span data-ttu-id="d2ff1-130">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/drawable** , und wählen Sie **neu**und dann **Vector Asset**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-130">Right-click the **app/res/drawable** folder and select **New**, then **Vector Asset**.</span></span>

1. <span data-ttu-id="d2ff1-131">Klicken Sie neben **ClipArt**auf die Schaltfläche Symbol.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-131">Click the icon button next to **Clip Art**.</span></span>

1. <span data-ttu-id="d2ff1-132">Geben `home` Sie im Fenster **Symbol auswählen** die Suchleiste ein, und wählen Sie dann das Symbol **Start** aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-132">In the **Select Icon** window, type `home` in the search bar, then select the **Home** icon and select **OK**.</span></span>

1. <span data-ttu-id="d2ff1-133">Ändern Sie \*\*\*\* den Namen `ic_menu_home`in.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-133">Change the **Name** to `ic_menu_home`.</span></span>

    ![Screenshot des Fensters "Vektorobjekt konfigurieren"](./images/create-icon.png)

1. <span data-ttu-id="d2ff1-135">Wählen Sie **weiter**und dann **Fertig stellen**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-135">Select **Next**, then **Finish**.</span></span>

1. <span data-ttu-id="d2ff1-136">Wiederholen Sie den vorherigen Schritt, um drei weitere Symbole zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-136">Repeat the previous step to create three more icons.</span></span>

    - <span data-ttu-id="d2ff1-137">Name: `ic_menu_calendar`, Symbol:`event`</span><span class="sxs-lookup"><span data-stu-id="d2ff1-137">Name: `ic_menu_calendar`, Icon: `event`</span></span>
    - <span data-ttu-id="d2ff1-138">Name: `ic_menu_signout`, Symbol:`exit to app`</span><span class="sxs-lookup"><span data-stu-id="d2ff1-138">Name: `ic_menu_signout`, Icon: `exit to app`</span></span>
    - <span data-ttu-id="d2ff1-139">Name: `ic_menu_signin`, Symbol:`person add`</span><span class="sxs-lookup"><span data-stu-id="d2ff1-139">Name: `ic_menu_signin`, Icon: `person add`</span></span>

#### <a name="create-the-menu"></a><span data-ttu-id="d2ff1-140">Erstellen des Menüs</span><span class="sxs-lookup"><span data-stu-id="d2ff1-140">Create the menu</span></span>

1. <span data-ttu-id="d2ff1-141">Klicken Sie mit der rechten Maustaste auf den Ordner **Res** , und wählen Sie **neu**und dann **Android-Ressourcenverzeichnis**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-141">Right-click the **res** folder and select **New**, then **Android Resource Directory**.</span></span>

1. <span data-ttu-id="d2ff1-142">Ändern Sie \*\*\*\* den Ressourcentyp `menu` in, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-142">Change the **Resource type** to `menu` and select **OK**.</span></span>

1. <span data-ttu-id="d2ff1-143">Klicken Sie mit der rechten Maustaste auf den neuen **Menü** Ordner, und wählen Sie **neu**und dann **Menü Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-143">Right-click the new **menu** folder and select **New**, then **Menu resource file**.</span></span>

1. <span data-ttu-id="d2ff1-144">Nennen Sie die `drawer_menu` Datei, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-144">Name the file `drawer_menu` and select **OK**.</span></span>

1. <span data-ttu-id="d2ff1-145">Wenn die Datei geöffnet wird, wählen Sie die Registerkarte **Text** aus, um den XML-Code anzuzeigen, und ersetzen Sie dann den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-145">When the file opens, select the **Text** tab to view the XML, then replace the entire contents with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <menu xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        tools:showIn="navigation_view">

        <group android:checkableBehavior="single">
            <item
                android:id="@+id/nav_home"
                android:icon="@drawable/ic_menu_home"
                android:title="Home" />

            <item
                android:id="@+id/nav_calendar"
                android:icon="@drawable/ic_menu_calendar"
                android:title="Calendar" />
        </group>

        <item android:title="Account">
            <menu>
                <item
                    android:id="@+id/nav_signin"
                    android:icon="@drawable/ic_menu_signin"
                    android:title="Sign In" />

                <item
                    android:id="@+id/nav_signout"
                    android:icon="@drawable/ic_menu_signout"
                    android:title="Sign Out" />
            </menu>
        </item>

    </menu>
    ```

#### <a name="update-application-theme-and-layout"></a><span data-ttu-id="d2ff1-146">Aktualisieren des Anwendungsdesigns und-Layouts</span><span class="sxs-lookup"><span data-stu-id="d2ff1-146">Update application theme and layout</span></span>

1. <span data-ttu-id="d2ff1-147">Öffnen Sie die Datei **App/res/Values/Styles. XML** , und ersetzen `Theme.AppCompat.Light.DarkActionBar` Sie durch `Theme.AppCompat.Light.NoActionBar`.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-147">Open the **app/res/values/styles.xml** file and replace `Theme.AppCompat.Light.DarkActionBar` with `Theme.AppCompat.Light.NoActionBar`.</span></span>

1. <span data-ttu-id="d2ff1-148">Fügen Sie die folgenden Zeilen innerhalb `style` des-Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-148">Add the following lines inside the `style` element.</span></span>

    ```xml
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    ```

1. <span data-ttu-id="d2ff1-149">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** .</span><span class="sxs-lookup"><span data-stu-id="d2ff1-149">Right-click the **app/res/layout** folder.</span></span>

1. <span data-ttu-id="d2ff1-150">Wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-150">Select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="d2ff1-151">Nennen Sie die `nav_header` Datei, und ändern Sie das `LinearLayout` **Stammelement** in, und wählen Sie dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-151">Name the file `nav_header` and change the **Root element** to `LinearLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="d2ff1-152">Öffnen Sie die Datei **nav_header. XML** , und wählen Sie die Registerkarte **Text** aus. ersetzen Sie den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-152">Open the **nav_header.xml** file and select the **Text** tab. Replace the entire contents with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="176dp"
        android:background="@color/colorPrimary"
        android:gravity="bottom"
        android:orientation="vertical"
        android:padding="16dp"
        android:theme="@style/ThemeOverlay.AppCompat.Dark">

        <ImageView
            android:id="@+id/user_profile_pic"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@mipmap/ic_launcher" />

        <TextView
            android:id="@+id/user_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingTop="8dp"
            android:text="Test User"
            android:textAppearance="@style/TextAppearance.AppCompat.Body1" />

        <TextView
            android:id="@+id/user_email"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="test@contoso.com" />

    </LinearLayout>
    ```

1. <span data-ttu-id="d2ff1-153">Öffnen Sie die Datei **App/res/Layout/activity_main. XML** , und aktualisieren Sie das `DrawerLayout` Layout auf a, indem Sie den vorhandenen XML-Code durch Folgendes ersetzen.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-153">Open the **app/res/layout/activity_main.xml** file and update the layout to a `DrawerLayout` by replacing the existing XML with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:id="@+id/drawer_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:fitsSystemWindows="true"
        tools:context=".MainActivity"
        tools:openDrawer="start">

        <RelativeLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

            <ProgressBar
                android:id="@+id/progressbar"
                android:layout_width="75dp"
                android:layout_height="75dp"
                android:layout_centerInParent="true"
                android:visibility="gone"/>

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:background="@color/colorPrimary"
                android:elevation="4dp"
                android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar" />

            <FrameLayout
                android:id="@+id/fragment_container"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_below="@+id/toolbar" />
        </RelativeLayout>

        <android.support.design.widget.NavigationView
            android:id="@+id/nav_view"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            app:headerLayout="@layout/nav_header"
            app:menu="@menu/drawer_menu" />

    </android.support.v4.widget.DrawerLayout>
    ```

1. <span data-ttu-id="d2ff1-154">Öffnen Sie **App/res/Values/Strings. XML** , und fügen Sie die `resources` folgenden Elemente innerhalb des-Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-154">Open **app/res/values/strings.xml** and add the following elements inside the `resources` element.</span></span>

    ```xml
    <string name="navigation_drawer_open">Open navigation drawer</string>
    <string name="navigation_drawer_close">Close navigation drawer</string>
    ```

1. <span data-ttu-id="d2ff1-155">Öffnen Sie die Datei **App/Java/com. example/graphtutorial/Main** File, und ersetzen Sie den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-155">Open the **app/java/com.example/graphtutorial/MainActivity** file and replace the entire contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.support.annotation.NonNull;
    import android.support.design.widget.NavigationView;
    import android.support.v4.view.GravityCompat;
    import android.support.v4.widget.DrawerLayout;
    import android.support.v7.app.ActionBarDrawerToggle;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.support.v7.widget.Toolbar;
    import android.view.Menu;
    import android.view.MenuItem;
    import android.view.View;
    import android.widget.FrameLayout;
    import android.widget.ProgressBar;
    import android.widget.TextView;

    public class MainActivity extends AppCompatActivity implements NavigationView.OnNavigationItemSelectedListener {
        private DrawerLayout mDrawer;
        private NavigationView mNavigationView;
        private View mHeaderView;
        private boolean mIsSignedIn = false;
        private String mUserName = null;
        private String mUserEmail = null;

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

            Menu menu = mNavigationView.getMenu();

            // Hide/show the Sign in, Calendar, and Sign Out buttons
            menu.findItem(R.id.nav_signin).setVisible(!isSignedIn);
            menu.findItem(R.id.nav_calendar).setVisible(isSignedIn);
            menu.findItem(R.id.nav_signout).setVisible(isSignedIn);

            // Set the user name and email in the nav drawer
            TextView userName = mHeaderView.findViewById(R.id.user_name);
            TextView userEmail = mHeaderView.findViewById(R.id.user_email);

            if (isSignedIn) {
                // For testing
                mUserName = "Megan Bowen";
                mUserEmail = "meganb@contoso.com";

                userName.setText(mUserName);
                userEmail.setText(mUserEmail);
            } else {
                mUserName = null;
                mUserEmail = null;

                userName.setText("Please sign in");
                userEmail.setText("");
            }
        }
    }
    ```

### <a name="add-fragments"></a><span data-ttu-id="d2ff1-156">Hinzufügen von Fragmenten</span><span class="sxs-lookup"><span data-stu-id="d2ff1-156">Add fragments</span></span>

<span data-ttu-id="d2ff1-157">In diesem Abschnitt erstellen Sie Fragmente für die Ansichten "Start" und "Kalender".</span><span class="sxs-lookup"><span data-stu-id="d2ff1-157">In this section you will create fragments for the home and calendar views.</span></span>

1. <span data-ttu-id="d2ff1-158">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-158">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="d2ff1-159">Nennen Sie die `fragment_home` Datei, und ändern Sie das `RelativeLayout` **Stammelement** in, und wählen Sie dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-159">Name the file `fragment_home` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="d2ff1-160">Öffnen Sie die Datei **fragment_home. XML** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-160">Open the **fragment_home.xml** file and replace its contents with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:orientation="vertical">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_horizontal"
                android:text="Welcome!"
                android:textSize="30sp" />

            <TextView
                android:id="@+id/home_page_username"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_horizontal"
                android:paddingTop="8dp"
                android:text="Please sign in"
                android:textSize="20sp" />
        </LinearLayout>

    </RelativeLayout>
    ```

1. <span data-ttu-id="d2ff1-161">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-161">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="d2ff1-162">Nennen Sie die `fragment_calendar` Datei, und ändern Sie das `RelativeLayout` **Stammelement** in, und wählen Sie dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-162">Name the file `fragment_calendar` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="d2ff1-163">Öffnen Sie die Datei **fragment_calendar. XML** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-163">Open the **fragment_calendar.xml** file and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="d2ff1-164">Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie dann **neu**und dann **Java-Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-164">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="d2ff1-165">Nennen Sie die `HomeFragment` Klasse, und \*\*\*\* legen Sie `android.support.v4.app.Fragment`die Oberklasse auf fest, und wählen Sie dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-165">Name the class `HomeFragment` and set the **Superclass** to `android.support.v4.app.Fragment`, then select **OK**.</span></span>

1. <span data-ttu-id="d2ff1-166">Öffnen Sie die Datei **HomeFragment** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-166">Open the **HomeFragment** file and replace its contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.os.Bundle;
    import android.support.annotation.NonNull;
    import android.support.annotation.Nullable;
    import android.support.v4.app.Fragment;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import android.widget.TextView;

    public class HomeFragment extends Fragment {
        private static final String USER_NAME = "userName";

        private String mUserName;

        public HomeFragment() {

        }

        public static HomeFragment createInstance(String userName) {
            HomeFragment fragment = new HomeFragment();

            // Add the provided username to the fragment's arguments
            Bundle args = new Bundle();
            args.putString(USER_NAME, userName);
            fragment.setArguments(args);
            return fragment;
        }

        @Override
        public void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            if (getArguments() != null) {
                mUserName = getArguments().getString(USER_NAME);
            }
        }

        @Nullable
        @Override
        public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
            View homeView = inflater.inflate(R.layout.fragment_home, container, false);

            // If there is a username, replace the "Please sign in" with the username
            if (mUserName != null) {
                TextView userName = homeView.findViewById(R.id.home_page_username);
                userName.setText(mUserName);
            }

            return homeView;
        }
    }
    ```

1. <span data-ttu-id="d2ff1-167">Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie dann **neu**und dann **Java-Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-167">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="d2ff1-168">Nennen Sie die `CalendarFragment` Klasse, und \*\*\*\* legen Sie `android.support.v4.app.Fragment`die Oberklasse auf fest, und wählen Sie dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-168">Name the class `CalendarFragment` and set the **Superclass** to `android.support.v4.app.Fragment`, then select **OK**.</span></span>

1. <span data-ttu-id="d2ff1-169">Öffnen Sie die **CalendarFragment** -Datei, und fügen Sie die `CalendarFragment` folgende Funktion zur-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-169">Open the **CalendarFragment** file and add the following function to the `CalendarFragment` class.</span></span>

    ```java
    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_calendar, container, false);
    }
    ```

1. <span data-ttu-id="d2ff1-170">Öffnen Sie die Datei **mainstatus. Java** , und fügen Sie der Klasse die folgenden Funktionen hinzu.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-170">Open the **MainActivity.java** file and add the the following functions to the class.</span></span>

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
    private void openCalendarFragment() {
        getSupportFragmentManager().beginTransaction()
                .replace(R.id.fragment_container, new CalendarFragment())
                .commit();
        mNavigationView.setCheckedItem(R.id.nav_calendar);
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

1. <span data-ttu-id="d2ff1-171">Ersetzen Sie die vorhandene `onNavigationItemSelected`-Funktion durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-171">Replace the existing `onNavigationItemSelected` function with the following.</span></span>

    ```java
    @Override
    public boolean onNavigationItemSelected(@NonNull MenuItem menuItem) {
        // Load the fragment that corresponds to the selected item
        switch (menuItem.getItemId()) {
            case R.id.nav_home:
                openHomeFragment(mUserName);
                break;
            case R.id.nav_calendar:
                openCalendarFragment();
                break;
            case R.id.nav_signin:
                signIn();
                break;
            case R.id.nav_signout:
                signOut();
                break;
        }

        mDrawer.closeDrawer(GravityCompat.START);

        return true;
    }
    ```

1. <span data-ttu-id="d2ff1-172">Fügen Sie am Ende der `onCreate` Funktion Folgendes hinzu, um das Home-Fragment zu laden, wenn die APP gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-172">Add the following at the end of the `onCreate` function to load the home fragment when the app starts.</span></span>

    ```java
    // Load the home fragment by default on startup
    if (savedInstanceState == null) {
        openHomeFragment(mUserName);
    }
    ```

1. <span data-ttu-id="d2ff1-173">Speichern Sie alle Änderungen.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-173">Save all of your changes.</span></span>

1. <span data-ttu-id="d2ff1-174">Wählen Sie im Menü **Ausführen** die Option **app ausführen**aus.</span><span class="sxs-lookup"><span data-stu-id="d2ff1-174">On the **Run** menu, select **Run 'app'**.</span></span>

<span data-ttu-id="d2ff1-175">Das Menü der APP sollte zwischen den beiden Fragmenten navigieren und sich ändern, wenn Sie auf die Schaltflächen **Anmelden** oder Abmelden klicken. \*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="d2ff1-175">The app's menu should work to navigate between the two fragments and change when you tap the **Sign in** or **Sign out** buttons.</span></span>

![Screenshot der Anwendung](./images/app-screens.png)
