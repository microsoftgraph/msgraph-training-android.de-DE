<!-- markdownlint-disable MD002 MD041 -->

Erstellen Sie zunächst ein neues Android Studio-Projekt.

1. Öffnen Sie Android Studio, und wählen **Sie auf der** Willkommensbildschirm die Option "Neues Android Studio-Projekt starten" aus.

1. Wählen Sie **im Dialogfeld "Neues Projekt** erstellen" die Option **"Leere Aktivität"** und dann **"Weiter" aus.**

    ![Screenshot des Dialogfelds "Neues Projekt erstellen" in Android Studio](./images/choose-project.png)

1. Legen Sie **im Dialogfeld** Projekt  konfigurieren den Namen auf , stellen Sie sicher, dass das Feld Sprache auf festgelegt ist, und stellen Sie sicher, dass die minimale `Graph Tutorial`  `Java` **API-Ebene** auf festgelegt `API 29: Android 10.0 (Q)` ist. Ändern Sie **den Paketnamen** und **den Speicherort** nach Bedarf. Wählen Sie **Fertig stellen** aus.

    ![Screenshot des Dialogfelds "Projekt konfigurieren"](./images/configure-project.png)

> [!IMPORTANT]
> Der Code und die Anweisungen in diesem Lernprogramm verwenden den Paketnamen **"com.example.graphtutorial".** Wenn Sie beim Erstellen des Projekts einen anderen Paketnamen verwenden, müssen Sie den Paketnamen überall dort verwenden, wo dieser Wert angezeigt wird.

## <a name="install-dependencies"></a>Installieren von Abhängigkeiten

Installieren Sie vor dem Wechsel einige zusätzliche Abhängigkeiten, die Sie später verwenden werden.

