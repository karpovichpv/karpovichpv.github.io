---
title: Lookup для Tekla Structures. Report properties 
date: 2026-02-21
categories:
  - Bim API
  - Renga
  - Renga Inspector 
tags:
  - Renga
  - RengaPlugins
  - RengaInspector
  - Inpector
author: Pavel Karpovich
pin: true
math: true
mermaid: true
image:
  path: /_posts/img/2026-02-21-renga_inspector_plugin_1.png
  alt: Renga Inpector plugin 
---

# Новый плагин 

Разработал плагин под названием Inspector для BIM системы Renga. Цель более быстрое понимание модели open api непосредственно в программе. Выбрал объект - получил результат, какой тип, его свойства, методы, поля. Фактически данный плагин является аналогом всем известных плагинов для AutoCAD [ARXDBG и MGDDBG](https://adn-cis.org/forum/index.php?topic=7274.0), или [RevitLookup](https://github.com/lookup-foundation/RevitLookup) для Revit, или [Tekla Lookup](http://github.com/karpovichpv/lookup) для TeklaStructures. 

Код построен на рефлексии. Хоть API модель Renga и довольно скудна на свойства, методы, часть свойств, методов обрабатывается в коде отдельно. К ним относятся:

* IBeamParams
* IColumnParams
* IEntityCollection
* ILayerCollection
* IMaterial через Id
* IParameterContainer
* IParameterContainer
* IPlacement3DCollection
* IPolyCurve2D
* IPropertyContainer
* IQuantityContainer
* и т.д.

С выходом новых версий данный список будет расширяться.

# Установка

Содержимое архива распаковать в папку `C:\Program Files\Renga Standard\Plugins\Inspector`

# Работа с плагином

1. Левой кнопкой мыши нажимаем на кнопку плагина на верхней панели
2. Далее должна отобразиться форма плагина
    - если ничего не было выбрано - отобразяться свойства объекта IProject
    - если были выбраны объекты - в левой панели будут отображены имена объектов, в правой свойства первого объекта
3. На строки в правой панели, которые имеют жирный шрифт, можно нажимать 2 щелчком левой кнопки - откроется новый экземпляр плагина с загруженными свойствами дочернего объекта

# Возможные проблемы

1. Если плагин не грузиться, следует проверить AecApp.log
2. Если плагин вдруг вылетел - в папке `C:\Program Files\Renga Standard\Plugins\Inspector` должен быть crash_log с текстом исключения
3. Объект IModel пока грузиться долго. Чем больше модель тем больше объектов надо получить. Чуть позже сделаю этот процесс более плавным и предсказуемым

# Примеры использования

![1](/_posts/img/2026-02-21-renga_inspector_plugin_usage_1.gif)
![2](/_posts/img/2026-02-21-renga_inspector_plugin_usage_2.gif)
![3](/_posts/img/2026-02-21-renga_inspector_plugin_usage_3.gif)