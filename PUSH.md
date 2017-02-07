# Ativando notificações PUSH

* Instalando Ionic Cloud Client
```
$ npm install @ionic/cloud --save
$ cp node_modules/@ionic/cloud/dist/bundle/ionic.cloud.min.js www/lib
```

* Incluir no www/index.html, abaixo do include do ionic.bundle.js
```
[...]
<script src="lib/ionic.cloud.min.js"></script>
[...]
```

* Alterar o id no arquivo ionic.config.json pro id obtido no painel do apps.ionic.io/apps

* Inserir o $ionicCloudProvider no bloco de config, antes do ".run(function($ionicPlatform)". Trocar o "APP_ID" pelo id configurado antes em ionic.config.json
```
.config(function($ionicCloudProvider) {
  $ionicCloudProvider.init({
    "core": {
      "app_id": "!!!O_SEU_APP_ID!!!"
    }
  });
})
```

* Criar um perfil com credenciais de push

Criar o app no firebase e copiar a "chave do servidor"
Colar a chave do servidor num novo "security profile" em Settings > Certificates no ionic.io
Ref: https://docs.ionic.io/services/profiles/#android-fcm-project--server-key

* Instalar o push plugin com as credenciais de SENDER_ID
```
$ cordova plugin add phonegap-plugin-push --variable SENDER_ID=!!!O_SEU_SENDER_ID!!! --save
```

* Acrescentar o SENDER_ID e eventuais configurações extras no www/js/app.js
```
"push": {
  "sender_id": "!!!O_SEU_SENDER_ID!!!",
  "pluginConfig": {
    "android": {
      "iconColor": "#343434"
    }
  }
}
```

* Injetar o push nas dependências de modulo, arquivo www/js/controllers.js
```
angular.module('myapp.controllers', ['ionic.cloud'])

.controller('MyCtrl', function($scope, $ionicPush) {
  ...
})
```
