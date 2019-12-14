<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="88e53-101">Erstellen Sie zunächst ein neues Android Studio-Projekt.</span><span class="sxs-lookup"><span data-stu-id="88e53-101">Begin by creating a new Android Studio project.</span></span>

1. <span data-ttu-id="88e53-102">Öffnen Sie Android Studio, und wählen Sie auf der Willkommensseite **ein neues Android Studio-Projekt starten** aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-102">Open Android Studio, and select **Start a new Android Studio project** on the welcome screen.</span></span>

1. <span data-ttu-id="88e53-103">Wählen Sie im Dialogfeld **Neues Projekt erstellen** die Option **leere Aktivität**aus, und wählen Sie dann **weiter**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-103">In the **Create New Project** dialog, select **Empty Activity**, then select **Next**.</span></span>

    ![Screenshot des Dialogfelds "Neues Projekt erstellen" in Android Studio](./images/choose-project.png)

1. <span data-ttu-id="88e53-105">Legen Sie im Dialogfeld **Projekt konfigurieren** den **Namen** fest `Graph Tutorial`, stellen Sie sicher \*\*\*\* , dass das Feld Sprache `Java`auf festgelegt ist, und stellen Sie sicher, `API 29: Android 10.0 (Q)`dass die **minimale API-Ebene** auf festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="88e53-105">In the **Configure your project** dialog, set the **Name** to `Graph Tutorial`, ensure the **Language** field is set to `Java`, and ensure the **Minimum API level** is set to `API 29: Android 10.0 (Q)`.</span></span> <span data-ttu-id="88e53-106">Ändern Sie den **Paketnamen** , und speichern Sie den **Speicherort** bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="88e53-106">Modify the **Package name** and **Save location** as needed.</span></span> <span data-ttu-id="88e53-107">Wählen Sie **Fertig stellen** aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-107">Select **Finish**.</span></span>

    ![Screenshot des Dialogfelds "Projekt konfigurieren"](./images/configure-project.png)

> [!IMPORTANT]
> <span data-ttu-id="88e53-109">Der Code und die Anweisungen in diesem Lernprogramm verwenden den Paketnamen **com. example. graphtutorial**.</span><span class="sxs-lookup"><span data-stu-id="88e53-109">The code and instructions in this tutorial use the package name **com.example.graphtutorial**.</span></span> <span data-ttu-id="88e53-110">Wenn Sie beim Erstellen des Projekts einen anderen Paketnamen verwenden, achten Sie darauf, dass Sie den Paketnamen überall dort verwenden, wo dieser Wert angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="88e53-110">If you use a different package name when creating the project, be sure to use your package name wherever you see this value.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="88e53-111">Installieren von Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="88e53-111">Install dependencies</span></span>

<span data-ttu-id="88e53-112">Installieren Sie vor dem Verschieben einige zusätzliche Abhängigkeiten, die Sie später verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="88e53-112">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="88e53-113">`com.google.android.material:material`, um die [Navigationsansicht](https://material.io/develop/android/components/navigation-view/) für die app verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="88e53-113">`com.google.android.material:material` to make the [navigation view](https://material.io/develop/android/components/navigation-view/) available to the app.</span></span>
- <span data-ttu-id="88e53-114">[Microsoft-Authentifizierungsbibliothek (MSAL) für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) , um die Azure AD Authentifizierung und Tokenverwaltung zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="88e53-114">[Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) to handle Azure AD authentication and token management.</span></span>
- <span data-ttu-id="88e53-115">[Microsoft Graph SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) zum tätigen von Anrufen an Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="88e53-115">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) for making calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="88e53-116">Erweitern Sie **Gradle-Skripts**, und öffnen Sie dann die Datei **Build. Gradle (Module: app)** .</span><span class="sxs-lookup"><span data-stu-id="88e53-116">Expand **Gradle Scripts**, then open the **build.gradle (Module: app)** file.</span></span>

1. <span data-ttu-id="88e53-117">Fügen Sie die folgenden Zeilen in `dependencies` den Wert ein.</span><span class="sxs-lookup"><span data-stu-id="88e53-117">Add the following lines inside the `dependencies` value.</span></span>

    ```Gradle
    implementation 'com.google.android.material:material:1.0.0'
    implementation 'com.microsoft.identity.client:msal:1.0.0'
    implementation 'com.microsoft.graph:microsoft-graph:1.6.0'
    ```

