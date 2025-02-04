---
title: API приложений для собраний
author: v-sdhakshina
description: В этой статье описаны ссылки на API приложений для собраний, доступные для клиентских приложений Teams и пакета SDK Bot Framework, а также примеры, примеры кода и коды ответов.
ms.topic: conceptual
ms.author: lajanuar
ms.localizationpriority: medium
ms.date: 04/07/2022
ms.openlocfilehash: f3d44317dbc8ea317e8fe3c5bdeb19404df75265
ms.sourcegitcommit: 10debe0f01574a21aab54bfac692a4c8373263a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2022
ms.locfileid: "68789865"
---
# <a name="meeting-apps-apis"></a>API приложений для собраний

Расширяемость собраний предоставляет API-интерфейсы для улучшения взаимодействия с собраниями. С помощью перечисленных API можно выполнить следующее:

* Создавать приложения или интегрировать существующие приложения в жизненные циклы собраний.
* Использовать API для информирования вашего приложения о собрании.
* Выбрать необходимые API для улучшения работы с собраниями.

> [!NOTE]
> Используйте [пакет SDK для JavaScript](/javascript/api/overview/msteams-client?view=msteams-client-js-latest&preserve-view=true) для Teams (*версия*: 1.10 и более поздние), для работы единого входа на боковой панели собрания.

В следующей таблице представлен список API, доступных в библиотеке JavaScript в Microsoft Teams и пакетах SDK Microsoft Bot Framework:

