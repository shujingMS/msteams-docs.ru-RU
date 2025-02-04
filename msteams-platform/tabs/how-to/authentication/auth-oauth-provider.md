---
title: Использование внешних поставщиков OAuth
description: Выполните проверку подлинности пользователей приложения с помощью внешних поставщиков OAuth и узнайте, как добавить его во внешний браузер.
ms.topic: how-to
ms.localizationpriority: high
ms.openlocfilehash: 4892dc23174e34015a02a9afff64269e01871fb5
ms.sourcegitcommit: c1032ea4f48c4bbf5446798ff7d46d7e6e9f55d2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2022
ms.locfileid: "68027321"
---
# <a name="use-external-oauth-providers"></a>Использование внешних поставщиков OAuth

[!INCLUDE [sdk-include](~/includes/sdk-include.md)]

Вы можете поддерживать внешних или сторонних поставщиков OAuth, таких как Google, GitHub, LinkedIn и Facebook с помощью обновленного API `authenticate()`:

```JavaScript
function authenticate(authenticateParameters: AuthenticatePopUpParameters): Promise<string>
```

Для поддержки внешних поставщиков OAuth в API `authenticate()` добавляются следующие данные.

* Параметр `isExternal`
* Два значения заполнителей в существующем параметре `url`

В следующей таблице приведен список параметров `authenticate()` API (`AuthenticatePopUpParameters`) и функций, а также их описания.

| Параметр| Описание|
| --- | --- |
|`isExternal` | Тип параметра — логический. Он указывает, что окно аутентификации открывается во внешнем браузере.|
|`height` |Предпочтительная высота всплывающего окна. Значение можно игнорировать, если он находится за пределами допустимых границ.|
|`url`  <br>|URL-адрес стороннего сервера приложений для всплывающего окна проверки подлинности со следующими двумя заполнителями параметров:</br> <br> - `oauthRedirectMethod`: Pass placeholder in `{}`. This placeholder is replaced by deeplink or web page by Teams platform, which informs app server if the call is coming from mobile platform.</br> <br> - `authId`: этот заполнитель заменяется UUID. Сервер приложений использует его для обслуживания сеанса.|
|`width`|Предпочтительная ширина всплывающего окна. Значение можно игнорировать, если он находится за пределами допустимых границ.|

