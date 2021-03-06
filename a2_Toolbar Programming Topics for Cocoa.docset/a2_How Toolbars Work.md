# How Toolbars Work

NSToolbar and NSToolbarItem classes provide you with a standard way to display a toolbar for a titled window below its title bar. These classes also provide users with a standard way to customize toolbars and save those customizations. This is what a toolbar looks like:

# 工具栏如何工作

NSToolbar和NSToolbar两个类为你提供了在标题窗口的标题栏下方显示工具栏的标准途径。这些类也为用户提供了定制工具栏和保存那些定制的标准途径。下面展示了工具栏看上去是什么样子的：

**Figure 1**  Example toolbar 

![ Example toolbar ]( http://cl.ly/image/0F101G3m0P1C/toolbar.jpg ).



To create a toolbar, you must create a delegate that provides important information:

* A list of default toolbar identifiers. This list is used when reverting to default, and constructing the initial toolbar.
The default set of toolbar items can also be specified using toolbar items found in the Interface Builder library.

* A list of allowed item identifiers. The allowed item list is used to construct the customization palette, if the toolbar is customizable.

* The toolbar item for a given item identifier.

When you create an NSToolbar, you give it an identifier. NSToolbar assumes all toolbars with the same identifier are the same, and automatically synchronizes changes. For instance, take a mail application. Your application’s compose windows’ toolbars would have the same identifier string. So, when you re-order items in one toolbar, the changes automatically propagate to any other compose windows currently open.

Most toolbars will contain simple clickable items that act like buttons. The simplest toolbar item is defined by its icon, label, palette label (used in the customize sheet), target, action, and tooltip. Most toolbars can be represented using these simple items. If you need something more complex in your toolbar, call setView: on a toolbar item to provide a custom view. For example, you could create a toolbar item that contains a pop-up menu or a text field and button.

```
Note: In OS X v10.4 and earlier versions of the operating system, only toolbar items that are pop-up buttons can have key equivalents. In OS X v10.5 and later, all visible controls in a toolbar can have key equivalents.
```

要创建一个工具栏，你必须创建一个提供重要信息的委托：

* 一个默认的工具栏ID列表。这个列表通常在重置工具栏到默认状态和初始化工具栏时使用。

* 一个被允许的工具栏项ID列表。被允许的工具栏项列表通常用于在工具栏是可定制的时创建定制调板。

* 给定项的工具栏的ID

当你创建一个NSToolbar时，会赋给它一个标识符。NSToolbar假设所有具有相同标识符的工具栏都是相同的，并且会自动同步更改。例如，Mail.app。你的app的众多组合窗口的工具栏将会拥有相同的标识符字符串。所以，当你重新排列一个工具栏中的项时，改变会自动传播给任何当前打开的其他组合窗口。

大多数工具栏会包含简单的可点击的并且动作像一个button的项。最简单的工具栏项是通过它的图标（icon），标签（label），调板标签（palette label，在定制sheet窗口中使用），目标（target），动作（action）以及工具提示（tooltip）。大多数工具栏可以使用这些简单的项被展示出来。如果你需要在你的工具栏中有一些更复杂的东西，在一个工具栏项上调用*setView:*方法以提供定制视图。比如说你可以创建一个包含一个弹出式菜单或文本域以及一个button的工具栏。

```
注意：在OS X 10.4以及更早版本的操作系统中，只有弹出式按钮项可以有一个快捷键。在OS X 10.5和之后的版本中，所有在工具栏中可见的NSControl子类对象都可以拥有快捷键。
```



There are a couple of standard toolbar item identifiers that NSToolbar knows about. **NSToolbarSeparatorItemIdentifier** is the standard vertical line separator. **NSToolbarSpaceItemIdentifier** is a fixed width space. **NSToolbarFlexibleSpaceItemIdentifier** is a variable width space. Additionally, there are **NSToolbarShowColorsItemIdentifier**, **NSToolbarShowFontsItemIdentifier**, **NSToolbarPrintItemIdentifier**, and **NSToolbarCustomizeToolbarItemIdentifier**. These items are accessible only by identifier.

If you need to change the action sent by a standard item, write a toolbarWillAddItem: notification method.

When a user customizes the toolbar of an application’s window, that customization is saved as a user preference. The customized toolbar is used thereafter when the application launches in place of the default set of toolbar items specified by the developer.


有几个NSToolbar知道的标准工具栏项标识符。**NSToolbarSeparatorItemIdentifier**是标准的垂直分割线。 **NSToolbarSpaceItemIdentifier**是一个固定的宽度空间。**NSToolbarFlexibleSpaceItemIdentifier** 是一个可变的宽度空间。此外，还有**NSToolbarShowColorsItemIdentifier**, **NSToolbarShowFontsItemIdentifier**, **NSToolbarPrintItemIdentifier**, 以及**NSToolbarCustomizeToolbarItemIdentifier**。这些项只能通过标识符才能使用。

如果你需要更改标准项所发送的动作，可以编写一个*toolbarWillAddItem:*通知方法。

当一个用户定制一个app的窗口中的工具栏时，定制信息会被保存在一个用户偏好中。定制过的工具栏会被使用，之后当app再启动时，定制好的工具栏项会替代app开发者指定的默认的项集合。



## Toolbar configurations

There are kinds of toolbars, and there are individual toolbar objects. A kind of toolbar is represented by string called the toolbar identifier. When you create an NSToolbar you supply a toolbar identifier so your toolbar delegate will know that the new instance is to be of that kind. For example, consider a Mail application that has two kinds of windows, Message and Mailbox. Each needs its own kind of toolbar: one appropriate for viewing a message and one appropriate for listing the messages in a mailbox. To implement this user interface, the application has two toolbar identifiers, "Message", and "Mailbox". Each message window has its own distinct toolbar object, but all message window toolbar objects have the same identifier: "Message". When you modify the toolbar in a window (either through the user interface or programmatically) you change the toolbar configuration of that kind of toolbar, not just of that instance. So, when you customize the toolbar in one message window, the new configuration automatically propagates to the toolbars in all other message windows currently open. If the toolbar is hidden in any message window, it will stay hidden, but when it is shown again, it looks like the others of its kind.

The toolbar’s identifier is fixed at object creation, but you can change other attributes of it with *setAllowsUserCustomization:*, *setAutosavesConfiguration:*, and *setDisplayMode:*.

## 工具栏配置

工具栏有多种类型，并且是独立的工具栏对象。一种工具栏通过工具栏标识符的字符串调用来展现。当你创建一个NSToolbar时，会提供一个工具栏标识符，所以你的工具栏委托会知道新的工具栏是哪种类型。比如，考虑拥有*消息窗口*和*信箱窗口*两种窗口类型的Mail.app。每个窗口都需要它自己的工具栏类型：一个适用于查看消息，另一个适用于在信箱中列出所有消息。要实现这样的用户界面，app应该具有两个工具栏，“消息”和“信箱”。每个消息窗口有它自己的独立的工具栏对象，但是所有信息窗口的工具栏对象都有同一个标识符：“消息”。当你在*消息窗口*中更改工具栏时（通过用户界面或者编程的方式），你会改变所有属于这个类型的工具栏的配置，而不仅仅是该实例的。所以，当你在一个*消息窗口*中定制工具栏的时候，新的配置会自动传递给所有当前打开的*消息窗口*中的工具栏。如果在某一个*消息窗口*中工具栏是隐藏着的，它也会继续保持隐藏，但是一旦当其重新显示，它的外观就会变得和其他该类型的工具栏一样。

工具栏的标识符在对象创建时就是固定的，但是你可以通过调用*setAllowsUserCustomization:*，*setAutosavesConfiguration:*，*setDisplayMode:*来更改工具栏的其他属性。


## Toolbar Items

Each item in an NSToolbar is an instance of NSToolbarItem. The visible parts of a toolbar item are its content, its text label, and its menu form representation.

The toolbar item’s content is either an NSImage or an NSView. An item whose content is an NSImage is called an image item. An item whose content is an NSView is called a view item. The search field shown in Figure 1 is a view item, the others are image items.

Print is a standard item provided by NSToolbar. The other items are custom items, supplied by this application. Blue text is a custom image item. Font Style and Font Size are custom view items.

The menu representation is used at two different times: when the toolbar is displayed using labels only and when the window is too small to show the complete toolbar and some items are displayed in an overflow menu.

## 工具栏项

每个工具栏项在NSToolbar中是一个NSToolbarItem实例。一个工具栏项的可视部分是它的内容，文本标签以及它的菜单表现形式。

工具栏项的内容是一个NSImage或者NSView实例。以NSImage实例做为内容的工具栏项被称作*image项*。以NSView实例做为内容的项被称作*view项*。如图Figure 1中搜索域是一个*view项*，其他的都是*image项*。

`打印`项是一个由NSToolbar提供的标准的工具栏项。其他的项都是定制项，由该app提供。蓝色文本是一个定制的*image项*。`字体风格`和`字体尺寸`是定制*view项*。

菜单展示会在两个不同的时间被使用：当工具栏只使用标签显示时和当窗口太小而无法完整显示工具栏时，一些项就会在*溢出菜单*中显示。


## NSWindow and the toolbar

NSWindow has a number of methods to support NSToolbar: The methods *toolbar* and *setToolbar:* are for attaching a toolbar to the window; the *toggleToolbarShown:* method is the action for the Hide Toolbar / Show Toolbar menu item; *runToolbarCustomizationPalette:* is the action method for the Customize Toolbar menu item.

Interface Builder predefines an application’s View menu with Show Toolbar and Customize Toolbar menu items and connects these items to *toggleToolbarShown:* and *runToolbarCustomizationPalette:* through the First Responder. NSWindow validates both toolbar menu items and toggles the title of the former menu item between “Show Toolbar” and “Hide Toolbar” to correspond with the actual state of the toolbar.


## NSWindow和工具栏

NSWindow有若干个支持NSToolbar的方法：方法*toolbar*和*setToolbar:*是为了将一个工具栏附加到窗口中；*toggleToolbarShown:*方法是为`隐藏工具栏`／`显示工具栏`两个菜单项提供的动作（action）；*runToolbarCustomizationPalette:*方法是为`定制工具栏`菜单项提供的动作（action）。

Interface Builder预定义了一个app的`视图`菜单及其中的`显示工具栏`以及`定制工具栏`菜单项，并且预先通过*First Responder*将它们连接到了*toggleToolbarShown:*和*runToolbarCustomizationPalette:*方法。NSWindow验证工具栏菜单项，并且将`显示工具栏`菜单项的标题在*“显示工具栏”*和*“隐藏工具栏”*之间进行切换以符合工具栏实际的状态。
