---
layout: post
tags: Python
author: "Adam Heinz"
date: 2024-06-02 01:00:00
---
AIDE MEMOIRE

How to use virtual environments in Python with Venv 
=================================================== 

Virtual environments enable us to manage project-specific dependencies in python using venv. 

For windows users, the virtual environment may be setup in powershell using: 

```powershell 
python -m venv .venv
```

Before using it, you may first need to change execution policies in Powershell: 

```powershell 
Set-ExecutionPolicy Unrestricted -Scope Process
``` 

Activate the virtual environment in PowerShell by running: 
```powershell 
.venv\Scripts\Activate.ps1
```

Success is visible when the path begins with (.venv)


You can close the virtual environment with: 
```powershell
deactivate
```

QED 

© Adam Heinz 

3 June 2024