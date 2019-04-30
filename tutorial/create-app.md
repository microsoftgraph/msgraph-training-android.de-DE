<!-- markdownlint-disable MD002 MD041 -->

Öffnen Sie Android Studio, und wählen Sie auf der Willkommensseite **ein neues Android Studio-Projekt starten** aus. Wählen Sie im Dialogfeld **Neues Projekt erstellen** die Option **Aktivität leeren**aus, und klicken Sie dann auf **weiter**.

![Screenshot des Dialogfelds "Neues Projekt erstellen" in Android Studio](./images/choose-project.png)

Legen Sie im Dialogfeld **Projekt konfigurieren** den **Namen** auf `Graph Tutorial`fest, stellen Sie sicher, dass das Feld `Java` **Sprache** auf festgelegt ist, und stellen Sie sicher `API 27: Android 8.1 (Oreo)`, dass die **minimale API-Stufe** auf festgelegt ist. Ändern Sie den **Paketnamen** , und speichern Sie den **Speicherort** bei Bedarf. Wählen Sie **Fertig stellen** aus.

![Screenshot des Dialogfelds "Projekt konfigurieren"](./images/configure-project.png)

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie genau denselben Namen für das Projekt eingeben, das in diesen Lab-Anweisungen angegeben ist. Der Projektname wird Teil des Namespaces im Code. Der Code in diesen Anweisungen hängt vom Namespace ab, der dem in diesen Anweisungen angegebenen Projektnamen entspricht. Wenn Sie einen anderen Projektnamen verwenden, wird der Code nicht kompiliert, es sei denn, Sie passen alle Namespaces an den Projektnamen an, den Sie beim Erstellen des Projekts eingeben.

Bevor Sie fortfahren, sollten Sie einige zusätzliche Abhängigkeiten installieren, die Sie später verwenden werden.

