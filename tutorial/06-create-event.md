<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a9195-101">In diesem Abschnitt fügen Sie die Möglichkeit zum Erstellen von Ereignissen im Kalender des Benutzers hinzu.</span><span class="sxs-lookup"><span data-stu-id="a9195-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="a9195-102">Öffnen **Sie GraphHelper,** und fügen Sie die folgenden `import` Anweisungen am Oberen Rand der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="a9195-102">Open **GraphHelper** and add the following `import` statements to the top of the file.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.Attendee;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.EmailAddress;
    import com.microsoft.graph.models.extensions.ItemBody;
    import com.microsoft.graph.models.generated.AttendeeType;
    import com.microsoft.graph.models.generated.BodyType;
    ```

1. <span data-ttu-id="a9195-103">Fügen Sie der Klasse die folgende Funktion `GraphHelper` hinzu, um ein neues Ereignis zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a9195-103">Add the following function to the `GraphHelper` class to create a new event.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphHelper.java" id="CreateEventSnippet":::

## <a name="update-new-event-fragment"></a><span data-ttu-id="a9195-104">Aktualisieren eines neuen Ereignisfragments</span><span class="sxs-lookup"><span data-stu-id="a9195-104">Update new event fragment</span></span>

1. <span data-ttu-id="a9195-105">Klicken Sie mit der rechten Maustaste auf den Ordner **"app/java/com.example.graphtutorial",** und wählen Sie **"Neu"** und dann **Java Klasse aus.**</span><span class="sxs-lookup"><span data-stu-id="a9195-105">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="a9195-106">Benennen Sie die `EditTextDateTimePicker` Klasse, und wählen Sie **OK aus.**</span><span class="sxs-lookup"><span data-stu-id="a9195-106">Name the class `EditTextDateTimePicker` and select **OK**.</span></span>

1. <span data-ttu-id="a9195-107">Öffnen Sie die neue Datei, und ersetzen Sie ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="a9195-107">Open the new file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/EditTextDateTimePicker.java" id="DateTimePickerSnippet":::

    <span data-ttu-id="a9195-108">Diese Klasse umschließt ein Steuerelement, zeigt eine Datums- und Uhrzeitauswahl an, wenn der Benutzer darauf tippt, und aktualisiert den Wert mit dem datums- und `EditText` uhrzeitauswahl.</span><span class="sxs-lookup"><span data-stu-id="a9195-108">This class wraps an `EditText` control, showing a date and time picker when the user taps it, and updating the value with the date and time picked.</span></span>

1. <span data-ttu-id="a9195-109">Öffnen **Sie "app/res/layout/fragment_new_event.xml",** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="a9195-109">Open **app/res/layout/fragment_new_event.xml** and replace its contents with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_new_event.xml":::

1. <span data-ttu-id="a9195-110">Öffnen **Sie NewEventFragment,** und fügen Sie die folgenden `import` Anweisungen am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="a9195-110">Open **NewEventFragment** and add the following `import` statements at the top of the file.</span></span>

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

1. <span data-ttu-id="a9195-111">Fügen Sie der Klasse die folgenden Member `NewEventFragment` hinzu.</span><span class="sxs-lookup"><span data-stu-id="a9195-111">Add the following members to the `NewEventFragment` class.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="InputsSnippet":::

1. <span data-ttu-id="a9195-112">Fügen Sie die folgenden Funktionen hinzu, um eine Statusleiste ein- und auszublenden.</span><span class="sxs-lookup"><span data-stu-id="a9195-112">Add the following functions to show and hide a progress bar.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="ProgressBarSnippet":::

1. <span data-ttu-id="a9195-113">Fügen Sie die folgenden Funktionen hinzu, um die Werte aus den Eingabesteuerelementen zu erhalten und die Funktion `GraphHelper.createEvent` aufaufrufen.</span><span class="sxs-lookup"><span data-stu-id="a9195-113">Add the following functions to get the values from the input controls and call the `GraphHelper.createEvent` function.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="CreateEventSnippet":::

1. <span data-ttu-id="a9195-114">Ersetzen Sie das vorhandene `onCreateView` durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="a9195-114">Replace the existing `onCreateView` with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="OnCreateViewSnippet":::

1. <span data-ttu-id="a9195-115">Speichern Sie die Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="a9195-115">Save your changes and restart the app.</span></span> <span data-ttu-id="a9195-116">Wählen Sie das **Menüelement "Neues Ereignis"** aus, füllen Sie das Formular aus, und wählen Sie **CREATE aus.**</span><span class="sxs-lookup"><span data-stu-id="a9195-116">Select the **New Event** menu item, fill in the form, and select **CREATE**.</span></span>

    ![Screenshot des Formulars zum Erstellen eines Ereignisses in der App](images/create-event.png)
