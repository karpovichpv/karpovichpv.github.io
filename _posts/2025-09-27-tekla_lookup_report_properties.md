---
title: Lookup для Tekla Structures. Report properties 
date: 2025-09-27
categories:
  - Bim API
  - TeklaStructures
  - Tekla Lookup 
tags:
  - TeklaStructures
  - TeklaStructuresOpenApi
  - TeklaLookup
author: Pavel Karpovich
pin: true
math: true
mermaid: true
image:
  path: /_posts/img/2025-05-02-renga_api_part_1_icon_in_renga.png
  alt: Image description
---

# Lookup для TeklaStructures. Обновление до версии 3.1

Обновил свою утилиту для Tekla Structures до версии 3.1. Основным мотивом было желание отобразить всевозможные значения ReportProperty для объекта ModelObject. Т.к. когда настраиваешь tpl шаблон или создаешь свою метку на основе шаблона далеко не всегда очевидно, какое свойство нужно записать, чтобы получить нужный результат. Ближе к делу:

1. Добавлен новый контрол. Содержит список свойств (ReportProperty), которые удалось получить для выбранного объекта
2. Т.к. список имен свойств заранее не известен, т.е. его не возможно получить из TeklaStructures - я его записал в файл с расширением .lst - report_properties.lst. Это обычный файл csv с разделителем или точка или точка с запятой. Также подходят стандартные файлы свойств TeklaStructures с тем же расширением .lst. Все просто - не хватает свойств - дописываем в файл, перезапускаем программу.
3. Файлов с расширением .lst может быть несколько рядом с программой

![TeklaStructures Lookup. Report properties for any ModelObject](https://karpovichpv.github.io/assests/img/2025-09-27_teklastructures_lookup_report_properties.png)

![TeklaStructures Lookup. Report properties file](https://karpovichpv.github.io/assests/img/2025-09-27_teklastructures_lookup_report_properties_file.png))

Были ещё мелки улучшения:
1. Переделал контрол пользовательских атрибутов - теперь можно копировать значения из программы
2. При выборе в программе элементов - данные элементы автоматически выбираются в модели 