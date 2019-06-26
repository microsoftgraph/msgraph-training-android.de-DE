<!-- markdownlint-disable MD002 MD041 -->

Erstellen Sie zunächst ein neues Android Studio-Projekt.

1. Öffnen Sie Android Studio, und wählen Sie auf der Willkommensseite **ein neues Android Studio-Projekt starten** aus.

1. Wählen Sie im Dialogfeld **Neues Projekt erstellen** die Option **leere Aktivität**aus, und wählen Sie dann **weiter**aus.

    ![Screenshot des Dialogfelds "Neues Projekt erstellen" in Android Studio](./images/choose-project.png)

1. Legen Sie im Dialogfeld **Projekt konfigurieren** den **Namen** fest `Graph Tutorial`, stellen Sie sicher **** , dass das Feld Sprache `Java`auf festgelegt ist, und stellen Sie sicher, `API 27: Android 8.1 (Oreo)`dass die **minimale API-Ebene** auf festgelegt ist. Ändern Sie den **Paketnamen** , und speichern Sie den **Speicherort** bei Bedarf. Wählen Sie **Fertig stellen** aus.

    ![Screenshot des Dialogfelds "Projekt konfigurieren"](./images/configure-project.png)

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie den exakt gleichen Namen für das Projekt eingeben, das in diesen Übungsanweisungen angegeben ist. Der Projektname wird Teil des Namespaces im Code. Der Code in diesen Anweisungen hängt vom Namespace ab, der dem in diesen Anweisungen angegebenen Projektnamen entspricht. Wenn Sie einen anderen Projektnamen verwenden, wird der Code nur dann kompiliert, wenn Sie alle Namespaces so anpassen, dass Sie dem Projektnamen entsprechen, den Sie beim Erstellen des Projekts eingeben.

## <a name="install-dependencies"></a>Installieren von Abhängigkeiten

Installieren Sie vor dem Verschieben einige zusätzliche Abhängigkeiten, die Sie später verwenden werden.

- `com.android.support:design`, um die Navigations Schublade-Layouts für die APP zur Verfügung zu stellen.
- [Microsoft-Authentifizierungsbibliothek (MSAL) für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) , um die Azure AD Authentifizierung und Tokenverwaltung zu verwalten.
- [Microsoft Graph SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) zum tätigen von Anrufen an Microsoft Graph.

1. Erweitern Sie **Gradle-Skripts**, und öffnen Sie dann die Datei **Build. Gradle (Module: app)** .

1. Fügen Sie die folgenden Zeilen in `dependencies` den Wert ein.

    ```Gradle
    implementation 'com.android.support:design:28.0.0'
    implementation 'com.microsoft.graph:microsoft-graph:1.4.0'
    implementation 'com.microsoft.identity.client:msal:0.2.2'
    ```

    > [!NOTE]
    > Wenn Sie eine andere SDK-Version verwenden, stellen Sie sicher, dass `28.0.0` Sie die so ändern, dass `com.android.support:appcompat-v7` Sie mit der Version der Abhängigkeit übereinstimmt, die bereits in **Build. gradle**vorhanden ist.

1. Fügen Sie `packagingOptions` in der `android` Datei **Build. gradle (Module: app)** einen in den Wert ein.

    ```Gradle
    packagingOptions {
        pickFirst 'META-INF/jersey-module-version'
    }
    ```

1. Speichern Sie Ihre Änderungen. Wählen Sie im Menü **Datei** die Option **Projekt mit Gradle-Dateien synchronisieren**aus.

## <a name="design-the-app"></a>Entwerfen der APP

