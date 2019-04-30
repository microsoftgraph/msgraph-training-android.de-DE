<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="37513-101">Öffnen Sie Android Studio, und wählen Sie auf der Willkommensseite **ein neues Android Studio-Projekt starten** aus.</span><span class="sxs-lookup"><span data-stu-id="37513-101">Open Android Studio, and select **Start a new Android Studio project** on the welcome screen.</span></span> <span data-ttu-id="37513-102">Wählen Sie im Dialogfeld **Neues Projekt erstellen** die Option **Aktivität leeren**aus, und klicken Sie dann auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="37513-102">In the **Create New Project** dialog, select **Empty Activity**, then choose **Next**.</span></span>

![Screenshot des Dialogfelds "Neues Projekt erstellen" in Android Studio](./images/choose-project.png)

<span data-ttu-id="37513-104">Legen Sie im Dialogfeld **Projekt konfigurieren** den **Namen** auf `Graph Tutorial`fest, stellen Sie sicher, dass das Feld `Java` **Sprache** auf festgelegt ist, und stellen Sie sicher `API 27: Android 8.1 (Oreo)`, dass die **minimale API-Stufe** auf festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="37513-104">In the **Configure your project** dialog, set the **Name** to `Graph Tutorial`, ensure the **Language** field is set to `Java`, and ensure the **Minimum API level** is set to `API 27: Android 8.1 (Oreo)`.</span></span> <span data-ttu-id="37513-105">Ändern Sie den **Paketnamen** , und speichern Sie den **Speicherort** bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="37513-105">Modify the **Package name** and **Save location** as needed.</span></span> <span data-ttu-id="37513-106">Wählen Sie **Fertig stellen** aus.</span><span class="sxs-lookup"><span data-stu-id="37513-106">Select **Finish**.</span></span>

![Screenshot des Dialogfelds "Projekt konfigurieren"](./images/configure-project.png)

> [!IMPORTANT]
> <span data-ttu-id="37513-108">Stellen Sie sicher, dass Sie genau denselben Namen für das Projekt eingeben, das in diesen Lab-Anweisungen angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="37513-108">Ensure that you enter the exact same name for the project that is specified in these lab instructions.</span></span> <span data-ttu-id="37513-109">Der Projektname wird Teil des Namespaces im Code.</span><span class="sxs-lookup"><span data-stu-id="37513-109">The project name becomes part of the namespace in the code.</span></span> <span data-ttu-id="37513-110">Der Code in diesen Anweisungen hängt vom Namespace ab, der dem in diesen Anweisungen angegebenen Projektnamen entspricht.</span><span class="sxs-lookup"><span data-stu-id="37513-110">The code inside these instructions depends on the namespace matching the project name specified in these instructions.</span></span> <span data-ttu-id="37513-111">Wenn Sie einen anderen Projektnamen verwenden, wird der Code nicht kompiliert, es sei denn, Sie passen alle Namespaces an den Projektnamen an, den Sie beim Erstellen des Projekts eingeben.</span><span class="sxs-lookup"><span data-stu-id="37513-111">If you use a different project name the code will not compile unless you adjust all the namespaces to match the project name you enter when you create the project.</span></span>

<span data-ttu-id="37513-112">Bevor Sie fortfahren, sollten Sie einige zusätzliche Abhängigkeiten installieren, die Sie später verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="37513-112">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="37513-113">`com.android.support:design`, um die Layouts der Navigations Schublade der App zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="37513-113">`com.android.support:design` to make the navigation drawer layouts available to the app.</span></span>
- <span data-ttu-id="37513-114">[Microsoft Authentication Library (MSAL) für Android für](https://github.com/AzureAD/microsoft-authentication-library-for-android) die Verarbeitung von Azure AD-Authentifizierung und Tokenverwaltung.</span><span class="sxs-lookup"><span data-stu-id="37513-114">[Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) to handle Azure AD authentication and token management.</span></span>
- <span data-ttu-id="37513-115">[Microsoft Graph SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) für Aufrufe an Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="37513-115">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) for making calls to the Microsoft Graph.</span></span>