|Метод| Описание| Источник|
|---|---|----|
|[**Получить пользовательский контекст**](#get-user-context-api)| Получите контекстную информацию для отображения соответствующего содержимого на вкладке Microsoft Teams.| [Пакет SDK библиотеки JavaScript для Microsoft Teams](/microsoftteams/platform/tabs/how-to/access-teams-context#get-context-by-using-the-microsoft-teams-javascript-library) |
|[**Получение участника**](#get-participant-api)| Получить информацию об участнике по идентификатору собрания и идентификатору участника. | [Пакет SDK для Microsoft Bot Framework](/dotnet/api/microsoft.bot.builder.teams.teamsinfo.getmeetingparticipantasync?view=botbuilder-dotnet-stable&preserve-view=true)
|[**Отправить уведомление о собрании**](#send-an-in-meeting-notification)| Предоставление сигналов собрания с помощью существующего API уведомлений о беседе для чата пользователя с ботом позволяет уведомлять пользователя о действиях, отображая уведомление в собрании. | [Пакет SDK для Microsoft Bot Framework](/dotnet/api/microsoft.bot.builder.teams.teamsactivityextensions.teamsnotifyuser?view=botbuilder-dotnet-stable&preserve-view=true) |
|[**Получить сведения о собрании**](#get-meeting-details-api)| Получите статические метаданные собрания. | [Пакет SDK для Microsoft Bot Framework](/dotnet/api/microsoft.bot.builder.teams.teamsinfo.getmeetinginfoasync?view=botbuilder-dotnet-stable&preserve-view=true) |
|[**Отправляйте субтитры в реальном времени**](#send-real-time-captions-api)| Отправка субтитров к текущему собранию в режиме реального времени. | [Пакет SDK библиотеки JavaScript для Microsoft Teams](/azure/cognitive-services/speech-service/speech-sdk?tabs=nodejs%2Cubuntu%2Cios-xcode%2Cmac-xcode%2Candroid-studio#get-the-speech-sdk&preserve-view=true) |
|[**Делитесь содержимым приложения на сцене**](build-apps-for-teams-meeting-stage.md#share-app-content-to-stage-api)| Поделитесь определенными частями приложения на сцене собрания с боковой панели приложения в собрании. | [Пакет SDK библиотеки JavaScript для Microsoft Teams](/javascript/api/@microsoft/teams-js/meeting) |
|[**Получать события собраний Teams в режиме реального времени**](#get-real-time-teams-meeting-events-api)|Узнавать о событиях собраний в режиме реального времени, таких как фактическое время начала и окончания.| [Пакет SDK для Microsoft Bot Framework](/dotnet/api/microsoft.bot.builder.teams.teamsactivityhandler.onteamsmeetingstartasync?view=botbuilder-dotnet-stable&preserve-view=true) |
| [**Получение состояния входящего звука**](#get-incoming-audio-state) | Позволяет приложению получить параметр состояния входящего звука для пользователя собрания.| [Пакет SDK библиотеки JavaScript для Microsoft Teams](/javascript/api/@microsoft/teams-js/microsoftteams.meeting?view=msteams-client-js-latest&preserve-view=true) |
| [**Переключение входящего звука**](#toggle-incoming-audio) | Позволяет приложению переключать параметр состояния входящего звука для пользователя собрания с включения звука или наоборот.| [Пакет SDK библиотеки JavaScript для Microsoft Teams](/javascript/api/@microsoft/teams-js/microsoftteams.meeting?view=msteams-client-js-latest&preserve-view=true) |

## <a name="get-user-context-api"></a>Получить API пользовательского контекста

Как определить и получить контекстную информацию для содержимого вкладки, можно узнать в разделе[Получение контекста вкладки Teams](../tabs/how-to/access-teams-context.md#get-context-by-using-the-microsoft-teams-javascript-library). `meetingId` используется вкладкой, работающей в контексте собрания, и добавляется для полезных данных ответа.

## <a name="get-participant-api"></a>Получить API участника

API `GetParticipant` должен иметь регистрацию бота и идентификатор для создания токенов авторизации Подробнее в разделе [Регистрация и идентификатор бота.](../build-your-first-app/build-bot.md).

> [!NOTE]
>
> * Тип пользователя не включен в API **getParticipantRole** .
> * Не кэшируйте роли участников, так как организатор собрания может изменить роли в любое время.
> * Сейчас API `GetParticipant` поддерживается только для списков рассылки или реестров до 350 участников.

### <a name="query-parameters"></a>Параметры запроса

> [!TIP]
> Получите идентификаторы участников и арендаторов на [вкладке проверки подлинности единого входа](../tabs/how-to/authentication/tab-sso-overview.md).

API `Meeting` должен иметь `meetingId`, `participantId` и `tenantId` в качестве параметров URL-адреса. Параметры доступны как часть пакета SDK клиента Teams и действия бота.

В следующей таблице приведены параметры запроса:

|Значение|Тип|Обязательный|Описание|
|---|---|----|---|
|**meetingId**| String | Да | Идентификатор собрания доступен через Bot Invoke и Teams Client SDK.|
|**participantId**| String | Да | Идентификатор участника — это идентификатор пользователя. Он доступен в Tab SSO, Bot Invoke и Teams Client SDK. Рекомендуется получить идентификатор участника из Tab SSO. |
|**tenantId**| Строка | Да | Идентификатор клиента требуется для пользователей клиентов. Он доступен в Tab SSO, Bot Invoke и Teams Client SDK. Рекомендуется получить идентификатор клиента из Tab SSO. |

### <a name="example"></a>Пример

# <a name="c"></a>[C#](#tab/dotnet)

```csharp
protected override async Task OnMessageActivityAsync(ITurnContext<IMessageActivity> turnContext, CancellationToken cancellationToken)
{
  TeamsMeetingParticipant participant = await TeamsInfo.GetMeetingParticipantAsync(turnContext, "yourMeetingId", "yourParticipantId", "yourParticipantTenantId").ConfigureAwait(false);
  TeamsChannelAccount member = participant.User;
  MeetingParticipantInfo meetingInfo = participant.Meeting;
  ConversationAccount conversation = participant.Conversation;

  await turnContext.SendActivityAsync(MessageFactory.Text($"The participant role is: {meetingInfo.Role}"), cancellationToken);
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```typescript
export class MyBot extends TeamsActivityHandler {
    constructor() {
        super();
        this.onMessage(async (context, next) => {
            TeamsMeetingParticipant participant = getMeetingParticipant(turnContext, "yourMeetingId", "yourParticipantId", "yourTenantId");
            let member = participant.user;
            let meetingInfo = participant.meeting;
            let conversation = participant.conversation;
            
            await context.sendActivity(`The participant role is: '${meetingInfo.role}'`);
            await next();
        });
    }
}
```

# <a name="json"></a>[JSON](#tab/json)

```http
GET /v1/meetings/{meetingId}/participants/{participantId}?tenantId={tenantId}
```

```json
{
   "user":{
      "id":"29:1JKiJGPAX9TTxtGxhVo0wLx_zwzo-gG8Z-X03306vBwi9p-xMTEbDXsT6KH7-0kkTS8cD-2zkrsoV6f5WJ6_aYw",
      "aadObjectId":"e236c4bf-88b1-4f3a-b1d7-8891dfc332b5",
      "name":"Bob Young",
      "givenName":"Bob",
      "surname":"Young",
      "email":"Bob.young@microsoft.com",
      "userPrincipalName":"Bob.young@microsoft.com",
      "tenantId":"2fe477ab-0efc-4dfd-bde2-484374e2c373",
      "userRole":"user"
   },
   "meeting":{
      "role ":"Presenter",
      "inMeeting":true
   },
   "conversation":{
      "id":"<conversation id>",
      "conversationType": "groupChat", 
      "isGroup":true
   }
}
```

---

| Имя свойства | Описание |
|---|---|
| **user.id** | Идентификатор пользователя. |
| **user.aadObjectId** | Идентификатор объекта Azure Active Directory пользователя. |
| **user.name** | Имя пользователя. |
| **user.givenName** | Имя пользователя.|
| **user.surname** | Фамилия пользователя. |
| **user.email** | Идентификатор почты пользователя. |
| **user.userPrincipalName** | Имя участника-пользователя. |
| **user.tenantId** | Идентификатор клиента Azure Active Directory. |
| **user.userRole** | Роль пользователя. Например, "admin" или "user". |
| **meeting.role** | Роль участника в собрании. Например, "Организатор", "Выступающий" или "Участник". |
| **meeting.inMeeting** | Значение, указывающее, находится ли участник в собрании. |
| **conversation.id** | Идентификатор чата собрания. |
| **conversation.isGroup** | Логическое значение, указывающее, имеет ли беседа более двух участников. |

### <a name="response-codes"></a>Коды ответа

В следующей таблице приведены коды ответов:

|Код ответа|Описание|
|---|---|
| **403** | Полученные сведения об участниках не передаются приложению. Если приложение не установлено в собрании, оно вызывает ошибочный отклик 403. Если администратор клиента отключает или блокирует приложение во время динамической миграции сайта, это вызывает ошибочный отклик 403. |
| **200** | Сведения об участнике успешно извлечены.|
| **401** | Приложение отвечает недопустимым токеном|
| **404** | Срок действия собрания истек или участники недоступны.|

## <a name="send-an-in-meeting-notification"></a>Отправить уведомление о собрании

Все пользователи на собрании получают уведомления, отправленные с помощью полезных данных уведомлений на собрании. Полезные данные уведомлений на собрании запускают уведомление на собрании и позволяют вам предоставлять сигналы собрания, которые доставляются с использованием существующего API-интерфейса уведомления о разговоре для чата пользователя с ботом. Вы можете отправить уведомление на собрании на основе действий пользователя. Эти полезные данные доступны через службы ботов.

> [!NOTE]
>
> * Когда вызывается уведомление о собрании, содержимое представляется в виде сообщения чата.
> * Сейчас отправка целевых уведомлений и поддержка веб-приложений не поддерживаются.
> * Вы должны вызвать функцию [submitTask()](../task-modules-and-cards/task-modules/task-modules-bots.md#submit-the-result-of-a-task-module) для автоматического закрытия после того, как пользователь выполнит действие в веб-представлении. Это требование для отправки приложения. Подробнее в статье [Модуль задач Teams SDK](/javascript/api/@microsoft/teams-js/microsoftteams.tasks?view=msteams-client-js-latest#submittask-string---object--string---string---&preserve-view=true).
> * Если вы хотите, чтобы ваше приложение поддерживало анонимных пользователей, полезная нагрузка начального запроса на вызов должна полагаться на `from.id` метаданные запроса`from` в объекте, а не на `from.aadObjectId` метаданные запроса. `from.id` — это идентификатор пользователя, а `from.aadObjectId` — идентификатор Microsoft Azure Active Directory (Azure AD) пользователя. Подробнее в разделе[использование модулей задач на вкладках и создание ](../task-modules-and-cards/task-modules/task-modules-tabs.md) и [ отправка модуля задач.](../messaging-extensions/how-to/action-commands/create-task-module.md?tabs=dotnet#the-initial-invoke-request).

### <a name="query-parameter"></a>Параметр запроса

В следующей таблице содержится параметр запроса:

|Значение|Тип|Обязательный|Описание|
|---|---|----|---|
|**conversationId**| Строка | Да | Идентификатор беседы доступен как часть вызова бота. |

### <a name="examples"></a>Примеры

`Bot ID` объявляется в манифесте, и бот получает объект результата.

> [!NOTE]
>
> * Параметр `completionBotId` `externalResourceUrl` является необязательным в запрашиваемом примере полезных данных
> * Параметры ширины и высоты `externalResourceUrl` должны быть в пикселях. Подробнее в [правилах разработки вкладок](design/designing-apps-in-meetings.md).
> * URL-адрес — это страница, которая загружается как `<iframe>` в уведомлении о собрании. Домен должен быть в массиве приложений `validDomains` в манифесте приложения.

# <a name="c"></a>[C#](#tab/dotnet)

```csharp
Activity activity = MessageFactory.Text("This is a meeting signal test");
activity.TeamsNotifyUser(true, "https://teams.microsoft.com/l/bubble/APP_ID?url=<url>&height=<height>&width=<width>&title=<title>&completionBotId=BOT_APP_ID");
await turnContext.SendActivityAsync(activity).ConfigureAwait(false);
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const replyActivity = MessageFactory.text('Hi'); // this could be an adaptive card instead
replyActivity.channelData = {
    notification: {
        alertInMeeting: true,
        externalResourceUrl: 'https://teams.microsoft.com/l/bubble/APP_ID?url=<url>&height=<height>&width=<width>&title=<title>&completionBotId=BOT_APP_ID’
    }
};
await context.sendActivity(replyActivity);
```

# <a name="json"></a>[JSON](#tab/json)

```http
POST /v3/conversations/{conversationId}/activities
```

```json

{
    "type": "message",
    "text": "John Phillips assigned you a weekly todo",
    "summary": "Don't forget to meet with Marketing next week",
    "channelData": {
        "notification": {
            "alertInMeeting": true,
            "externalResourceUrl": "https://teams.microsoft.com/l/bubble/APP_ID?url=<url>&height=<height>&width=<width>&title=<title>&<completionBotId>=<BOT_APP_ID>"
        }
    },
    "replyToId": "1493070356924"
}
```

---

| Имя свойства | Описание |
|---|---|
| **type** | Тип действия. |
| **text** | Текстовое содержимое сообщения. |
| **summary** | Сводный текст сообщения. |
| **channelData.notification.alertInMeeting** | Логическое значение, указывающее, должно ли отображаться уведомление пользователю во время собрания. |
| **channelData.notification.externalResourceUrl** | Значение URL-адреса внешнего ресурса уведомления.|
| **replyToId** | Идентификатор родительского или корневого сообщения потока. |
| **APP_ID** | Идентификатор приложения, объявленный в манифесте. |
| **completionBotId** | Идентификатор приложения бота |

### <a name="response-codes"></a>Коды ответа

В следующей таблице содержатся коды ответов:

|Код ответа|Описание|
|---|---|
| **201** | Действие с сигналом успешно отправлено. |
| **401** | Приложение отвечает недопустимым токеном |
| **403** | Приложению не удалось отправить сигнал. Код отклика 403 может возникать по разным причинам, например, когда администратор клиента отключает и аварийно завершает работу приложения во время динамической миграции сайта. В этом случае полезные данные содержат подробное сообщение об ошибке. |
| **404** | Чат собрания не существует. |

## <a name="get-meeting-details-api"></a>Получить API сведений о собрании

API сведений о собрании позволяет вашему приложению получать статические метаданные собрания. Метаданные предоставляют точки данных, которые не изменяются динамически. API доступен через службы ботов. Сейчас как частные запланированные или повторяющиеся собрания, так и запланированные или повторяющиеся собрания канала поддерживают API с различными разрешениями RSC соответственно.

API `Meeting Details` должен иметь регистрацию бота и идентификатор бота. Для получения `TurnContext` требуется bot SDK. Чтобы использовать API сведений о собрании, вы должны получить различные разрешения RSC в зависимости от области собрания, например закрытого собрания или собрания канала.

### <a name="prerequisite"></a>Предварительное условие

Чтобы использовать API сведений о собрании, вы должны получить различные разрешения RSC в зависимости от области собрания, например закрытого собрания или собрания канала.

<br>

<details>

<summary><b>Для манифеста приложения версии 1.12 и более поздних</b></summary>

Используйте следующий пример, чтобы настроить манифест `webApplicationInfo` и `authorization` свойства вашего приложения для любого индивидуального собрания:

```json
"webApplicationInfo": {
    "id": "<bot id>",
    "resource": "https://RscPermission",
},
"authorization": {
    "permissions": {
        "resourceSpecific": [
            {
                "name": "OnlineMeeting.ReadBasic.Chat",
                "type": "Application"
            }
        ]
    }
}
 ```

Используйте следующий пример, чтобы настроить манифест `webApplicationInfo` и `authorization` свойства вашего приложения для любого собрания канала:

```json
"webApplicationInfo": {
    "id": "<bot id>",
    "resource": "https://RscPermission",
},
"authorization": {
    "permissions": {
        "resourceSpecific": [
            {
                "name": "ChannelMeeting.ReadBasic.Group",
                "type": "Application"
            }
        ]
    }
}
 ```

<br>

</details>

<br>

<details>

<summary><b>Для манифеста приложения версии 1.11 и более ранних версий</b></summary>

Используйте следующий пример, чтобы настроить свойство `webApplicationInfo` манифеста приложения для любого индивидуального собрания:

```json
"webApplicationInfo": {
    "id": "<bot id>",
    "resource": "https://RscPermission",
    "applicationPermissions": [
      "OnlineMeeting.ReadBasic.Chat"
    ]
}
 ```

Используйте следующий пример для настройки свойства `webApplicationInfo` манифеста приложения для любого собрания канала:

```json
"webApplicationInfo": {
    "id": "<bot id>",
    "resource": "https://RscPermission",
    "applicationPermissions": [
      "ChannelMeeting.ReadBasic.Group"
    ]
}
 ```

<br>

</details>

> [!NOTE]
>
> * Бот может автоматически получать события начала или окончания собрания из всех собраний, созданных во всех каналах, путем добавления `ChannelMeeting.ReadBasic.Group` в манифест для разрешения RSC.
> * Для один-на-один вызов `organizer` является инициатором чата, а для групповых вызовов `organizer` — инициатором звонка. Для собраний в общедоступных каналах `organizer` — это пользователь, создавший запись канала.

### <a name="query-parameter"></a>Параметр запроса

В следующей таблице перечислены параметры запроса:

|Значение|Тип|Обязательный|Описание|
|---|---|----|---|
|**meetingId**| String | Да | Идентификатор собрания доступен через Bot Invoke и Teams Client SDK. |

### <a name="example"></a>Пример

# <a name="c"></a>[C#](#tab/dotnet)

```csharp
MeetingInfo result = await TeamsInfo.GetMeetingInfoAsync(turnContext);
await turnContext.SendActivityAsync(JsonConvert.SerializeObject(result));
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript

Not available

```

# <a name="json"></a>[JSON](#tab/json)

```http
GET /v1/meetings/{meetingId}
```

Текст отклика JSON для API сведений о собрании выглядит следующим образом:

* **Запланированные собрания:**

    ```json

    {
       "details":  { 
             "id": "<meeting ID>", 
             "msGraphResourceId": "MSowYmQ0M2I4OS1lN2QxLTQxNzAtOGZhYi00OWJjYjkwOTk1YWYqMCoqMTk6bWVldGluZ19OVEkyT0RjM01qUXROV1UyW", 
             "scheduledStartTime": "2022-04-24T22:00:00Z", 
             "scheduledEndTime": "2022-04-24T23:00:00Z", 
             "joinUrl": "https://teams.microsoft.com/l/xx", 
             "title": "All Hands", 
             "type": "Scheduled" 
         },
        "conversation": { 
             "isGroup": true, 
             "conversationType": "groupChat", 
             "id": "meeting chat ID" 
             }, 
        "organizer": { 
             "id": "<organizer user ID>", 
             "aadObjectId": "<AAD object ID>",
             "objectId": "<organizer object ID>",
             "tenantId": "<Tenant ID>" 
         }
    } 
    ```

* **Запланированные собрания каналов:**

    ```json
    { 
        "details": { 
        "msGraphResourceId": "MSoxNmUwYjdiYi05M2Q1LTQzNTItOTllMC0yM2VlNWYyZmZmZTIqMTY2MDc1ODYwNzc0MCoqMTk6a0RtQkpEWFZsYWl0QWhHcVB2SzBtRExZbHVTWnJub01WX1MxeFNkTjQxNDFAdGhyZWFkLnRhY3Yy", 
        "scheduledStartTime": "2022-08-17T18:00:00Z", 
        "scheduledEndTime": "2022-08-17T18:30:00Z", 
        "type": "ChannelScheduled", 
        "id": "MCMxOTprRG1CSkRYVmxhaXRBaEdxUHZLMG1ETFlsdVNacm5vTVZfUzF4U2RONDE0MUB0aHJlYWQudGFjdjIjMTY2MDc1ODYwNzc0MA==", 
        "joinUrl": "https://teams.microsoft.com/l/meetup-join/19%3akDmBJDXVlaitAhGqPvK0mDLYluSZrnoMV_S1xSdN4141%40thread.tacv2/1660758607740?context=%7b%22Tid%22%3a%229f044231-b634-4bdd-b29d-2776e3dbd699%22%2c%22Oid%22%3a%2216e0b7bb-93d5-4352-99e0-23ee5f2fffe2%22%7d", 
        "title": "Test channel meeting"
    }, 
    "conversation": { 
        "isGroup": true, 
        "conversationType": "channel", 
        "id": "19:kDmBJDXVlaitAhGqPvK0mDLYluSZrnoMV_S1xSdN4141@thread.tacv2;messageid=1660758607740"
    }, 
    "organizer": { 
        "tenantId": "9f044231-b634-4bdd-b29d-2776e3dbd699", 
        "objectId": "16e0b7bb-93d5-4352-99e0-23ee5f2fffe2", 
        "id": "29:1q4D6ekLXEAALkrqyLXUIcwtVSdXx31bf6vMdfahmkTb9euYVYSsN9x4133pXLV_I2idpVriFe40e19XEZt57bQ", 
        "aadObjectId": "16e0b7bb-93d5-4352-99e0-23ee5f2fffe2"
    }
    }
    ```

* **1-на-один:**

    ```json
    {
        "details": {
             "id": "<meeting ID>",
             "type": "OneToOneCall"
         },
        "conversation": {
             "isGroup": true,
             "conversationType": "groupChat",
             "id": "meeting chat ID"
         },
        "organizer  ": {
             "id": "<organizer user ID>",
             "aadObjectId": "<AAD object ID>",
             "objectId": "<organizer object ID>",
             "tenantId": "<Tenant ID>" 
         }
    }
    
    ```

* **Групповые вызовы:**

    ```json
    {
        "details": {
             "id": "<meeting ID>",
             "type": "GroupCall",
             "joinUrl": "https://teams.microsoft.com/l/xx"
         },
        "conversation": {
             "isGroup": true,
             "conversationType": "groupChat",
             "id": "meeting chat ID"
         },
        "organizer": {
             "id": "<organizer user ID>",
             "objectId": "<organizer object ID>",
             "aadObjectId": "<AAD object ID>",
             "tenantId": "<Tenant ID>" 
         }
    }
    
    ```

* **Мгновенные собрания:**

    ```json
    { 
       "details": { 
             "id": "<meeting ID>", 
             "msGraphResourceId": "MSowYmQ0M2I4OS1lN2QxLTQxNzAtOGZhYi00OWJjYjkwOTk1YWYqMCoqMTk6bWVldGluZ19OVEkyT0RjM01qUXROV1UyW", 
             "scheduledStartTime": "2022-04-24T22:00:00Z", 
             "scheduledEndTime": "2022-04-24T23:00:00Z", 
             "joinUrl": "https://teams.microsoft.com/l/xx", 
             "title": "All Hands", 
             "type": "MeetNow" 
         }, 
        "conversation": { 
             "isGroup": true, 
             "conversationType": "groupChat", 
             "id": "meeting chat ID" 
         },
        "organizer": { 
             "id": "<organizer user ID>", 
             "aadObjectId": "<AAD object ID>", 
             "tenantId": "<Tenant ID>" ,
             "objectId": "<organizer object ID>"
         }
    }
    
    ```

---

| Имя свойства | Описание |
|---|---|
| **details.id** | Идентификатор собрания, закодированный как строка BASE64. |
| **details.msGraphResourceId** | MsGraphResourceId, используемый специально для вызовов MS API Graph. |
| **details.scheduledStartTime** | Запланированное время начала собрания в формате UTC. |
| **details.scheduledEndTime** | Запланированное время окончания собрания в формате UTC. |
| **details.joinUrl** | URL-адрес, используемый для присоединения к собранию. |
| **details.title** | Название собрания. |
| **details.type** | Тип собрания (OneToOneCall, GroupCall, Scheduled, Повторяющийся, MeetNow, ChannelScheduled и ChannelRecurring). |
| **conversation.isGroup** | Логическое значение, указывающее, имеет ли беседа более двух участников. |
| **conversation.conversationType** | Тип беседы. |
| **conversation.id** | Идентификатор чата собрания. |
| **organizer.id** | Идентификатор пользователя организатора. |
| **organizer.aadObjectId** | Идентификатор объекта Azure Active Directory организатора. |
| **organizer.tenantId** | Идентификатор клиента Azure Active Directory организатора. |

В случае типа повторяющегося собрания

**startDate**: указывает дату начала применения шаблона. Значение startDate должно соответствовать значению даты свойства start ресурса события. Обратите внимание, что первое собрание может не произойти в эту дату, если оно не соответствует шаблону.

**endDate**: указывает дату прекращения применения шаблона. Обратите внимание, что последнее вхождение собрания может не произойти в эту дату, если оно не соответствует шаблону.

## <a name="send-real-time-captions-api"></a>API отправки субтитров в режиме реального времени

API отправки субтитров в режиме реального времени предоставляет конечную точку POST для субтитров перевода в режиме реального времени (CART) Teams, закрытых субтитров с человеческим типом. Текстовое содержимое, отправляемое в эту конечную точку, отображается конечным пользователям в собрании Teams, если для них включены субтитры.

### <a name="cart-url"></a>URL-адрес CART

ВЫ можете получить URL-адрес CART для конечной точки POST на странице **Параметры собрания** в собрании Teams. Подробнее в разделе [Субтитры CART на собрании Microsoft Teams](https://support.microsoft.com/office/use-cart-captions-in-a-microsoft-teams-meeting-human-generated-captions-2dd889e8-32a8-4582-98b8-6c96cf14eb47). Вам не нужно изменять URL-адрес CART, чтобы использовать субтитры CART.

#### <a name="query-parameter"></a>Параметр запроса

URL-адрес CART включает следующие параметры запроса:

|Значение|Тип|Обязательный|Описание|
|---|---|----|----|
|**meetingId**| String | Да |Идентификатор собрания доступен через Bot Invoke и Teams Client SDK. <br/>Например, meetingid=%7b%22tId%22%3a%2272f234bf-86f1-41af-91ab-2d7cd0321b47%22%2c%22oId%22%3a%22e071f268-4241-47f8-8cf3-fc6b84437f23%22%2c%22thId%22%3a%2219%3ameeting_NzJiMjNkMGQtYzk3NS00ZDI1LWJjN2QtMDgyODVhZmI3NzJj%40thread.v2%22%2c%22mId%22%3a%220%22%7d|
|**токен**| String | Да |Токен авторизации<br/> Например, token=04751eac |

#### <a name="example"></a>Пример

```http
https://api.captions.office.microsoft.com/cartcaption?meetingid=%7b%22tId%22%3a%2272f234bf-86f1-41af-91ab-2d7cd0321b47%22%2c%22oId%22%3a%22e071f268-4241-47f8-8cf3-fc6b84437f23%22%2c%22thId%22%3a%2219%3ameeting_NzJiMjNkMGQtYzk3NS00ZDI1LWJjN2QtMDgyODVhZmI3NzJj%40thread.v2%22%2c%22mId%22%3a%220%22%7d&token=gjs44ra
```

### <a name="method"></a>Метод

|Ресурс|Метод|Описание|
|----|----|----|
|/cartcaption|POST|Обработка субтитров для собрания, которое было начато|

> [!NOTE]
> Убедитесь, что тип содержимого для всех запросов — обычный текст в кодировке UTF-8. Текст запроса содержит только субтитры.

#### <a name="example"></a>Пример

```http
POST /cartcaption?meetingid=04751eac-30e6-47d9-9c3f-0b4ebe8e30d9&token=04751eac&lang=en-us HTTP/1.1
Host: api.captions.office.microsoft.com
Content-Type: text/plain
Content-Length: 22
Hello I’m Cortana, welcome to my meeting. 
```

> [!NOTE]  
> Каждый запрос POST создает новую строку субтитров. Чтобы у конечного пользователя было достаточно времени для чтения содержимого, ограничьте текст каждого POST-запроса до 80–120 символов.

### <a name="error-codes"></a>Коды ошибок

В следующей таблице приведены коды ошибок:

|Код ошибки|Описание|
|---|---|
| **400** | Неправильный запрос. В тексте отклика есть дополнительные сведения. Например, представлены не все необходимые параметры.|
| **401** | Недостаточно полномочий. Неверный или просроченный токен. Если вы получаете эту ошибку, создайте новый URL-адрес CART в Teams. |
| **404** | Собрание не найдено или не началось. Если вы получили эту ошибку, убедитесь, что вы начали собрание и выберите начальные субтитры. После включения субтитров на собрании вы можете начать публиковать субтитры на собрании.|
| **500** |Внутренняя ошибка сервера. Для получения дополнительной информации [обратитесь в службу поддержки или оставьте отзыв](../feedback.md).|

## <a name="get-real-time-teams-meeting-events-api"></a>Получите API событий собраний Teams в реальном времени

> [!NOTE]
> События собраний Teams в режиме реального времени поддерживаются только для запланированных собраний.

Пользователь может получать информацию о собраниях в режиме реального времени. Как только приложение связывается с собранием, фактическое время начала и окончания собрания передается боту. Фактическое время начала и окончания собрания отличается от запланированного времени начала и окончания. API сведений о собрании предоставляет запланированное время начала и окончания. Событие предоставляет фактическое время начала и окончания.

Вы должны быть знакомы с объектом`TurnContext`, доступным через Bot SDK. Объект `Activity` в `TurnContext` содержит полезные данные с фактическим временем начала и окончания. Для проведения собраний в реальном времени требуется зарегистрированный идентификатор бота на платформе Teams. Бот может автоматически получать событие начала или окончания собрания, добавляя `ChannelMeeting.ReadBasic.Group` в манифест.

### <a name="prerequisite"></a>Предварительное условие

Манифест вашего приложения должен иметь свойство `webApplicationInfo` для получения событий начала и окончания собрания. Используйте следующие примеры для настройки манифеста:

<br>

<details>

<summary><b>Для манифеста приложения версии 1.12 и более поздних</b></summary>

```json
"webApplicationInfo": {
    "id": "<bot id>",
    "resource": "https://RscPermission",
    },
"authorization": {
    "permissions": {
        "resourceSpecific": [
            {
                "name": "OnlineMeeting.ReadBasic.Chat",
                "type": "Application"
            }
        ]    
    }
}
 ```

<br>

</details>

<br>

<details>

<summary><b>Для манифеста приложения версии 1.11 и более ранних версий</b></summary>

```json
"webApplicationInfo": {
    "id": "<bot id>",
    "resource": "https://RscPermission",
    "applicationPermissions": [
      "OnlineMeeting.ReadBasic.Chat"
    ]
}
 ```

<br>

</details>

### <a name="example-of-getting-meetingstartendeventvalue"></a>Пример получения `MeetingStartEndEventvalue`

Бот получает событие через обработчик `OnEventActivityAsync`. Чтобы десериализовать полезные данные JSON вводится объект модели для получения метаданных для собрания. Метаданные собрания в свойстве `value` в полезных данных события. Создается объект модели `MeetingStartEndEventvalue`, переменные элементы которого соответствуют ключам в свойстве `value` в полезных данных события.

> [!NOTE]
>
> * Получить идентификатор собрания от `turnContext.ChannelData`.
> * Не используйте идентификатор беседы в качестве идентификатора собрания.
> * Не используйте идентификатор собрания из полезных данных событий `turncontext.activity.value`.

В следующем коде показано, как захватить метаданные собрания `MeetingType`, `Title`, `Id`, `JoinUrl`, `StartTime` и `EndTime` из события начала и окончания собрания:

Событие начала собрания

```csharp
protected override async Task OnTeamsMeetingStartAsync(MeetingStartEventDetails meeting, ITurnContext<IEventActivity> turnContext, CancellationToken cancellationToken)
{
    await turnContext.SendActivityAsync(JsonConvert.SerializeObject(meeting));
}
```

Событие окончания собрания

```csharp
protected override async Task OnTeamsMeetingEndAsync(MeetingEndEventDetails meeting, ITurnContext<IEventActivity> turnContext, CancellationToken cancellationToken)
{
    await turnContext.SendActivityAsync(JsonConvert.SerializeObject(meeting));
}
```

### <a name="example-of-meeting-start-event-payload"></a>Пример полезных данных события начала собрания

В следующем коде приводится пример полезных данных события начала собрания:

```json
{ 
    "name": "application/vnd.microsoft.meetingStart", 
    "type": "event", 
    "timestamp": "2021-04-29T16:10:41.1252256Z", 
    "id": "123", 
    "channelId": "msteams", 
    "serviceUrl": "https://microsoft.com", 
    "from": { 
        "id": "userID", 
        "aadObjectId": "aadOnjectId" 
    }, 
    "conversation": { 
        "isGroup": true, 
        "tenantId": "tenantId", 
        "id": "thread id" 
    }, 
    "recipient": { 
        "id": "user Id", 
        "name": "user name" 
    }, 
    "entities": [ 
        { 
            "locale": "en-US", 
            "country": "US", 
            "type": "clientInfo" 
        } 
    ], 
    "channelData": { 
        "tenant": { 
            "id": "channel id" 
        }, 
        "source": null, 
        "meeting": { 
            "id": "meeting id" 
        } 
    }, 
    "value": { 
        "MeetingType": "Scheduled", 
        "Title": "Meeting Start/End Event", 
        "Id": "meeting id", 
        "JoinUrl": "url" 
        "StartTime": "2021-04-29T16:17:17.4388966Z" 
    }, 
    "locale": "en-US" 
}
```

### <a name="example-of-meeting-end-event-payload"></a>Пример полезных данных события окончания собрания

В следующем коде приводится пример полезных данных события окончания собрания:

```json
{ 
    "name": "application/vnd.microsoft.meetingEnd", 
    "type": "event", 
    "timestamp": "2021-04-29T16:17:17.4388966Z", 
    "id": "123", 
    "channelId": "msteams", 
    "serviceUrl": "https://microsoft.com", 
    "from": { 
        "id": "user id", 
        "aadObjectId": "aadObjectId" 
    }, 
    "conversation": { 
        "isGroup": true, 
        "tenantId": "tenantId", 
        "id": "thread id" 
    }, 
    "recipient": { 
        "id": "user id", 
        "name": "user name" 
    }, 
    "entities": [ 
        { 
            "locale": "en-US", 
            "country": "US", 
            "type": "clientInfo" 
        } 
    ], 
    "channelData": { 
        "tenant": { 
            "id": "channel id" 
        }, 
        "source": null, 
        "meeting": { 
            "id": "meeting Id" 
        } 
    }, 
    "value": { 
        "MeetingType": "Scheduled", 
        "Title": "Meeting Start/End Event in Canary", 
        "Id": "19:meeting_NTM3ZDJjOTUtZGRhOS00MzYxLTk5NDAtMzY4M2IzZWFjZGE1@thread.v2", 
        "JoinUrl": "url", 
        "EndTime": "2021-04-29T16:17:17.4388966Z" 
    }, 
    "locale": "en-US" 
}
```

| Имя свойства | Описание |
|---|---|
| **name** | Имя пользователя.|
| **type** | Тип действия. |
| **timestamp** | Локальная дата и время сообщения, выраженные в формате ISO-8601. |
| **id** | Идентификатор действия. |
| **channelId** | Канал, с которым связано это действие. |
| **serviceUrl** | URL-адрес службы, куда должны отправляться ответы на это действие. |
| **from.id** | Идентификатор пользователя, отправившего запрос. |
| **from.aadObjectId** | Идентификатор объекта Azure Active Directory пользователя, отправившего запрос. |
| **conversation.isGroup** | Логическое значение, указывающее, имеет ли беседа более двух участников. |
| **conversation.tenantId** | Идентификатор клиента Azure Active Directory беседы или собрания. |
| **conversation.id** | Идентификатор чата собрания. |
| **recipient.id** | Идентификатор пользователя, получающего запрос. |
| **recipient.name** | Имя пользователя, получающего запрос. |
| **entities.locale** | сущность, которая содержит метаданные о языковом стандарте. |
| **entities.country** | сущность, которая содержит метаданные о стране. |
| **entities.type** | сущность, которая содержит метаданные о клиенте. |
| **channelData.tenant.id** | Идентификатор клиента Azure Active Directory. |
| **channelData.source** | Имя источника, из которого запускается или вызывается событие. |
| **channelData.meeting.id** | Идентификатор по умолчанию, связанный с собранием. |
| **Значение. MeetingType** | Тип собрания. |
| **Значение. Название** | Тема собрания. |
| **Значение. Id** | Идентификатор по умолчанию, связанный с собранием. |
| **Значение. JoinUrl** | URL-адрес присоединения собрания. |
| **Значение. Starttime** | Время начала собрания в формате UTC. |
| **Значение. EndTime** | Время окончания собрания в формате UTC. |
| **locale**| Языковой стандарт сообщения, заданный клиентом. |

## <a name="get-incoming-audio-state"></a>Получение состояния входящего звука

`getIncomingClientAudioState` API позволяет приложению получить параметр состояния входящего звука для пользователя собрания. API доступен через клиентский SDK Teams.

> [!NOTE]
>
> * API `getIncomingClientAudioState` для мобильных устройств в настоящее время доступен в [общедоступной предварительной версии для разработчиков](../resources/dev-preview/developer-preview-intro.md).
> * Согласие на использование конкретного ресурса доступно для манифеста версии 1.12 и более поздних версий, поэтому этот API не работает для манифеста версии 1.11 и более ранних версий.

### <a name="manifest"></a>Манифест

```JSON
"authorization": {
    "permissions": {
      "resourceSpecific": [
        {
          "name": "OnlineMeetingParticipant.ToggleIncomingAudio.Chat",
          "type": "Delegated"
        }
      ]
    }
  }
```
  
### <a name="example"></a>Пример

```javascript
callback = (errcode, result) => {
        if (errcode) {
            // Handle error code
        }
        else {
            // Handle success code
        }
    }

microsoftTeams.meeting.getIncomingClientAudioState(this.callback)
```

### <a name="query-parameter"></a>Параметр запроса

В следующей таблице содержится параметр запроса:

|Значение|Тип|Обязательный|Описание|
|---|---|----|---|
|**callback**| String | Да | Обратный вызов содержит два параметра `error` и `result`. *Ошибка* может содержать тип `SdkError` ошибки или `null` при успешном получении звука. *Результат* может содержать значение true или false при успешном получении звука или значение NULL при сбое выборки звука. Входящий звук отключается, если результат имеет значение true, и отключается, если результат имеет значение false. |
  
### <a name="response-codes"></a>Коды ответа

В следующей таблице приведены коды ответов:

|Код ответа|Описание|
|---|---|
| **500** | Внутренняя ошибка. |
| **501** | API не поддерживается в текущем контексте.|
| **1000** | Приложение не имеет необходимых разрешений для предоставления общего доступа к этапу.|

## <a name="toggle-incoming-audio"></a>Переключение входящего звука

`toggleIncomingClientAudio` API позволяет приложению переключать параметр состояния входящего звука для пользователя собрания с включения звука или наоборот. API доступен через клиентский SDK Teams.

> [!NOTE]
>
> * API `toggleIncomingClientAudio` для мобильных устройств в настоящее время доступен в [общедоступной предварительной версии для разработчиков](../resources/dev-preview/developer-preview-intro.md).
> * Согласие на использование конкретного ресурса доступно для манифеста версии 1.12 и более поздних версий, поэтому этот API не работает для манифеста версии 1.11 и более ранних версий.

### <a name="manifest"></a>Манифест

```JSON
"authorization": {
 "permissions": {
  "resourceSpecific": [
   {
    "name": "OnlineMeetingParticipant.ToggleIncomingAudio.Chat",
    "type": "Delegated"
   }
  ]
 }
}
```

### <a name="example"></a>Пример

```javascript
callback = (error, result) => {
        if (error) {
            // Handle error code
        }
        else {
            // Handle success code
        }
    }

microsoftTeams.meeting.toggleIncomingClientAudio(this.callback)
```
  
### <a name="query-parameter"></a>Параметр запроса

В следующей таблице содержится параметр запроса:

|Значение|Тип|Обязательный|Описание|
|---|---|----|---|
|**callback**| String | Да | Обратный вызов содержит два параметра `error` и `result`. *Ошибка* может содержать тип `SdkError` ошибки или `null` при успешном выполнении переключателя. *Результат* может содержать значение true или false при успешном выполнении переключателя или значение NULL при сбое переключателя. Входящий звук отключается, если результат имеет значение true, и отключается, если результат имеет значение false.
  
### <a name="response-code"></a>Код ответа

В следующей таблице приведены коды ответов:

|Код ответа|Описание|
|---|---|
| **500** | Внутренняя ошибка. |
| **501** | API не поддерживается в текущем контексте.|
| **1000** | Приложение не имеет необходимых разрешений для предоставления общего доступа к этапу.|

## <a name="code-sample"></a>Пример кода

|Название примера | Описание | C# | Node.js |
|----------------|-----------------|--------------|--------------|
| Расширяемость собраний | Пример расширяемости собраний Teams для передачи маркеров. | [Просмотр](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meetings-token-app/csharp) | [View](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meetings-token-app/nodejs) |
| Бот пузырькового содержимого собрания | Пример расширяемости собраний Teams для взаимодействия с ботом пузырьков содержимого на собрании. | [Просмотр](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meetings-content-bubble/csharp) |  [Просмотр](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meetings-content-bubble/nodejs)|
| Боковая панель собрания | Пример расширяемости собраний Teams для взаимодействия с боковой панелью в собрании. | [Просмотр](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meetings-sidepanel/csharp) | [Просмотр](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meetings-sidepanel/nodejs)|
| Вкладка "Сведения" в собрании | Пример расширяемости собраний Teams для взаимодействия с вкладкой "Сведения" в собрании. | [Просмотр](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meetings-details-tab/csharp) | [Просмотр](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meetings-details-tab/nodejs)|
| Пример событий собрания | Пример приложения для отображения событий собраний Teams в реальном времени|[View](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meetings-events/csharp)|[Просмотр](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meetings-events/nodejs)|
| Образец собрания для набора сотрудников |Пример приложения для демонстрации опыта собраний для сценария набора сотрудников.|[View](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meeting-recruitment-app/csharp)|[Просмотр](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/meeting-recruitment-app/nodejs)|
| Установка приложения с помощью QR-кода |Пример приложения, которое создает QR-код и устанавливает приложение с помощью QR-кода|[View](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/app-installation-using-qr-code/csharp)|[Просмотр](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/app-installation-using-qr-code/nodejs)|

## <a name="see-also"></a>Дополнительные ресурсы

* [Проверка подлинности Microsoft Teams для вкладок](../tabs/how-to/authentication/auth-flow-tab.md)
* [Приложения для собраний Teams](teams-apps-in-meetings.md)
* [Пакет SDK Live Share](teams-live-share-overview.md)
* [Запись собрания Teams в облаке](/microsoftteams/cloud-recording)

## <a name="next-steps"></a>Дальнейшие действия

[Создание вкладок для собрания](build-tabs-for-meeting.md)
