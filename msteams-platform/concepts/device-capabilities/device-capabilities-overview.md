---
title: Возможности устройства — обзор
author: Rajeshwari-v
description: Узнайте, как интегрировать собственные возможности устройства, такие как расположение и мультимедиа (камера, микрофон, галерея, QR или сканер штрихкодов) с приложением Microsoft Teams.
ms.author: surbhigupta
ms.localizationpriority: medium
ms.topic: overview
ms.openlocfilehash: 04ae1a0b21c12ef7dda5d4bf8dfa799ac5726d15
ms.sourcegitcommit: 75d0072c021609af33ce584d671f610d78b3aaef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2022
ms.locfileid: "68100569"
---
# <a name="device-capabilities"></a>Возможности устройств

Microsoft Teams постоянно совершенствует возможности разработчиков в соответствии с собственными встроенными возможностями. Расширенная платформа Teams позволяет партнерам интегрировать возможности устройств, такие как камера, QR или сканер штрихкодов, фотоальбом, микрофон и расположение, со своими веб-приложениями. Такая интеграция сокращает ограничения для разработки приложений, ускоряет цикл разработки и создает новые сценарии или варианты использования для сообщества разработчиков.

Разрешения устройств в браузере отличаются. Ранее браузер обрабатывал предоставление разрешений на доступ, а теперь эти разрешения обрабатываются в Teams. Дополнительные сведения см. в статье [Разрешения устройств в браузере](browser-device-permissions.md).

## <a name="native-device-capabilities"></a>Встроенные возможности устройства

Мобильное приложение или ПК имеет встроенные устройства, такие как камера и микрофон, которые называются возможностями. Вы можете получить доступ к следующим возможностям устройства в мобильной или настольной версии с помощью выделенных API, доступных в [клиентском SDK JavaScript для Microsoft Teams](/javascript/api/overview/msteams-client?view=msteams-client-js-latest&preserve-view=true):

* Возможности мультимедиа, такие как
  * Камера
  * Микрофон
  * Галерея
  * Функция сканирования QR- или штрихкода
* Расположение

Получив доступ к возможностям устройства, вы можете интегрировать их с платформой Teams для улучшения взаимодействия.

## <a name="request-device-permissions"></a>Запрос разрешений устройства

Используйте инструменты, представленные в [клиентском пакете SDK Microsoft Teams для JavaScript](/javascript/api/overview/msteams-client?view=msteams-client-js-latest&preserve-view=true), чтобы запросить необходимые [разрешения](native-device-permissions.md) для доступа к собственным возможностям устройства. Хотя доступ к этим возможностям является стандартным в современных веб-браузерах, необходимо сообщить Teams о возможностях, которые вы используете, обновив манифест приложения. Это обновление позволяет запрашивать разрешения во время работы приложения на мобильных или настольных клиентах Teams.

## <a name="integrate-device-capabilities"></a>Интеграция возможностей устройства

После получения доступа к возможностям устройства используйте мультимедийные API-интерфейсы Teams [ для интеграции возможностей мультимедиа](media-capabilities.md) с платформой Teams в целях улучшения взаимодействия с пользователем. Эти интегрированные возможности позволяют приложению выполнять следующие действия:

* Запись и передача изображений.
* Сканирование QR или штрихкода с помощью [элемента управления сканером](qr-barcode-scanner-capability.md).
* Запись звука через микрофон.
* Отправка сведений о местоположении с помощью [средства выбора расположения](location-capability.md).

Кроме того, вы можете интегрировать [элемент управления "выбор людей"](people-picker-capability.md), встроенный в Teams, который позволяет искать и выбирать людей в веб-интерфейсе приложения.

## <a name="code-sample"></a>Пример кода

| Название примера           | Описание | Node.js    |
|:---------------------|:--------------|:---------|
|Разрешения для устройств | В этой статье описывается, как продемонстрировать пример приложения на вкладке Teams для разрешений устройства. |[Просмотр](<https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/tab-device-permissions/nodejs>)|