1. <span data-ttu-id="88e53-118">Fügen Sie `packagingOptions` in der Datei `android` **Build. gradle (Module: app)** einen Wert in den Wert ein.</span><span class="sxs-lookup"><span data-stu-id="88e53-118">Add a `packagingOptions` value inside the `android` value in the **build.gradle (Module: app)** file.</span></span>

    ```Gradle
    packagingOptions {
        pickFirst 'META-INF/jersey-module-version'
    }
    ```

1. <span data-ttu-id="88e53-119">Speichern Sie Ihre Änderungen.</span><span class="sxs-lookup"><span data-stu-id="88e53-119">Save your changes.</span></span> <span data-ttu-id="88e53-120">Wählen Sie im Menü **Datei** die Option **Projekt mit Gradle-Dateien synchronisieren**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-120">On the **File** menu, select **Sync Project with Gradle Files**.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="88e53-121">Entwerfen der APP</span><span class="sxs-lookup"><span data-stu-id="88e53-121">Design the app</span></span>

<span data-ttu-id="88e53-122">Die Anwendung wird eine Navigations Schublade verwenden, um zwischen verschiedenen Ansichten zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="88e53-122">The application will use a navigation drawer to navigate between different views.</span></span> <span data-ttu-id="88e53-123">In diesem Schritt aktualisieren Sie die Aktivität so, dass ein Navigations Fach Layout verwendet wird, und fügen Fragmente für die Ansichten hinzu.</span><span class="sxs-lookup"><span data-stu-id="88e53-123">In this step you will update the activity to use a navigation drawer layout, and add fragments for the views.</span></span>

### <a name="create-a-navigation-drawer"></a><span data-ttu-id="88e53-124">Erstellen einer Navigations Schublade</span><span class="sxs-lookup"><span data-stu-id="88e53-124">Create a navigation drawer</span></span>

<span data-ttu-id="88e53-125">In diesem Abschnitt erstellen Sie Symbole für das Navigationsmenü der APP, erstellen ein Menü für die Anwendung und aktualisieren das Design und das Layout der Anwendung so, dass Sie mit einer Navigations Schublade kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="88e53-125">In this section you will create icons for the app's navigation menu, create a menu for the application, and update the application's theme and layout to be compatible with a navigation drawer.</span></span>

#### <a name="create-icons"></a><span data-ttu-id="88e53-126">Erstellen von Symbolen</span><span class="sxs-lookup"><span data-stu-id="88e53-126">Create icons</span></span>

1. <span data-ttu-id="88e53-127">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/drawable** , und wählen Sie **neu**und dann **Vector Asset**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-127">Right-click the **app/res/drawable** folder and select **New**, then **Vector Asset**.</span></span>

1. <span data-ttu-id="88e53-128">Klicken Sie neben **ClipArt**auf die Schaltfläche Symbol.</span><span class="sxs-lookup"><span data-stu-id="88e53-128">Click the icon button next to **Clip Art**.</span></span>

1. <span data-ttu-id="88e53-129">Geben `home` Sie im Fenster **Symbol auswählen** die Suchleiste ein, und wählen Sie dann das Symbol **Start** aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="88e53-129">In the **Select Icon** window, type `home` in the search bar, then select the **Home** icon and select **OK**.</span></span>

1. <span data-ttu-id="88e53-130">Ändern Sie \*\*\*\* den Namen `ic_menu_home`in.</span><span class="sxs-lookup"><span data-stu-id="88e53-130">Change the **Name** to `ic_menu_home`.</span></span>

    ![Screenshot des Fensters "Vektorobjekt konfigurieren"](./images/create-icon.png)

1. <span data-ttu-id="88e53-132">Wählen Sie **weiter**und dann **Fertig stellen**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-132">Select **Next**, then **Finish**.</span></span>

1. <span data-ttu-id="88e53-133">Wiederholen Sie den vorherigen Schritt, um drei weitere Symbole zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="88e53-133">Repeat the previous step to create three more icons.</span></span>

    - <span data-ttu-id="88e53-134">Name: `ic_menu_calendar`, Symbol:`event`</span><span class="sxs-lookup"><span data-stu-id="88e53-134">Name: `ic_menu_calendar`, Icon: `event`</span></span>
    - <span data-ttu-id="88e53-135">Name: `ic_menu_signout`, Symbol:`exit to app`</span><span class="sxs-lookup"><span data-stu-id="88e53-135">Name: `ic_menu_signout`, Icon: `exit to app`</span></span>
    - <span data-ttu-id="88e53-136">Name: `ic_menu_signin`, Symbol:`person add`</span><span class="sxs-lookup"><span data-stu-id="88e53-136">Name: `ic_menu_signin`, Icon: `person add`</span></span>

#### <a name="create-the-menu"></a><span data-ttu-id="88e53-137">Erstellen des Menüs</span><span class="sxs-lookup"><span data-stu-id="88e53-137">Create the menu</span></span>

