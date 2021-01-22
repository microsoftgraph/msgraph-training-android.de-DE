<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2bbcb-101">In dieser Übung integrieren Sie Microsoft Graph in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="2bbcb-102">Für diese Anwendung verwenden Sie das [Microsoft Graph SDK for Java,](https://github.com/microsoftgraph/msgraph-sdk-java) um Aufrufe an Microsoft Graph zu senden.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="2bbcb-103">Abrufen von Kalenderereignissen von Outlook</span><span class="sxs-lookup"><span data-stu-id="2bbcb-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="2bbcb-104">In diesem Abschnitt erweitern Sie die Klasse, um eine Funktion hinzuzufügen, um die Ereignisse des Benutzers für die aktuelle Woche zu erhalten, und aktualisieren, um diese `GraphHelper` `CalendarFragment` neuen Funktionen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-104">In this section you will extend the `GraphHelper` class to add a function to get the user's events for the current week and update `CalendarFragment` to use these new functions.</span></span>

1. <span data-ttu-id="2bbcb-105">Öffnen **Sie GraphHelper,** und fügen Sie die folgenden `import` Anweisungen am Oberen Rand der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-105">Open **GraphHelper** and add the following `import` statements to the top of the file.</span></span>

    ```java
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import com.microsoft.graph.requests.extensions.IEventCollectionRequestBuilder;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    ```

