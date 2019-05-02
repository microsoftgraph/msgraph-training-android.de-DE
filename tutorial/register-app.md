<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="adf34-101">In dieser Übung erstellen Sie eine neue Azure AD native-Anwendung mithilfe des Azure Active Directory Admin Center.</span><span class="sxs-lookup"><span data-stu-id="adf34-101">In this exercise you will create a new Azure AD native application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="adf34-102">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com). Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.</span><span class="sxs-lookup"><span data-stu-id="adf34-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="adf34-103">Wählen Sie **Azure Active Directory** in der linken Navigationsleiste aus, und wählen Sie dann **App** -Registrierungen unter **Manage**aus.</span><span class="sxs-lookup"><span data-stu-id="adf34-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="adf34-104">Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="adf34-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="adf34-105">Wählen Sie **Neue Registrierung** aus.</span><span class="sxs-lookup"><span data-stu-id="adf34-105">Select **New registration**.</span></span> <span data-ttu-id="adf34-106">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="adf34-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="adf34-107">Legen Sie **Name** auf `Android Graph Tutorial` fest.</span><span class="sxs-lookup"><span data-stu-id="adf34-107">Set **Name** to `Android Graph Tutorial`.</span></span>
    - <span data-ttu-id="adf34-108">Legen Sie **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** fest.</span><span class="sxs-lookup"><span data-stu-id="adf34-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="adf34-109">Lassen Sie **URI umleiten** leer.</span><span class="sxs-lookup"><span data-stu-id="adf34-109">Leave **Redirect URI** empty.</span></span>

    ![Screenshot der Seite "Registrieren einer Anwendung"](./images/aad-register-an-app.png)

1. <span data-ttu-id="adf34-111">Wählen Sie **Registrieren** aus.</span><span class="sxs-lookup"><span data-stu-id="adf34-111">Choose **Register**.</span></span> <span data-ttu-id="adf34-112">Kopieren Sie auf der Seite mit dem **Android-Graph-Lernprogramm** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, dann benötigen Sie ihn im nächsten Schritt.</span><span class="sxs-lookup"><span data-stu-id="adf34-112">On the **Android Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](./images/aad-application-id.png)

1. <span data-ttu-id="adf34-114">Wählen Sie den Link Umleitungs- **URI hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="adf34-114">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="adf34-115">Suchen Sie auf der Seite **URIs umleiten** nach dem Abschnitt Empfohlene Umleitungs- **URIs für öffentliche Clients (Mobil, Desktop)** .</span><span class="sxs-lookup"><span data-stu-id="adf34-115">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="adf34-116">Wählen Sie den URI aus, `msal` der mit beginnt, und kopieren Sie ihn, und wählen Sie dann **Speichern**aus.</span><span class="sxs-lookup"><span data-stu-id="adf34-116">Select the URI that begins with `msal` and copy it, then choose **Save**.</span></span> <span data-ttu-id="adf34-117">Speichern Sie den kopierten Umleitungs-URI, den Sie im nächsten Schritt benötigen.</span><span class="sxs-lookup"><span data-stu-id="adf34-117">Save the copied redirect URI, you will need it in the next step.</span></span>

    ![Screenshot der Seite "Umleitungs-URIs"](./images/aad-redirect-uris.png)