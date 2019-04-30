<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1b6a8-101">In dieser Übung integrieren Sie Microsoft Graph in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="1b6a8-102">Für diese Anwendung verwenden Sie das [Microsoft Graph-SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) , um Aufrufe von Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="1b6a8-103">Abrufen von Kalenderereignissen aus Outlook</span><span class="sxs-lookup"><span data-stu-id="1b6a8-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="1b6a8-104">Erweitern Sie zunächst die `GraphHelper` Klasse, um eine Funktion hinzuzufügen, um die Ereignisse des Benutzers abzurufen.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-104">Start by extending the `GraphHelper` class to add a function to get the user's events.</span></span> <span data-ttu-id="1b6a8-105">Öffnen Sie die **GraphHelper** -Datei, und `import` fügen Sie am Anfang der Datei die folgenden Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-105">Open the **GraphHelper** file and add the following `import` statements to the top of the file.</span></span>

```java
import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
import com.microsoft.graph.requests.extensions.IEventCollectionPage;
import java.util.LinkedList;
import java.util.List;
```

<span data-ttu-id="1b6a8-106">Fügen Sie der `GraphHelper` Klasse die folgenden Funktionen hinzu.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-106">Add the following functions to the `GraphHelper` class.</span></span>

```java
public void getEvents(String accessToken, ICallback<IEventCollectionPage> callback) {
    mAccessToken = accessToken;

    // Use query options to sort by created time
    final List<Option> options = new LinkedList<Option>();
    options.add(new QueryOption("orderby", "createdDateTime DESC"));


    // GET /me/events
    mClient.me().events().buildRequest(options)
            .select("subject,organizer,start,end")
            .get(callback);

}

// Debug function to get the JSON representation of a Graph
// object
public String serializeObject(Object object) {
    return mClient.getSerializer().serializeObject(object);
}
```

<span data-ttu-id="1b6a8-107">Überlegen Sie sich, was `getEvents` der Code in tut.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-107">Consider what the code in `getEvents` is doing.</span></span>

- <span data-ttu-id="1b6a8-108">Die URL, die aufgerufen wird, `/v1.0/me/events`lautet.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-108">The URL that will be called is `/v1.0/me/events`.</span></span>
- <span data-ttu-id="1b6a8-109">Die `select` Funktion schränkt die für jedes Ereignis zurückgegebenen Felder auf diejenigen ein, die die Ansicht tatsächlich verwendet.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-109">The `select` function limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="1b6a8-110">Der `QueryOption` Name `orderby` wird verwendet, um die Ergebnisse nach dem Datum und der Uhrzeit der Erstellung zu sortieren, wobei das neueste Element zuerst angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-110">The `QueryOption` named `orderby` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="1b6a8-111">Aktualisieren `CalendarFragment` Sie nun, um diese neuen Funktionen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-111">Now update `CalendarFragment` to use these new functions.</span></span> <span data-ttu-id="1b6a8-112">Fügen Sie am `import` Anfang der **CalendarFragment** -Datei die folgenden Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-112">Add the following `import` statements to the top of the **CalendarFragment** file.</span></span>

```java
import android.util.Log;
import android.widget.ListView;
import android.widget.ProgressBar;
import com.microsoft.graph.concurrency.ICallback;
import com.microsoft.graph.core.ClientException;
import com.microsoft.graph.models.extensions.Event;
import com.microsoft.graph.requests.extensions.IEventCollectionPage;
import com.microsoft.identity.client.AuthenticationCallback;
import com.microsoft.identity.client.AuthenticationResult;
import com.microsoft.identity.client.exception.MsalException;
import java.util.List;
```

<span data-ttu-id="1b6a8-113">Fügen Sie der `CalendarFragment` Klasse die folgenden Member hinzu.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-113">Add the following members to the `CalendarFragment` class.</span></span>

```java
private List<Event> mEventList = null;
private ProgressBar mProgress = null;
```

