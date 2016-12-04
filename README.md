# Ionic Push notification

Implementando Push Notifications no Ionic 1.

## Começando

Para criar um aplicativo Android pelo CLI do Ionic
```
ionic start PushApp
```

```
ionic platform add android
```

## Adicionando o plugin de push notification

```
cordova plugin add phonegap-plugin-push
```

## Configurações

Agora temos que configurar nosso app.js que fica na pasta `www/`.

Nele devemos iniciar o plugin passando as configurações necessárias que são as seguintes:

```json
{
"android" : {
  "senderID" : "Seu ID de Remetente",
  "icon" : "icon"
},
"ios" : {
  "alert" : "true",
  "badge" : "true",
  "sound" : "true"
},
"windows" : {}
}
```

A configuração de inicialização deve ser feita dentro de `PushNotification.init`. Você pode passar esse objeto para uma variável para pode reutilizar melhor.

```js
var push = PushNotification.init({
  "android" : {
    "senderID" : "Seu ID de Remetente",
    icon : "icon"
  },
  "ios" : {
    "alert" : "true",
    "badge" : "true",
    "sound" : "true"
  },
  "windows" : {}
});
```

## Configurado eventos

Agora devemos configurar a parte dos eventos.

Sempre que seu aplicativo é inicializado, ele irá fazer uma requisição ao Firebase e o Firebase retornará um device token. Isso identifica seu dispostivo no Firebase.

Geralmente, seu aplicativo terá de mandar esse token para seu servidor, para que ele possa fazer push notifications de forma individual.

Para armazenar esse token, devemos criar um evento que capture esse token quando o Firebase responder a requisição.

```js
push.on('registration', function(data){
  console.log(data);
  // sua lógica
});
```

Dentro deste evento, você irá configurar como irá mandar isso para o servidor. Seja colocando esse `data` dentro do seu localStorage e enviando para seu servidor assim que fizer login ou algo do tipo.

Bom, nesse ponto, você já pode entrar no seu console do Firebase e mandar uma mensagem que ela chegará no seu dispostivo.

Agora, se ainda quiser tratar essa notification, você também pode.

Imagina que seu push server irá passar um parâmetro indicando qual view do seu aplicativo deve ser aberta. Isso também pode ser feito usando o evento __notification__.

```js
push.on('notification', function(data){
  alert('Notificação');
  console.log(data.message);
  console.log(data.title);
  console.log(data.count);
  console.log(data.sound);
  console.log(data.image);
  console.log(data.additionalData);
});
```

## Documentação da API
Esse plugin implementa uma série de outras features, você pode conferir no link abaixo.
https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/API.md

## Créditos
http://tsdn.tecnospeed.com.br/blog-do-desenvolvimento-tecnospeed/post/notificacao-push-em-android-utilizando-ionic-e-nodejs-2
