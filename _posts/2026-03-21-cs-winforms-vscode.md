---
title: "C#: Windows Forms: VS Code"
author: "Adam Heinz"
date: 2026-03-21T12:00:00-00:00
categories:
  C#
tags:
  - Forms
---
How to create Windows Forms in VS Code.  


# Overview

VS Code provides an alternative to Visual Studio Community Edition. Getting started with creating a GUI with Windows Forms requires using the PowerShell terminal.

# Getting started

Install the `C#` extension in VS Code, and install `.Net` on the computer. 

# App folder
In your repo create a new folder or directory named 'app'. 

Navigate to the app folder: 

```powershell
cd app
```

# Preview options available

```powershell
dotnet new
```

The terminal will print something like: 

```
The 'dotnet new' command creates a .NET project based on a template.

Common templates are:
Template Name        Short Name  Language    Tags
-------------------  ----------  ----------  -----------------------
Blazor Web App       blazor      [C#]        Web/Blazor/WebAssembly 
Class Library        classlib    [C#],F#,VB  Common/Library
Console App          console     [C#],F#,VB  Common/Console
MSTest Test Project  mstest      [C#],F#,VB  Test/MSTest/Desktop/Web
Windows Forms App    winforms    [C#],VB     Common/WinForms        
WPF Application      wpf         [C#],VB     Common/WPF

An example would be:
   dotnet new console
```

# Create New Windows Forms project

Use `winforms` - the short name - to create a new Windows form project

```powershell
dotnet new winforms
```

The terminal will print: 

```
The template "Windows Forms App" was created successfully.
```

# Directory structure

The initial directory structure will be created: 

```
app/
├── bin/
├── obj/
├── app.csproj
├── app.csproj.user
├── Form1.cs
├── Form1.Designer.cs
├── Program.cs
└── ...
```

This includes a first, blank form, `Form1.cs`.

# Inspect 

Verify everything is working by running the project: 

```powershell
dotnet run
```

This will preview the first (blank) form. 


QED 

© Adam Heinz 

22 March 2026
