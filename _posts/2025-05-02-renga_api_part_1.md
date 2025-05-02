---
title: "Renga API. �������. ����� 1"
date: 2025-05-02
---

� ����� ���� ��� API � ��� ������. �� �������� ����������� ��������� � �������������� ���� ����, ������� �� ����� ������ ������������ ����������. ������������� ���������� ���������� ���������������� ���� ������������, ��� ��� ������������ ��������� ������ �� � ��������� ������������� ������������ �������� ������������� ���������.

����� ������ ���� � ������ ����������� � API BIM ���������:
1. �������� �������� � ����������� � ���������/ui �������� ���������
2. �������������� ��������� ������. ��� ������������ �������� �� �������� � �� - ��������/��������/�������������� ��������
3. �������������� ��������
4. ������ ������� �������� ������
5. ������� ��������� �������, �������� �� �� ���-�� ������. �� ifc, obj �� pdf � ���
6. ��������� ����� �������� 3d ������ � ������ � ���
7. ������ ����� CLI ��� ������ �������� ��� �������� ��������� �������������

� ����������� ������� � �������� ���������, ��� ��� ������ ����������� � API �����.

������� ����������� �� �������������� ���������� � API. � ���� �� C# ������� ���� ������������ ������ �� ������� ����� ����� ����������������. ������ ��������������� ��� ����� ��������� ���������� ������. ��� ����� ���� ������� ������ � ������, ����������� ��������� � ���� ��������. ���� ��� ����������:
1. ������� ����� SDK � ������������ �����
2. ���������� ����� �� ��������� ������. � ����� �������� ������ ��� ������������ ������������
3. � ���. ����� ���������� SDK ������� � ����� ��� ����� ��������!

���� ������� �������, �� ��� ����� ����� ������:
1. ������� �� .net core ������ 8.
2. ������� �� .NetFramework. � ����������� ������ 4.8
3. ���������� �� .net core ��� .NetFramework

�����, ��� ����� ������ �� ����������� ������� C#! � TeklaStructures �� ��� ��� �� �������� ��������� ������������ .net core - ��-�� �������� �������� ��������� ��������� ������ ������� �� .NetFramerwok. 

���� ��� ��������� ����� COM. �� ����� ����������� �������������, � ������ � ������ ��� ������ ���� �� ����� �������. ���� �� �����, � �� �����, ��� ��� �� ������� ����� �������� ��������� ����� ���� ���-�� ��������.

��� ��������� ������� � ������������� .net core ���������� ������� 3 ����. ������, ��������� ������ �� ���������� � ����� �������:

```xml
<ItemGroup>
		<COMFileReference�Include="..\..\..\RengaSDK\tlb\RengaCOMAPI.tlb">
		</COMFileReference>
	</ItemGroup>
 
	<ItemGroup>
	��<Reference�Include="Renga.NET8.PluginUtility">
	����<HintPath>..\..\..\RengaSDK\Net\Renga.NET8.PluginUtility.dll</HintPath>
	��</Reference>
	</ItemGroup>
```

������ - ������� ���� ``PluginName.rndesc`` � �������� ��� � ����� 

```cmd
C:\Program Files\Renga *\Plugins\PluginName
```

� ����� ����� ������ ���� ��������� xml ���:

```xml
<RengaPlugin>
	<Name>PluginName</Name>
	<Version>1.0</Version>
	<Copyright>Copyright</Copyright>
	<RequiredAPIVersion>2.30</RequiredAPIVersion>
	<PluginType>net8</PluginType>
	<PluginFilename>pathtodll\PluginName.dll</PluginFilename>�
	<Vendor>Vendor</Vendor>
</RengaPlugin>
```

������ �������� ��� ���. ���-�� ���� ������:

```c#
using�Renga;
using�System.Reflection;
 
namespace�PluginName
{
	public�class�Plugin�:�IPlugin
	{
		private�ActionEventSource?�_followAction;
 
		public�bool�Initialize(string�pluginFolder)
		{
			IApplication�app�=�new�Renga.Application();
			IUI�ui�=�app.UI;
			IUIPanelExtension�panel�=�ui.CreateUIPanelExtension();
			IAction�button�=�ui.CreateAction();
			button.ToolTip�=�name;
			button.DisplayName�=�name;
 
			_followAction�=�new�ActionEventSource(button);
			_followAction.Triggered�+=�(sender,�args)�=>
			{
				ui.ShowMessageBox(MessageIcon.MessageIcon_Info,�$"{name}�plugin",�"Hello�world!");
			};
  
			panel.AddToolButton(button);
			ui.AddExtensionToPrimaryPanel(panel);
 
			return�true;
		}
 
		public�void�Stop()
		{
			_followAction?.Dispose();
		}
 	}
}
```

����� �������� ��������� ����� ����� ������� ������ ������� �� ������� ������.
![](2025.05.02_plugin_in_program.png)
��� ��� ��������� � ���� ������ ����� �������� ���� ������. ����� ������, �� ������ ��?
� ��������� ������ �������� �� ������������ ���������� � ���������������� ��������� ��������� ���������.