<span data-ttu-id="1b6a8-114">Fügen Sie nun der `CalendarFragment` Klasse die folgenden Funktionen hinzu, um die Statusanzeige auszublenden und anzuzeigen und um einen Rückruf für `getEvents` die Funktion `GraphHelper`in bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-114">Now add the following functions to the `CalendarFragment` class to hide and show the progress bar, and to provide a callback for the `getEvents` function in `GraphHelper`.</span></span>

```java
private void showProgressBar() {
    getActivity().runOnUiThread(new Runnable() {
        @Override
        public void run() {
            mProgress.setVisibility(View.VISIBLE);
        }
    });
}

private void hideProgressBar() {
    getActivity().runOnUiThread(new Runnable() {
        @Override
        public void run() {
            mProgress.setVisibility(View.GONE);
        }
    });
}

private ICallback<IEventCollectionPage> getEventsCallback() {
    return new ICallback<IEventCollectionPage>() {
        @Override
        public void success(IEventCollectionPage iEventCollectionPage) {
            mEventList = iEventCollectionPage.getCurrentPage();

            // Temporary for debugging
            String jsonEvents = GraphHelper.getInstance().serializeObject(mEventList);
            Log.d("GRAPH", jsonEvents);

            hideProgressBar();
        }

        @Override
        public void failure(ClientException ex) {
            Log.e("GRAPH", "Error getting events", ex);
            hideProgressBar();
        }
    };
}
```

<span data-ttu-id="1b6a8-115">Überschreiben Sie schließlich `onCreate` die-Funktion `GraphHelper` in der-Klasse, um die Ereignisse des Benutzers von Microsoft Graph abzurufen.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-115">Finally, override the `onCreate` function in the `GraphHelper` class to get the user's events from Microsoft Graph.</span></span>

```java
@Override
public void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    mProgress = getActivity().findViewById(R.id.progressbar);
    showProgressBar();

    // Get a current access token
    AuthenticationHelper.getInstance()
            .acquireTokenSilently(new AuthenticationCallback() {
                @Override
                public void onSuccess(AuthenticationResult authenticationResult) {
                    final GraphHelper graphHelper = GraphHelper.getInstance();

                    // Get the user's events
                    graphHelper.getEvents(authenticationResult.getAccessToken(),
                            getEventsCallback());
                }

                @Override
                public void onError(MsalException exception) {
                    Log.e("AUTH", "Could not get token silently", exception);
                    hideProgressBar();
                }

                @Override
                public void onCancel() {
                    hideProgressBar();
                }
            });
}
```

<span data-ttu-id="1b6a8-116">Beachten Sie diesen Code.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-116">Notice what this code does.</span></span> <span data-ttu-id="1b6a8-117">Zuerst wird aufgerufen `acquireTokenSilently` , um das Zugriffstoken abzurufen.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-117">First, it calls `acquireTokenSilently` to get the access token.</span></span> <span data-ttu-id="1b6a8-118">Das Aufrufen dieser Methode jedes Mal, wenn ein Zugriffstoken benötigt wird, ist eine bewährte Vorgehensweise, da es die MSAL-und Token-Refresh-Fähigkeiten nutzt.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-118">Calling this method every time an access token is needed is a best practice because it takes advantage of MSAL's caching and token refresh abilities.</span></span> <span data-ttu-id="1b6a8-119">Intern prüft MSAL auf ein zwischengespeichertes Token und überprüft dann, ob es abgelaufen ist.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-119">Internally, MSAL checks for a cached token, then checks if it is expired.</span></span> <span data-ttu-id="1b6a8-120">Wenn das Token vorhanden und nicht abgelaufen ist, wird nur das zwischengespeicherte Token zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-120">If the token is present and not expired, it just returns the cached token.</span></span> <span data-ttu-id="1b6a8-121">Wenn es abgelaufen ist, versucht es, das Token zu aktualisieren, bevor es zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-121">If it is expired, it attempts to refresh the token before returning it.</span></span>

<span data-ttu-id="1b6a8-122">Sobald das Token abgerufen wurde, ruft der Code die `getEvents` -Methode auf, um die Ereignisse des Benutzers abzurufen.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-122">Once the token is retrieved, the code then calls the `getEvents` method to get the user's events.</span></span>