1. <span data-ttu-id="88e53-138">Klicken Sie mit der rechten Maustaste auf den Ordner **Res** , und wählen Sie **neu**und dann **Android-Ressourcenverzeichnis**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-138">Right-click the **res** folder and select **New**, then **Android Resource Directory**.</span></span>

1. <span data-ttu-id="88e53-139">Ändern Sie den **Ressourcentyp** in, `menu` und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-139">Change the **Resource type** to `menu` and select **OK**.</span></span>

1. <span data-ttu-id="88e53-140">Klicken Sie mit der rechten Maustaste auf den neuen **Menü** Ordner, und wählen Sie **neu**und dann **Menü Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-140">Right-click the new **menu** folder and select **New**, then **Menu resource file**.</span></span>

1. <span data-ttu-id="88e53-141">Nennen Sie die `drawer_menu` Datei, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-141">Name the file `drawer_menu` and select **OK**.</span></span>

1. <span data-ttu-id="88e53-142">Wenn die Datei geöffnet wird, wählen Sie die Registerkarte **Text** aus, um den XML-Code anzuzeigen, und ersetzen Sie dann den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="88e53-142">When the file opens, select the **Text** tab to view the XML, then replace the entire contents with the following.</span></span>

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

#### <a name="update-application-theme-and-layout"></a><span data-ttu-id="88e53-143">Aktualisieren des Anwendungsdesigns und-Layouts</span><span class="sxs-lookup"><span data-stu-id="88e53-143">Update application theme and layout</span></span>

1. <span data-ttu-id="88e53-144">Öffnen Sie die Datei **App/res/Values/Styles. XML** , und ersetzen `Theme.AppCompat.Light.DarkActionBar` Sie durch `Theme.AppCompat.Light.NoActionBar`.</span><span class="sxs-lookup"><span data-stu-id="88e53-144">Open the **app/res/values/styles.xml** file and replace `Theme.AppCompat.Light.DarkActionBar` with `Theme.AppCompat.Light.NoActionBar`.</span></span>

1. <span data-ttu-id="88e53-145">Fügen Sie die folgenden Zeilen innerhalb `style` des-Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="88e53-145">Add the following lines inside the `style` element.</span></span>

    ```xml
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    ```

1. <span data-ttu-id="88e53-146">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** .</span><span class="sxs-lookup"><span data-stu-id="88e53-146">Right-click the **app/res/layout** folder.</span></span>

1. <span data-ttu-id="88e53-147">Wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-147">Select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="88e53-148">Nennen Sie die `nav_header` Datei, und ändern Sie das `LinearLayout` **Stammelement** in, und wählen Sie dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-148">Name the file `nav_header` and change the **Root element** to `LinearLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="88e53-149">Öffnen Sie die Datei **nav_header. XML** , und wählen Sie die Registerkarte **Text** aus. ersetzen Sie den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="88e53-149">Open the **nav_header.xml** file and select the **Text** tab. Replace the entire contents with the following.</span></span>

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

1. <span data-ttu-id="88e53-150">Öffnen Sie die Datei **App/res/Layout/activity_main. XML** , und aktualisieren Sie das `DrawerLayout` Layout auf a, indem Sie den vorhandenen XML-Code durch Folgendes ersetzen.</span><span class="sxs-lookup"><span data-stu-id="88e53-150">Open the **app/res/layout/activity_main.xml** file and update the layout to a `DrawerLayout` by replacing the existing XML with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
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

            <androidx.appcompat.widget.Toolbar
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

        <com.google.android.material.navigation.NavigationView
            android:id="@+id/nav_view"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            app:headerLayout="@layout/nav_header"
            app:menu="@menu/drawer_menu" />

    </androidx.drawerlayout.widget.DrawerLayout>
    ```

1. <span data-ttu-id="88e53-151">Öffnen Sie **App/res/Values/Strings. XML** , und fügen Sie die `resources` folgenden Elemente innerhalb des-Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="88e53-151">Open **app/res/values/strings.xml** and add the following elements inside the `resources` element.</span></span>

    ```xml
    <string name="navigation_drawer_open">Open navigation drawer</string>
    <string name="navigation_drawer_close">Close navigation drawer</string>
    ```

1. <span data-ttu-id="88e53-152">Öffnen Sie die Datei **App/Java/com. example/graphtutorial/Main** File, und ersetzen Sie den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="88e53-152">Open the **app/java/com.example/graphtutorial/MainActivity** file and replace the entire contents with the following.</span></span>

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

