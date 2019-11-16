<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="50fdb-101">In dieser Übung erstellen Sie mithilfe des Azure Active Directory Admin Center eine neue Azure AD systemeigene Anwendung.</span><span class="sxs-lookup"><span data-stu-id="50fdb-101">In this exercise you will create a new Azure AD native application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="50fdb-102">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com). Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.</span><span class="sxs-lookup"><span data-stu-id="50fdb-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="50fdb-103">Wählen Sie in der linken Navigationsleiste **Azure Active Directory** aus, und wählen Sie dann **App-Registrierungen** unter **Verwalten** aus.</span><span class="sxs-lookup"><span data-stu-id="50fdb-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="50fdb-104">Ein Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="50fdb-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="50fdb-105">Wählen Sie **Neue Registrierung** aus.</span><span class="sxs-lookup"><span data-stu-id="50fdb-105">Select **New registration**.</span></span> <span data-ttu-id="50fdb-106">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="50fdb-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="50fdb-107">Legen Sie **Name** auf `Android Graph Tutorial` fest.</span><span class="sxs-lookup"><span data-stu-id="50fdb-107">Set **Name** to `Android Graph Tutorial`.</span></span>
    - <span data-ttu-id="50fdb-108">Legen Sie **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** fest.</span><span class="sxs-lookup"><span data-stu-id="50fdb-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="50fdb-109">Legen Sie unter **Umleitungs-URI**das Dropdown auf **Public Client/Native (Mobile #a0 Desktop)** fest, `msauth://YOUR_PACKAGE_NAME/callback`und legen `YOUR_PACKAGE_NAME` Sie den Wert auf ersetzt durch den Paketnamen Ihres Projekts fest.</span><span class="sxs-lookup"><span data-stu-id="50fdb-109">Under **Redirect URI**, set the dropdown to **Public client/native (mobile & desktop)** and set the value to `msauth://YOUR_PACKAGE_NAME/callback`, replacing `YOUR_PACKAGE_NAME` with your project's package name.</span></span>

    ![Screenshot der Seite "Anwendung registrieren"](./images/aad-register-an-app.png)

1. <span data-ttu-id="50fdb-111">Wählen Sie **registrieren**aus.</span><span class="sxs-lookup"><span data-stu-id="50fdb-111">Select **Register**.</span></span> <span data-ttu-id="50fdb-112">Kopieren Sie auf der Seite **Android Graph Tutorial** den Wert der **Anwendungs-ID (Client)** , und speichern Sie Sie, um Sie im nächsten Schritt zu benötigen.</span><span class="sxs-lookup"><span data-stu-id="50fdb-112">On the **Android Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Ein Screenshot der Anwendungs-ID der neuen App-Registrierung](./images/aad-application-id.png)
