Configuring VSCODE so it is possible to load modules living in the project directory from other scrips not living in project root. Example with the following structure: 
```
PROJECT ROOT
|_main.py
|_Folder1
	|_module.py
|_Folder2
	|_script.py
```
	
By default, running script.py with the following code will not work:
```
Import Folder1.module as mod
```
The reason is that VSCode runs scrip.py from Folder and therefore does not know  where to find Folder1. 

Here is the fix. This will create two files (launch.json and settings.json). Launch.json controls settings when running the debugger and settings.json controls running python without debugger. 

Step1: Navigate to the debugger on the left side bar and click "create a lauch.json file". 

![image](https://github.com/MxBoud/vscode_settings/assets/21281558/8669612a-0399-43b5-bd61-235681f8a020)


Step 2: Open the launch.json file

![image](https://github.com/MxBoud/vscode_settings/assets/21281558/8167d8a4-adaa-4f39-82dc-ae6953226e53)


 and add the following :
 ```
"env": {
                "PYTHONPATH": "${workspaceFolder}"}
        }
```
The total code should look like this: 

```
.vscode/launch.json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "justMyCode": true,
            "env": {
                "PYTHONPATH": "${workspaceFolder}"}
        }
    ]
}
```

Step 3 : Create .vcode/settings.json and add the following:
```

{
"terminal.integrated.env.linux": {
    "PYTHONPATH": "${workspaceFolder}/src:${env:PYTHONPATH}"
  },
  "terminal.integrated.env.osx": {
    "PYTHONPATH": "${workspaceFolder}/src:${env:PYTHONPATH}"
  },
  "terminal.integrated.env.windows": {"PYTHONPATH": "${workspaceFolder};${env:PYTHONPATH}
```

This solution was strongly inspired of: 
From <https://github.com/microsoft/vscode-python/issues/12085> 