- `com.google.android.material:material` , um die [Navigationsansicht](https://material.io/develop/android/components/navigation-view/) für die App verfügbar zu machen.
- [Microsoft Authentication Library (MSAL) für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) zum Verarbeiten der Azure AD-Authentifizierung und Tokenverwaltung.
- [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) zum Aufrufen von Microsoft Graph.

1. Erweitern **Sie Gradle Scripts,** und öffnen Sie dann **"build.gradle" (Modul: Graph_Tutorial.app).**

1. Fügen Sie die folgenden Zeilen in den Wert `dependencies` ein.

    :::code language="gradle" source="../demo/GraphTutorial/app/build.gradle" id="DependenciesSnippet":::

1. Fügen Sie `packagingOptions` einen Wert innerhalb des `android` Werts in **build.gradle (Module: Graph_Tutorial.app) hinzu.**

    ```Gradle
    packagingOptions {
        pickFirst 'META-INF/jersey-module-version'
    }
    ```

1. Fügen Sie das Azure Maven-Repository für die MicrosoftDeviceSDK-Bibliothek, eine Abhängigkeit von MSAL, hinzu. Öffnen **Sie build.gradle (Project: Graph_Tutorial)**. Fügen Sie dem Wert innerhalb `repositories` des Werts Folgendes `allprojects` hinzu.

    ```Gradle
    maven {
        url 'https://pkgs.dev.azure.com/MicrosoftDeviceSDK/DuoSDK-Public/_packaging/Duo-SDK-Feed/maven/v1'
    }
    ```

1. Speichern Sie Ihre Änderungen. Wählen Sie **im Menü "Datei"** die Option **"Projekt mit Gradledateien synchronisieren" aus.**

## <a name="design-the-app"></a>Entwerfen der App

Die Anwendung verwendet eine Navigationsschubschubte, um zwischen verschiedenen Ansichten zu navigieren. In diesem Schritt aktualisieren Sie die Aktivität so, dass ein Navigationsschubschub layout verwendet wird, und fügen Fragmente für die Ansichten hinzu.

### <a name="create-a-navigation-drawer"></a>Erstellen einer Navigationsschubte

In diesem Abschnitt erstellen Sie Symbole für das Navigationsmenü der App, erstellen ein Menü für die Anwendung und aktualisieren das Design und Layout der Anwendung so, dass sie mit einer Navigationsschubschubte kompatibel sind.

#### <a name="create-icons"></a>Erstellen von Symbolen

1. Klicken Sie mit der rechten Maustaste auf den **Ordner "app/res/drawable",** und wählen **Sie "Neu"** und dann **"Vector Asset" aus.**

1. Klicken Sie auf die Symbolschaltfläche neben **ClipArt.**

1. Geben Sie **im Fenster "Symbol** auswählen" die Suchleiste ein, wählen Sie dann das Startsymbol `home` aus, und wählen Sie OK **aus.** 

1. Ändern Sie **den Namen** in `ic_menu_home` .

    ![Screenshot des Fensters "Vector Asset konfigurieren"](./images/create-icon.png)

1. Wählen Sie **"Weiter"** und dann **"Fertig stellen" aus.**

1. Wiederholen Sie den vorherigen Schritt, um vier weitere Symbole zu erstellen.

    - Name: `ic_menu_calendar` , Symbol: `event`
    - Name: `ic_menu_add_event` , Symbol: `add box`
    - Name: `ic_menu_signout` , Symbol: `exit to app`
    - Name: `ic_menu_signin` , Symbol: `person add`

#### <a name="create-the-menu"></a>Erstellen des Menüs

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"res",** und wählen **Sie "Neu"** und dann **"Android-Ressourcenverzeichnis" aus.**

1. Ändern Sie den **Ressourcentyp,** `menu` und wählen Sie OK **aus.**

1. Klicken Sie mit der rechten Maustaste auf den neuen **Menüordner,** und wählen **Sie "Neu"** und dann **"Menüressourcendatei" aus.**

1. Benennen Sie die `drawer_menu` Datei, und wählen Sie **OK aus.**

1. Wenn die Datei geöffnet wird, wählen Sie die Registerkarte **"Code"** aus, um den XML-Code anzuzeigen, und ersetzen Sie dann den gesamten Inhalt durch Folgendes.

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/menu/drawer_menu.xml":::

#### <a name="update-application-theme-and-layout"></a>Aktualisieren des Anwendungsdesigns und -layouts

1. Öffnen Sie **die Datei "app/res/values/styles.xml"** und ersetzen Sie `Theme.AppCompat.Light.DarkActionBar` sie durch `Theme.AppCompat.Light.NoActionBar` .

1. Fügen Sie die folgenden Zeilen innerhalb des Elements `style` hinzu.

    ```xml
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    ```

1. Klicken Sie mit der rechten Maustaste **auf den Ordner "app/res/layout".**

1. Wählen **Sie "Neu"** und dann **"Layoutressourcendatei" aus.**

1. Benennen Sie die `nav_header` Datei, und ändern Sie das **Stammelement** in `LinearLayout` , und wählen Sie dann OK **aus.**

1. Öffnen Sie **dienav_header.xml,** und wählen Sie die Registerkarte **"Text"** aus. Ersetzen Sie den gesamten Inhalt durch Folgendes.

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/nav_header.xml":::

1. Öffnen Sie **die Datei "app/res/layout/activity_main.xml",** und aktualisieren Sie das Layout auf a, indem Sie die vorhandene XML durch `DrawerLayout` Folgendes ersetzen.

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/activity_main.xml":::

1. Öffnen **Sie "app/res/values/strings.xml" und** fügen Sie die folgenden Elemente innerhalb des Elements `resources` hinzu.

    ```xml
    <string name="navigation_drawer_open">Open navigation drawer</string>
    <string name="navigation_drawer_close">Close navigation drawer</string>
    ```

1. Öffnen Sie **die Datei "app/java/com.example/graphtutorial/MainActivity",** und ersetzen Sie den gesamten Inhalt durch Folgendes.

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

### <a name="add-fragments"></a>Hinzufügen von Fragmenten

In diesem Abschnitt erstellen Sie Fragmente für die Homepage- und Kalenderansichten.

1. Klicken Sie mit der rechten Maustaste auf den **Ordner "app/res/layout",** und wählen **Sie "Neu"** und dann **"Layoutressourcendatei" aus.**

1. Benennen Sie die `fragment_home` Datei, und ändern Sie das **Stammelement** in `RelativeLayout` , und wählen Sie dann OK **aus.**

1. Öffnen Sie **fragment_home.xml** Datei, und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_home.xml":::

1. Klicken Sie mit der rechten Maustaste auf den **Ordner "app/res/layout",** und wählen **Sie "Neu"** und dann **"Layoutressourcendatei" aus.**

1. Benennen Sie die `fragment_calendar` Datei, und ändern Sie das **Stammelement** in `RelativeLayout` , und wählen Sie dann OK **aus.**

1. Öffnen Sie **fragment_calendar.xml** Datei, und ersetzen Sie den Inhalt durch Folgendes.

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

1. Klicken Sie mit der rechten Maustaste auf den **Ordner "app/res/layout",** und wählen **Sie "Neu"** und dann **"Layoutressourcendatei" aus.**

1. Benennen Sie die `fragment_new_event` Datei, und ändern Sie das **Stammelement** in `RelativeLayout` , und wählen Sie dann OK **aus.**

1. Öffnen Sie **fragment_new_event.xml** Datei, und ersetzen Sie den Inhalt durch Folgendes.

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

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** aus, und klicken Sie **dann Java Klasse**.

1. Benennen Sie die `HomeFragment` Klasse, und wählen Sie dann **OK aus.**

1. Öffnen Sie **die Datei HomeFragment,** und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/HomeFragment.java" id="HomeSnippet":::

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.**

1. Benennen Sie die `CalendarFragment` Klasse, und wählen Sie dann **OK aus.**

1. Öffnen Sie **die CalendarFragment-Datei,** und ersetzen Sie den Inhalt durch Folgendes.

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

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.**

1. Benennen Sie die `NewEventFragment` Klasse, und wählen Sie dann **OK aus.**

1. Öffnen Sie **die Datei NewEventFragment,** und ersetzen Sie den Inhalt durch Folgendes.

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

1. Öffnen Sie **die Datei "MainActivity.java",** und fügen Sie der Klasse die folgenden Funktionen hinzu.

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

1. Ersetzen Sie die vorhandene `onNavigationItemSelected`-Funktion durch Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="OnNavItemSelectedSnippet":::

1. Speichern Sie alle Änderungen.

1. Wählen Sie **im Menü "Ausführen"** die Option **"App ausführen" aus.**

Das Menü der App sollte funktionieren, um zwischen den beiden  Fragmenten zu navigieren und zu ändern, wenn Sie auf die Schaltflächen "Anmelden" oder **"Abmelden"** tippen.

![Screenshot der Anwendung](./images/app-screens.png)
