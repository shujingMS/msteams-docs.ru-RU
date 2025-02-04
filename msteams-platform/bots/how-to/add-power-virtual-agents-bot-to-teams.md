---
title: Добавление чатбота Power Virtual Agents
author: surbhigupta
description: Узнайте, как интегрировать бота Power Virtual Agents в платформу Teams, чтобы создавать диалоговых ботов и интегрировать их с Teams.
ms.topic: how-to
ms.localizationpriority: medium
ms.author: lajanuar
ms.openlocfilehash: 310b7d8a5e04259a205763b45cb2d2315c19c72a
ms.sourcegitcommit: c7fbb789b9654e9b8238700460b7ae5b2a58f216
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2022
ms.locfileid: "66485204"
---
# <a name="add-power-virtual-agents-chatbot"></a>Добавление чатбота Power Virtual Agents

Power Virtual Agents является интерактивным решением графического интерфейса без использования кода, которое позволяет каждому участнику вашей команды создавать многофункциональные боты для бесед, которые легко интегрируются с платформой Teams. Все содержимое, создаваемое в Power Virtual Agents, естественным образом отображается в Teams. Боты Power Virtual Agents взаимодействуют с пользователями на холсте чата Teams. ИТ-администраторы, бизнес-аналитики, специалисты по доменам и опытные разработчики приложений могут проектировать, разрабатывать и публиковать интеллектуальные виртуальные агенты для Teams без необходимости настройки среды разработки. Они могут создать веб-службу или напрямую зарегистрироваться в Bot Framework.

В этом документе описывается, как сделать бота доступным в Teams через портал Power Virtual Agents и добавить бота в Teams с помощью App Studio.

Power Virtual Agents позволяет создавать мощных ботов, которые могут отвечать на вопросы, заданные клиентами, другими сотрудниками или посетителями вашего веб-сайта или службы.

Этих ботов можно легко создать, не прибегая к помощи специалистов по обработке и анализу данных или разработчиков.

> [!NOTE]
>
> * Благодаря добавлению чат-бота в Microsoft Teams некоторые данные, такие как содержимое бота и содержимое чата пользователя, перечислены в Teams. Это означает, что поток ваших данных выходит за [пределы соответствия требованиям вашей организации и географических или региональных границ](/power-virtual-agents/data-location). <br/>
> * Не следует использовать Microsoft Power Platform для создания приложений, предназначенных для публикации в магазине приложений Teams. Приложения Microsoft Power Platform можно публиковать только в магазине приложений организации.

## <a name="make-your-chatbot-available-in-teams-through-the-power-virtual-agents-portal"></a>Сделайте своего бота доступным в Teams через портал Power Virtual Agents.

Чтобы сделать бота доступным в Teams через портал Power Virtual Agents, необходимо выполнить следующие действия:

Чтобы сделать бота доступным в Teams, необходимо выполнить следующие действия:

1. **Опубликовать последнее содержимое бота**  
После создания бота на портале Power Virtual Agents необходимо опубликовать его, прежде чем пользователи Teams смогут взаимодействовать с ним. Дополнительные сведения см. в статье [Публикация последнего содержимого бота](/power-virtual-agents/publication-fundamentals-publish-channels#publish-the-latest-bot-content).

   ![публикация на портале power virtual agents](../../assets/images/pva-publish.png)

1. **Настройка канала Teams**  
После публикации бота добавьте канал Teams, чтобы сделать бота доступным для пользователей Teams.

   ![каналы на портале power virtual agents](../../assets/images/pva-channels.png)

1. **Создание идентификатора приложения для чат-бота**  
После добавления канала Teams в бот в диалоговом окне создается **ИД приложения**. Идентификатор приложения — это уникальный идентификатор, созданный Майкрософт для вашего бота. Сохраните идентификатор приложения, чтобы создать пакет приложения для Teams.

## <a name="add-your-bot-to-teams-using-app-studio"></a>Добавление бота в Teams с помощью App Studio

Если в экземпляре Teams [включена загрузка пользовательских приложений](/microsoftteams/admin-settings), вы можете использовать Teams App Studio, чтобы напрямую загрузить своего бота и сразу начать его использовать. Чтобы поделиться своим ботом, вы можете попросить своего администратора сделать вашего бота доступным в каталоге приложений клиента или вы можете отправить пакет своего приложения другим пользователям и попросить их загрузить его самостоятельно.

1. **Установка App Studio в Teams**  
App Studio — это приложение Teams. Установите App Studio из магазина Teams, что упрощает процесс создания и регистрации бота в Teams:

   1. Выберите значок магазина приложений в экземпляре Teams и найдите **App Studio**.

      &emsp;&emsp; <img  width="450px" alt="Finding App Studio in the Store" src="../../assets/images/get-started/app-studio-store.png"/>

   1. Выберите плитку **App Studio** и нажмите **Установить** во всплывающем диалоговом окне.

      &emsp;&emsp; <img  width="450px" alt="Installing App Studio" src="../../assets/images/get-started/app-studio-install.png"/>

1. **Создание манифеста приложения Teams в App Studio**  
Боты в Teams определяются JSON-файлом манифеста приложения, в котором содержатся основные сведения о вашем боте и его возможностях. В **App Studio** выберите **Редактор манифеста** и нажмите **Создать новое приложение**.

    ![создание нового приложения](../../assets/images/get-started/create-new-app.png)

1. **Добавление сведений о боте**  
Заполните все необходимые поля. Полное описание каждого поля см. в [определении схемы манифеста](../../resources/schema/manifest-schema.md).

    ![добавление сведений о приложении](../../assets/images/get-started/add-app-details.png)

1. **Настройка бота**. Чтобы настроить бота, выполните следующие действия:
     1. Откройте вкладку **Боты**.
     1. Выберите **Настройка** > **Существующий бот** и введите имя бота.

   ![Настройка бота](../../assets/images/get-started/bot-set-up.png)

   На следующем рисунке показано, как настроить существующий бот:

   ![настройка существующего бота](../../assets/images/get-started/existing-bot-set-up.png)

1. **Добавление идентификатора приложения**  
Чтобы добавить идентификатор приложения, выполните следующие действия.  
    1. Выберите **Подключение к другому идентификатору бота** и **ИД приложения**, скопированный ранее.
    1. Выберите **Область** > **Личная** > **Сохранить**.

    ![добавление идентификатора приложения](../../assets/images/get-started/add-app-id.png)

1. **Добавление допустимых доменов для бота**  
Этот шаг требуется только в том случае, если бот требует, чтобы пользователь выполнил вход. Выберите **Домены и разрешения** и в поле **Допустимые домены** введите следующие данные:

    ```bash
       token.botframework.com
    ```

1. **Тестирование и распространение бота**  
Откройте вкладку **Тестирование и распространение** и выберите **Установить**, чтобы добавить бота непосредственно в экземпляр Teams. Кроме того, вы можете скачать готовый пакет приложения, чтобы поделиться им с пользователями Teams, или предоставить его своему администратору, чтобы ваш бот был доступен в каталоге приложений клиента.

1. **Начать чат**. Процесс настройки для добавления бота Power Virtual Agents чата в Teams завершен. Теперь вы можете начать беседу с ботом в личном чате.

## <a name="next-step"></a>Следующий этап

> [!div class="nextstepaction"]
> [Создание виртуального помощника](~/samples/virtual-assistant.md)

## <a name="see-also"></a>Дополнительные ресурсы

* [Power Virtual Agents](/power-virtual-agents/fundamentals-what-is-power-virtual-agents)  
* [Создание бота для Teams с помощью Microsoft Power Virtual Agents](../bot-features.md#bots-with-power-virtual-agents)
* [Портал Power Virtual Agents](https://powervirtualagents.microsoft.com)
* [Публикация бота Power Virtual Agents](/power-virtual-agents/publication-fundamentals-publish-channels)
* [Безопасность и соответствие в Microsoft Teams](/MicrosoftTeams/security-compliance-overview)
* [Бот Power Virtual Agents для управления персоналом](/power-virtual-agents/teams/fundamentals-get-started-teams)
