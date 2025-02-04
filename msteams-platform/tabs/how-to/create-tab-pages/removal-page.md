---
title: Создать страницу удаления вкладки
author: surbhigupta
description: Узнайте, как включить перенастройку вкладки после установки. Расширение взаимодействия с пользователем путем поддержки параметров удаления и изменения в приложении Microsoft Teams.
ms.localizationpriority: high
ms.topic: conceptual
ms.author: lajanuar
ms.openlocfilehash: 423cc386ca416fe116eb0bcb62c1238cae5547ff
ms.sourcegitcommit: 9ea9a70d2591bce6b8c980d22014e160f7b45f91
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/02/2022
ms.locfileid: "68819942"
---
# <a name="create-a-removal-page"></a>Создать страницу удаления

Вы можете расширить и улучшить взаимодействие с пользователем, поддерживая параметры удаления и изменения в своем приложении. Teams позволяет пользователям переименовывать или удалять вкладку канала или группы, и вы можете разрешить пользователям изменять конфигурацию вкладки после установки. Кроме того, функция удаления вкладки предоставляет пользователям возможность после удаления удалить или заархивировать содержимое.

[!INCLUDE [sdk-include](~/includes/sdk-include.md)]

## <a name="enable-your-tab-to-be-reconfigured-after-installation"></a>Включить перенастройку вкладки после установки

`manifest.json` определяет функции и возможности вкладки. Свойство экземпляра `canUpdateConfiguration` табуляции принимает логическое значение, указывающее, может ли пользователь изменить или перенастроить вкладку после ее создания. В следующей таблице приведены сведения о свойстве:

|Имя| Тип| Максимальный размер | Обязательный | Описание|
|---|---|---|---|---|
|`canUpdateConfiguration`|Логический|||Значение, указывающее, может ли пользователь обновлять конфигурацию вкладки после ее создания. Значение по умолчанию: `true`. |

Когда вкладка загружается в канал или групповой чат, Teams добавляет раскрывающееся меню правой кнопкой мыши для вкладки. Доступные параметры определяются параметром `canUpdateConfiguration`. В следующей таблице приведены сведения о настройках:

| `canUpdateConfiguration`| true   | false | description |
| ----------------------- | :----: | ----- | ----------- |
|     Settings            |   √    |       |Страница `configurationUrl` перезагружается в iFrame, что позволяет пользователю перенастроить вкладку. |
|     Переименовать              |   √    |   √   | Пользователь может изменить отображение имени вкладки на панели вкладок.          |
|     Удалить              |   √    |   √   |  `removeURL` Если свойство и значение включены на **страницу конфигурации**, **страница удаления** загружается в iFrame и представляется пользователю. Если страница удаления не включена, пользователю будет предоставлено диалоговое окно подтверждения.          |

## <a name="create-a-tab-removal-page-for-your-application"></a>Создайте страницу удаления вкладки для своего приложения

Необязательная страница удаления — это HTML-страница, которая отображается при удалении вкладки. URL-адрес страницы удаления определяется методом `setConfig()` (или `setSettings()` до TeamsJS версии 2.0.0) на странице конфигурации. Как и все страницы в приложении, страница удаления должна соответствовать [предварительным требованиям вкладки Teams](../../../tabs/how-to/tab-requirements.md).

### <a name="register-a-remove-handler"></a>Зарегистрировать обработчик удаления

При необходимости в логике страницы удаления можно вызвать `registerOnRemoveHandler((RemoveEvent) => {}` обработчик событий, когда пользователь удаляет существующую конфигурацию вкладки. Метод принимает интерфейс [`RemoveEvent`](/javascript/api/@microsoft/teams-js/pages.config.removeevent?view=msteams-client-js-latest&preserve-view=true) и выполняет код в обработчике, когда пользователь пытается удалить содержимое. Метод используется для выполнения операций очистки, таких как удаление базового ресурса, питающего содержимое вкладки. Одновременно можно зарегистрировать только один обработчик удаления.

Интерфейс `RemoveEvent` описывает объект двумя способами:

* Функция`notifySuccess()` обязательна. Это указывает на то, что удаление базового ресурса прошло успешно и его содержимое может быть удалено.

* Функция `notifyFailure(string)` необязательна. Это означает, что удаление базового ресурса завершилось сбоем и его содержимое не может быть удалено. Необязательный параметр строки указывает причину сбоя. Если эта строка задана, она отображается для пользователя; иначе отображается общая ошибка.

