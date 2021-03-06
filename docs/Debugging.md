---
id: debugging
title: Debugging
layout: docs
category: Guides
permalink: docs/debugging.html
next: performance
previous: timers
---

## Ativando Atalhos do Teclado
O React Native suporta alguns atalhos de teclado no Simulador do iOS. Eles são descritos abaixo. Para ativá-los, abra o menu Hardware, selecione Teclado e verifique se "Connect Hardware Keyboard" está marcado.

## Acessando o menu do desenvolvedor no aplicativo

Você pode acessar o menu do desenvolvedor agitando o seu dispositivo ou selecionando "Shake Gesture" dentro do menu Hardware no iOS Simulator. Você também pode usar o atalho de teclado `⌘D` quando seu aplicativo estiver sendo executado no iOS Simulator, ou `⌘M` quando executado em um emulador Android.

![](img/DeveloperMenu.png)

> O Menu do Desenvolvedor está desativado nas versões release (produção).

## Recarregando JavaScript

Ao invés de recompilar seu aplicativo toda vez que você fizer uma alteração, você poderá recarregar o código JavaScript do aplicativo instantaneamente. Para fazer isso, selecione "Recarregar" no Menu do desenvolvedor. Você também pode pressionar `⌘R` no iOS Simulator ou tocar em `R` duas vezes em emuladores Android.

### Recarregamento automático

Você pode acelerar seus tempos de desenvolvimento fazendo com que seu aplicativo seja recarregado automaticamente sempre que seu código for alterado. O recarregamento automático pode ser ativado selecionando "Ativar recarregamento ao vivo" no Menu do desenvolvedor.

