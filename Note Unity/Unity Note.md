# Unity Note

[TOC]

## Gizmo







[参考链接](https://blog.theknightsofunity.com/custom-gizmos-ondrawgizmos/)





# 贝塞尔曲线



Handles.DrawBezier(startPos, endPos, startTan, endTan, color, null, linewidth);





# 悬浮窗体创建



```c#
BeginWindows();
windowrect = GUI.Window(5, windowrect, mywindowfunction, "TEST WINDOW");
EndWindows();
```



窗体内容

```c#
    public static void mywindowfunction(int windowid)
    {
        GUILayout.BeginVertical();
        if (GUILayout.Button("窗体内按钮1"))
        {
            Debug.Log("SHow1");
        }
        if (GUILayout.Button("窗体内按钮2"))
        {
            Debug.Log("SHow1");
        }
        GUILayout.EndVertical();
        GUI.DragWindow(new Rect(0, 0, 10000, 10000));
        //定义窗体可以活动的范围
    }
```