#### <a name="use-the-getconfig-function"></a>Использование функции `getConfig()`

Вы можете использовать `getConfig()` (ранее `getSettings()`) для назначения содержимого вкладки для удаления. Функция `getConfig()` возвращает обещание, которое разрешается с объектом Config и предоставляет допустимые значения свойств параметров, которые можно извлечь.

#### <a name="use-the-getcontext-function"></a>Использование функции `getContext()`

Вы можете использовать `getContext()` для получения текущего контекста, в котором запущен фрейм. Функция `getContext()` возвращает обещание, которое будет разрешаться с помощью объекта Context. Объект Context предоставляет допустимые `Context` значения свойств, которые можно использовать в логике страницы удаления для определения содержимого, отображаемого на странице удаления.

#### <a name="include-authentication"></a>Включить проверку подлинности

Перед тем, как разрешить пользователю удалять содержимое вкладки, требуется проверка подлинности. Контекстные сведения можно использовать для создания запросов аутентификации и URL-адресов страниц авторизации. См. [Поток проверки подлинности Microsoft Teams для вкладок](~/tabs/how-to/authentication/auth-flow-tab.md) Убедитесь, что все домены, используемые на страницах вкладок, перечислены в массиве `validDomains` манифеста приложения.

Ниже приведен образец блока кода удаления вкладки:

# <a name="teamsjs-v2"></a>[TeamsJS версии 2](#tab/teamsjs-v2)

```html
<body>
  <button onclick="onClick()">Delete this tab and all underlying data?</button>
  <script>
    await microsoftTeams.app.initialize();
    pages.config.registerOnRemoveHandler((removeEvent) => {
      // Here you can designate the tab content to be removed and/or archived.
        const configPromise = pages.getConfig();
        configPromise.
            then((configuration) => {
                configuration.contentUrl = "...";
                removeEvent.notifySuccess()}).
            catch((error) => {removeEvent.notifyFailure("failure message")});
    });

    const onClick() => {
        pages.config.setValidityState(true);
    }
  </script>
</body>
```

# <a name="teamsjs-v1"></a>[TeamsJS версии 1](#tab/teamsjs-v1)

```html
<body>
  <button onclick="onClick()">Delete this tab and all underlying data?</button>
  <script>
    microsoftTeams.initialize();
    microsoftTeams.settings.registerOnRemoveHandler((removeEvent) => {
      // Here you can designate the tab content to be removed and/or archived.
        microsoftTeams.settings.getSettings((settings) => {
        settings.contentUrl = "..."
        });
        removeEvent.notifySuccess();
    });

    const onClick() => {
        microsoftTeams.settings.setValidityState(true);
    }
  </script>
</body>
```

***

Когда пользователь выбирает **Удалить** в раскрывающемся меню вкладки, Teams загружает необязательную `removeUrl` страницу, назначенную **на странице конфигурации**, в iFrame. Пользователю отображается кнопка, загруженная с `onClick()` функцией, которая вызывает `pages.config.setValidityState(true)` и включает кнопку **Удалить** , показанную в нижней части страницы удаления iFrame.

После действия обработчика удаления`removeEvent.notifySuccess()` или `removeEvent.notifyFailure()` уведомления Teams о результатах удаления содержимого.

>[!NOTE]
>
> * Чтобы гарантировать, что контроль авторизованного пользователя над вкладкой не запрещен, Teams удаляет вкладку как в случае успеха, так и в случае сбоя.
> * После вызова обработчика событий `registerOnRemoveHandler` у вас будет 15 секунд, чтобы отреагировать на метод. По умолчанию Teams включает кнопку **Удалить** через пять секунд, даже если вы не вызываете `setValidityState(true)`.
> * Когда пользователь выбирает **Удалить**, Teams удаляет вкладку через 30 секунд независимо от того, завершены ли действия.

## <a name="next-step"></a>Следующий этап

> [!div class="nextstepaction"]
> [Вкладки на мобильных устройствах](~/tabs/design/tabs-mobile.md)

## <a name="see-also"></a>Дополнительные ресурсы

* [Создание вкладок для Teams](../../what-are-tabs.md)
* [Схема манифеста для Teams](../../../resources/schema/manifest-schema.md)
* [Интерфейс RemoveEvent](/javascript/api/@microsoft/teams-js/pages.config.removeevent)
* [Получение контекста для вкладки](../access-teams-context.md)
* [Создание личной вкладки](../create-personal-tab.md)
* [Создание вкладки канала или вкладки группы](../create-channel-group-tab.md)
