<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren. Für diese Anwendung verwenden Sie das [Microsoft Graph-SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) , um Anrufe an Microsoft Graph zu tätigen.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen aus Outlook

In diesem Abschnitt erweitern Sie die `GraphHelper` Klasse so, dass eine Funktion hinzugefügt wird, um die Ereignisse `CalendarFragment` des Benutzers abzurufen und diese neuen Funktionen zu aktualisieren.

1. Öffnen Sie die Datei **GraphHelper** , und fügen `import` Sie die folgenden Anweisungen am Anfang der Datei hinzu.

    ```java
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import java.util.LinkedList;
    import java.util.List;
    ```

1. Fügen Sie der `GraphHelper` -Klasse die folgenden Funktionen hinzu.

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
    > Überprüfen Sie, was `getEvents` der Code in tut.
    >
    > - Die URL, die aufgerufen wird `/v1.0/me/events`.
    > - Die `select` -Funktion schränkt die für die einzelnen Ereignisse zurückgegebenen Felder auf genau diejenigen ein, die die Ansicht tatsächlich verwendet wird.
    > - Der `QueryOption` Name `orderby` wird verwendet, um die Ergebnisse nach dem Datum und der Uhrzeit der Erstellung zu sortieren, wobei das neueste Element zuerst angezeigt wird.

1. Fügen Sie die `import` folgenden Anweisungen am Anfang der **CalendarFragment** -Datei hinzu.

    ```java
    import android.util.Log;
    import android.widget.ListView;
    import android.widget.ProgressBar;
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalException;
    import java.util.List;
    ```

1. Fügen Sie der `CalendarFragment` Klasse die folgenden Member hinzu.

    ```java
    private List<Event> mEventList = null;
    private ProgressBar mProgress = null;
    ```

1. Fügen Sie der `CalendarFragment` Klasse die folgenden Funktionen hinzu, um die Statusleiste auszublenden und anzuzeigen, und um einen Rückruf `getEvents` für die `GraphHelper`Funktion in bereitzustellen.

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

1. Überschreiben `onCreate` Sie die- `CalendarFragment` Funktion in der-Klasse, um die Ereignisse des Benutzers aus Microsoft Graph abzurufen.

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
                    public void onSuccess(IAuthenticationResult authenticationResult) {
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

Beachten Sie, was dieser Code tut. Zunächst wird aufgerufen `acquireTokenSilently` , um das Zugriffstoken abzurufen. Das Aufrufen dieser Methode jedes Mal, wenn ein Zugriffstoken benötigt wird, ist eine bewährte Vorgehensweise, da Sie die Möglichkeiten zum Zwischenspeichern von MSAL und zum Aktualisieren von Tokens nutzt. Intern überprüft MSAL ein zwischengespeichertes Token und überprüft dann, ob es abgelaufen ist. Wenn das Token vorhanden und nicht abgelaufen ist, wird nur das zwischengespeicherte Token zurückgegeben. Wenn er abgelaufen ist, wird versucht, das Token zu aktualisieren, bevor es zurückgegeben wird.

Sobald das Token abgerufen wurde, ruft der Code die `getEvents` -Methode auf, um die Ereignisse des Benutzers abzurufen.

Sie können nun die app ausführen, sich anmelden und auf das Navigationselement **Kalender** im Menü tippen. Es sollte ein JSON-Dump der Ereignisse im Debug-Protokoll in Android Studio angezeigt werden.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

Nun können Sie das JSON-Dump durch etwas ersetzen, um die Ergebnisse auf eine benutzerfreundliche Weise anzuzeigen. In `ListView` diesem Abschnitt fügen Sie ein zum Kalender Fragment hinzu, erstellen ein Layout für jedes Element in der `ListView`und erstellen einen benutzerdefinierten Listen Adapter für den `ListView` , der die entsprechenden `Event` `TextView` Felder in der Ansicht zuordnet.

1. Ersetzen Sie `TextView` die in **App/res/Layout/fragment_calendar. XML** durch `ListView`eine.

    ```xml
    <ListView
        android:id="@+id/eventlist"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:divider="@color/colorPrimary"
        android:dividerHeight="1dp" />
    ```

1. Klicken Sie mit der rechten Maustaste auf den Ordner **App/res/Layout** , und wählen Sie **neu**und dann **Layout-Ressourcendatei**aus.

1. Nennen Sie die `event_list_item`Datei, ändern Sie das **Stammelement** in `RelativeLayout`, und wählen Sie **OK**aus.

1. Öffnen Sie die Datei **event_list_item. XML** , und ersetzen Sie den Inhalt durch Folgendes.

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

1. Klicken Sie mit der rechten Maustaste auf den Ordner **App/Java/com. example. graphtutorial** , und wählen Sie dann **neu**und dann **Java-Klasse**aus.

1. Nennen Sie die `EventListAdapter` Klasse, und wählen Sie **OK**aus.

1. Öffnen Sie die Datei **EventListAdapter** , und ersetzen Sie den Inhalt durch Folgendes.

    ```java
    package com.example.graphtutorial;

    import android.content.Context;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import android.widget.ArrayAdapter;
    import android.widget.TextView;
    import androidx.annotation.NonNull;
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

1. Öffnen Sie die **CalendarFragment** -Klasse, und fügen Sie der-Klasse die folgende Funktion hinzu.

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

1. Fügen Sie die folgende Codezeile in der `success` Außerkraftsetzung nach `mEventList = iEventCollectionPage.getCurrentPage();` der-Verbindung hinzu.

    ```java
    addEventsToList();
    ```

1. Führen Sie die APP aus, melden Sie sich an, und tippen Sie auf das Navigationselement **Kalender** . Die Liste der Ereignisse sollte angezeigt werden.

    ![Ein Screenshot der Ereignistabelle](./images/calendar-list.png)