- `com.android.support:design`, um die Layouts der Navigations Schublade der App zur Verfügung zu stellen.
- [Microsoft Authentication Library (MSAL) für Android für](https://github.com/AzureAD/microsoft-authentication-library-for-android) die Verarbeitung von Azure AD-Authentifizierung und Tokenverwaltung.
- [Microsoft Graph SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) für Aufrufe an Microsoft Graph.

Erweitern Sie **Gradle-Skripts**, und öffnen Sie dann die Datei **Build. Gradle (Module: app)** . Fügen Sie die folgenden Zeilen in `dependencies` den Wert ein.

```Gradle
implementation 'com.android.support:design:28.0.0'
implementation 'com.microsoft.graph:microsoft-graph:1.1.+'
implementation 'com.microsoft.identity.client:msal:0.2.+'
```

> [!NOTE]
> Wenn Sie eine andere SDK-Version verwenden, müssen Sie die `28.0.0` so ändern, dass Sie mit der Version `com.android.support:appcompat-v7` der Abhängigkeit übereinstimmt, die bereits in **Build. gradle**vorhanden ist.

Fügen Sie `packagingOptions` einen inneren `android` Wert in der Datei **Build. gradle (Module: app)** hinzu.

```Gradle
packagingOptions {
    pickFirst 'META-INF/jersey-module-version'
}
```

Speichern Sie Ihre Änderungen. Wählen Sie im Menü **Datei** die Option **Projekt mit Gradle-Dateien synchronisieren**aus.

## <a name="design-the-app"></a>Entwerfen der APP

Die Anwendung verwendet eine [Navigations Schublade](https://developer.android.com/training/implementing-navigation/nav-drawer) , um zwischen verschiedenen Ansichten zu navigieren. In diesem Schritt aktualisieren Sie die Aktivität auf die Verwendung eines Layouts für die Navigation und fügen Fragmente für die Ansichten hinzu.

### <a name="create-a-navigation-drawer"></a>Erstellen einer Navigations Schublade

Erstellen Sie zunächst Symbole für das Navigationsmenü der app. Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/drawable** , und wählen Sie **neu**und dann **Vektorobjekt**aus. Klicken Sie neben **ClipArt**auf die Schaltfläche Symbol. Geben `home` Sie im Fenster **Symbol auswählen** in die Suchleiste ein, und wählen Sie dann das Symbol **Start** und dann **OK**aus. Ändern Sie **** den Namen `ic_menu_home`in.

![Screenshot des Fensters "Vector-Ressource konfigurieren"](./images/create-icon.png)

Klicken Sie auf **weiter**und dann auf **Fertig stellen**. Wiederholen Sie diesen Schritt, um zwei weitere Symbole zu erstellen.

- Name: `ic_menu_calendar`, Icon:`event`
- Name: `ic_menu_signout`, Icon:`exit to app`
- Name: `ic_menu_signin`, Icon:`person add`

Erstellen Sie als nächstes ein Menü für die Anwendung. Klicken Sie mit der rechten Maustaste auf den Ordner **Res** , und wählen Sie **neu**und dann **Android Resource Directory**aus. Ändern Sie **** den Ressourcentyp `menu` in, und klicken Sie auf **OK**.

Klicken Sie mit der rechten Maustaste auf den neuen **Menü** Ordner, und wählen Sie **neu**und dann **Menü Ressourcendatei**aus. Benennen Sie die `drawer_menu` Datei, und klicken Sie auf **OK**. Wenn die Datei geöffnet wird, klicken Sie auf die Registerkarte **Text** , um den XML-Code anzuzeigen, und ersetzen Sie dann den gesamten Inhalt durch Folgendes.

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

Aktualisieren Sie nun das Anwendungsdesign, damit es mit einer Navigations Schublade kompatibel ist. Öffnen Sie die Datei **App/res/Values/Styles. XML** . Ersetzen `Theme.AppCompat.Light.DarkActionBar` Sie `Theme.AppCompat.Light.NoActionBar`durch. Fügen Sie dann die folgenden Zeilen innerhalb `style` des-Elements hinzu.

```xml
<item name="windowActionBar">false</item>
<item name="windowNoTitle">true</item>
<item name="android:statusBarColor">@android:color/transparent</item>
```

Erstellen Sie als nächstes eine Kopfzeile für das Menü. Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** . Klicken Sie auf **neu**und dann auf **Layout-Ressourcendatei**. Benennen Sie die `nav_header` Datei, und ändern Sie das `LinearLayout` **Stammelement** in. Wählen Sie **OK** aus.

Öffnen Sie die Datei **nav_header. XML** , und klicken Sie auf die Registerkarte **Text** . ersetzen Sie den gesamten Inhalt durch Folgendes.

```xml
<?xml version="1.0" encoding="utf-8"?>
<<?xml version="1.0" encoding="utf-8"?>
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

Öffnen Sie nun die Datei **App/res/Layout/activity_main. XML** . Aktualisieren Sie das Layout auf `DrawerLayout` a, indem Sie den vorhandenen XML-Code durch Folgendes ersetzen.

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

Öffnen Sie als nächstes **App/res/Values/Strings. XML**. Fügen Sie die folgenden Elemente innerhalb `resources` des-Elements hinzu.

```xml
<string name="navigation_drawer_open">Open navigation drawer</string>
<string name="navigation_drawer_close">Close navigation drawer</string>
```

Öffnen Sie schließlich die Datei " **App/Java/com. example/graphtutorial/Main-** file". Ersetzen Sie den gesamten Inhalt durch Folgendes.

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

Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout Ressourcendatei**aus. Benennen Sie die `fragment_home` Datei, und ändern Sie das `RelativeLayout` **Stammelement** in. Wählen Sie **OK** aus.

Öffnen Sie die Datei **fragment_home. XML** , und ersetzen Sie Ihren Inhalt durch Folgendes.

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

Klicken Sie dann mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout Ressourcendatei**aus. Benennen Sie die `fragment_calendar` Datei, und ändern Sie das `RelativeLayout` **Stammelement** in. Wählen Sie **OK** aus.

Öffnen Sie die Datei **fragment_calendar. XML** , und ersetzen Sie Ihren Inhalt durch Folgendes.

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

Klicken Sie nun mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie **neu**und dann **Java Class**aus. Benennen Sie die `HomeFragment` Klasse, und **** legen Sie `android.support.v4.app.Fragment`die Superclass auf fest. Wählen Sie **OK** aus. Öffnen Sie die **HomeFragment** -Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

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

Klicken Sie dann mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie **neu**und dann **Java Class**aus. Benennen Sie die `CalendarFragment` Klasse, und **** legen Sie `android.support.v4.app.Fragment`die Superclass auf fest. Wählen Sie **OK** aus.

Öffnen Sie die **CalendarFragment** -Datei, und fügen Sie die `CalendarFragment` folgende Funktion zur Klasse hinzu.

```java
@Nullable
@Override
public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
    return inflater.inflate(R.layout.fragment_calendar, container, false);
}
```

Nachdem die Fragmente implementiert wurden, aktualisieren Sie die `MainActivity` Klasse, um das `onNavigationItemSelected` Ereignis zu behandeln und die Fragmente zu verwenden. Fügen Sie zunächst die folgenden Funktionen zur Klasse hinzu.

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

Ersetzen Sie als nächstes die `onNavigationItemSelected` vorhandene Funktion durch Folgendes.

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

Fügen Sie abschließend am Ende der `onCreate` Funktion Folgendes ein, um das StartFragment beim Starten der APP zu laden.

```java
// Load the home fragment by default on startup
if (savedInstanceState == null) {
    openHomeFragment(mUserName);
}
```

Speichern Sie alle Änderungen. Wählen Sie im Menü **Ausführen** die Option **app ausführen**aus. Das Menü der APP sollte funktionieren, um zwischen den beiden Fragmenten zu navigieren und sich zu ändern, wenn Sie auf die Schaltflächen **Anmelden** oder Abmelden tippen. ****

![Screenshot der Anwendung](./images/welcome-page.png)