<span data-ttu-id="1b6a8-123">Sie können jetzt die app ausführen, sich anmelden und auf das Navigationselement **Kalender** im Menü tippen.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-123">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="1b6a8-124">Es sollte ein JSON-Dump der Ereignisse im Debug-Protokoll in Android Studio angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-124">You should see a JSON dump of the events in the debug log in Android Studio.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="1b6a8-125">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="1b6a8-125">Display the results</span></span>

<span data-ttu-id="1b6a8-126">Jetzt können Sie den JSON-Dump durch einen anderen ersetzen, um die Ergebnisse auf benutzerfreundliche Weise anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-126">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="1b6a8-127">Ersetzen Sie zunächst den `TextView` in **App/res/Layout/fragment_calendar. XML** durch einen `ListView`.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-127">Start by replacing the `TextView` in **app/res/layout/fragment_calendar.xml** with a `ListView`.</span></span>

```xml
<ListView
    android:id="@+id/eventlist"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:divider="@color/colorPrimary"
    android:dividerHeight="1dp" />
```

<span data-ttu-id="1b6a8-128">Erstellen Sie als nächstes ein Layout für jedes Element in `ListView`der.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-128">Next, create a layout for each item in the `ListView`.</span></span> <span data-ttu-id="1b6a8-129">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-129">Right-click the **app/res/layout** folder and choose **New**, then **Layout resource file**.</span></span> <span data-ttu-id="1b6a8-130">Benennen Sie die `event_list_item`Datei, ändern Sie das **Stammelement** in, und klicken Sie auf `RelativeLayout` **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-130">Name the file `event_list_item`, change the **Root element** to `RelativeLayout`, and choose **OK**.</span></span> <span data-ttu-id="1b6a8-131">Öffnen Sie die Datei **event_list_item. XML** , und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-131">Open the **event_list_item.xml** file and replace its contents with the following.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">

    <TextView
        android:id="@+id/eventsubject"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Subject"
        android:textSize="20sp" />

    <TextView
        android:id="@+id/eventorganizer"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/eventsubject"
        android:text="Adele Vance"
        android:textSize="15sp" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/eventorganizer"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingEnd="2sp"
            android:text="Start:"
            android:textSize="15sp"
            android:textStyle="bold" />

        <TextView
            android:id="@+id/eventstart"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="1:30 PM 2/19/2019"
            android:textSize="15sp" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingStart="5sp"
            android:paddingEnd="2sp"
            android:text="End:"
            android:textSize="15sp"
            android:textStyle="bold" />

        <TextView
            android:id="@+id/eventend"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="1:30 PM 2/19/2019"
            android:textSize="15sp" />
    </LinearLayout>
</RelativeLayout>
```

<span data-ttu-id="1b6a8-132">Erstellen Sie jetzt einen benutzerdefinierten Listen Adapter `ListView`für.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-132">Now create a custom list adapter for the `ListView`.</span></span> <span data-ttu-id="1b6a8-133">Dies ist erforderlich, um die Ansichten für die Elemente in der Liste zu erstellen und die Felder der einzelnen `Event` der entsprechenden `TextView` in der Ansicht zuordnen.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-133">This is necessary to create the views for the items in the list, and to map the fields of each `Event` to the appropriate `TextView` in the view.</span></span>

<span data-ttu-id="1b6a8-134">Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie **neu**und dann **Java Class**aus.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-134">Right-click the **app/java/com.example.graphtutorial** folder and choose **New**, then **Java Class**.</span></span> <span data-ttu-id="1b6a8-135">Benennen Sie die `EventListAdapter` Klasse, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-135">Name the class `EventListAdapter` and choose **OK**.</span></span> <span data-ttu-id="1b6a8-136">Öffnen Sie die **EventListAdapter** -Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-136">Open the **EventListAdapter** file and replace its contents with the following.</span></span>

```java
package com.example.graphtutorial;

import android.content.Context;
import android.support.annotation.NonNull;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.TextView;

import com.microsoft.graph.models.extensions.DateTimeTimeZone;
import com.microsoft.graph.models.extensions.Event;