### <a name="add-fragments"></a><span data-ttu-id="88e53-153">Hinzufügen von Fragmenten</span><span class="sxs-lookup"><span data-stu-id="88e53-153">Add fragments</span></span>

<span data-ttu-id="88e53-154">In diesem Abschnitt erstellen Sie Fragmente für die Ansichten "Start" und "Kalender".</span><span class="sxs-lookup"><span data-stu-id="88e53-154">In this section you will create fragments for the home and calendar views.</span></span>

1. <span data-ttu-id="88e53-155">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-155">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="88e53-156">Nennen Sie die `fragment_home` Datei, und ändern Sie das `RelativeLayout` **Stammelement** in, und wählen Sie dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-156">Name the file `fragment_home` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="88e53-157">Öffnen Sie die Datei **fragment_home. XML** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="88e53-157">Open the **fragment_home.xml** file and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="88e53-158">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-158">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="88e53-159">Nennen Sie die `fragment_calendar` Datei, und ändern Sie das `RelativeLayout` **Stammelement** in, und wählen Sie dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-159">Name the file `fragment_calendar` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="88e53-160">Öffnen Sie die Datei **fragment_calendar. XML** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="88e53-160">Open the **fragment_calendar.xml** file and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="88e53-161">Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie dann **neu**und dann **Java-Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-161">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="88e53-162">Nennen Sie die `HomeFragment` Klasse, und legen Sie `androidx.fragment.app.Fragment`die Ober **Klasse auf fest** , und wählen Sie dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-162">Name the class `HomeFragment` and set the **Superclass** to `androidx.fragment.app.Fragment`, then select **OK**.</span></span>

1. <span data-ttu-id="88e53-163">Öffnen Sie die Datei **HomeFragment** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="88e53-163">Open the **HomeFragment** file and replace its contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.os.Bundle;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import android.widget.TextView;
    import androidx.annotation.NonNull;
    import androidx.annotation.Nullable;
    import androidx.fragment.app.Fragment;

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

1. <span data-ttu-id="88e53-164">Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie dann **neu**und dann **Java-Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-164">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="88e53-165">Nennen Sie die `CalendarFragment` Klasse, und legen Sie `androidx.fragment.app.Fragment`die Ober **Klasse auf fest** , und wählen Sie dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-165">Name the class `CalendarFragment` and set the **Superclass** to `androidx.fragment.app.Fragment`, then select **OK**.</span></span>

1. <span data-ttu-id="88e53-166">Öffnen Sie die Datei **CalendarFragment** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="88e53-166">Open the **CalendarFragment** file and replace its contents with the following.</span></span>

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

        @Nullable
        @Override
        public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
            return inflater.inflate(R.layout.fragment_calendar, container, false);
        }
    }
    ```

1. <span data-ttu-id="88e53-167">Öffnen Sie die Datei **mainstatus. Java** , und fügen Sie der Klasse die folgenden Funktionen hinzu.</span><span class="sxs-lookup"><span data-stu-id="88e53-167">Open the **MainActivity.java** file and add the the following functions to the class.</span></span>

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

1. <span data-ttu-id="88e53-168">Ersetzen Sie die vorhandene `onNavigationItemSelected`-Funktion durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="88e53-168">Replace the existing `onNavigationItemSelected` function with the following.</span></span>

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

1. <span data-ttu-id="88e53-169">Fügen Sie am Ende der `onCreate` Funktion Folgendes hinzu, um das Home-Fragment zu laden, wenn die APP gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="88e53-169">Add the following at the end of the `onCreate` function to load the home fragment when the app starts.</span></span>

    ```java
    // Load the home fragment by default on startup
    if (savedInstanceState == null) {
        openHomeFragment(mUserName);
    }
    ```

1. <span data-ttu-id="88e53-170">Speichern Sie alle Änderungen.</span><span class="sxs-lookup"><span data-stu-id="88e53-170">Save all of your changes.</span></span>

1. <span data-ttu-id="88e53-171">Wählen Sie im Menü **Ausführen** die Option **app ausführen**aus.</span><span class="sxs-lookup"><span data-stu-id="88e53-171">On the **Run** menu, select **Run 'app'**.</span></span>

<span data-ttu-id="88e53-172">Das Menü der APP sollte zwischen den beiden Fragmenten navigieren und sich ändern, wenn Sie auf die Schaltflächen **Anmelden** oder **Abmelden** klicken.</span><span class="sxs-lookup"><span data-stu-id="88e53-172">The app's menu should work to navigate between the two fragments and change when you tap the **Sign in** or **Sign out** buttons.</span></span>

![Screenshot der Anwendung](./images/app-screens.png)
