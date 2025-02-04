---
title: Поделиться в Teams из личного приложения или вкладки
description: Узнайте, как включить кнопку "Поделиться в Teams" на личном приложении или вкладке, ограничениях и интерфейсе конечного пользователя.
ms.topic: reference
ms.localizationpriority: medium
ms.openlocfilehash: cd4de40fdb557300ad957df03f463a0879f44b0e
ms.sourcegitcommit: d92e14fad6567fe91fd52ee6c213836740316683
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2022
ms.locfileid: "67605063"
---
# <a name="share-to-teams-from-personal-app-or-tab"></a>Поделиться в Teams из личного приложения или вкладки

Общий доступ к Teams позволяет пользователям предоставлять общий доступ к содержимому из личного приложения или вкладки другому пользователю, группе или каналу в Teams. Пользователи могут выбрать "Поделиться в Teams", чтобы открыть интерфейс "Поделиться в Teams" во всплывающем окне. Всплывающее окно позволяет пользователям добавлять другого пользователя, группу или канал для совместного использования содержимого.

На следующем рисунке показано всплывающее окно "Поделиться в Teams":

:::image type="content" source="../../assets/images/share-to-teams/share-to-teams.PNG" alt-text="share-to-teams-pop-up (общий доступ к командам)":::

## <a name="enable-share-to-teams-button"></a>Кнопка "Разрешить общий доступ в Teams"

> [!NOTE]
> Убедитесь, что у вас есть [клиентский пакет SDK JavaScript для Microsoft Teams](../../tabs/how-to/using-teams-client-sdk.md) или клиентский [пакет SDK JavaScript версии 2](../../tabs/how-to/using-teams-client-sdk.md) (`@microsoft/teams-js@1.11.0-beta.7` или более поздней версии), чтобы включить общий доступ к Teams для личного приложения или вкладки.

Чтобы включить общий доступ к Teams:

1. Создайте личное приложение или вкладку с помощью **клиентского пакета SDK для Javascript для Teams**.

2. Кнопка **"Поделиться в Teams** ".

3. На кнопке "Поделиться в Teams" вызовите `microsoftTeams.sharing.shareWebContent` полезные данные содержимого.

В следующем примере объясняется, как создать полезные данные содержимого:

```json
microsoftTeams.sharing.shareWebContent({
        content: [
          {
            type: 'URL',
            url: '<URL to be shared>',
            message: 'Default message to be loaded in the compose box',
            preview: true
          }
        ]
      });
```

Полезные данные содержат следующие параметры:

| Имя свойства | Назначение |
|---|---|
| `type` | Тип должен быть `URL` |
| `url` | `URL` для совместного использования |
|`message`| Сообщение по умолчанию для загрузки в поле создания |
| `preview` | Настройка включения предварительного `true` просмотра URL-адресов |

На следующем рисунке показан параметр "Поделиться в Teams":

:::image type="content" source="../../assets/images/share-to-teams/share-button.PNG" alt-text="Кнопка &quot;Поделиться в teams&quot;":::

## <a name="response-codes"></a>Коды ответа

В следующей таблице приведены коды ответов:

|Код ответа|Описание|
|---|---|
| **100** | API не поддерживается на текущей платформе. |
| **404** | Указанный файл не найден в указанном расположении. |
| **500** | Внутренняя ошибка при выполнении необходимой операции. |
| **501** | API не поддерживается в текущем контексте. |
| **1000** | Разрешения, отклоненные пользователем. |
| **2000** | Проблема с сетью. |
| **3000** | Базовое оборудование не поддерживает эту возможность. |
| **4000** | Один или несколько недопустимых аргументов. |
| **5000** | Пользователь не авторизован для этой операции. |
| **6000** | Не удалось завершить операцию из-за нехватки ресурсов. |
| **7000** | Платформа регулирует запрос из-за слишком частого вызова API. |
| **8000** | Пользователь прерывает операцию. |
| **9000** | Код платформы является старым и не реализует этот API. |
| **10000** | Возвращаемое значение слишком велико и превысило границы нашего размера. |

## <a name="limitations"></a>Ограничения

Ограничения для добавления кнопки "Поделиться в Teams":

* Кнопку "Поделиться в Teams" можно разместить или встраить в приложение, работающее в Teams.
* Вы можете добавить кнопку "Поделиться в Teams" в приложение, созданное с помощью клиентского **пакета SDK javascript для Teams**.

## <a name="end-user-share-to-teams-experience"></a>Взаимодействие с конечным пользователем в Teams

После того как вы включите кнопку "Поделиться командой" на личном приложении или вкладке, вы сможете поделиться содержимым. Чтобы получить доступ, выполните следующие действия.

1. Откройте личное приложение или вкладку и выберите " **Поделиться в Teams"**.

    :::image type="content" source="../../assets/images/share-to-teams/share-button.PNG" alt-text="Кнопка &quot;Поделиться в teams&quot;":::

2. Добавьте другого пользователя, группу или канал, чтобы поделиться содержимым.

    :::image type="content" source="../../assets/images/share-to-teams/add-recepient.PNG" alt-text="add-recipient":::

3. Нажмите **Поделиться**.

   :::image type="content" source="../../assets/images/share-to-teams/add-notes.PNG" alt-text="add-note":::

4. Выберите **"Вид** ", чтобы связаться с беседой, в которой была предоставлена ссылка.

   :::image type="content" source="../../assets/images/share-to-teams/link-shared.PNG" alt-text="share-to-teams-link-shared":::

## <a name="see-also"></a>См. также

* [Поделиться в Teams из веб-приложений](share-to-teams-from-web-apps.md)
* [Создание личной вкладки](../../tabs/how-to/create-personal-tab.md)
