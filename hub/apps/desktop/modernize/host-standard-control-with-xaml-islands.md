---
description: This article demonstrates how to host a standard UWP control in a WPF app by using XAML Islands.
title: Host a standard UWP control in a WPF app using XAML Islands
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, wrapped controls, standard controls, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
---

# Host a standard UWP control in a WPF app using XAML Islands

This article demonstrates two ways to host a standard UWP control (that is, a first-party UWP control provided by the Windows SDK or WinUI library) in a WPF app by using [XAML Islands](xaml-islands.md):

* It shows how to host a UWP [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) and [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) controls by using [wrapped controls](xaml-islands.md#wrapped-controls) in the Windows Community Toolkit. These controls wrap the interface and functionality of a small set of useful UWP controls. You can add them directly to the design surface of your WPF or Windows Forms project and then use them like any other WPF or Windows Forms control in the designer.
* It also shows how to host a UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) control by using the [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) control in the Windows Community Toolkit. Because only a small set of UWP controls are available as wrapped controls, you can use [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) to host any other standard UWP control.

Although this article demonstrates how to host these controls in a WPF app, the process is similar for a Windows Forms app.

## Create a WPF project

Before getting started, follow these instructions to create a WPF project and configure it to host XAML Islands. If you have an existing WPF project, you can adapt these steps and code examples for your project.

1. In Visual Studio 2019, create a new **WPF App (.NET Framework)** or **WPF App (.NET Core)** project.

2. Make sure [package references](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) are enabled:

    1. In Visual Studio, click **Tools -> NuGet Package Manager -> Package Manager Settings**.
    2. Make sure **PackageReference** is selected for **Default package management format**.

3. Right-click your WPF project in **Solution Explorer** and choose **Manage NuGet Packages**.

4. In the **NuGet Package Manager** window, make sure that **Include prerelease** is selected.

