---<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Chat entre 2 personnes</title>
</head>
<body>
  <div id="chat">
    <div id="messages"></div>
    <input type="text" id="messageInput" placeholder="Tape ton message…">
    <button onclick="sendMessage()">Envoyer</button>
  </div>
  <script src="chat.js"></script>
</body>
</html>
title: Tutorial - Introduction
sidebar_label: Introduction
slug: introduction
---const express = require('express');
const http = require('http');
const socketIo = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIo(server);

app.use(express.static(__dirname + '/public'));

io.on('connection', (socket) => {
  console.log('Un utilisateur est connecté');

  socket.on('chat message', (msg) => {
    socket.broadcast.emit('chat message', msg); // envoie à l’autre utilisateur
  });

  socket.on('disconnect', () => {
    console.log('Utilisateur déconnecté');
  });
});

server.listen(3000, () => {
  console.log('Serveur démarré sur http://localhost:3000');
});

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Getting started

Welcome to the Socket.IO tutorial!

In this tutorial we'll create a basic chat application. It requires almost no basic prior knowledge of Node.JS or Socket.IO, so it’s ideal for users of all knowledge levels.

## Introduction

Writing a chat application with popular web applications stacks like LAMP (PHP) has normally been very hard. It involves polling the server for changes, keeping track of timestamps, and it’s a lot slower than it should be.

Sockets have traditionally been the solution around which most real-time chat systems are architected, providing a bi-directional communication channel between a client and a server.

This means that the server can *push* messages to clients. Whenever you write a chat message, the idea is that the server will get it and push it to all other connected clients.

## How to use this tutorial

### Tooling

Any text editor (from a basic text editor to a complete IDE such as [VS Code](https://code.visualstudio.com/)) should be sufficient to complete this tutorial.

Additionally, at the end of each step you will find a link to some online platforms ([CodeSandbox](https://codesandbox.io) and [StackBlitz](https://stackblitz.com), namely), allowing you to run the code directly from your browser:

![Screenshot of the CodeSandbox platform](/images/codesandbox.png)

### Syntax settings

In the Node.js world, there are two ways to import modules:

- the standard way: ECMAScript modules (or ESM)

```js
import { Server } from "socket.io";
```

Reference: https://nodejs.org/api/esm.html

- the legacy way: CommonJS

```js
const { Server } = require("socket.io");
```

Reference: https://nodejs.org/api/modules.html

Socket.IO supports both syntax. 

:::tip

We recommend using the ESM syntax in your project, though this might not always be feasible due to some packages not supporting this syntax.

:::

For your convenience, throughout the tutorial, each code block allows you to select your preferred syntax:

<Tabs groupId="lang">
  <TabItem value="cjs" label="CommonJS" default>

```js
const { Server } = require("socket.io");
```

  </TabItem>
  <TabItem value="mjs" label="ES modules">

```js
import { Server } from "socket.io";
```

  </TabItem>
</Tabs>


Ready? Click "Next" to get started.
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Chat entre 2 personnes</title>
  <style>
    #messages { border: 1px solid #ccc; height: 200px; overflow-y: auto; margin-bottom: 10px; padding: 5px;}
  </style>
</head>
<body>
  <div id="messages"></div>
  <input id="input" autocomplete="off" /><button onclick="send()">Envoyer</button>
  <script src="/socket.io/socket.io.js"></script>
  <script>
    const socket = io();
    const messages = document.getElementById('messages');
    const input = document.getElementById('input');

    function send() {
      if (input.value) {
        messages.innerHTML += `<div><b>Moi:</b> ${input.value}</div>`;
        socket.emit('chat message', input.value);
        input.value = '';
      }
    }

    socket.on('chat message', function(msg){
      messages.innerHTML += `<div><b>Lui/Elle:</b> ${msg}</div>`;
      // Notification simple
      if (document.hidden) {
        if (Notification.permission === "granted") {
          new Notification("Nouveau message reçu !");
        }
      }
    });

    // Gestion des notifications
    if (window.Notification && Notification.permission !== "granted") {
      Notification.requestPermission();
    }

    input.addEventListener("keyup", function(event) {
      if (event.key === "Enter") send();
    });
  </script>
</body>
</html>
const { Server } = require("socket.io");
