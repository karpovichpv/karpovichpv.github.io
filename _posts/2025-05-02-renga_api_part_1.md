---
# Basic Options
title: "Renga API. Заметки. Часть 1"
date: 2025-05-02
categories: [Bim API, Renga]
tags: [Renga, RengaAPI]

# Additional Options
author: Pavel Karpovich 
pin: true    # pins post to top
math: true   # enables math formatting
mermaid: true # enables mermaid diagrams
image:
  path: /_posts/img/2025-05-02-renga_api_part_1_icon_in_renga.png
  alt: Image description
---

У Ренги есть своё API и это хорошо. Не возможно представить программу в проектировании чего либо, которая не имеет своего программного интерфейса. Пользователям определённо необходимо автоматизировать свою деятельность, так как разработчики программы просто не в состоянии предусмотреть всевозможные сценарии использования программы. <!--more-->

Какие должны быть в идеале возможности у API BIM программы:
1. Создание плагинов и встраивание в интерфейс/ui основной программы
2. Редактирование элементов модели. Это классические операции по аналогии с БД - создание/удаление/редактирование объектов
3. Редактирование чертежей
4. Чтение свойств объектов модели
5. Экспорт элементов моделей, чертежей во всё что-то угодно. От ifc, obj до pdf и двг
6. Получение сетки объектов 3d модели и работа с ней
7. Работа через CLI или другим способом без открытия программы пользователем

В последующих статьях я подробно рассмотрю, как эти пункты реализованы в API Ренги.

Сегодня остановимся на первоначальном знакомстве с API. Я пишу на C# поэтому буду рассказывать именно со стороны этого языка программирования. Начать программировать для Ренги оказалось невероятно просто. Без каких либо сложных танцев с бубном, подключения библиотек и тому подобное. Итак что необходимо:
1. Скачать пакет SDK с официального сайта
2. Установить Ренгу на локальную машину. Я купил домашнюю версию для тестирования возможностей
3. И все. После распаковки SDK примеры в папке уже будут работать!

Если немного сложнее, то под Ренгу можно писать:
1. Плагины на .net core версии 8.
2. Плагины на .NetFramework. Я использовал версию 4.8
3. Приложения на .net core или .NetFramework

Круто, что можно писать на современных версиях C#! В TeklaStructures до сих пор не возможно нормально использовать .net core - из-за обратной совмести вынуждены постоянно делать проекты на .NetFramerwok. 

Само апи построено через COM. Со всеми вытекающими последствиями, а именно в теории оно должно быть не очень быстрым. Хотя по факту, я не думаю, что это на текущем этапе развития программы будет хоть как-то заметным.

Для написания плагина с использование .net core необходимо сделать 3 вещи. Первое, настроить ссылки на библиотеки в файле проекта:

```xml
<ItemGroup>
		<COMFileReference Include="..\..\..\RengaSDK\tlb\RengaCOMAPI.tlb">
		</COMFileReference>
	</ItemGroup>
 
	<ItemGroup>
	  <Reference Include="Renga.NET8.PluginUtility">
	    <HintPath>..\..\..\RengaSDK\Net\Renga.NET8.PluginUtility.dll</HintPath>
	  </Reference>
	</ItemGroup>
```

Второе - создать файл ``PluginName.rndesc`` и положить его в папку 

```cmd
C:\Program Files\Renga *\Plugins\PluginName
```

В самом файле должно быть следующий xml код:

```xml
<RengaPlugin>
	<Name>PluginName</Name>
	<Version>1.0</Version>
	<Copyright>Copyright</Copyright>
	<RequiredAPIVersion>2.30</RequiredAPIVersion>
	<PluginType>net8</PluginType>
	<PluginFilename>pathtodll\PluginName.dll</PluginFilename> 
	<Vendor>Vendor</Vendor>
</RengaPlugin>
```

Третье написать сам код. Что-то типа такого:

```c#
using Renga;
using System.Reflection;
 
namespace PluginName
{
	public class Plugin : IPlugin
	{
		private ActionEventSource? _followAction;
 
		public bool Initialize(string pluginFolder)
		{
			IApplication app = new Renga.Application();
			IUI ui = app.UI;
			IUIPanelExtension panel = ui.CreateUIPanelExtension();
			IAction button = ui.CreateAction();
			button.ToolTip = name;
			button.DisplayName = name;
 
			_followAction = new ActionEventSource(button);
			_followAction.Triggered += (sender, args) =>
			{
				ui.ShowMessageBox(MessageIcon.MessageIcon_Info, $"{name} plugin", "Hello world!");
			};
  
			panel.AddToolButton(button);
			ui.AddExtensionToPrimaryPanel(panel);
 
			return true;
		}
 
		public void Stop()
		{
			_followAction?.Dispose();
		}
 	}
}
```

После загрузки программы можно будет увидеть иконку плагина на главной панели.

![Renga API simple button](https://karpovichpv.github.io/assests/img/2025-05-02-renga_api_part_1_icon_in_renga.png)

Вот так элегантно в пару кликов можно встроить свой плагин. Очень просто, не правда ли?
В следующее статье пройдёмся по возможностям добавления в пользовательский интерфейс различных контролов.
