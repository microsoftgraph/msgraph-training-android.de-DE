<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="04f13-101">In dieser Übung erstellen Sie mithilfe des Azure Active Directory Admin Center eine neue Azure AD systemeigene Anwendung.</span><span class="sxs-lookup"><span data-stu-id="04f13-101">In this exercise you will create a new Azure AD native application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="04f13-102">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com). Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.</span><span class="sxs-lookup"><span data-stu-id="04f13-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="04f13-103">Wählen Sie in der linken Navigationsleiste **Azure Active Directory** aus, und wählen Sie dann **App** -Registrierungen unter **Manage**aus.</span><span class="sxs-lookup"><span data-stu-id="04f13-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="04f13-104">Ein Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="04f13-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="04f13-105">Wählen Sie **Neue Registrierung** aus.</span><span class="sxs-lookup"><span data-stu-id="04f13-105">Select **New registration**.</span></span> <span data-ttu-id="04f13-106">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="04f13-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="04f13-107">Legen Sie **Name** auf `Android Graph Tutorial` fest.</span><span class="sxs-lookup"><span data-stu-id="04f13-107">Set **Name** to `Android Graph Tutorial`.</span></span>
    - <span data-ttu-id="04f13-108">Legen Sie **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** fest.</span><span class="sxs-lookup"><span data-stu-id="04f13-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="04f13-109">Lassen Sie **URI umleiten** leer.</span><span class="sxs-lookup"><span data-stu-id="04f13-109">Leave **Redirect URI** empty.</span></span>

    ![Screenshot der Seite "Anwendung registrieren"](./images/aad-register-an-app.png)

1. <span data-ttu-id="04f13-111">Wählen Sie **registrieren**aus.</span><span class="sxs-lookup"><span data-stu-id="04f13-111">Select **Register**.</span></span> <span data-ttu-id="04f13-112">Kopieren Sie auf der Seite **Android Graph Tutorial** den Wert der **Anwendungs-ID (Client)** , und speichern Sie Sie, um Sie im nächsten Schritt zu benötigen.</span><span class="sxs-lookup"><span data-stu-id="04f13-112">On the **Android Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Ein Screenshot der Anwendungs-ID der neuen App-Registrierung](./images/aad-application-id.png)

1. <span data-ttu-id="04f13-114">Klicken Sie auf den Link Umleitungs- **URI hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="04f13-114">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="04f13-115">Suchen Sie auf der Seite "Umleitungs- **URIs** " den Abschnitt vorgeschlagene Umleitungs- **URIs für öffentliche Clients (Mobile, Desktop)** .</span><span class="sxs-lookup"><span data-stu-id="04f13-115">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="04f13-116">Wählen Sie den URI aus, `msal` der mit beginnt, und kopieren Sie ihn, und wählen Sie dann **Speichern**aus.</span><span class="sxs-lookup"><span data-stu-id="04f13-116">Select the URI that begins with `msal` and copy it, then select **Save**.</span></span> <span data-ttu-id="04f13-117">Speichern Sie den kopierten Umleitungs-URI, den Sie im nächsten Schritt benötigen.</span><span class="sxs-lookup"><span data-stu-id="04f13-117">Save the copied redirect URI, you will need it in the next step.</span></span>

    ![Screenshot der Seite "Umleitungs-URIs"](./images/aad-redirect-uris.png)