<span data-ttu-id="37513-116">Erweitern Sie **Gradle-Skripts**, und öffnen Sie dann die Datei **Build. Gradle (Module: app)** .</span><span class="sxs-lookup"><span data-stu-id="37513-116">Expand **Gradle Scripts**, then open the **build.gradle (Module: app)** file.</span></span> <span data-ttu-id="37513-117">Fügen Sie die folgenden Zeilen in `dependencies` den Wert ein.</span><span class="sxs-lookup"><span data-stu-id="37513-117">Add the following lines inside the `dependencies` value.</span></span>

```Gradle
implementation 'com.android.support:design:28.0.0'
implementation 'com.microsoft.graph:microsoft-graph:1.1.+'
implementation 'com.microsoft.identity.client:msal:0.2.+'
```

> [!NOTE]
> <span data-ttu-id="37513-118">Wenn Sie eine andere SDK-Version verwenden, müssen Sie die `28.0.0` so ändern, dass Sie mit der Version `com.android.support:appcompat-v7` der Abhängigkeit übereinstimmt, die bereits in **Build. gradle**vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="37513-118">If you are using a different SDK version, make sure to change the `28.0.0` to match the version of the `com.android.support:appcompat-v7` dependency already present in **build.gradle**.</span></span>

<span data-ttu-id="37513-119">Fügen Sie `packagingOptions` einen inneren `android` Wert in der Datei **Build. gradle (Module: app)** hinzu.</span><span class="sxs-lookup"><span data-stu-id="37513-119">Add a `packagingOptions` inside the `android` value in the **build.gradle (Module: app)** file.</span></span>

```Gradle
packagingOptions {
    pickFirst 'META-INF/jersey-module-version'
}
```

<span data-ttu-id="37513-120">Speichern Sie Ihre Änderungen.</span><span class="sxs-lookup"><span data-stu-id="37513-120">Save your changes.</span></span> <span data-ttu-id="37513-121">Wählen Sie im Menü **Datei** die Option **Projekt mit Gradle-Dateien synchronisieren**aus.</span><span class="sxs-lookup"><span data-stu-id="37513-121">On the **File** menu, select **Sync Project with Gradle Files**.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="37513-122">Entwerfen der APP</span><span class="sxs-lookup"><span data-stu-id="37513-122">Design the app</span></span>

