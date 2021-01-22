<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung integrieren Sie Microsoft Graph in die Anwendung. Für diese Anwendung verwenden Sie das [Microsoft Graph SDK for Java,](https://github.com/microsoftgraph/msgraph-sdk-java) um Aufrufe an Microsoft Graph zu senden.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen von Outlook

In diesem Abschnitt erweitern Sie die Klasse, um eine Funktion hinzuzufügen, um die Ereignisse des Benutzers für die aktuelle Woche zu erhalten, und aktualisieren, um diese `GraphHelper` `CalendarFragment` neuen Funktionen zu verwenden.

1. Öffnen **Sie GraphHelper,** und fügen Sie die folgenden `import` Anweisungen am Oberen Rand der Datei hinzu.

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

1. Fügen Sie der Klasse die folgenden Funktionen `GraphHelper` hinzu.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphHelper.java" id="GetEventsSnippet":::

    > [!NOTE]
    > Überlegen Sie, was der Code `getCalendarView` gerade macht.
    >
    > - Die URL, die aufgerufen wird, lautet `/v1.0/me/calendarview`.
    >   - Die `startDateTime` Parameter `endDateTime` und Abfrageparameter definieren den Anfang und das Ende der Kalenderansicht.
    >   - Der Header bewirkt, dass Microsoft Graph die Start- und Endzeiten der einzelnen Ereignisse in der Zeitzone des `Prefer: outlook.timezone` Benutzers zurück gibt.
    >   - Die `select`-Funktion beschränkt die Felder, die für jedes Ereignis zurückgegeben werden, auf nur diejenigen, die von der Ansicht tatsächlich verwendet werden.
    >   - Die `orderby` Funktion sortiert die Ergebnisse nach Startzeit.
    >   - Die `top` Funktion fordert 25 Ergebnisse pro Seite an.
    > - Ein Rückruf ist definiert ( ), um zu überprüfen, ob weitere Ergebnisse verfügbar sind, und um bei Bedarf `pagingCallback` zusätzliche Seiten an fordern.

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** aus, und klicken Sie **dann Java Klasse**. Benennen Sie die `GraphToIana` Klasse, und wählen Sie **OK aus.**

1. Öffnen Sie die neue Datei, und ersetzen Sie ihren Inhalt durch Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphToIana.java" id="GraphToIanaSnippet":::

1. Fügen Sie die `import` folgenden Anweisungen am oberen Rand der Datei **CalendarFragment** hinzu.

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

1. Fügen Sie der Klasse den folgenden Member `CalendarFragment` hinzu.

    ```java
    private List<Event> mEventList = null;
    ```

1. Fügen Sie der Klasse die folgenden Funktionen `CalendarFragment` hinzu, um die Statusleiste auszublenden und einblenden.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="ProgressBarSnippet":::

1. Fügen Sie die folgende Funktion hinzu, um einen Rückruf für die `getCalendarView` Funktion in `GraphHelper` bereitstellen.

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

1. Ersetzen Sie die `onCreateView` vorhandene Funktion in der Klasse durch `CalendarFragment` Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="OnCreateViewSnippet":::

    Beachten Sie, was dieser Code macht. Zunächst ruft sie das `acquireTokenSilently` Zugriffstoken auf. Das Aufrufen dieser Methode bei jedem Benötigten eines Zugriffstokens ist eine bewährte Methode, da sie die Zwischenspeicherungs- und Tokenaktualisierungsfähigkeiten von MSAL nutzt. Intern prüft MSAL auf ein zwischengespeichertes Token und überprüft dann, ob es abgelaufen ist. Wenn das Token vorhanden und nicht abgelaufen ist, gibt es nur das zwischengespeicherte Token zurück. Wenn es abgelaufen ist, wird versucht, das Token vor der Rückgabe zu aktualisieren.

    Nachdem das Token abgerufen wurde, ruft der Code die Methode auf, um die Ereignisse `getCalendarView` des Benutzers abzurufen.

1. Führen Sie die App aus, melden Sie sich an, und tippen Sie im Menü auf das **Navigationselement** "Kalender". Es sollte ein JSON-Dump der Ereignisse im Debugprotokoll in Android Studio angezeigt werden.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

Jetzt können Sie das JSON-Dump durch eine benutzerfreundliche Anzeige der Ergebnisse ersetzen. In diesem Abschnitt fügen Sie dem Kalenderfragment ein Layout hinzu, erstellen ein Layout für jedes Element in der und erstellen einen benutzerdefinierten Listenadapter für den, der die Felder der einzelnen Felder den entsprechenden in der Ansicht zu `ListView` `ListView` `ListView` `Event` `TextView` ordnet.

1. Ersetzen Sie `TextView` das in **app/res/layout/fragment_calendar.xml** durch ein `ListView` .

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_calendar.xml" highlight="6-11":::

1. Klicken Sie mit der rechten Maustaste auf den **Ordner "app/res/layout",** und wählen **Sie "Neu"** und dann **"Layoutressourcendatei" aus.**

1. Benennen Sie die `event_list_item` Datei, ändern Sie das **Stammelement** in `RelativeLayout` , und wählen Sie OK **aus.**

1. Öffnen Sie **event_list_item.xml** Datei, und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/event_list_item.xml":::

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** aus, und klicken Sie **dann Java Klasse**.

1. Benennen Sie die `EventListAdapter` Klasse, und wählen Sie **OK aus.**

1. Öffnen Sie die **EventListAdapter-Datei,** und ersetzen Sie ihren Inhalt durch Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/EventListAdapter.java" id="EventListAdapterSnippet":::

1. Öffnen Sie **die CalendarFragment-Klasse,** und fügen Sie der Klasse die folgende Funktion hinzu.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="AddEventsToListSnippet":::

1. Ersetzen Sie den temporären Debugcode aus der `success` Außerkraftsetzung durch `addEventsToList();` .

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="SuccessSnippet" highlight="5":::

1. Führen Sie die App aus, melden Sie sich an, und tippen Sie **auf das Navigationselement** "Kalender". Die Liste der Ereignisse sollte angezeigt werden.

    ![Ein Screenshot der Tabelle mit Ereignissen](./images/calendar-list.png)
