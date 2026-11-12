# BringTech Android — versão inicial

Aplicativo Android nativo em Kotlin + Jetpack Compose para conectar diretamente a um scanner OBD-II Bluetooth pareado com o celular.

## O que esta versão faz
- Lista os dispositivos Bluetooth já pareados no Android.
- Dá preferência a dispositivos cujo nome contém `OBD`.
- Tenta conectar usando Bluetooth clássico SPP/RFCOMM (UUID padrão 00001101-0000-1000-8000-00805F9B34FB).
- Envia comandos AT de inicialização para adaptadores compatíveis com ELM327.
- Consulta PIDs OBD-II padrão: RPM, velocidade, temperatura do líquido de arrefecimento, MAP, TPS e tensão do módulo.
- Exibe os valores em um painel escuro com conta-giros.

## Requisitos / compatibilidade
- Android 6.0 ou superior (a compilação está configurada com minSdk 23).
- Scanner OBD-II que ofereça Bluetooth clássico SPP e seja compatível com comandos ELM327.
- O scanner precisa estar pareado nas Configurações Bluetooth do Android antes de conectar no BringTech.
- Alguns scanners anunciados como OBD2 usam BLE, Wi-Fi, protocolos proprietários ou clones ELM327 com comportamento diferente. Esta versão ainda não implementa BLE; nesses casos será necessário adaptar a camada de comunicação.
- Os PIDs disponíveis dependem da ECU do veículo. `--` significa que o parâmetro não foi recebido/suportado.

## Como gerar o APK
1. Instale o Android Studio em um computador.
2. Extraia o ZIP.
3. No Android Studio, escolha **Open** e selecione a pasta `BringTech_Android` extraída.
4. Aguarde o Gradle sincronizar. Se o Android Studio solicitar, instale o Android SDK 35 e aceite as licenças.
5. Conecte seu Android ao computador com depuração USB habilitada.
6. Use **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
7. O APK de depuração será gerado em `app/build/outputs/apk/debug/app-debug.apk`.

## Primeiro teste
1. Com o carro desligado, conecte o scanner à porta OBD-II.
2. Ligue a ignição (motor pode permanecer desligado).
3. No Android, abra Configurações > Bluetooth e pareie com `OBD2`.
4. Feche outros aplicativos que estejam conectados ao scanner.
5. Abra BringTech, conceda as permissões Bluetooth, toque em **Atualizar** e selecione `OBD2`.
6. Toque em **Conectar**.

## Limitações importantes desta versão
- Este ZIP contém o código-fonte; não contém um APK já compilado.
- A conexão SPP ainda precisa ser testada no scanner físico informado pelo usuário.
- O polling inicial é sequencial e experimental; adaptadores lentos podem exigir ajuste de intervalos.
- Não controla mapas, ignição ou injeção. É um painel de leitura OBD-II, não uma ECU programável.
- Não use os dados deste aplicativo como único instrumento para condução ou decisões de segurança.

## Estrutura
- `app/src/main/java/com/bringtech/app/MainActivity.kt`: interface, conexão Bluetooth e parser ELM327.
- `app/src/main/AndroidManifest.xml`: permissões e atividade principal.
- `app/build.gradle.kts`: dependências e configuração Android.

## Compilar sem computador (GitHub Actions)
O projeto inclui `.github/workflows/build-apk.yml`. Para usar pelo celular:
1. Crie um repositório no GitHub pelo navegador.
2. Envie os arquivos extraídos do projeto para a raiz do repositório, preservando a pasta `.github/workflows`.
3. Abra a aba **Actions**, selecione **Build BringTech APK** e toque em **Run workflow**.
4. Quando terminar, abra a execução e baixe o artefato **BringTech-debug-APK**.
