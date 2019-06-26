<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3c9cd-101">In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="3c9cd-102">Für diese Anwendung verwenden Sie das [Microsoft Graph-SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) , um Anrufe an Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="3c9cd-103">Abrufen von Kalenderereignissen aus Outlook</span><span class="sxs-lookup"><span data-stu-id="3c9cd-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="3c9cd-104">In diesem Abschnitt erweitern Sie die `GraphHelper` Klasse so, dass eine Funktion hinzugefügt wird, um die Ereignisse `CalendarFragment` des Benutzers abzurufen und diese neuen Funktionen zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-104">In this section you will extend the `GraphHelper` class to add a function to get the user's events and update `CalendarFragment` to use these new functions.</span></span>

1. <span data-ttu-id="3c9cd-105">Öffnen Sie die Datei **GraphHelper** , und fügen `import` Sie die folgenden Anweisungen am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-105">Open the **GraphHelper** file and add the following `import` statements to the top of the file.</span></span>

    ```java
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import java.util.LinkedList;
    import java.util.List;
    ```

1. <span data-ttu-id="3c9cd-106">Fügen Sie der `GraphHelper` -Klasse die folgenden Funktionen hinzu.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-106">Add the following functions to the `GraphHelper` class.</span></span>

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

    > [!NOTE]
    > <span data-ttu-id="3c9cd-107">Überprüfen Sie, was `getEvents` der Code in tut.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-107">Consider what the code in `getEvents` is doing.</span></span>
    >
    > - <span data-ttu-id="3c9cd-108">Die URL, die aufgerufen wird `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-108">The URL that will be called is `/v1.0/me/events`.</span></span>
    > - <span data-ttu-id="3c9cd-109">Die `select` Funktion schränkt die für die einzelnen Ereignisse zurückgegebenen Felder auf nur #a0 ein, die von der Ansicht tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-109">The `select` function limits the fields returned for each events to just > those the view will actually use.</span></span>
    > - <span data-ttu-id="3c9cd-110">Der `QueryOption` Name `orderby` wird verwendet, um die Ergebnisse nach dem Datum und der Uhrzeit der Erstellung zu sortieren, wobei das neueste Element zuerst angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-110">The `QueryOption` named `orderby` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="3c9cd-111">Fügen Sie die `import` folgenden Anweisungen am Anfang der **CalendarFragment** -Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-111">Add the following `import` statements to the top of the **CalendarFragment** file.</span></span>

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

1. <span data-ttu-id="3c9cd-112">Fügen Sie der `CalendarFragment` Klasse die folgenden Member hinzu.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-112">Add the following members to the `CalendarFragment` class.</span></span>

    ```java
    private List<Event> mEventList = null;
    private ProgressBar mProgress = null;
    ```

1. <span data-ttu-id="3c9cd-113">Fügen Sie der `CalendarFragment` Klasse die folgenden Funktionen hinzu, um die Statusleiste auszublenden und anzuzeigen, und um einen Rückruf `getEvents` für die `GraphHelper`Funktion in bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-113">Add the following functions to the `CalendarFragment` class to hide and show the progress bar, and to provide a callback for the `getEvents` function in `GraphHelper`.</span></span>

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

1. <span data-ttu-id="3c9cd-114">Überschreiben `onCreate` Sie die- `GraphHelper` Funktion in der-Klasse, um die Ereignisse des Benutzers aus Microsoft Graph abzurufen.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-114">Override the `onCreate` function in the `GraphHelper` class to get the user's events from Microsoft Graph.</span></span>

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

<span data-ttu-id="3c9cd-115">Beachten Sie, was dieser Code tut.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-115">Notice what this code does.</span></span> <span data-ttu-id="3c9cd-116">Zunächst wird aufgerufen `acquireTokenSilently` , um das Zugriffstoken abzurufen.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-116">First, it calls `acquireTokenSilently` to get the access token.</span></span> <span data-ttu-id="3c9cd-117">Das Aufrufen dieser Methode jedes Mal, wenn ein Zugriffstoken benötigt wird, ist eine bewährte Vorgehensweise, da Sie die Möglichkeiten zum Zwischenspeichern von MSAL und zum Aktualisieren von Tokens nutzt.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-117">Calling this method every time an access token is needed is a best practice because it takes advantage of MSAL's caching and token refresh abilities.</span></span> <span data-ttu-id="3c9cd-118">Intern überprüft MSAL ein zwischengespeichertes Token und überprüft dann, ob es abgelaufen ist.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-118">Internally, MSAL checks for a cached token, then checks if it is expired.</span></span> <span data-ttu-id="3c9cd-119">Wenn das Token vorhanden und nicht abgelaufen ist, wird nur das zwischengespeicherte Token zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-119">If the token is present and not expired, it just returns the cached token.</span></span> <span data-ttu-id="3c9cd-120">Wenn er abgelaufen ist, wird versucht, das Token zu aktualisieren, bevor es zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-120">If it is expired, it attempts to refresh the token before returning it.</span></span>

<span data-ttu-id="3c9cd-121">Sobald das Token abgerufen wurde, ruft der Code die `getEvents` -Methode auf, um die Ereignisse des Benutzers abzurufen.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-121">Once the token is retrieved, the code then calls the `getEvents` method to get the user's events.</span></span>

<span data-ttu-id="3c9cd-122">Sie können nun die app ausführen, sich anmelden und auf das Navigationselement **Kalender** im Menü tippen.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-122">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="3c9cd-123">Es sollte ein JSON-Dump der Ereignisse im Debug-Protokoll in Android Studio angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-123">You should see a JSON dump of the events in the debug log in Android Studio.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="3c9cd-124">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="3c9cd-124">Display the results</span></span>

<span data-ttu-id="3c9cd-125">Nun können Sie das JSON-Dump durch etwas ersetzen, um die Ergebnisse auf eine benutzerfreundliche Weise anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-125">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="3c9cd-126">In `ListView` diesem Abschnitt fügen Sie ein zum Kalender Fragment hinzu, erstellen ein Layout für jedes Element in der `ListView`und erstellen einen benutzerdefinierten Listen Adapter für den `ListView` , der die entsprechenden `Event` `TextView` Felder in der Ansicht zuordnet.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-126">In this section, you will add a `ListView` to the calendar fragment, create a layout for each item in the `ListView`, and create a custom list adapter for the `ListView` that maps the fields of each `Event` to the appropriate `TextView` in the view.</span></span>

1. <span data-ttu-id="3c9cd-127">Ersetzen Sie `TextView` die in **App/res/Layout/fragment_calendar. XML** durch `ListView`eine.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-127">Replace the `TextView` in **app/res/layout/fragment_calendar.xml** with a `ListView`.</span></span>

    ```xml
    <ListView
        android:id="@+id/eventlist"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:divider="@color/colorPrimary"
        android:dividerHeight="1dp" />
    ```

1. <span data-ttu-id="3c9cd-128">Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-128">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="3c9cd-129">Nennen Sie die `event_list_item`Datei, ändern Sie das **Stammelement** in `RelativeLayout`, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-129">Name the file `event_list_item`, change the **Root element** to `RelativeLayout`, and select **OK**.</span></span>

1. <span data-ttu-id="3c9cd-130">Öffnen Sie die Datei **event_list_item. XML** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-130">Open the **event_list_item.xml** file and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="3c9cd-131">Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie dann **neu**und dann **Java-Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-131">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="3c9cd-132">Nennen Sie die `EventListAdapter` Klasse, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-132">Name the class `EventListAdapter` and select **OK**.</span></span>

1. <span data-ttu-id="3c9cd-133">Öffnen Sie die Datei **EventListAdapter** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-133">Open the **EventListAdapter** file and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="3c9cd-134">Öffnen Sie die **CalendarFragment** -Klasse, und fügen Sie der-Klasse die folgende Funktion hinzu.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-134">Open the **CalendarFragment** class and add the following function to the class.</span></span>

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

1. <span data-ttu-id="3c9cd-135">Fügen Sie die folgende Codezeile in der `success` Außerkraftsetzung nach `mEventList = iEventCollectionPage.getCurrentPage();` der-Verbindung hinzu.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-135">Add the following line of code in the `success` override after the `mEventList = iEventCollectionPage.getCurrentPage();` line.</span></span>

    ```java
    addEventsToList();
    ```

1. <span data-ttu-id="3c9cd-136">Führen Sie die APP aus, melden Sie sich an, und tippen Sie auf das Navigationselement **Kalender** .</span><span class="sxs-lookup"><span data-stu-id="3c9cd-136">Run the app, sign in, and tap the **Calendar** navigation item.</span></span> <span data-ttu-id="3c9cd-137">Die Liste der Ereignisse sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="3c9cd-137">You should see the list of events.</span></span>

    ![Ein Screenshot der Ereignistabelle](./images/calendar-list.png)
