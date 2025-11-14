---
tags:
  - odin-inspector
authors: 
  - Yuumix
comments: true
---
# Odin Inspector 自动打开 GettingStartedWindow 的问题

Odin Inspector 是依靠 EditorPrefs 来控制窗口的，初始为 true，显示过之后，变为 false。

``` csharp
[Button]
private static void SetPrefs()
{
    EditorPrefs.SetBool("ODIN_INSPECTOR_SHOW_GETTING_STARTED", true);
    EditorPrefs.SetBool("ACCEPTED_ODIN_3_0_PERSONAL_EULA", true);
}
```
