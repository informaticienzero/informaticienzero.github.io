---
layout: default
title: "Utiliser C# 6 dans Razor avec ASP.NET MVC 5"
date: 2017-09-12 10:07:24
permalink: /:title/
---
Au travail, j'utilise Visual Studio 2017 (yeah !) pour un projet ASP.NET MVC 5. Alors tant qu'à avoir accès à C# version 6, autant en profiter. Sauf que Razor n'aime pas vraiment. Pour le convaincre sans passer pour autant à MVC 6, il existe un moyen très simple. Il suffit d'installer le Nugget [CodeDOM Providers for .NET Compiler](https://www.nuget.org/packages/Microsoft.CodeDom.Providers.DotNetCompilerPlatform/), qui remplace l'ancien compilateur par Roselyn, ce qui, selon la page officielle, permet également d’accélérer la compilation. Et voilà, ne reste plus qu'à profiter !

Voici [la source](https://dusted.codes/using-csharp-6-features-in-aspdotnet-mvc-5-razor-views) qui m'a aidé à résoudre ce problème.