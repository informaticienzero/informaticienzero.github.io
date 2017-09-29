---
layout: default
title: "Débogguer avec Electron"
date: 2017-04-25 22:24:41
permalink: /:title/
---
Depuis quelques temps, je m'amuse avec Node.js et notamment [Electron](https://electron.atom.io/), un framework permettant de faire des applications natives en HTML, CSS et JavaScript. L'application tourne dans un conteneur Chromium, ce qui permet d'avoir accès à tout ce qu'on connait des applications front-end classiques comme jQuery ou **les dev tools**.

<!--excerpt-->

{% highlight javascript %}
const electron = require('electron');
const BrowserWindow = electron.BrowserWindow;

let mainWindow;

function createWindow() {
	mainWindow = new electron.BrowserWindow({
		width: 400,
		height: 600,
		title: "Title"
	});

	// That's the main line.
	mainWindow.webContents.openDevTools();
	mainWindow.loadURL(`file://${__dirname}/index.html`);
}
{% endhighlight %}

Et heureusement qu'ils sont là, ces dev tools. C'est grâce à la console de Chromium que j'ai pu trouver pourquoi mon tableau sur lequel j'itérais était vide et donc pourquoi le `forEach` me claquait une exception. En effet, à un moment, je lisais un JSON qui contenait la clé `environment` , que j'avais mis au pluriel dans le code. Forcément, si je ne sais pas écrire. C'est vrai que je ne peux pas féliciter JavaScript sur la clarté des erreurs. Mais au moins, je m'amuse dans ma veille.
