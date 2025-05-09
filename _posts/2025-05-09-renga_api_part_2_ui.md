---
title: Renga API. Заметки. Часть 2. Встраивание в UI
date: 2025-05-09
categories:
  - Bim API
  - Renga
  - RengaUI
tags:
  - Renga
  - RengaAPI
  - RengaUI
author: Pavel Karpovich
pin: true
math: true
mermaid: true
image:
  path: /_posts/img/2025-05-02-renga_api_part_1_icon_in_renga.png
  alt: Image description
---
# Renga API. Заметки. Часть 2. Встраивание в UI

В RengaAPI, есть возможность встраивать базовые элементы UI в интерфейс. Это различного вида кнопки и меню. На самом деле их не очень много:
1. ToolButton - обычная кнопка. Нажали - получили выполнение кода
2. DropDownButton - выпадающее меню с кнопками. Нажали выбрали то, что требуется. Можно добавлять разделители
3. SplitButton - кнопка с выпадающим меню других кнопок. Можно нажать, как на саму кнопку и вызвать действие, а можно нажать немного правее и тогда кнопка сработает, как выпадающее меню

Выше описанные элементы могут быть размещены в либо в главной панели или в панели действий сбоку (см. скриншот).

Также возможно добавление своих элементов в контекстное меню. Это может быть:
1. ContextItem - просто элемент, на который можно нажать
2. NodeItem - можно организовать, что-то типа выпадающего списка

Давайте кратко глянем из чего состоит создание одной кнопки

В методе Initialize создаём экземпляр интерфейса IUIPanelExtension и добавляем туда AddToolButton методом IAction тип 
```c#
public bool Initialize(string pluginFolder)
{
	Application app = new();
	IUI ui = app.UI;
 
	string icoPath = Path.GetFullPath(Path.Combine(pluginFolder, "ico.png"));
	_icon = ui.CreateImage();
	_icon.LoadFromFile(icoPath);
 
	IUIPanelExtension panelExtension = ui.CreateUIPanelExtension();
	panelExtension.AddToolButton(CreateAction(ui, "OrdynaryButton"));
	ui.AddExtensionToActionsPanel(panelExtension, ViewType.ViewType_View3D);
 
	return true;
}
```

Метод CreateAction выглядит следующим способом. Можно навесить на IAction название кнопки, иконку, а также действие. Также можно попробовать сделать элементарный чекбокс.
```c#
private IAction CreateAction(IUI ui, string displayName)
{
	IAction action = ui.CreateAction();
	action.DisplayName = displayName;
	action.Icon = _icon;
 
	ActionEventSource events = new(action);
	events.Triggered += (s, e) =>
	{
		ui.ShowMessageBox(MessageIcon.MessageIcon_Info, "Plugin message", displayName + " Handler");
	};
 
	_eventSources.Add(events);
 
	return action;
}
```

На этом возможности добавления контролов заканчиваются. В принципе, больше и не нужно. Главное, что можно также вызывать внешний UI написанный на WPF и это самое важное. Однако добавить UI получилось только для плагина, написанного на .NetFramework. С net8 возникли проблемы с зависимостями. Скорее всего они решаемы.