1. <span data-ttu-id="2bbcb-106">Fügen Sie der Klasse die folgenden Funktionen `GraphHelper` hinzu.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-106">Add the following functions to the `GraphHelper` class.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphHelper.java" id="GetEventsSnippet":::

    > [!NOTE]
    > <span data-ttu-id="2bbcb-107">Überlegen Sie, was der Code `getCalendarView` gerade macht.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-107">Consider what the code in `getCalendarView` is doing.</span></span>
    >
    > - <span data-ttu-id="2bbcb-108">Die URL, die aufgerufen wird, lautet `/v1.0/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-108">The URL that will be called is `/v1.0/me/calendarview`.</span></span>
    >   - <span data-ttu-id="2bbcb-109">Die `startDateTime` Parameter `endDateTime` und Abfrageparameter definieren den Anfang und das Ende der Kalenderansicht.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-109">The `startDateTime` and `endDateTime` query parameters define the start and end of the calendar view.</span></span>
    >   - <span data-ttu-id="2bbcb-110">Der Header bewirkt, dass Microsoft Graph die Start- und Endzeiten der einzelnen Ereignisse in der Zeitzone des `Prefer: outlook.timezone` Benutzers zurück gibt.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-110">the `Prefer: outlook.timezone` header causes the Microsoft Graph to return the start and end times of each event in the user's time zone.</span></span>
    >   - <span data-ttu-id="2bbcb-111">Die `select`-Funktion beschränkt die Felder, die für jedes Ereignis zurückgegeben werden, auf nur diejenigen, die von der Ansicht tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-111">The `select` function limits the fields returned for each events to just those the view will actually use.</span></span>
    >   - <span data-ttu-id="2bbcb-112">Die `orderby` Funktion sortiert die Ergebnisse nach Startzeit.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-112">The `orderby` function sorts the results by start time.</span></span>
    >   - <span data-ttu-id="2bbcb-113">Die `top` Funktion fordert 25 Ergebnisse pro Seite an.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-113">The `top` function requests 25 results per page.</span></span>
    > - <span data-ttu-id="2bbcb-114">Ein Rückruf ist definiert ( ), um zu überprüfen, ob weitere Ergebnisse verfügbar sind, und um bei Bedarf `pagingCallback` zusätzliche Seiten an fordern.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-114">A callback is defined (`pagingCallback`) to check if there are more results available and to request additional pages if needed.</span></span>

1. <span data-ttu-id="2bbcb-115">Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** aus, und klicken Sie **dann Java Klasse**.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-115">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="2bbcb-116">Benennen Sie die `GraphToIana` Klasse, und wählen Sie **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="2bbcb-116">Name the class `GraphToIana` and select **OK**.</span></span>

1. <span data-ttu-id="2bbcb-117">Öffnen Sie die neue Datei, und ersetzen Sie ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-117">Open the new file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphToIana.java" id="GraphToIanaSnippet":::

1. <span data-ttu-id="2bbcb-118">Fügen Sie die `import` folgenden Anweisungen am oberen Rand der Datei **CalendarFragment** hinzu.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-118">Add the following `import` statements to the top of the **CalendarFragment** file.</span></span>

    ```java
    import android.util.Log;
    import android.widget.ListView;
    import com.google.android.material.snackbar.BaseTransientBottomBar;
    import com.google.android.material.snackbar.Snackbar;
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalException;
    import java.time.DayOfWeek;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    import java.time.temporal.ChronoUnit;
    import java.time.temporal.TemporalAdjusters;
    import java.util.List;
    ```

1. <span data-ttu-id="2bbcb-119">Fügen Sie der Klasse den folgenden Member `CalendarFragment` hinzu.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-119">Add the following member to the `CalendarFragment` class.</span></span>

    ```java
    private List<Event> mEventList = null;
    ```

1. <span data-ttu-id="2bbcb-120">Fügen Sie der Klasse die folgenden Funktionen `CalendarFragment` hinzu, um die Statusleiste auszublenden und einblenden.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-120">Add the following functions to the `CalendarFragment` class to hide and show the progress bar.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="ProgressBarSnippet":::

1. <span data-ttu-id="2bbcb-121">Fügen Sie die folgende Funktion hinzu, um einen Rückruf für die `getCalendarView` Funktion in `GraphHelper` bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-121">Add the following function to provide a callback for the `getCalendarView` function in `GraphHelper`.</span></span>

    ```java
    private ICallback<List<Event>> getCalendarViewCallback() {
        return new ICallback<List<Event>>() {
            @Override
            public void success(List<Event> eventList) {
                mEventList = eventList;

                // Temporary for debugging
                String jsonEvents = GraphHelper.getInstance().serializeObject(mEventList);
                Log.d("GRAPH", jsonEvents);

                hideProgressBar();
            }

            @Override
            public void failure(ClientException ex) {
                hideProgressBar();
                Log.e("GRAPH", "Error getting events", ex);
                Snackbar.make(getView(),
                    ex.getMessage(),
                    BaseTransientBottomBar.LENGTH_LONG).show();
            }
        };
    }
    ```

1. <span data-ttu-id="2bbcb-122">Ersetzen Sie die `onCreateView` vorhandene Funktion in der Klasse durch `CalendarFragment` Folgendes.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-122">Replace the existing `onCreateView` function in the `CalendarFragment` class with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="OnCreateViewSnippet":::

    <span data-ttu-id="2bbcb-123">Beachten Sie, was dieser Code macht.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-123">Notice what this code does.</span></span> <span data-ttu-id="2bbcb-124">Zunächst ruft sie das `acquireTokenSilently` Zugriffstoken auf.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-124">First, it calls `acquireTokenSilently` to get the access token.</span></span> <span data-ttu-id="2bbcb-125">Das Aufrufen dieser Methode bei jedem Benötigten eines Zugriffstokens ist eine bewährte Methode, da sie die Zwischenspeicherungs- und Tokenaktualisierungsfähigkeiten von MSAL nutzt.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-125">Calling this method every time an access token is needed is a best practice because it takes advantage of MSAL's caching and token refresh abilities.</span></span> <span data-ttu-id="2bbcb-126">Intern prüft MSAL auf ein zwischengespeichertes Token und überprüft dann, ob es abgelaufen ist.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-126">Internally, MSAL checks for a cached token, then checks if it is expired.</span></span> <span data-ttu-id="2bbcb-127">Wenn das Token vorhanden und nicht abgelaufen ist, gibt es nur das zwischengespeicherte Token zurück.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-127">If the token is present and not expired, it just returns the cached token.</span></span> <span data-ttu-id="2bbcb-128">Wenn es abgelaufen ist, wird versucht, das Token vor der Rückgabe zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-128">If it is expired, it attempts to refresh the token before returning it.</span></span>

    <span data-ttu-id="2bbcb-129">Nachdem das Token abgerufen wurde, ruft der Code die Methode auf, um die Ereignisse `getCalendarView` des Benutzers abzurufen.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-129">Once the token is retrieved, the code then calls the `getCalendarView` method to get the user's events.</span></span>

1. <span data-ttu-id="2bbcb-130">Führen Sie die App aus, melden Sie sich an, und tippen Sie im Menü auf das **Navigationselement** "Kalender".</span><span class="sxs-lookup"><span data-stu-id="2bbcb-130">Run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="2bbcb-131">Es sollte ein JSON-Dump der Ereignisse im Debugprotokoll in Android Studio angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-131">You should see a JSON dump of the events in the debug log in Android Studio.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="2bbcb-132">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="2bbcb-132">Display the results</span></span>

<span data-ttu-id="2bbcb-133">Jetzt können Sie das JSON-Dump durch eine benutzerfreundliche Anzeige der Ergebnisse ersetzen.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-133">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="2bbcb-134">In diesem Abschnitt fügen Sie dem Kalenderfragment ein Layout hinzu, erstellen ein Layout für jedes Element in der und erstellen einen benutzerdefinierten Listenadapter für den, der die Felder der einzelnen Felder den entsprechenden in der Ansicht zu `ListView` `ListView` `ListView` `Event` `TextView` ordnet.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-134">In this section, you will add a `ListView` to the calendar fragment, create a layout for each item in the `ListView`, and create a custom list adapter for the `ListView` that maps the fields of each `Event` to the appropriate `TextView` in the view.</span></span>

1. <span data-ttu-id="2bbcb-135">Ersetzen Sie `TextView` das in **app/res/layout/fragment_calendar.xml** durch ein `ListView` .</span><span class="sxs-lookup"><span data-stu-id="2bbcb-135">Replace the `TextView` in **app/res/layout/fragment_calendar.xml** with a `ListView`.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_calendar.xml" highlight="6-11":::

1. <span data-ttu-id="2bbcb-136">Klicken Sie mit der rechten Maustaste auf den **Ordner "app/res/layout",** und wählen **Sie "Neu"** und dann **"Layoutressourcendatei" aus.**</span><span class="sxs-lookup"><span data-stu-id="2bbcb-136">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="2bbcb-137">Benennen Sie die `event_list_item` Datei, ändern Sie das **Stammelement** in `RelativeLayout` , und wählen Sie OK **aus.**</span><span class="sxs-lookup"><span data-stu-id="2bbcb-137">Name the file `event_list_item`, change the **Root element** to `RelativeLayout`, and select **OK**.</span></span>

1. <span data-ttu-id="2bbcb-138">Öffnen Sie **event_list_item.xml** Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-138">Open the **event_list_item.xml** file and replace its contents with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/event_list_item.xml":::

1. <span data-ttu-id="2bbcb-139">Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** aus, und klicken Sie **dann Java Klasse**.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-139">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="2bbcb-140">Benennen Sie die `EventListAdapter` Klasse, und wählen Sie **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="2bbcb-140">Name the class `EventListAdapter` and select **OK**.</span></span>

1. <span data-ttu-id="2bbcb-141">Öffnen Sie die **EventListAdapter-Datei,** und ersetzen Sie ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-141">Open the **EventListAdapter** file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/EventListAdapter.java" id="EventListAdapterSnippet":::

1. <span data-ttu-id="2bbcb-142">Öffnen Sie **die CalendarFragment-Klasse,** und fügen Sie der Klasse die folgende Funktion hinzu.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-142">Open the **CalendarFragment** class and add the following function to the class.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="AddEventsToListSnippet":::

1. <span data-ttu-id="2bbcb-143">Ersetzen Sie den temporären Debugcode aus der `success` Außerkraftsetzung durch `addEventsToList();` .</span><span class="sxs-lookup"><span data-stu-id="2bbcb-143">Replace the temporary debugging code from the `success` override with `addEventsToList();`.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="SuccessSnippet" highlight="5":::

1. <span data-ttu-id="2bbcb-144">Führen Sie die App aus, melden Sie sich an, und tippen Sie **auf das Navigationselement** "Kalender".</span><span class="sxs-lookup"><span data-stu-id="2bbcb-144">Run the app, sign in, and tap the **Calendar** navigation item.</span></span> <span data-ttu-id="2bbcb-145">Die Liste der Ereignisse sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2bbcb-145">You should see the list of events.</span></span>

    ![Ein Screenshot der Tabelle mit Ereignissen](./images/calendar-list.png)