5. Select the **Browse** tab, search for the [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) package (version v6.0.0-preview7 or later), and install the package. This package provides everything you need to use the wrapped UWP controls for WPF (including [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) and [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) and the [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) control.
    > [!NOTE]
    > Windows Forms apps must use the [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) package (version v6.0.0-preview7 or later).

## Host an InkCanvas and InkToolbar by using wrapped controls

Now that you have configured your project to use UWP XAML Islands, you are now ready to add the [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) and [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) wrapped UWP controls to the app.

1. In **Solution Explorer**, open the **MainWindow.xaml** file.

2. In the **Window** element near the top of the XAML file, add the following attribute. This references the XAML namespace for the [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) and [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) wrapped UWP control.

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    After adding this attribute, the **Window** element should now look similar to this.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

3. In the **MainWindow.xaml** file, replace the existing `<Grid>` element with the following XAML. This XAML adds an [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) and [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) control (prefixed by the **Controls** keyword you defined earlier as a namespace) to the `<Grid>`.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400"
            Margin="10,10,10,10" VerticalAlignment="Top" />
    </Grid>
    ```

    > [!NOTE]
    > You can also add these and other wrapped controls to the window by dragging them from the **Windows Community Toolkit** section of the **Toolbox** to the designer.

4. Save the **MainWindow.xaml** file.

    If you have a device which supports a digital pen, like a Surface, and you're running this lab on a physical machine, you could now build and run the app and draw digital ink on the screen with the pen. However, if you don't have a pen capable device and you try to sign with your mouse, nothing will happen. This is happening because the **InkCanvas** control is enabled only for digital pens by default. However, you can change this behavior.

5. Open the **MainWindow.xaml.cs** file.

6. Add the following namespace declaration at the top of the file:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. Locate the `MainWindow()` constructor. Add the following line of code after the `InitializeComponent()` method and save the code file.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    You can use the **InkPresenter** object to customize the default inking experience. This code uses the **InputDeviceTypes** property to enable mouse as well as pen input.

8. Press F5 again to rebuild and run the app in the debugger. If you're using a computer with a mouse, confirm that you can draw something in the ink canvas space with the mouse.

## Host a CalendarView by using the host control

Now that you have added the [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) and [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) wrapped UWP controls to the app, you are now ready to use the [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) control to add a [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) to the app.

> [!NOTE]
> The [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) control is provided by the [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) package. This package is included with the [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) package you installed earlier.

1. In **Solution Explorer**, open the **MainWindow.xaml** file.

2. In the **Window** element near the top of the XAML file, add the following attribute. This references the XAML namespace for the [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) control.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    After adding this attribute, the **Window** element should now look similar to this.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

4. In the **MainWindow.xaml** file, replace the existing `<Grid>` element with the following XAML. This XAML adds a row to the grid and adds a [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) object to the last row. To host a UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) control, this XAML sets the `InitialTypeName` property to the fully qualified name of the control. This XAML also defines an event handler for the `ChildChanged` event, which is raised when the hosted control has been rendered.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400" 
            Margin="10,10,10,10" VerticalAlignment="Top" />
        <xamlhost:WindowsXamlHost x:Name="myCalendar" InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Row="2" 
              Margin="10,10,10,10" Width="600" Height="300" ChildChanged="MyCalendar_ChildChanged"  />
    </Grid>
    ```

5. Save the **MainWindow.xaml** file and open the **MainWindow.xaml.cs** file.

7. Add the following namespace declaration at the top of the file:

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. Add the following `ChildChanged` event handler method to the `MainWindow` class and save the code file. When the host control has been rendered, this event handler runs and creates a simple event handler for the `SelectedDatesChanged` event of the calendar control.

    ```csharp
    private void MyCalendar_ChildChanged(object sender, EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    MessageBox.Show("The user selected a new date: " + 
                        args.AddedDates[0].DateTime.ToString());
                }
            };
        }
    }
    ```

11. Press F5 again to rebuild and run the app in the debugger. Confirm that the calendar control now displays on the bottom of the window.

## Package the app

You can optionally package the WPF app in an [MSIX package](https://docs.microsoft.com/windows/msix) for deployment. MSIX is the modern app packaging technology for Windows, and it is based on a combination of MSI, APPX, App-V and ClickOnce installation technologies.

The following instructions show you how to package the all the components in the solution in an MSIX package by using the [Windows Application Packaging Project](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) in Visual Studio 2019. These steps are necessary only if you want to package the WPF app in an MSIX package.

1. Add a new [Windows Application Packaging Project](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) to your solution. As you create the project, select **Windows 10, version 1903 (10.0; Build 18362)** for both the **Target version** and **Minimum version**.

2. In the packaging project, right-click the **Applications** node and choose **Add reference**. In the list of projects, select the WPF project in your solution and click **OK**.

3. If your WPF project targets .NET Core 3, you must edit the packaging project file. These changes are currently required for packaging WPF apps that target .NET Core 3 and that host UWP controls.

    1. In Solution Explorer, right-click the packaging project node and select **Edit Project File**.
    2. Locate the `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` element in the file. Replace this element with the following XML.

        ``` xml
        <ItemGroup>
            <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
            </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
            <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
                <SourceProject></SourceProject>
                <TargetPath Condition="'%(FileName)%(Extension)'=='resources.pri'">app_resources.pri</TargetPath>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. Save the project file and close it.

4. Configure your solution to target a specific platform such as x86 or x64. This is required to build the WPF app into an MSIX package using the Windows Application Packaging Project.

    1. In **Solution Explorer**, right-click the solution node and select **Properties** -> **Configuration Properties** -> **Configuration Manager**.
    2. Under **Active solution platform**, select **x64** or **x86**.
    3. In the row for your WPF project, in the **Platform** column select **New**.
    4. In the **New Solution Platform** dialog, select **x64** or **x86** (the same platform you selected for **Active solution platform**) and click **OK**.
    5. Close the open dialog boxes.

5. Build and run the packaging project. Confirm that the WPF runs and the UWP custom control displays as expected.

## Related topics

* [UWP controls in desktop applications](xaml-islands.md)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