<span data-ttu-id="37513-123">Die Anwendung verwendet eine [Navigations Schublade](https://developer.android.com/training/implementing-navigation/nav-drawer) , um zwischen verschiedenen Ansichten zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="37513-123">The application will use a [navigation drawer](https://developer.android.com/training/implementing-navigation/nav-drawer) to navigate between different views.</span></span> <span data-ttu-id="37513-124">In diesem Schritt aktualisieren Sie die Aktivität auf die Verwendung eines Layouts für die Navigation und fügen Fragmente für die Ansichten hinzu.</span><span class="sxs-lookup"><span data-stu-id="37513-124">In this step you will update the activity to use a navigation drawer layout, and add fragments for the views.</span></span>

### <a name="create-a-navigation-drawer"></a><span data-ttu-id="37513-125">Erstellen einer Navigations Schublade</span><span class="sxs-lookup"><span data-stu-id="37513-125">Create a navigation drawer</span></span>

<span data-ttu-id="37513-126">Erstellen Sie zunächst Symbole für das Navigationsmenü der app.</span><span class="sxs-lookup"><span data-stu-id="37513-126">Start by creating icons for the app's navigation menu.</span></span> <span data-ttu-id="37513-127">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/drawable** , und wählen Sie **neu**und dann **Vektorobjekt**aus.</span><span class="sxs-lookup"><span data-stu-id="37513-127">Right-click the **app/res/drawable** folder and select **New**, then **Vector Asset**.</span></span> <span data-ttu-id="37513-128">Klicken Sie neben **ClipArt**auf die Schaltfläche Symbol.</span><span class="sxs-lookup"><span data-stu-id="37513-128">Click the icon button next to **Clip Art**.</span></span> <span data-ttu-id="37513-129">Geben `home` Sie im Fenster **Symbol auswählen** in die Suchleiste ein, und wählen Sie dann das Symbol **Start** und dann **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="37513-129">In the **Select Icon** window, type `home` in the search bar, then select the **Home** icon and choose **OK**.</span></span> <span data-ttu-id="37513-130">Ändern Sie \*\*\*\* den Namen `ic_menu_home`in.</span><span class="sxs-lookup"><span data-stu-id="37513-130">Change the **Name** to `ic_menu_home`.</span></span>

![Screenshot des Fensters "Vector-Ressource konfigurieren"](./images/create-icon.png)

<span data-ttu-id="37513-132">Klicken Sie auf **weiter**und dann auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="37513-132">Choose **Next**, then **Finish**.</span></span> <span data-ttu-id="37513-133">Wiederholen Sie diesen Schritt, um zwei weitere Symbole zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="37513-133">Repeat this step to create two more icons.</span></span>

- <span data-ttu-id="37513-134">Name: `ic_menu_calendar`, Icon:`event`</span><span class="sxs-lookup"><span data-stu-id="37513-134">Name: `ic_menu_calendar`, Icon: `event`</span></span>
- <span data-ttu-id="37513-135">Name: `ic_menu_signout`, Icon:`exit to app`</span><span class="sxs-lookup"><span data-stu-id="37513-135">Name: `ic_menu_signout`, Icon: `exit to app`</span></span>
- <span data-ttu-id="37513-136">Name: `ic_menu_signin`, Icon:`person add`</span><span class="sxs-lookup"><span data-stu-id="37513-136">Name: `ic_menu_signin`, Icon: `person add`</span></span>

<span data-ttu-id="37513-137">Erstellen Sie als nächstes ein Menü für die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="37513-137">Next, create a menu for the application.</span></span> <span data-ttu-id="37513-138">Klicken Sie mit der rechten Maustaste auf den Ordner **Res** , und wählen Sie **neu**und dann **Android Resource Directory**aus.</span><span class="sxs-lookup"><span data-stu-id="37513-138">Right-click the **res** folder and choose **New**, then **Android Resource Directory**.</span></span> <span data-ttu-id="37513-139">Ändern Sie \*\*\*\* den Ressourcentyp `menu` in, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="37513-139">Change the **Resource type** to `menu` and choose **OK**.</span></span>

<span data-ttu-id="37513-140">Klicken Sie mit der rechten Maustaste auf den neuen **Menü** Ordner, und wählen Sie **neu**und dann **Menü Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="37513-140">Right-click the new **menu** folder and choose **New**, then **Menu resource file**.</span></span> <span data-ttu-id="37513-141">Benennen Sie die `drawer_menu` Datei, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="37513-141">Name the file `drawer_menu` and choose **OK**.</span></span> <span data-ttu-id="37513-142">Wenn die Datei geöffnet wird, klicken Sie auf die Registerkarte **Text** , um den XML-Code anzuzeigen, und ersetzen Sie dann den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37513-142">When the file opens, choose the **Text** tab to view the XML, then replace the entire contents with the following.</span></span>

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

<span data-ttu-id="37513-143">Aktualisieren Sie nun das Anwendungsdesign, damit es mit einer Navigations Schublade kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="37513-143">Now update the application's theme to be compatible with a navigation drawer.</span></span> <span data-ttu-id="37513-144">Öffnen Sie die Datei **App/res/Values/Styles. XML** .</span><span class="sxs-lookup"><span data-stu-id="37513-144">Open the **app/res/values/styles.xml** file.</span></span> <span data-ttu-id="37513-145">Ersetzen `Theme.AppCompat.Light.DarkActionBar` Sie `Theme.AppCompat.Light.NoActionBar`durch.</span><span class="sxs-lookup"><span data-stu-id="37513-145">Replace `Theme.AppCompat.Light.DarkActionBar` with `Theme.AppCompat.Light.NoActionBar`.</span></span> <span data-ttu-id="37513-146">Fügen Sie dann die folgenden Zeilen innerhalb `style` des-Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="37513-146">Then add the following lines inside the `style` element.</span></span>

```xml
<item name="windowActionBar">false</item>
<item name="windowNoTitle">true</item>
<item name="android:statusBarColor">@android:color/transparent</item>
```

<span data-ttu-id="37513-147">Erstellen Sie als nächstes eine Kopfzeile für das Menü.</span><span class="sxs-lookup"><span data-stu-id="37513-147">Next, create a header for the menu.</span></span> <span data-ttu-id="37513-148">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** .</span><span class="sxs-lookup"><span data-stu-id="37513-148">Right-click the **app/res/layout** folder.</span></span> <span data-ttu-id="37513-149">Klicken Sie auf **neu**und dann auf **Layout-Ressourcendatei**.</span><span class="sxs-lookup"><span data-stu-id="37513-149">Choose **New**, then **Layout resource file**.</span></span> <span data-ttu-id="37513-150">Benennen Sie die `nav_header` Datei, und ändern Sie das `LinearLayout` **Stammelement** in.</span><span class="sxs-lookup"><span data-stu-id="37513-150">Name the file `nav_header` and change the **Root element** to `LinearLayout`.</span></span> <span data-ttu-id="37513-151">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="37513-151">Choose **OK**.</span></span>

<span data-ttu-id="37513-152">Öffnen Sie die Datei **nav_header. XML** , und klicken Sie auf die Registerkarte **Text** . ersetzen Sie den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37513-152">Open the **nav_header.xml** file and choose the **Text** tab. Replace the entire contents with the following.</span></span>

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

<span data-ttu-id="37513-153">Öffnen Sie nun die Datei **App/res/Layout/activity_main. XML** .</span><span class="sxs-lookup"><span data-stu-id="37513-153">Now, open the **app/res/layout/activity_main.xml** file.</span></span> <span data-ttu-id="37513-154">Aktualisieren Sie das Layout auf `DrawerLayout` a, indem Sie den vorhandenen XML-Code durch Folgendes ersetzen.</span><span class="sxs-lookup"><span data-stu-id="37513-154">Update the layout to a `DrawerLayout` by replacing the existing XML with the following.</span></span>

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

<span data-ttu-id="37513-155">Öffnen Sie als nächstes **App/res/Values/Strings. XML**.</span><span class="sxs-lookup"><span data-stu-id="37513-155">Next, open **app/res/values/strings.xml**.</span></span> <span data-ttu-id="37513-156">Fügen Sie die folgenden Elemente innerhalb `resources` des-Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="37513-156">Add the following elements inside the `resources` element.</span></span>

```xml
<string name="navigation_drawer_open">Open navigation drawer</string>
<string name="navigation_drawer_close">Close navigation drawer</string>
```

<span data-ttu-id="37513-157">Öffnen Sie schließlich die Datei " **App/Java/com. example/graphtutorial/Main-** file".</span><span class="sxs-lookup"><span data-stu-id="37513-157">Finally, open the **app/java/com.example/graphtutorial/MainActivity** file.</span></span> <span data-ttu-id="37513-158">Ersetzen Sie den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37513-158">Replace the entire contents with the following.</span></span>

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

### <a name="add-fragments"></a><span data-ttu-id="37513-159">Hinzufügen von Fragmenten</span><span class="sxs-lookup"><span data-stu-id="37513-159">Add fragments</span></span>

<span data-ttu-id="37513-160">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="37513-160">Right-click the **app/res/layout** folder and choose **New**, then **Layout resource file**.</span></span> <span data-ttu-id="37513-161">Benennen Sie die `fragment_home` Datei, und ändern Sie das `RelativeLayout` **Stammelement** in.</span><span class="sxs-lookup"><span data-stu-id="37513-161">Name the file `fragment_home` and change the **Root element** to `RelativeLayout`.</span></span> <span data-ttu-id="37513-162">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="37513-162">Choose **OK**.</span></span>

<span data-ttu-id="37513-163">Öffnen Sie die Datei **fragment_home. XML** , und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37513-163">Open the **fragment_home.xml** file and replace its contents with the following.</span></span>

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

<span data-ttu-id="37513-164">Klicken Sie dann mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="37513-164">Next, right-click the **app/res/layout** folder and choose **New**, then **Layout resource file**.</span></span> <span data-ttu-id="37513-165">Benennen Sie die `fragment_calendar` Datei, und ändern Sie das `RelativeLayout` **Stammelement** in.</span><span class="sxs-lookup"><span data-stu-id="37513-165">Name the file `fragment_calendar` and change the **Root element** to `RelativeLayout`.</span></span> <span data-ttu-id="37513-166">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="37513-166">Choose **OK**.</span></span>

<span data-ttu-id="37513-167">Öffnen Sie die Datei **fragment_calendar. XML** , und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37513-167">Open the **fragment_calendar.xml** file and replace its contents with the following.</span></span>

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

<span data-ttu-id="37513-168">Klicken Sie nun mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie **neu**und dann **Java Class**aus.</span><span class="sxs-lookup"><span data-stu-id="37513-168">Now, right-click the **app/java/com.example.graphtutorial** folder and choose **New**, then **Java Class**.</span></span> <span data-ttu-id="37513-169">Benennen Sie die `HomeFragment` Klasse, und \*\*\*\* legen Sie `android.support.v4.app.Fragment`die Superclass auf fest.</span><span class="sxs-lookup"><span data-stu-id="37513-169">Name the class `HomeFragment` and set the **Superclass** to `android.support.v4.app.Fragment`.</span></span> <span data-ttu-id="37513-170">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="37513-170">Choose **OK**.</span></span> <span data-ttu-id="37513-171">Öffnen Sie die **HomeFragment** -Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37513-171">Open the **HomeFragment** file and replace its contents with the following.</span></span>

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

<span data-ttu-id="37513-172">Klicken Sie dann mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie **neu**und dann **Java Class**aus.</span><span class="sxs-lookup"><span data-stu-id="37513-172">Next, right-click the **app/java/com.example.graphtutorial** folder and choose **New**, then **Java Class**.</span></span> <span data-ttu-id="37513-173">Benennen Sie die `CalendarFragment` Klasse, und \*\*\*\* legen Sie `android.support.v4.app.Fragment`die Superclass auf fest.</span><span class="sxs-lookup"><span data-stu-id="37513-173">Name the class `CalendarFragment` and set the **Superclass** to `android.support.v4.app.Fragment`.</span></span> <span data-ttu-id="37513-174">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="37513-174">Choose **OK**.</span></span>

<span data-ttu-id="37513-175">Öffnen Sie die **CalendarFragment** -Datei, und fügen Sie die `CalendarFragment` folgende Funktion zur Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="37513-175">Open the **CalendarFragment** file and add the following function to the `CalendarFragment` class.</span></span>

```java
@Nullable
@Override
public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
    return inflater.inflate(R.layout.fragment_calendar, container, false);
}
```

<span data-ttu-id="37513-176">Nachdem die Fragmente implementiert wurden, aktualisieren Sie die `MainActivity` Klasse, um das `onNavigationItemSelected` Ereignis zu behandeln und die Fragmente zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="37513-176">Now that the fragments are implemented, update the `MainActivity` class to handle the `onNavigationItemSelected` event and use the fragments.</span></span> <span data-ttu-id="37513-177">Fügen Sie zunächst die folgenden Funktionen zur Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="37513-177">First, add the following functions to the class.</span></span>

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

<span data-ttu-id="37513-178">Ersetzen Sie als nächstes die `onNavigationItemSelected` vorhandene Funktion durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37513-178">Next, replace the existing `onNavigationItemSelected` function with the following.</span></span>

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

<span data-ttu-id="37513-179">Fügen Sie abschließend am Ende der `onCreate` Funktion Folgendes ein, um das StartFragment beim Starten der APP zu laden.</span><span class="sxs-lookup"><span data-stu-id="37513-179">Finally, add the following at the end of the `onCreate` function to load the home fragment when the app starts.</span></span>

```java
// Load the home fragment by default on startup
if (savedInstanceState == null) {
    openHomeFragment(mUserName);
}
```

<span data-ttu-id="37513-180">Speichern Sie alle Änderungen.</span><span class="sxs-lookup"><span data-stu-id="37513-180">Save all of your changes.</span></span> <span data-ttu-id="37513-181">Wählen Sie im Menü **Ausführen** die Option **app ausführen**aus.</span><span class="sxs-lookup"><span data-stu-id="37513-181">On the **Run** menu, choose **Run 'app'**.</span></span> <span data-ttu-id="37513-182">Das Menü der APP sollte funktionieren, um zwischen den beiden Fragmenten zu navigieren und sich zu ändern, wenn Sie auf die Schaltflächen **Anmelden** oder Abmelden tippen. \*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="37513-182">The app's menu should work to navigate between the two fragments and change when you tap the **Sign in** or **Sign out** buttons.</span></span>

![Screenshot der Anwendung](./images/welcome-page.png)