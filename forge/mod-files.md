---
title: Файлы мода
layout: page
description: Начало работы с Forge
permalink: /forge/mod-files # https://toml.io/en/
---

## Файлы мода

### mods.toml
Файл `mods.toml` определяет метаданные вашего(их) мода(ов). Он также содержит дополнительную информацию, которая отображается в меню "Моды" и инструкции по загрузке модов в игру.
Файл использует формат [`TOML`](https://toml.io/en/) — язык очевидных минимальных решений. Он должен находиться в папке `META-INF` внутри ресурса проекта (например, для основного набора исходных кодов — `src/main/resources/META-INF/mods.toml`{: .filepath}). 
Пример файла `mods.toml` может выглядеть так:

```toml
modLoader="javafml"
loaderVersion="[52,)"

license="All Rights Reserved"
issueTrackerURL="https://github.com/MinecraftForge/MinecraftForge/issues"
showAsResourcePack=false
clientSideOnly=false

[[mods]]
modId="examplemod"
version="1.0.0.0"
displayName="Example Mod"
updateJSONURL="https://files.minecraftforge.net/net/minecraftforge/forge/promotions_slim.json"
displayURL="https://minecraftforge.net"
logoFile="logo.png"
credits="I'd like to thank my mother and father."
authors="Author"
description='''
  Lets you craft dirt into diamonds. This is a traditional mod that has existed for eons. It is ancient. The holy Notch created it. Jeb rainbowfied it. Dinnerbone made it upside down. Etc.
  '''
displayTest="MATCH_VERSION"

[[dependencies.examplemod]]
modId="forge"
mandatory=true
versionRange="[52,)"
ordering="NONE"
side="BOTH"

[[dependencies.examplemod]]
modId="minecraft"
mandatory=true
versionRange="[1.21.1,)"
ordering="NONE"
side="BOTH"
```
Файл `mods.toml` разбит на три части: общие свойства, не связанные с модом напрямую, свойства модов и конфигурации зависимостей. Каждый раздел и его свойства подробно объяснены ниже. Обязательность означает, что значение должно быть указано, иначе будет выброшено исключение.

Общие свойства, не связанные с модом
Это свойства, связанные с самим JAR-файлом, указывающие, как его загружать, и любую глобальную информацию. Например: тип загрузчика, версия, лицензия, URL для отслеживания ошибок, ресурсы и т.д.

|      Свойство      |   Тип   | Обязательно? |                                                                       Описание                                                                        |                             Пример                             |
|:------------------:|:-------:|:------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------:|
|     modLoader      | string  |      да      | Загрузчик языка, используемый модом. Может поддерживать альтернативные структуры, такие как объекты Kotlin или разные методы определения точки входа, |                           "javafml"                            |
|   loaderVersion    | string  |      да      |                          Допустимый диапазон версий загрузчика как Maven Version Range. Для javafml — основная версия Forge.                          |                            "[46,)"                             |
|      license       | string  |      да      |                               Лицензия, под которой распространяется мод. Можно указать SPDX идентификатор или ссылку.                                |                             "MIT"                              |
| showAsResourcePack | boolean |     нет      |                                           Если true, ресурсы мода отображаются как отдельный ресурс-пакет.                                            |                              true                              |
|   clientSideOnly   | boolean |     нет      |                                          Если true, при запуске на сервере не загружать моды из этого файла.                                          |                              true                              |
|      services      |  array  |     нет      |                                       Сервисы, используемые модом, выражаются через файлы или module-info.java.                                       | 	["net.minecraftforge.forgespi.language.IModLanguageProvider"] |
|     properties     |  table  |     нет      |                                                 Таблица свойств для подстановок, например для версий.                                                 |                    { "example" = "1.2.3" }                     |
|  issueTrackerURL   | string  |     нет      |                                                    URL, куда можно отправлять отчеты о проблемах.                                                     |             "https://forums.minecraftforge.net/"               |

> Важно! Cвойство `services` эквивалентно использованию директивы `uses` в модуле, позволяя загружать сервисы.
{: .prompt-warning }

### Специфичные свойства мода
Свойства, специфичные для мода, привязаны к указанному моду с помощью заголовка [[mods]]. Это [массив таблиц](https://toml.io/en/v1.0.0#array-of-tables); все свойства ключ/значение будут привязаны к этому моду до следующего заголовка.

```toml
# Properties for examplemod1
[[mods]]
modId = "examplemod1"

# Properties for examplemod2
[[mods]]
modId = "examplemod2"
```