Дополнительные сведения о параметрах см. в описании функции [authenticate(AuthenticatePopUpParameters](/javascript/api/@microsoft/teams-js/authentication#@microsoft-teams-js-authentication-authenticate) ).

## <a name="add-authentication-to-external-browsers"></a>Добавление проверки подлинности во внешние браузеры

> [!NOTE]
>
> * В настоящее время можно добавить проверку подлинности во внешние браузеры только для вкладок на мобильных устройствах.
> * Используйте бета-версию JS SDK для использования функций. Бета-версии доступны через [NPM](https://www.npmjs.com/package/@microsoft/teams-js/v/1.12.0-beta.2).

На следующем изображении обеспечивается поток для добавления проверки подлинности во внешние браузеры:

 :::image type="content" source="../../../assets/images/tabs/tabs-authenticate-OAuth.PNG" alt-text="authenticate-OAuth":::

**Добавление проверки подлинности во внешние браузеры**

1. 1. Инициация внешнего процесса аутентификации.

   Стороннее приложение вызывает функцию SDK `authentication.authenticate` со значение true для `isExternal` для инициации внешнего процесса аутентификации.

   Переданное значение `url` содержит заполнители для `{authId}` и `{oauthRedirectMethod}`.  

    ```JavaScript
    import { authentication } from "@microsoft/teams-js";
    authentication.authenticate({
       url: 'https://3p.app.server/auth?oauthRedirectMethod={oauthRedirectMethod}&authId={authId}',
       isExternal: true,
       successCallback: function (result) {
       //sucess 
       } failureCallback: function (reason) {
       //failure 
        }
    });
    ```

2. 2. Ссылка Teams во внешнем браузере.

   Клиенты Teams открывают URL-адрес во внешнем браузере после замены `oauthRedirectMethod` и `authId` подходящими значениями.

   #### <a name="example"></a>Пример

   ```http
    https://3p.app.server/auth?oauthRedirectMethod=deeplink&authId=1234567890 
   ```

3. 3. Ответ сервера стороннего приложения.

   Сервер стороннего приложения `url` получает и сохраняет следующие два параметра запроса:

   | Параметр | Описание|
   | --- | --- |
   | `oauthRedirectMethod` |Указывает, как стороннее приложение должно отправлять ответ на запрос проверки подлинности обратно в Teams с одним из двух значений: deeplink (прямая ссылка) или page (страница).|
   |`authId` | ИД запроса request-id, созданный Teams для этого конкретного запроса на проверку подлинности, который необходимо отправить обратно в Teams с помощью deeplink.|

    > [!TIP]
    > Стороннее приложение может маршалировать `authId`, `oauthRedirectMethod` в параметре запроса OAuth `state` при создании URL-адреса входа для OAuthProvider. Параметр `state` содержит переданные `authId` и `oauthRedirectMethod`, когда OAuthProvider перенаправляет обратно на сторонний сервер, а стороннее приложение использует значения для отправки ответа на проверку подлинности обратно в Teams, как описано в **6. Ответ сервера стороннего приложения на запрос Teams**.

4. 4. Перенаправление сервера стороннего приложения на указанный `url`.

   Сервер стороннего приложения перенаправляет на страницу аутентификации поставщиков OAuth во внешнем браузере. `redirect_uri` является выделенным маршрутом на сервере стороннего приложения. Вы можете зарегистрировать `redirect_uri` в консоли разработчика поставщиков OAuth как статический, параметры должны быть отправлены через объект state.

   #### <a name="example"></a>Пример

    ```http
    https://accounts.google.com/o/oauth2/v2/auth?redirect_uri=https://3p.app.server/authredirect&state={"authId":"…","oauthRedirectMethod":"…"}&client_id=…    &response_type=code&access_type=offline&scope= … 
    ```

5. 5. Вход во внешний браузер.

   User signs in to the external browser. The OAuth providers redirects back to the `redirect_uri` with the auth code and the state object.

6. Сервер приложений 3P проверяет и отвечает на Teams.

   Сервер стороннего приложения обрабатывает ответ и проверяет `oauthRedirectMethod`, который возвращается внешним поставщиком OAuth в объекте состояния, чтобы определить, должен ли ответ возвращаться через прямую ссылку обратного вызова проверки безопасности или через веб-страницу, которая вызывает `notifySuccess()`.

      ```JavaScript
      const state = JSON.parse(req.query.state)
      if (state.oauthRedirectMethod === 'deeplink') {
         return res.redirect('msteams://teams.microsoft.com/l/auth-callback?authId=${state.authId}&result=${req.query.code}')
      }
      else {
      // continue redirecting to a web-page that will call notifySuccess() – usually this method is used in Teams-Web
      …
      ```

7. 7. Создание прямой ссылки сторонним приложением.

   Стороннее приложение создает прямую ссылку для мобильного приложения Teams в следующем формате и отправляет код аутентификации с кодом сеанса обратно в Teams.

   ```JavaScript
   return res.redirect(`msteams://teams.microsoft.com/l/auth-callback?authId=${state.authId}&result=${req.query.code}`)
   ```

8. Teams вызывает обратный вызов успеха и отправляет результат.

    Teams делает успешный вызов и отправляет результат (код аутентификации) в стороннее приложение. Стороннее приложение получает код при успешном вызове и использует код для получения маркера, а затем сведений о пользователе и обновления пользовательского интерфейса.

      ```JavaScript
      successCallback: function (result) { 
      … 
      } 
      ```

## <a name="see-also"></a>См. также

* [Настройка поставщиков удостоверений](~/concepts/authentication/authentication.md)
* [Проверка подлинности Microsoft Teams для вкладок](auth-flow-tab.md)
