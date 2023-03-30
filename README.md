# Unity Editor Config

# Introduction:

### EditorConfig

- EditorConfig helps maintain consistent coding styles for multiple developers working on the same project across various editors and IDEs.
- The EditorConfig project consists of **a file format** for defining coding styles and a collection of **text editor plugins** that enable editors to read the file format and adhere to the defined styles. EditorConfig files are easily readable and work nicely with version control systems.
- You can add an EditorConfig file to your project or codebase to enforce consistent coding styles for everyone that works in the codebase.
- EditorConfig settings take precedence over global Visual Studio text editor settings. You can tailor each codebase to use text editor settings specific to that project.
- You can still set your own personal editor preferences in the Visual Studio **Options** dialog box. Those settings apply whenever you are working in a codebase without an *.editorconfig* file, or when the *.editorconfig* file doesn't override a particular setting. An example of such a preference is indent style—tabs or spaces.
- To read more about editor config :
    - **[EditorConfig](https://editorconfig.org/)**
    - [**EditorConfig](https://github.com/editorconfig) (Github Repo)**
    - ****[Code style preferences](https://learn.microsoft.com/en-us/visualstudio/ide/create-portable-custom-editor-options?view=vs-2022#add-an-editorconfig-file-to-a-project) (Microsoft)**
    - ****[Code-style rule options](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/code-style-rule-options?view=vs-2022&viewFallbackFrom=vs-2017) (Microsoft)**
    - **[Microsfoft Unity Analyser List](https://github.com/microsoft/Microsoft.Unity.Analyzers/tree/main/doc)**

### Solution Items

- Sometimes you will have files that you wish to include within a solution but that don't belong to any single project. These are usually non-code items, such as documentation or notes.
- However, they can be source code files or resources that you wish to share between multiple [assemblies](http://www.blackwasp.co.uk/Assemblies.aspx) without having the "master copy" held within a particular project. This type of file can be added to your solution as a ***solution item***.
- Solution items can be any type of file. They are linked to the solution, rather than a project.
- The files can be stored anywhere but it is commonplace to place them within the solution's folder structure, making it easier to add them to repositories of revision control systems.
- **DO NOT CHANGE THE NAME OF THIS FOLDER**
- More on **Solution Items :** [http://www.blackwasp.co.uk/VSSolutionItems.aspx#:~:text=Solution items can be any,repositories of revision control systems](http://www.blackwasp.co.uk/VSSolutionItems.aspx#:~:text=Solution%20items%20can%20be%20any,repositories%20of%20revision%20control%20systems)

# Setup  :

### STEP 1 : Adding Microsoft Unity Analyzer :

Unity has [a set of custom C# warnings](https://github.com/microsoft/Microsoft.Unity.Analyzers), called analyzers, that check for common issues with your source code. These analyzers ship out of the box with Visual Studio.Due to how Unity handles its `.csproj` files, it does not seem possible to install packages automatically.

1. You will need to download the analyzers from the NuGet website manually : [https://www.nuget.org/api/v2/package/Microsoft.Unity.Analyzers/1.14.0](https://www.nuget.org/api/v2/package/Microsoft.Unity.Analyzers/1.14.0)
2. Create a “NuGet” folder in the root of your unity project where the solution file is present.
3. When you're done, open the package file using a tool such as 7zip and extract `Microsoft.Unity.Analyzers.dll` and place it inside the NuGet folder you just created.
    ![Untitled](https://user-images.githubusercontent.com/59344201/228808052-75590680-8dbe-45a0-8b23-6cf51bd93d93.png)

4. Do not place it inside `Assets` or `Packages`, as that will cause Unity to try to process the `.dll`, which will make it output an error in the console.
    
    ![Untitled 1](https://user-images.githubusercontent.com/59344201/228808295-10870659-6a0a-4443-80ee-35b28391b79d.png)


### STEP 2 : Create Omnisharp file :

1. Next, create a json file with a name `omnisharp.json` (case sensitive) file at the root folder of your project. 

    ![Untitled 2](https://user-images.githubusercontent.com/59344201/228808839-d4f31de9-662d-4329-83db-02c420c1dacc.png)

2. Analyzer support in OmniSharp is experimental at the moment, so we need to enable it explicitly. We also need to point it to the `.dll` file we just extracted.
3. Add this code to this newly created `omnisharp.json` and save it.
    
    ```jsx
    {
      "RoslynExtensionsOptions": {
        "EnableAnalyzersSupport": true,
        "LocationPaths": ["./NuGet/microsoft.unity.analyzers.1.14.0"]
      }
    }
    ```
    
4. Keep in mind the current version of `microsoft.unity.analyzers` at the making of this document is **1.14.0**, if you download any other version, you’ll have to specify that version in the `omnisharp.json`
5. LocationPaths `"./NuGet/microsoft.unity.analyzers.1.14.0"` is a relative path pointing to the folder containing the `.dll` file in the root project. For consistency, we will be creating a folder names “**NuGet**” (case sensitive) when we were setting up the analyzer, as mentioned in Step 1.

### STEP 3 : Create .editorConfig file :

Note that while it is possible to activate these analyzers, the suppressors they ship with the package (that turn off other C# warnings that may conflict with these custom ones) may not be picked up by OmniSharp at the moment, [according to this thread](https://github.com/microsoft/Microsoft.Unity.Analyzers/issues/122#issuecomment-743747554). You can still turn off specific rules manually by creating a `.editorconfig` file. 

**Work flow for `.editorconfig` file with Unity :** 

1. Once all the above steps are done, open the unity project from Unity hub.
2. Create and compile a new C# script. This will create a new `Assembly-CSharp` and `Assembly-CSharp.Player` project and it will reflect in the Solution Explorer sidebar.
    
    ![Untitled 3](https://user-images.githubusercontent.com/59344201/228809230-9931461a-61d0-42b9-9a65-62f7647d0c75.png)

    
3. Solution Explorer can be accessed from View > Solution Explorer

    ![Untitled 4](https://user-images.githubusercontent.com/59344201/228809521-a5470682-4fbd-4c5e-896a-161f91b8d13f.png)

    
4. Once the `Assembly-CSharp` and `Assembly-CSharp.Player` projects are created, right click on the Solution (top most row on the Solution Explorer Sidebar), hover mouse over “Add” and click on “New EditorConfig”.
    
    ![Untitled 5](https://user-images.githubusercontent.com/59344201/228809919-702f36c4-001c-427b-bea2-618609c2ad8a.png)

    
5. If every thing goes right, you should see a new folder now automatically created called “***solution item***” with a `.editorconfig` file also generated. 
    
    ![Untitled 6](https://user-images.githubusercontent.com/59344201/228810064-2cf52c60-bbb2-459d-bd80-de679b9088cc.png)

6. If you try to open this `.editorconfig` file , this is what it’ll look like. (It might take some time to load)

    ![Untitled 7](https://user-images.githubusercontent.com/59344201/228810183-750ba9e6-c8ff-4923-9529-60116f3c3796.png)

    <aside>
    💡 **IMPORTANT :** 
    If you try to open the editor file, it’ll look like this very friendly UI. But it has a **serious bug** right now. You’d think the UI would be updating as per you make changes to the file, but no. The UI is extraordinarily buggy and not getting fixed any time soon.
    
    These links will give you a better idea : 
     https://github.com/dotnet/roslyn/issues/54556
     https://github.com/dotnet/roslyn/issues/58609
    
    **!! PLEASE DO NOT MAKE ANY CHANGES TO THE CONFIG FILE !!**
    
    </aside>
    
7. **As soon as you can see the `.editorconfig` file, save the project and close Visual Studio as well as the Unity Project.**
8. You’ll now see that a new file has been created in the root of the project directory with mostly 0KB size.
    
    ![Untitled 8](https://user-images.githubusercontent.com/59344201/228810313-917c0773-8ee2-48b5-8cbd-ea231b7edf15.png)

9. Replace that`.editorconfig` file with the one provided from [**github](https://github.com/AutoVRse-Enterprise/Unity-CSharp-Styleguide/blob/main/.editorconfig).** 
10. Now if you open the Unity project again, go to Assets Tab > Open C# Project 
    
    ![Untitled 9](https://user-images.githubusercontent.com/59344201/228810523-aa02dab4-41a7-432a-97e3-d15f8828d44d.png)
    
11. Visual Studio will start compiling all the scripts again. The bottom left of the IDE would be doing background tasks now that you could see. 
    
    ![Untitled 10](https://user-images.githubusercontent.com/59344201/228810526-f49a410d-268a-4bbb-97c6-b3537e1d9463.png)
    
12. Wait for all the tasks to get completed. Once its done, it should say ready at the bottom left.
    
    ![Untitled 11](https://user-images.githubusercontent.com/59344201/228810531-06ff57a5-33e2-47ad-86f2-97807494cd84.png)
    
13. As a fail-safe measure, make sure “**Follow project coding conventions”** is checked in VS Studio. To check, go to Tools > Options > Text Editor > General, check the option and click OK.
With this `**User preferences for this file type are overridden by this project's coding conventions.**`
    
    ![Untitled 12](https://user-images.githubusercontent.com/59344201/228810536-9566cb0f-9ebc-4ccf-b7ba-d97a29b13c93.png)
    
14. If everything goes right, you should now see a lot of warning and squiggly lines in your code. This means the editor config is now working 😄

### STEP 4 : Configure Code Cleanup :

1. We first need to configure our code clean up profile. There are two profiles in Visual Studio. 
2. To open this dialog box, click the expander arrow next to the code cleanup broom icon at the bottom and then choose **Configure Code Cleanup**
    
    ![Untitled 13](https://user-images.githubusercontent.com/59344201/228810542-9c8c3163-7f1f-492f-89c9-72205a252404.png)
    
3. Alternatively, you can go to **Analyse** > **Configure Code Cleanup**
    
    ![Untitled 14](https://user-images.githubusercontent.com/59344201/228810546-2d08a404-1442-4e4e-b2db-9ffdd20ab74f.png)
    
4. Configure which code styles you want to apply (in one of two profiles) in the **Configure Code Cleanup** dialog box. 
5. By Default Profile 1 would be selected with a few ***rules*** in **Included fixers.** 
6. Select all the rules from and **Available fixers** (you can hold shift and select them all) and  
click on the up arrow button.
    
    ![Untitled 15](https://user-images.githubusercontent.com/59344201/228810514-aa7b2873-bfbd-449e-b9f4-efbfa98d97e2.png)
    
    1. Click **OK** and the dialogue box will close.
    2. You now have a code cleanup profile that is ready to clean any C# file with the new `.editorconfig` file.  
    

### STEP 5 : Testing Code Cleanup :

- After you've configured code cleanup, you can either click on the broom icon or press **Ctrl**+**K**, **Ctrl**+**E (Keep the Ctrl pressed, notice VS studio waiting for a new key press on the bottom left)** to run code cleanup.
- You can also run code cleanup across your entire project or solution. Right-click on the project or solution name in **Solution Explorer**, select **Analyze and Code Cleanup**, and then select **Run Code Cleanup**.
    
    ![Untitled 16](https://user-images.githubusercontent.com/59344201/228810519-4670039c-74f3-40d8-980f-24049898d175.png)
    
- It might take some time to apply all the chages as there is a possibility of vs studio not fixing the code automatically sometimes, if this happens please fix the code manually.

