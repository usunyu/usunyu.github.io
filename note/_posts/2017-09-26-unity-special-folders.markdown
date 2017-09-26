---
layout: post
title: Special Folder Names in your Assets Folder
---

#### Hidden Folders:
Folders that start with a dot (e.g. ".UnitTests/", ".svn/") are ignored by Unity. Any assets in there are not imported, and any scripts in there are not compiled. They will not show up in the Project view.


#### "Standard Assets":
Scripts in here are always compiled first. Scripts are output to either Assembly-CSharp-firstpass, Assembly-UnityScript-firstpass, or Assembly-Boo-firstpass, depending on the language. See http://docs.unity3d.com/Documentation/Manual/ScriptCompileOrderFolders.html


Scripts inside the Standard Assets folder will be compiled earlier than your other scripts. So, placing scripts in Standard Assets is one way for C# scripts to be able to access .js scripts or vice-versa.


#### "Editor":
The Editor folder name is a special name which allows your scripts access to the Unity Editor Scripting API. If your script uses any classes or functionality from the UnityEditor namespace, it has to be placed in a folder called Editor.


Scripts inside an Editor folder will not be included in your game's build. They are only used in the Unity Editor.


You can have multiple Editor folders throughout your project.


Note: An Editor folder not located in another special folder can be placed/nested anywhere in the project. However, if it's in "Standard Assets", "Pro Standard Assets", or "Plugins", it must be a direct child of these folders. Otherwise, it will not get processed. For example, it's ok to have a path like "My Extension/Scripts/Editor", but if placed in a special folder, it must always be "Standard Assets/Editor/My Extension/Scripts", or "Pro Standard Assets/Editor/My Extension/Scripts", or "Plugins/Editor/My Extension/Scripts".


#### "Plugins":
The "Plugins" folder is where you must put any native plugins, which you want to be accessible by your scripts. They will also be automatically included in your build. Take note that this folder may not be in any subfolder (it has to reside within the top-level Assets folder).


In Windows, native plugins exist as .dll files, in Mac OS X, they are .bundle files, and in Linux, they are .so files.
Like the Standard Assets folder, any scripts in here are compiled earlier, allowing them to be accessed by other scripts (of any language) that are outside the Plugins folder.


#### "Plugins/x86":
If you are building for 32-bit or a universal (both 32 and 64 bit) platform, and if this subfolder exists, any native plugin files in this folder will automatically be included in your build. If this folder does not exist, Unity will look for native plugins inside the parent Plugins folder instead.



#### "Plugins/x86_64":
If you are building for 64-bit or a universal (both 32 and 64 bit) platform, and if this subfolder exists, any native plugin files in this folder will automatically be included in your build. If this folder does not exist, Unity will look for native plugins inside the parent Plugins folder instead.


If you are making a universal build, it's recommended you make both the x86 and x86_64 subfolders. Then have the 32-bit and 64-bit versions of your native plugins in the proper subfolder correspondingly.


#### "Plugins/Android":
Place here any Java .jar files you want included in your Android project, used for Java-based plugins. Any .so file (when having Android NDK-based plugins) will also be included. See http://docs.unity3d.com/Documentation/Manual/PluginsForAndroid.html


#### "Plugins/iOS":
A limited, simple way to automatically add (as symbolic links) any .a, .m, .mm, .c, or .cpp files into the generated Xcode project. See http://docs.unity3d.com/Documentation/Manual/PluginsForIOS.html


If you need more control how to automatically add files to the Xcode project, you should make use of the PostprocessBuildPlayer feature. Doing so does not require you to place such files in the Plugins/iOS folder. See http://docs.unity3d.com/Documentation/Manual/BuildPlayerPipeline.html


#### "Resources":
The Resources folder is a special folder which allows you to access assets by file path and name in your scripts, rather than by the usual (and recommended) method of direct references (as variables in scripts, wherein you use drag-and-drop in the Unity Editor).


For this reason, caution is advised when using it. All assets you put in the Resources folder are always included in your build (even if it turned out that they are unused), because, since you are using them via scripts, Unity then has no way of determining which Resources-based assets are used or not.


You can have multiple Resources folders throughout your project, so it is not recommended to have an asset in one Resources folder and have another asset with that same name in another Resources folder.


Once your game is built, all assets in all Resources folders get packed into the game's archive for assets. This means the Resources folder technically doesn't exist anymore in your final build, even though your code will still access them via the paths that existed while it was in your project.


Also see http://docs.unity3d.com/Documentation/Manual/LoadingResourcesatRuntime.html


Take note when assets are accessed as variables of MonoBehaviour scripts, those assets get loaded into memory once that MonoBehaviour script is instantiated (i.e. its game object or prefab is now in the scene). This may be undesirable, if the asset is too large and you want more control of when it gets loaded into memory.


Consider putting such large assets in a Resources folder, and load them via Resources.Load. When not used anymore, you can free the memory it took up by calling Object.Destroy on the object, followed by Resources.UnloadUnusedAssets.


#### "Editor Default Resources":
This folder functions like a Resources folder, but is meant for editor scripts only. Use this if your editor plugin needs to load assets (e.g. icons, GUI skins, etc.) while making sure said assets won't get included in the user's build (putting such files in a normal Resources folder would have meant that those assets would be included in the user's game when built).


As editor scripts aren't MonoBehaviour scripts, you can't do the usual way of accessing assets (i.e. dragging-and-dropping via the Inspector). The "Editor Default Resources" is for a convenient way around this.


To access assets inside the "Editor Default Resources", you need to use EditorGUIUtility.Load.


Take note that unlike Resources.Load, EditorGUIUtility.Load requires you to specify the filename extension of the asset you're trying to load. So it has to be "myPlugin/mySkin.guiskin" instead of "myPlugin/mySkin".


To free memory used by EditorGUIUtility.Load, call Object.Destroy on the object, then call EditorUtility.UnloadUnusedAssets.
Afaik, only one "Editor Default Resources" folder can be present, and it has to be directly under the top Assets folder.


#### "Gizmos":
The gizmos folder holds all the texture/icon assets for use with Gizmos.DrawIcon. Texture assets placed inside this folder can be called by name, and drawn on-screen as a gizmo in the editor.


#### "WebPlayerTemplates":
Used to replace the default web page used for web builds. Any scripts placed here will not be compiled at all. This folder has to be in your top-level Assets folder (it should not be in any subfolder in your Assets directory).


#### "StreamingAssets":
Any files in here are copied to the build folder as is, without any changes (except for mobile and web builds, where they get embedded into the final build file). The path where they are can vary per platform but is accessible via Application.streamingAssetsPath (http://docs.unity3d.com/Documentation/ScriptReference/Application-streamingAssetsPath.html) Also see http://docs.unity3d.com/Documentation/Manual/StreamingAssets.html


#### Reference:
* [Special Folder Names in your Assets Folder](http://wiki.unity3d.com/index.php/Special_Folder_Names_in_your_Assets_Folder)
