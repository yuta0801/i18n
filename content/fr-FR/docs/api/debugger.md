## Classe : Debugger

> Un transport alternatif pour le protocole de débogage distant de Chrome.

Processus : [Main](../glossary.md#main-process)

Les outils de développement de chrome a une [liaison spéciale](https://chromedevtools.github.io/devtools-protocol/) disponible lors de l’exécution de JavaScript qui permet l’interaction avec les pages et leur instrumentation.

```javascript
const { BrowserWindow } = require('electron')
let win = new BrowserWindow()

try {
  win.webContents.debugger.attach('1.1')
} catch (err) {
  console.log('Debugger attach failed : ', err)
}

win.webContents.debugger.on('detach', (event, reason) => {
  console.log('Debugger detached due to : ', reason)
})

win.webContents.debugger.on('message', (event, method, params) => {
  if (method === 'Network.requestWillBeSent') {
    if (params.request.url === 'https://www.github.com') {
      win.webContents.debugger.detach()
    }
  }
})

win.webContents.debugger.sendCommand('Network.enable')
```

### Événements d’instance

#### Événement : 'detach'

Retourne :

* `event` Événement
* `reason` String - Motif du retrait du débogueur.

Emitted when the debugging session is terminated. This happens either when `webContents` is closed or devtools is invoked for the attached `webContents`.

#### Événement : 'message'

Retourne :

* `event` Événement
* `method` String - Nom de la méthode.
* `params` unknown - Event parameters defined by the 'parameters' attribute in the remote debugging protocol.

Emitted whenever the debugging target issues an instrumentation event.

### Méthodes d’instance

#### `debugger.attach([protocolVersion])`

* `protocolVersion` String (facultatif) - Version de protocole de débogage demandée.

Attache le débogueur au `webContents`.

#### `debugger.isAttached()`

Retourne `Boolean` - Si un débogueur est attaché au `webContents`.

#### `debugger.detach()`

Détache le débogueur depuis le `webContents`.

#### `debugger.sendCommand(method[, commandParams])`

* `method` String - Method name, should be one of the methods defined by the [remote debugging protocol](https://chromedevtools.github.io/devtools-protocol/).
* `commandParams` any (optional) - JSON object with request parameters.

Returns `Promise<any>` - A promise that resolves with the response defined by the 'returns' attribute of the command description in the remote debugging protocol or is rejected indicating the failure of the command.

Envoi une commande au débogueur.