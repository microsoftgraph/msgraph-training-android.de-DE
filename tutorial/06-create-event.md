<!-- markdownlint-disable MD002 MD041 -->

In diesem Abschnitt fügen Sie die Möglichkeit zum Erstellen von Ereignissen im Kalender des Benutzers hinzu.

1. Öffnen **Sie GraphHelper,** und fügen Sie die folgenden `import` Anweisungen am Oberen Rand der Datei hinzu.

    ```java
    import com.microsoft.graph.models.extensions.Attendee;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.EmailAddress;
    import com.microsoft.graph.models.extensions.ItemBody;
    import com.microsoft.graph.models.generated.AttendeeType;
    import com.microsoft.graph.models.generated.BodyType;
    ```

1. Fügen Sie der Klasse die folgende Funktion `GraphHelper` hinzu, um ein neues Ereignis zu erstellen.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphHelper.java" id="CreateEventSnippet":::

## <a name="update-new-event-fragment"></a>Aktualisieren eines neuen Ereignisfragments

1. Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.** Benennen Sie die `EditTextDateTimePicker` Klasse, und wählen Sie **OK aus.**

1. Öffnen Sie die neue Datei, und ersetzen Sie ihren Inhalt durch Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/EditTextDateTimePicker.java" id="DateTimePickerSnippet":::

    Diese Klasse umschließt ein Steuerelement, zeigt eine Datums- und Uhrzeitauswahl an, wenn der Benutzer darauf tippt, und aktualisiert den Wert mit dem datums- und `EditText` uhrzeitauswahl.

1. Öffnen **Sie "app/res/layout/fragment_new_event.xml",** und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_new_event.xml":::

1. Öffnen **Sie NewEventFragment,** und fügen Sie die folgenden `import` Anweisungen am Anfang der Datei hinzu.

    ```java
    import android.util.Log;
    import android.widget.Button;
    import com.google.android.material.snackbar.BaseTransientBottomBar;
    import com.google.android.material.snackbar.Snackbar;
    import com.google.android.material.textfield.TextInputLayout;
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalException;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    ```

1. Fügen Sie der Klasse die folgenden Member `NewEventFragment` hinzu.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="InputsSnippet":::

1. Fügen Sie die folgenden Funktionen hinzu, um eine Statusleiste ein- und auszublenden.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="ProgressBarSnippet":::

1. Fügen Sie die folgenden Funktionen hinzu, um die Werte aus den Eingabesteuerelementen zu erhalten und die Funktion `GraphHelper.createEvent` aufaufrufen.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="CreateEventSnippet":::

1. Ersetzen Sie das vorhandene `onCreateView` durch Folgendes.

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="OnCreateViewSnippet":::

1. Speichern Sie die Änderungen, und starten Sie die App neu. Wählen Sie das **Menüelement "Neues Ereignis"** aus, füllen Sie das Formular aus, und wählen Sie **CREATE aus.**

    ![Screenshot des Formulars zum Erstellen eines Ereignisses in der App](images/create-event.png)