Você pode até mesmo dar um passo além e manter seu aplicativo em execução, já que novas versões de seus arquivos são injetadas no pacote do JavaScript automaticamente ativando o [Hot Reloading](https://facebook.github.io/react-native/blog/2016/03/24/introducing-hot-reloading.html) no Menu do Desenvolvedor. Isso permitirá que você mantenha a execução do aplicativo por meio de recarregamentos.

> Existem alguns casos em que o Hot Reloading não pode ser implementado perfeitamente. Se você encontrar algum problema, faça um recarregamento total para redefinir seu aplicativo.

Você precisará reconstruir seu aplicativo para que as alterações entrem em vigor em determinadas situações:

*Você adicionou novos recursos ao pacote do aplicativo nativo, como uma imagem em `Images.xcassets` no iOS ou na pasta `res/drawable` no Android.
* Você modificou o código nativo (Objective-C/Swift para iOS ou Java/C++ para Android).

## Erros e avisos no aplicativo

Erros e avisos são exibidos dentro do seu aplicativo em compilações de desenvolvimento.

### Erros

Erros no aplicativo são exibidos em um alerta de tela cheia com um fundo vermelho dentro do seu aplicativo. Essa tela é conhecida como RedBox. Você pode usar o `console.error()` para ativar manualmente um.

### Avisos

Avisos serão exibidos na tela com um fundo amarelo. Esses alertas são conhecidos como YellowBoxes. Clique nos alertas para mostrar mais informações ou descartá-los.

Como no RedBox, você pode usar o `console.warn()` para acionar um YellowBox.

YellowBoxes pode ser desabilitado durante o desenvolvimento usando `console.disableYellowBox = true;`. Avisos específicos podem ser ignorados programaticamente definindo uma matriz de prefixos que devem ser ignorados: `console.ignoredYellowBox = ['Warning: ...'];`.

No CI/Xcode,as YellowBoxes podem ser desabilitadas configurando a variável `IS_TESTING`.

>  RedBoxes e YellowBoxes são automaticamente desativados em compilações de release (produção).

## Ferramentas para desenvolvedores do Chrome

Para depurar o código JavaScript no Chrome, selecione "Depurar JS Remotamente" no Menu do Desenvolvedor. Isso abrirá uma nova guia em [http://localhost:8081/debugger-ui](http://localhost:8081/debugger-ui).

Selecione Ferramentas → Ferramentas do Desenvolvedor no menu do Chrome para abrir as [Ferramentas de desenvolvimento](https://developer.chrome.com/devtools). Você também pode acessar o DevTools usando atalhos de teclado (`⌘⌥I` no macOS, 'Ctrl''Shift''I' no Windows).  Você também pode querer ativar  [Pausar ao detectar exceções](http://stackoverflow.com/questions/2233339/javascript-is-there-a-way-to-get-chrome-to-break-on-all-errors/17324511#17324511) para uma melhor experiência durante a depuração.

> Nota: A extensão no Google Chorme "React Developer Tools"  não funciona com o React Native, mas você pode usar sua versão autônoma. Leia [está seção](docs/debugging.html#react-developer-tools) para saber como.

### Debugando usando um depurador JavaScript personalizado

Para usar um depurador JavaScript personalizado no lugar das Ferramentas do desenvolvedor do Chrome, defina a variável de ambiente  `REACT_DEBUGGER` como um comando que iniciará seu depurador personalizado. Você pode então selecionar "Depurar JS Remotamente" no Menu do Desenvolvedor para iniciar a depuração.

O depurador receberá uma lista de todas as raízes do projeto, separadas por um espaço. Por exemplo, se você definir o `REACT_DEBUGGER="node /path/to/launchDebugger.js --port 2345 --type ReactNative"`, então o comando `node /path/to/launchDebugger.js --port 2345 --type ReactNative /path/to/reactNative/app` será usado para iniciar seu depurador.

> Comandos de depuração personalizados executados dessa maneira devem ser processos de curta duração e não devem produzir mais de 200 kilobytes de saída.

## Ferramenta para Desenvolvedores React

Você pode usar [a versão independente da Ferramenta para Desenvolvedores React](https://github.com/facebook/react-devtools/tree/master/packages/react-devtools) para depurar a hierarquia do componente React.  Para usá-lo, instale o pacote `react-devtools` globalmente:

```
npm install -g react-devtools
```

Agora execute o `react-devtools` do terminal para iniciar o aplicativo DevTools autônomo:

```
react-devtools
```

![React DevTools](img/ReactDevTools.png)

Ele deve se conectar ao seu simulador dentro de alguns segundos.

> Nota: se você preferir evitar instalações globais, você pode adicionar `react-devtools` como uma dependência do projeto. Adicione o pacote `react-devtools` ao seu projeto usando `npm install --save-dev react devtools`, depois adicione `"react-devtools":"react-devtools"` ao `scripts` em seu `package.json` e depois execute `npm run react-devtools` na pasta do seu projeto para abrir o DevTools.

### Integração com o React Native Inspector

Abra o menu do desenvolvedor no aplicativo e escolha "Show Inspector". Ele exibirá uma sobreposição que permite tocar em qualquer elemento da interface do usuário e ver informações sobre ele:

![React Native Inspector](img/Inspector.gif)

No entanto, quando o `react-devtools` estiver em execução, o Inspector entrará em um modo especial e, em vez disso, usará o DevTools como interface principal. Neste modo, clicar em algo no simulador mostrará os componentes relevantes nos DevTools:

![React DevTools Inspector Integration](img/ReactDevToolsInspector.gif)

Você pode escolher "Ocultar inspetor" no mesmo menu para sair desse modo.

### Inspeção de instâncias de componentes

Quando depurar o JavaScript no Chrome, você pode inspecionar os acessórios e o estado dos componentes do React no console do navegador.

Primeiro, siga as instruções para depuração no Chrome para abrir o console do Chrome.

Certifique-se de que a lista suspensa no canto superior esquerdo do console do Chrome está dizendo `debuggerWorker.js`. **Esta etapa é essencial.**

E então, selecione um componente React no React DevTools. Existe uma caixa de pesquisa no topo que ajuda você a encontrar um pelo nome. Assim que você selecioná-lo, ele estará disponível como `$r` no console do Google Chrome, permitindo que você inspecione suas propriedades, estado e instância.

![React DevTools Chrome Console Integration](img/ReactDevToolsDollarR.gif)

## Monitor de Desempenho

Você pode habilitar uma sobreposição de desempenho para ajudá-lo a depurar problemas de desempenho selecionando "Perf Monitor" no Menu do Desenvolvedor.

<hr style="margin-top:25px; margin-bottom:25px;"/>

#  Depuração em aplicativos ejetados

<div class="banner-crna-ejected" style="margin-top:25px">
  <h3>Projetos com somente código nativo</h3>
  <p>
   O restante deste guia aplica-se apenas a projetos feitos com <code>react-native init</code>
    ou para aqueles feitos com Create React Native App, que já foram ejetados. Para
    mais informações sobre ejetar, por favor, veja <a href="https://github.com/react-community/create-react-native-app/blob/master/EJECTING.md" target="_blank">guide</a> em
    o repositório React Native App.
  </p>
</div>

## Acessando logs do console

Você pode exibir os registros do console para um aplicativo iOS ou Android usando os seguintes comandos em um terminal enquanto o aplicativo está em execução:

```
$ react-native log-ios
$ react-native log-android
```

Você também pode acessá-los através de `Debug → Open System Log...` no simulador de iOS ou executando`adb logcat *:S ReactNative:V ReactNativeJS:V` em um terminal enquanto um aplicativo Android estiver sendo executado em um dispositivo ou emulador .

> Se você estiver usando o Create React Native App, os logs do console já aparecerão na mesma saída do terminal do empacotador.

## Depuração em um dispositivo com ferramentas do desenvolvedor Chrome

> Se você estiver usando o Create React Native App, isso já está configurado para você.

Em dispositivos iOS, abra o arquivo [`RCTWebSocketExecutor.m`](https://github.com/facebook/react-native/blob/master/Libraries/WebSocket/RCTWebSocketExecutor.m) e altere "localhost" para o endereço IP do seu computador, selecione "Debug JS Remotamente" no Menu do Desenvolvedor.

Nos dispositivos Android 5.0+ conectados via USB, você pode usar o[`adb` command line tool](http://developer.android.com/tools/help/adb.html) para configurar o encaminhamento de porta do dispositivo para o seu computador:

`adb reverse tcp:8081 tcp:8081`

Como alternativa, selecione "Dev Settings" no Menu de desenvolvimento e atualize a configuração "Debug server host for device" para corresponder ao endereço IP do seu computador.

> Se você encontrar algum problema, é possível que uma das suas extensões do Chrome interaja de formas inesperadas com o depurador. Tente desativar todas as suas extensões e reativá-las uma a uma até encontrar a extensão problemática.

### Debugando com [Stetho](http://facebook.github.io/stetho/) no Android

1. Em ```android/app/build.gradle```, adicione estas linhas no `dependencies` seção:

   ```gradle
   compile 'com.facebook.stetho:stetho:1.3.1'
   compile 'com.facebook.stetho:stetho-okhttp3:1.3.1'
   ```

2. Em ```android/app/src/main/java/com/{yourAppName}/MainApplication.java```, adicione as seguintes importações:

   ```java
   import com.facebook.react.modules.network.ReactCookieJarContainer;
   import com.facebook.stetho.Stetho;
   import okhttp3.OkHttpClient;
   import com.facebook.react.modules.network.OkHttpClientProvider;
   import com.facebook.stetho.okhttp3.StethoInterceptor;
   import java.util.concurrent.TimeUnit;
   ```

3. Em ```android/app/src/main/java/com/{yourAppName}/MainApplication.java``` adicione as funções:
   ```java
   public void onCreate() {
         super.onCreate();
         Stetho.initializeWithDefaults(this);
         OkHttpClient client = new OkHttpClient.Builder()
         .connectTimeout(0, TimeUnit.MILLISECONDS)
         .readTimeout(0, TimeUnit.MILLISECONDS)
         .writeTimeout(0, TimeUnit.MILLISECONDS)
         .cookieJar(new ReactCookieJarContainer())
         .addNetworkInterceptor(new StethoInterceptor())
         .build();
         OkHttpClientProvider.replaceOkHttpClient(client);
   }
   ```

4. Execute  ```react-native run-android ```

5. Em uma nova aba do Chrome, : ```chrome://inspect```, quando clicar em 'Inspecionar dispositivo' (aquele seguido por "Powered by Stetho").

## Debugando código nativo

Quando trabalhar com código nativo, como ao escrever módulos nativos, você pode iniciar o aplicativo no Android Studio ou no Xcode e aproveite os recursos de depuração nativos (configuração de pontos de interrupção etc.) como você faria no caso de criar um aplicativo nativo padrão.