import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.util.List;
import java.util.TimeZone;

public class EventListAdapter extends ArrayAdapter<Event> {
    private Context mContext;
    private int mResource;
    private ZoneId mLocalTimeZoneId;

    // Used for the ViewHolder pattern
    // https://developer.android.com/training/improving-layouts/smooth-scrolling
    static class ViewHolder {
        TextView subject;
        TextView organizer;
        TextView start;
        TextView end;
    }

    public EventListAdapter(Context context, int resource, List<Event> events) {
        super(context, resource, events);
        mContext = context;
        mResource = resource;
        mLocalTimeZoneId = TimeZone.getDefault().toZoneId();
    }

    @NonNull
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        Event event = getItem(position);

        ViewHolder holder;

        if (convertView == null) {
            LayoutInflater inflater = LayoutInflater.from(mContext);
            convertView = inflater.inflate(mResource, parent, false);

            holder = new ViewHolder();
            holder.subject = convertView.findViewById(R.id.eventsubject);
            holder.organizer = convertView.findViewById(R.id.eventorganizer);
            holder.start = convertView.findViewById(R.id.eventstart);
            holder.end = convertView.findViewById(R.id.eventend);

            convertView.setTag(holder);
        } else {
            holder = (ViewHolder) convertView.getTag();
        }

        holder.subject.setText(event.subject);
        holder.organizer.setText(event.organizer.emailAddress.name);
        holder.start.setText(getLocalDateTimeString(event.start));
        holder.end.setText(getLocalDateTimeString(event.end));

        return convertView;
    }

    // Convert Graph's DateTimeTimeZone format to
    // a LocalDateTime, then return a formatted string
    private String getLocalDateTimeString(DateTimeTimeZone dateTime) {
        ZonedDateTime localDateTime = LocalDateTime.parse(dateTime.dateTime)
                .atZone(ZoneId.of(dateTime.timeZone))
                .withZoneSameInstant(mLocalTimeZoneId);

        return String.format("%s %s",
                localDateTime.format(DateTimeFormatter.ofLocalizedDate(FormatStyle.MEDIUM)),
                localDateTime.format(DateTimeFormatter.ofLocalizedTime(FormatStyle.SHORT)));
    }
}
```

<span data-ttu-id="1b6a8-137">Aktualisieren Sie schließlich die `CalendarFragment` Klasse, um die `EventListAdapter` zum Initialisieren der `ListView`zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-137">Finally, update the `CalendarFragment` class to use the `EventListAdapter` to initialize the `ListView`.</span></span> <span data-ttu-id="1b6a8-138">Öffnen Sie die **CalendarFragment** -Klasse, und fügen Sie die folgende Funktion zur-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-138">Open the **CalendarFragment** class and add the following function to the class.</span></span>

```java
private void addEventsToList() {
    getActivity().runOnUiThread(new Runnable() {
        @Override
        public void run() {
            ListView eventListView = getView().findViewById(R.id.eventlist);

            EventListAdapter listAdapter = new EventListAdapter(getActivity(),
                    R.layout.event_list_item, mEventList);

            eventListView.setAdapter(listAdapter);
        }
    });
}
```

<span data-ttu-id="1b6a8-139">Fügen Sie die folgende Codezeile in der `success` Außerkraftsetzung nach `mEventList = iEventCollectionPage.getCurrentPage();` der Leitung hinzu.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-139">Add the following line of code in the `success` override after the `mEventList = iEventCollectionPage.getCurrentPage();` line.</span></span>

```java
addEventsToList();
```

<span data-ttu-id="1b6a8-140">Führen Sie die APP aus, melden Sie sich an, und tippen Sie auf das Navigationselement **Kalender** .</span><span class="sxs-lookup"><span data-stu-id="1b6a8-140">Run the app, sign in, and tap the **Calendar** navigation item.</span></span> <span data-ttu-id="1b6a8-141">Die Liste der Ereignisse sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-141">You should see the list of events.</span></span>

![Screenshot der Ereignistabelle](./images/calendar-list.png)
