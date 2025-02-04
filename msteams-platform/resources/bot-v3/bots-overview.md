---
title: Добавление ботов в приложения Microsoft Teams
description: В этом модуле вы узнаете, как приступить к разработке ботов в Microsoft Teams и каковы все требования для добавления бота в Teams
ms.topic: conceptual
ms.localizationpriority: medium
ms.date: 05/20/2018
ms.openlocfilehash: a1563d72ada21810393d7a0118b5a2b94463a27b
ms.sourcegitcommit: 69a45722c5c09477bbff3ba1520e6c81d2d2d997
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2022
ms.locfileid: "67312214"
---
# <a name="add-bots-to-microsoft-teams-apps"></a>Добавление ботов в приложения Microsoft Teams

[!include[v3-to-v4-SDK-pointer](~/includes/v3-to-v4-pointer-bots.md)]

Создавайте и подключайте интеллектуальных ботов для естественного взаимодействия с пользователями Microsoft Teams через чат. Или предоставьте простого бота на основе команд, который будет использоваться в качестве интерфейса «командной строки» для более широкого взаимодействия с приложением Teams. Вы можете создать бота только для уведомлений, который может передавать сведения, относящиеся к вашим пользователям, непосредственно в канале или личном сообщении. Вы даже можете использовать существующего бота на основе Bot Framework и добавить поддержку для Teams, чтобы улучшить вашу функцию.

> [!IMPORTANT]
> В настоящее время боты доступны в облаке сообщества для государственных организаций (GCC) и GCC-High но недоступны в Министерства обороны США (DOD).

:::image type="content" source="../../assets/images/bot_example.png" alt-text="Пример бота, помогающего пользователю":::

## <a name="what-you-need-to-know-bots"></a>Что необходимо знать о ботах

Бот отображается так же, как и любой другой участник группы, с которым вы взаимодействуете в беседе, за исключением того, что он имеет шестиугольный значок аватара и всегда находится в сети.

Бот ведет себя по-разному в зависимости от типа беседы, в которых он участвует. Боты в Teams поддерживают несколько видов бесед, называемых областями в [манифесте приложения](~/resources/schema/manifest-schema.md).

* `teams`: также называются беседами каналов.
* `personal`: беседы между ботом и одним пользователем.
* `groupChat` Беседа между ботом и двумя или более пользователями.

Дополнительные сведения см. в статье [Беседа с ботом Microsoft Teams](~/resources/bot-v3/bot-conversations/bots-conversations.md).

С помощью приложений Teams вы можете сделать бот звездой вашего интерфейса или просто вспомогательным средством. Боты распространяются как часть более широкого пакета приложений, который может включать в себя другие возможности, такие как [вкладки](~/tabs/what-are-tabs.md) или [расширения для сообщений](~/messaging-extensions/what-are-messaging-extensions.md).

## <a name="bot-apis"></a>Интерфейсы API бота

Teams поддерживает большую [часть Microsoft Bot Framework.](https://dev.botframework.com/) (Если у вас уже есть бот, основанный на Bot Framework, вы можете легко адаптировать его для работы в Teams.) Мы рекомендуем использовать C# или Node.js, чтобы воспользоваться нашими [пакетами SDK](/microsoftteams/platform/#pivot=sdk-tools). Эти пакеты расширяют базовые классы и методы пакета SDK Bot Builder:

* Использование специальных типов карточек, например карточек соединителя Office 365.
* Использование и настройка данных каналов Teams для действий.
* Обработка запросов на расширение для работы с сообщениями.

Расширения SDK устанавливают зависимости, включая пакет SDK Bot Builder.

* **.NET** Чтобы использовать расширения Microsoft Teams для пакета SDK Bot Builder для .NET, установите пакет NuGet [Microsoft.Bot.Connector.Teams](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams) в проекте Visual Studio. Для разработки Node.js функция BotBuilder для Microsoft Teams была включена в [пакет SDK Bot Framework](https://github.com/microsoft/botframework-sdk), начиная с версии 4.6.

> [!IMPORTANT]
> Вы можете разрабатывать приложения Teams с помощью любой другой технологии веб-программирования и напрямую вызывать [REST API Bot Framework](/bot-framework/rest-api/bot-framework-rest-overview), но вы должны выполнять всю обработку маркеров самостоятельно.

*Портал разработчика для Teams* помогает создавать и настраивать манифест приложения, а также может создавать бот Bot Framework для вас. Он также содержит библиотеку управления React и интерактивный построитель карточек.

## <a name="outgoing-webhooks"></a>Исходящие веб-перехватчики

Исходящие веб-перехватчики позволяют создавать простого бота для базового взаимодействия, например запуска рабочего процесса или других простых команд, которые могут потребоваться. Исходящие веб-перехватчики существуют только в команде, в которой вы их создаете, и предназначены для простых процессов, характерных для рабочего процесса вашей компании. Дополнительные сведения см. в статье [Исходящие веб-перехватчики](~/webhooks-and-connectors/how-to/add-outgoing-webhook.md).

## <a name="build-a-great-teams-bot"></a>Создание отличного бота Teams

В следующих статьях описывается процесс создания отличного бота для Teams:

* [Создание бота](~/resources/bot-v3/bots-create.md): воспользуйтесь отличными инструментами, документацией и сообществом, предоставляемыми командой Bot Framework.
* [Обращение к боту](~/resources/bot-v3/bot-conversations/bots-conversations.md): добавьте основной поток беседы и используйте функции, характерных для канала. При разработке в .NET или Node.js используйте наши расширения для пакета SDK Bot Builder, чтобы упростить работу.
* [Использование карточек в боте](~/resources/bot-v3/bots-cards.md): создавайте карточки для общения и принятия ответов пользователей.
* [Реагирование на события бота](~/resources/bot-v3/bots-notifications.md)
* [Боты только для уведомлений](~/resources/bot-v3/bots-notification-only.md): использование ботов для отправки уведомлений в приложении.
* [Получение контекста](~/resources/bot-v3/bots-context.md): получить сведения о пользователе.
* [Меню ботов](~/resources/bot-v3/bots-menus.md): использование меню в ботах.
* [Боты и файлы](~/resources/bot-v3/bots-files.md): отправка и получение файлов от ботов.
* [Использование вкладок с ботами](~/resources/bot-v3/bots-with-tabs.md): совместная работа вкладок и ботов.
* [Тестирование бота](~/resources/bot-v3/bots-test.md): добавьте бота для личных или групповых бесед, чтобы проверить его возможности в действии.

## <a name="see-also"></a>Дополнительные ресурсы

[Примеры Bot Framework](https://github.com/Microsoft/BotBuilder-Samples/blob/master/README.md).
