---
title: Расширение приложения личной вкладки Teams в Microsoft 365
description: Узнайте, как обновить личное приложение вкладки для запуска в Outlook и Office в дополнение к Microsoft Teams.
ms.date: 10/10/2022
ms.topic: tutorial
ms.custom: m365apps
ms.localizationpriority: medium
ms.openlocfilehash: 910a94f34d14c8dbb8a35099d438469fea52155b
ms.sourcegitcommit: 10debe0f01574a21aab54bfac692a4c8373263a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2022
ms.locfileid: "68789982"
---
# <a name="extend-a-teams-personal-tab-across-microsoft-365"></a>Расширение личной вкладки Teams в Microsoft 365

Личные вкладки — это отличный способ расширить возможности Microsoft Teams. Используя личные вкладки, вы можете предоставить пользователю доступ к приложению прямо в Teams, при этом ему не понадобится выходить из интерфейса или снова входить в систему. В этой предварительной версии личные вкладки могут подсвечиваться в других приложениях Microsoft 365. В этом руководстве показано, как создать существующую личную вкладку Teams и обновить ее для запуска в классических и веб-интерфейсах Outlook и Office, а также в приложении Office для Android.

Обновление личного приложения для запуска в Outlook и Office включает следующие действия.

> [!div class="checklist"]
>
> * [Обновите манифест приложения](#update-the-app-manifest).
> * [Обновите ссылки на пакет SDK TeamsJS](#update-sdk-references).
> * [Измените заголовки политики безопасности содержимого](#configure-content-security-policy-headers).
> * [Обновите регистрацию приложения Microsoft Azure Active Directory (Azure AD) для единого Sign-On (SSO).](#update-azure-ad-app-registration-for-sso)
> * [Загрузка неопубликованного приложения в Teams](#sideload-your-app-in-teams).

В оставшейся части этого руководства вы узнаете, как просмотреть личную вкладку в других приложениях Microsoft 365.

## <a name="prerequisites"></a>Предварительные требования

Чтобы завершить работу с этим руководством, вам потребуется:

* Клиент изолированной программной среды Microsoft 365 Developer Program
* Клиент песочницы, зарегистрированный в *целевых выпусках Office 365*
* Компьютер с приложениями Office, установленными из приложений Microsoft 365 *бета-канал*.
* (Необязательно) Устройство или эмулятор Android с приложением Office для Android, установленным и зарегистрированным в *бета-версии программы*
* (Необязательно) Расширение [Teams Toolkit](https://aka.ms/teams-toolkit) для Microsoft Visual Studio Code, помогающее обновлять код.

> [!div class="nextstepaction"]
> [Необходимые условия для установки](prerequisites.md)

## <a name="prepare-your-personal-tab-for-the-upgrade"></a>Подготовьте личную вкладку к обновлению

Если у вас есть личные вкладки, создайте копию или ветвь рабочего проекта для тестирования и обновите идентификатор приложения в манифесте приложения, чтобы использовать новый идентификатор (отличный от рабочего идентификатора приложения для тестирования).

Если вы хотите использовать пример кода для работы с этим руководством, выполните действия по настройке, описанные в [разделе Пример списка задач](https://github.com/OfficeDev/TeamsFx-Samples/tree/main/todo-list-with-Azure-backend), чтобы создать личное приложение вкладки с помощью расширения Набора средств Teams для Visual Studio Code, а затем вернитесь к этой статье, чтобы обновить его для Microsoft 365.

Кроме того, вы можете использовать простой единый вход в *приложение hello world* , которое уже включает Microsoft 365 в следующем разделе [краткого руководства](#quickstart) , а затем перейти к [загрузке неопубликованного приложения в Teams](#sideload-your-app-in-teams).

### <a name="quickstart"></a>Быстрый запуск

Чтобы начать с [личной вкладки](https://github.com/OfficeDev/TeamsFx-Samples/tree/ga/todo-list-with-Azure-backend-M365), которая уже включена для запуска в Outlook и Office, используйте расширение Набора средств Teams для Visual Studio Code.

1. В Visual Studio Code откройте палитру команд (`Ctrl+Shift+P`), введите `Teams: Create a new Teams app`.
1. Выберите **Создать новое приложение Teams**.
1. Выберите **личную вкладку С поддержкой единого входа**.

    :::image type="content" source="images/toolkit-tab-sample.png" alt-text="На снимке экрана показан пример списка todo (Работает в Teams, Outlook и Office) в наборе средств Teams.":::
1. Выберите предпочтительный язык программирования.
1. Выберите расположение на локальном компьютере для папки рабочей области и введите имя приложения.
1. Откройте палитру команд (`Ctrl+Shift+P`) и введите`Teams: Provision in the cloud`, чтобы создать необходимые ресурсы приложения (Служба приложений план, учетная запись хранения, приложение-функция, управляемое удостоверение) в учетной записи Azure.
1. Выберите подписку и группу ресурсов.
1. Выберите **Подготовка**.
1. Откройте палитру команд (`Ctrl+Shift+P`) и введите `Teams: Deploy to the cloud`, чтобы развернуть образец кода на подготовленных ресурсах в Azure и запустить приложение.
1. Нажмите **Развернуть**.

Здесь можно перейти к [загрузке неопубликованного приложения в Teams](#sideload-your-app-in-teams) и предварительному просмотру приложения в Outlook и Office. (Манифест приложения и вызовы API TeamsJS уже обновлены для Microsoft 365.)

## <a name="update-the-app-manifest"></a>Обновите манифест приложения

Чтобы включить личную вкладку Teams для запуска в Outlook и Office, необходимо использовать версию схемы `1.13` [манифеста разработчика Teams](../resources/schema/manifest-schema.md).

Существует два варианта обновления манифеста приложения:

# <a name="teams-toolkit"></a>[Набор средств Teams](#tab/manifest-teams-toolkit)

1. Откройте палитру команд: `Ctrl+Shift+P`.
1. Запустите команду `Teams: Upgrade Teams manifest` и выберите файл манифеста приложения. Изменения будут внесены на месте.

# <a name="manual-steps"></a>[Выполнение вручную](#tab/manifest-manual)

Откройте манифест приложения Teams и обновите `$schema` и `manifestVersion` со следующими значениями:

```json
{
    "$schema" : "https://developer.microsoft.com/json-schemas/teams/v1.13/MicrosoftTeams.schema.json",
    "manifestVersion" : "1.13"
}
```

---

Если вы использовали Teams Toolkit для создания личного приложения, вы также можете использовать его для проверки изменений в файле манифеста и выявления ошибок. Откройте палитру команд (`Ctrl+Shift+P`) и найдите **Teams: Проверить файл манифеста**.

## <a name="update-sdk-references"></a>Обновление ссылок на SDK

Для запуска в Outlook и Office приложение должно ссылаться на пакет `@microsoft/teams-js@2.0.0` npm (или более поздней версии). Хотя код с более ранними версиями поддерживается в Outlook и Office, предупреждения об устаревании регистрируются, а поддержка более ранних версий TeamsJS в Outlook и Office в конечном итоге прекратится.

С помощью набора средств Teams можно определить и автоматизировать необходимые изменения кода для обновления с версий TeamsJS версии 1.x до TeamsJS версии 2.x.x. Кроме того, можно выполнить те же действия вручную. Дополнительные сведения см. в [статье Пакет SDK для клиента JavaScript для Microsoft Teams](../tabs/how-to/using-teams-client-sdk.md#whats-new-in-teamsjs-version-20) .

1. Откройте *палитру команд*: `Ctrl+Shift+P`.
1. Выполните команду `Teams: Upgrade Teams JS SDK and code references`.

По завершении файл *package.json* будет ссылаться `@microsoft/teams-js@2.0.0` (или выше), а `*.js/.ts` файлы и `*.jsx/.tsx` будут обновлены следующим образом:

> [!div class="checklist"]
>
> * Инструкции импорта для teams-js@2.x.x
> * [Вызовы функций, перечислений и интерфейсов](../tabs/how-to/using-teams-client-sdk.md#whats-new-in-teamsjs-version-20) для teams-js@2.x.x.
> * `TODO`напоминания о примечаниях к областям, на которые могут повлиять изменения интерфейса [контекста](../tabs/how-to/using-teams-client-sdk.md#updates-to-the-context-interface)
> * Напоминания в комментариях `TODO` о том, что [ следует преобразовать функции обратного вызова в обещания](../tabs/how-to/using-teams-client-sdk.md#callbacks-converted-to-promises)

> [!IMPORTANT]
> Код *внутри.html* файлов не поддерживается средствами обновления и требует внесения изменений вручную.

## <a name="configure-content-security-policy-headers"></a>Настройка заголовков Content Security Policy

Как и в Microsoft Teams, приложения вкладок размещаются в [элементах iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) в веб-клиентах Office и Outlook.

Если в приложении используются заголовки [политики безопасности содержимого](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) (CSP), убедитесь, что в [заголовках CSP разрешены все следующие предки кадров](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors) :

|Ведущий сайт Microsoft 365| разрешение для фрейма-предка|
|--|--|
| Teams | `teams.microsoft.com` |
| Office | `*.microsoft365.com`, `*.office.com` |
| Outlook | `outlook.live.com`, `outlook.office.com`, `outlook.office365.com`, `outlook-sdf.office.com`, `outlook-sdf.office365.com` |

## <a name="update-azure-ad-app-registration-for-sso"></a>Обновление регистрации приложения Azure AD для единого входа

[Единый вход Azure Active Directory (AD)](../tabs/how-to/authentication/tab-sso-overview.md) для личных вкладок работает так же, как и в Teams. Однако необходимо добавить несколько идентификаторов клиентских приложений в Azure AD регистрации приложения вкладки на *портале Регистрация приложений* клиента.

1. Войдите на [портал Microsoft Azure](https://portal.azure.com) с помощью учетной записи арендатора песочницы.
1. Откройте колонку **регистрации приложений**.
1. Выберите имя приложения с личной вкладкой, чтобы открыть регистрацию приложения.
1. Выберите **Открыть API** (в разделе *Управление*).

    :::image type="content" source="images/azure-app-registration-clients.png" alt-text="На снимке экрана показан пример авторизации идентификаторов клиента из колонки *Регистрация приложений* на портал Azure.":::

1. В разделе **Авторизованные клиентские приложения** убедитесь, что добавлены все следующие `Client Id` значения:

    |Клиентское приложение Microsoft 365 | Идентификатор клиента |
    |--|--|
    |Классическое, мобильное приложение Teams |1fec8e78-bce4-4aaf-ab1b-5451cc387264 |
    |Веб-приложение Teams |5e3ce6c0-2b1f-4285-8d4b-75ee78787346 |
    |Office Web  |4765445b-32c6-49b0-83e6-1d93765276ca|
    |Классические приложения Office  | 0ec893e0-5785-4de6-99da-4ed124e5296c |
    |Мобильный Office  | d3590ed6-52b3-4102-aeff-aad2292ab01c |
    |Классический outlook, мобильный | d3590ed6-52b3-4102-aeff-aad2292ab01c |
    |Outlook Web | bc59ab01-8403-45c6-8796-ac3ef710b3e3|

    > [!NOTE]
    > Некоторые клиентские приложения Microsoft 365 используют идентификаторы клиентов.

## <a name="sideload-your-app-in-teams"></a>Загрузка неопубликованного приложения в Teams

Последний шаг для запуска приложения в Office и Outlook — загрузка обновленного [пакета приложения](..//concepts/build-and-test/apps-package.md) личной вкладки в Microsoft Teams.

1. Упаковайте приложение Teams ([манифест](../resources/schema/manifest-schema.md) и [значки приложений](/microsoftteams/platform/resources/schema/manifest-schema#icons)) в ZIP-файл. Если вы использовали Teams Toolkit для создания приложения, это легко сделать с помощью параметра **пакета метаданных Zip Teams** в меню **Развертывание** Teams Toolkit:

    :::image type="content" source="images/toolkit-zip-teams-metadata-package.png" alt-text="На снимке экрана показан пример варианта пакета метаданных Zip Teams в расширении Набора средств Teams для Visual Studio Code.":::

1. Войдите в Teams с помощью учетной записи клиента песочницы и переключитесь в режим *предварительной версии для разработчиков*. Выберите меню с многоточием (**...**) рядом с профилем пользователя, затем выберите: **О программе** > **Предварительная версия для разработчиков**.

    :::image type="content" source="images/teams-dev-preview.png" alt-text="На снимках экрана показано, как выбрать параметр &quot;Предварительная версия для разработчиков&quot;.":::

1. Выберите **Приложения**, чтобы открыть область **Управление приложениями**. Затем выберите **Отправить приложение**.

    :::image type="content" source="images/teams-manage-your-apps.png" alt-text="На снимку экрана показан пример панели &quot;Управление приложениями&quot; и &quot;Опубликовать приложение&quot;.":::

1. Выберите **Параметр Отправить настроенное приложение** и выберите пакет приложения.

    :::image type="content" source="images/teams-upload-custom-app.png" alt-text="На снимке экрана показан пример, на котором показан параметр отправки приложения am в Teams.":::

После загрузки неопубликованного приложения в Teams ваша личная вкладка будет доступна в Outlook и Office. Вы должны войти с теми же учетными данными, которые использовались для загрузки неопубликованного приложения в Teams. При запуске приложения Office для Android необходимо перезапустить приложение, чтобы использовать личное приложение вкладки из приложения Office.

Вы можете закрепить приложение для быстрого доступа или найти его во всплывающем меню с многоточием (**...**) среди последних приложений на боковой панели слева. Закрепление приложения в Teams не закрепляет его как приложение в Office или Outlook.

## <a name="preview-your-personal-tab-in-other-microsoft-365-experiences"></a>Предварительный просмотр личной вкладки в других приложениях Microsoft 365.

Ниже описано, как просмотреть приложение, работающее в Office и Outlook, веб-клиентах и классических клиентах Windows.

> [!NOTE]
> Если вы используете пример приложения Teams Toolkit и удаляете его из Teams, оно будет удалено из каталогов **Других приложений** в Outlook и Office.

### <a name="outlook-on-windows"></a>Outlook для Windows

Чтобы просмотреть приложение, работающее в Outlook на рабочем столе Windows:

1. Запустите Outlook и войдите в систему, используя свою учетную запись разработчика.
1. На боковой панели выберите  **Другие приложения**. Заголовок неопубликованного приложения отображается среди установленных приложений.
1. Щелкните значок приложения, чтобы запустить приложение в Outlook.

    :::image type="content" source="images/outlook-desktop-more-apps.png" alt-text="На снимке экрана показан параметр с многоточием (Дополнительные приложения) на боковой панели классического клиента Outlook для просмотра установленных личных вкладок.":::

### <a name="outlook-on-the-web"></a>Outlook в Интернете

Чтобы просмотреть свое приложение в Outlook в Интернете:

1. Перейдите на [страницу Outlook в Интернете](https://outlook.office.com) и войдите с помощью учетной записи клиента разработки.
1. На боковой панели выберите  **Другие приложения**. Заголовок неопубликованного приложения отображается среди установленных приложений.
1. Щелкните значок приложения, чтобы запустить и просмотреть приложение, работающее в Outlook в Интернете.

    :::image type="content" source="images/outlook-web-more-apps.png" alt-text="На снимке экрана показан пример с многоточием (Дополнительные приложения) на боковой панели outlook.com для просмотра установленных личных вкладок.":::

### <a name="office-on-windows"></a>Office для Windows

Чтобы просмотреть приложение, работающее в Office на рабочем столе Windows:

1. Запустите Office и войдите в систему, используя учетную запись разработчика.
1. Щелкните значок **Приложения** на боковой панели. Заголовок неопубликованного приложения отображается среди установленных приложений.
1. Щелкните значок приложения, чтобы запустить приложение в Office.

    :::image type="content" source="images/office-desktop-more-apps.png" alt-text="На снимке экрана показан пример с многоточием (Дополнительные приложения) на боковой панели классического клиента Office для просмотра установленных личных вкладок.":::

### <a name="office-on-the-web"></a>Office в Интернете

Чтобы предварительно просмотреть приложение, работающее в Office в Интернете, выполните указанные ниже действия.

1. Войдите в **office.com** с использованием тестовых учетных данных клиента.
1. Щелкните значок **Приложения** на боковой панели. Заголовок неопубликованного приложения отображается среди установленных приложений.
1. Щелкните значок приложения, чтобы запустить приложение в Office в Интернете.

    :::image type="content" source="images/office-web-more-apps.png" alt-text="На снимках экрана показан пример параметра (Дополнительные приложения) на боковой панели office.com для просмотра установленных личных вкладок.":::

### <a name="office-app-for-android"></a>Приложение Office для Android

> [!NOTE]
> Перед установкой приложения выполните [действия, чтобы установить последнюю бета-версию приложения Office](prerequisites.md#mobile) и стать частью бета-версии программы.

Чтобы просмотреть приложение, запущенное в приложении Office для Android, выполните приведенные далее действия.

1. Запустите приложение Office и выполните вход с помощью учетной записи клиента разработки. Если приложение Office для Android уже работало до загрузки неопубликованного приложения в Teams, его необходимо перезапустить, чтобы увидеть в установленных приложениях.
1. Щелкните значок **Приложения** . Загруженное неопубликованное приложение отображается среди установленных приложений.
1. Щелкните значок приложения, чтобы запустить приложение в приложении Office для Android.

    :::image type="content" source="images/office-mobile-apps.png" alt-text="На снимках экрана показан пример параметра &quot;Приложения&quot; на боковой панели приложения Office для просмотра установленных личных вкладок.":::

## <a name="troubleshooting"></a>Устранение неполадок

В настоящее время в клиентах Outlook и Office поддерживается подмножество типов и возможностей приложений Teams. Эта поддержка со временем расширяется.

Ознакомьтесь с [разделом Поддержка Microsoft 365](../tabs/how-to/using-teams-client-sdk.md#microsoft-365-support-running-teams-apps-in-office-and-outlook) , чтобы проверить поддержку узла для различных возможностей TeamsJS.

Общие сведения о поддержке узлов и платформ Microsoft 365 для приложений Teams см. в статье [Расширение приложений Teams в Microsoft 365](overview.md).

Вы можете проверить поддержку данной возможности узла во время выполнения, вызвав `isSupported()` функцию для этой возможности (пространство имен) и настроив поведение приложения соответствующим образом. Это позволяет вашему приложению освещать пользовательский интерфейс и функциональные возможности на узлах, которые его поддерживают, а также предоставлять изящный резервный интерфейс на узлах, которые этого не сделали. Дополнительные сведения см. в разделе [Дифференциация взаимодействия с приложением](../tabs/how-to/using-teams-client-sdk.md#differentiate-your-app-experience).

Используйте [каналы сообщества разработчиков Microsoft Teams](/microsoftteams/platform/feedback), чтобы сообщать о проблемах и оставлять отзывы.

### <a name="debugging"></a>Отладка

В наборе средств Teams можно выполнять отладку (`F5`) приложения вкладок, работающего в Office и Outlook, а также Teams.

:::image type="content" source="images/toolkit-debug-targets.png" alt-text="На снимке экрана показан пример раскрывающегося меню отладки в Teams в наборе средств Teams.":::

При первом запуске локальной отладки в Office или Outlook вам будет предложено войти в учетную запись клиента Microsoft 365 и установить самозаверяющий тестовый сертификат. Вам также будет предложено установить Teams вручную. Выберите **Установить в Teams** , чтобы открыть окно браузера и вручную установить приложение. Затем нажмите **кнопку Продолжить** , чтобы продолжить отладку приложения в Office или Outlook.

:::image type="content" source="images/toolkit-dialog-teams-install.png" alt-text="На снимке экрана показан пример диалогового окна Набор средств для установки в Teams.":::

Предоставьте отзыв и сообщите о любых проблемах с отладкой набора средств Teams в [Microsoft Teams Framework (TeamsFx).](https://github.com/OfficeDev/TeamsFx/issues)

#### <a name="mobile-debugging"></a>Мобильная отладка

Отладка набора средств Teams (`F5`) пока не поддерживается в приложении Office для Android. Вот как выполнить удаленную отладку приложения, работающего в приложении Office для Android:

1. При отладке с помощью физического устройства Android подключите его к компьютеру разработки и включите параметр [отладки по USB](https://developer.android.com/studio/debug/dev-options). Это включено по умолчанию с помощью эмулятора Android.
1. Запустите приложение Office с устройства Android.
1. Откройте свой профиль **Me > Settings > Разрешить отладку** и установите флажок **Включить удаленную отладку**.

    :::image type="content" source="images/office-android-enable-remote-debugging.png" alt-text="На снимке экрана показан пример переключателя Включить удаленную отладку.":::

1. Оставьте **параметры**.
1. Оставьте экран профиля.
1. Выберите **Приложения** и запустите неопубликованное приложение для запуска в приложении Office.
1. Убедитесь, что устройство Android подключено к компьютеру разработки. На компьютере разработки откройте в браузере страницу проверки средств разработки. Например, перейдите по адресу `edge://inspect/#devices` в Microsoft Edge, чтобы отобразить список отладочных веб-представлений Android WebView.
1. Найдите с `Microsoft Teams Tab` помощью URL-адреса вкладки и выберите **проверить** , чтобы начать отладку приложения с помощью средств разработки.

    :::image type="content" source="images/office-android-debug.png" alt-text="На снимке экрана показан пример списка веб-представлений в средстве разработки.":::

1. Отладка приложения вкладки в Android WebView. Таким же образом вы [удаленно отлаживать](/microsoft-edge/devtools-guide-chromium/remote-debugging) обычный веб-сайт на устройстве Android.

## <a name="code-sample"></a>Пример кода

| **Название примера** | **Описание** | **Node.js** |
|---------------|--------------|--------|
| Список дел | Редактируемый список дел с единым входом, созданным с помощью React и Функции Azure. Работает только в Teams (используйте этот пример приложения, чтобы попробовать процесс обновления, описанный в этом руководстве). | [Просмотр](https://github.com/OfficeDev/TeamsFx-Samples/tree/ga/todo-list-with-Azure-backend)  |
| Список задач (Microsoft 365) | Редактируемый список дел с единым входом, созданным с помощью React и Функции Azure. Работает в Teams, Outlook, Office. | [Просмотр](https://github.com/OfficeDev/TeamsFx-Samples/tree/ga/todo-list-with-Azure-backend-M365)|
| Редактор изображений (Microsoft 365) | Создание, изменение, открытие и сохранение образов с помощью Microsoft API Graph. Работает в Teams, Outlook, Office. | [Просмотр](https://github.com/OfficeDev/m365-extensibility-image-editor) |
| Пример страницы запуска (Microsoft 365) | Демонстрирует проверку подлинности единого входа и возможности пакета SDK TeamsJS, доступные на разных узлах. Работает в Teams, Outlook, Office. | [Просмотр](https://github.com/OfficeDev/microsoft-teams-library-js/tree/main/apps/sample-app) |
| Приложение Northwind Orders | Демонстрируется использование пакета SDK microsoft TeamsJS версии 2 для расширения приложений Teams для других ведущих приложений Microsoft 365. Работает в Teams, Outlook, Office. Оптимизировано для мобильных устройств.| [Просмотр](https://github.com/microsoft/app-camp/tree/main/experimental/ExtendTeamsforM365) |

## <a name="next-step"></a>Следующий этап

Опубликуйте приложение, чтобы его можно было обнаруживать в Teams, в Outlook и в Office:

> [!div class="nextstepaction"]
> [Публикация приложений Teams для Outlook и Office](publish.md)