Die Anwendung wird eine [Navigations Schublade](https://developer.android.com/training/implementing-navigation/nav-drawer) verwenden, um zwischen verschiedenen Ansichten zu navigieren. In diesem Schritt aktualisieren Sie die Aktivität so, dass ein Navigations Fach Layout verwendet wird, und fügen Fragmente für die Ansichten hinzu.

### <a name="create-a-navigation-drawer"></a>Erstellen einer Navigations Schublade

In diesem Abschnitt erstellen Sie Symbole für das Navigationsmenü der APP, erstellen ein Menü für die Anwendung und aktualisieren das Design und das Layout der Anwendung so, dass Sie mit einer Navigations Schublade kompatibel sind.

#### <a name="create-icons"></a>Erstellen von Symbolen

1. Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/drawable** , und wählen Sie **neu**und dann **Vector Asset**aus.

1. Klicken Sie neben **ClipArt**auf die Schaltfläche Symbol.

1. Geben `home` Sie im Fenster **Symbol auswählen** die Suchleiste ein, und wählen Sie dann das Symbol **Start** aus, und klicken Sie dann auf **OK**.

1. Ändern Sie **** den Namen `ic_menu_home`in.

    ![Screenshot des Fensters "Vektorobjekt konfigurieren"](./images/create-icon.png)

1. Wählen Sie **weiter**und dann **Fertig stellen**aus.

1. Wiederholen Sie den vorherigen Schritt, um drei weitere Symbole zu erstellen.

    - Name: `ic_menu_calendar`, Symbol:`event`
    - Name: `ic_menu_signout`, Symbol:`exit to app`
    - Name: `ic_menu_signin`, Symbol:`person add`

#### <a name="create-the-menu"></a>Erstellen des Menüs

1. Klicken Sie mit der rechten Maustaste auf den Ordner **Res** , und wählen Sie **neu**und dann **Android-Ressourcenverzeichnis**aus.

1. Ändern Sie **** den Ressourcentyp `menu` in, und wählen Sie **OK**aus.

1. Klicken Sie mit der rechten Maustaste auf den neuen **Menü** Ordner, und wählen Sie **neu**und dann **Menü Ressourcendatei**aus.

1. Nennen Sie die `drawer_menu` Datei, und wählen Sie **OK**aus.

1. Wenn die Datei geöffnet wird, wählen Sie die Registerkarte **Text** aus, um den XML-Code anzuzeigen, und ersetzen Sie dann den gesamten Inhalt durch Folgendes.

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

#### <a name="update-application-theme-and-layout"></a>Aktualisieren des Anwendungsdesigns und-Layouts

1. Öffnen Sie die Datei **App/res/Values/Styles. XML** , und ersetzen `Theme.AppCompat.Light.DarkActionBar` Sie durch `Theme.AppCompat.Light.NoActionBar`.

1. Fügen Sie die folgenden Zeilen innerhalb `style` des-Elements hinzu.

    ```xml
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    ```

1. Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** .

1. Wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.

1. Nennen Sie die `nav_header` Datei, und ändern Sie das `LinearLayout` **Stammelement** in, und wählen Sie dann **OK**aus.

1. Öffnen Sie die Datei **nav_header. XML** , und wählen Sie die Registerkarte **Text** aus. ersetzen Sie den gesamten Inhalt durch Folgendes.

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

1. Öffnen Sie die Datei **App/res/Layout/activity_main. XML** , und aktualisieren Sie das `DrawerLayout` Layout auf a, indem Sie den vorhandenen XML-Code durch Folgendes ersetzen.

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

1. Öffnen Sie **App/res/Values/Strings. XML** , und fügen Sie die `resources` folgenden Elemente innerhalb des-Elements hinzu.

    ```xml
    <string name="navigation_drawer_open">Open navigation drawer</string>
    <string name="navigation_drawer_close">Close navigation drawer</string>
    ```

1. Öffnen Sie die Datei **App/Java/com. example/graphtutorial/Main** File, und ersetzen Sie den gesamten Inhalt durch Folgendes.

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

### <a name="add-fragments"></a>Hinzufügen von Fragmenten

In diesem Abschnitt erstellen Sie Fragmente für die Ansichten "Start" und "Kalender".

1. Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.

1. Nennen Sie die `fragment_home` Datei, und ändern Sie das `RelativeLayout` **Stammelement** in, und wählen Sie dann **OK**aus.

1. Öffnen Sie die Datei **fragment_home. XML** , und ersetzen Sie den Inhalt durch Folgendes.

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

1. Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.

1. Nennen Sie die `fragment_calendar` Datei, und ändern Sie das `RelativeLayout` **Stammelement** in, und wählen Sie dann **OK**aus.

1. Öffnen Sie die Datei **fragment_calendar. XML** , und ersetzen Sie den Inhalt durch Folgendes.

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

1. Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie dann **neu**und dann **Java-Klasse**aus.

1. Nennen Sie die `HomeFragment` Klasse, und **** legen Sie `android.support.v4.app.Fragment`die Oberklasse auf fest, und wählen Sie dann **OK**aus.

1. Öffnen Sie die Datei **HomeFragment** , und ersetzen Sie den Inhalt durch Folgendes.

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

1. Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie dann **neu**und dann **Java-Klasse**aus.

1. Nennen Sie die `CalendarFragment` Klasse, und **** legen Sie `android.support.v4.app.Fragment`die Oberklasse auf fest, und wählen Sie dann **OK**aus.

1. Öffnen Sie die **CalendarFragment** -Datei, und fügen Sie die `CalendarFragment` folgende Funktion zur-Klasse hinzu.

    ```java
    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_calendar, container, false);
    }
    ```

1. Öffnen Sie die Datei **mainstatus. Java** , und fügen Sie der Klasse die folgenden Funktionen hinzu.

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

1. Ersetzen Sie die vorhandene `onNavigationItemSelected`-Funktion durch Folgendes.

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

1. Fügen Sie am Ende der `onCreate` Funktion Folgendes hinzu, um das Home-Fragment zu laden, wenn die APP gestartet wird.

    ```java
    // Load the home fragment by default on startup
    if (savedInstanceState == null) {
        openHomeFragment(mUserName);
    }
    ```

1. Speichern Sie alle Änderungen.

1. Wählen Sie im Menü **Ausführen** die Option **app ausführen**aus.

Das Menü der APP sollte zwischen den beiden Fragmenten navigieren und sich ändern, wenn Sie auf die Schaltflächen **Anmelden** oder Abmelden klicken. ****

![Screenshot der Anwendung](./images/app-screens.png)
