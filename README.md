# INTRODUÇÃO - DESENVOLVENDO PARA A PLATAFORMA CONFLUENCE

Referência: [github.com/joseflexa/confluence-plugin-exercises](https://github.com/joseflexa/confluence-plugin-exercises/)

## PRÉ-REQUISITOS PARA O AMBIENTE DE DESENVOLVIMENTO

- [Oracle JDK 8](http://www.oracle.com/technetwork/pt/java/javase/downloads/jdk8-downloads-2133151.html) (variável JAVA_HOME devidamente configurada e disponível no path do SO).
- [Atlas JDK](https://developer.atlassian.com/server/framework/atlassian-sdk/)
- [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/)

### CRIANDO UM PLUGIN

Para criar um projeto de plugin para o Confluence deve-se utilizar o comando abaixo:

```bash
atlas-create-confluence-plugin
```

Ao final do download dos pacotes (mavem) será solicitado os seguintes dados:

- groupId (identificador do pacote)
- artifactId (identificador do projeto)
- versão
- package (sugere o nome do groupId automaticamente)

Após confirmar as informações, será criada uma estrutura de arquivos e diretórios conforme abaixo: (o nome do diretório raiz será o mesmo informado em *"package"*.

```bash

RAIZ_DO_PROJETO
├── LICENSE
├── pom.xml
├── README
└── src
    ├── main
    │   ├── java
    │   │   └── br
    │   │       └── rnp
    │   │           └── confluence
    │   │               └── plugin
    │   │                   ├── api
    │   │                   │   └── MyPluginComponent.java
    │   │                   └── impl
    │   │                       └── MyPluginComponentImpl.java
    │   └── resources
    │       ├── atlassian-plugin.xml
    │       ├── css
    │       │   └── teste.css
    │       ├── images
    │       │   ├── pluginIcon.png
    │       │   └── pluginLogo.png
    │       ├── js
    │       │   └── teste.js
    │       ├── META-INF
    │       │   └── spring
    │       │       └── plugin-context.xml
    │       └── teste.properties
    └── test ...
```

### CONFIGURAÇÃO INICIAL DO PLUGIN

Após criação do archetype via SDK, o fluxo básico de desenvolvimento é:

1. atualizar o arquivo *pom.xml*

    Este arquivo contém informações básicas sobre o plugin:

- nome e link da Organização.
- descrição do plugin.
- podem ser adicionados dependências externas (padrão mavem) e/ou configurações diversas, se necessário.

    path do arquivo:

    ```bash
    RAIZ_DO_PROJETO\pom.xml
    ```
2. atualizar o arquivo *atlassian-plugin.xml*

    Por padrão, este arquivo contém as seguintes informações/recursos:

- ícone e logo do plugin.
- referência para os properties de internacionalização (i18n).
- assets do plugin: (Web resources) css, img, js e etc..
- podem ser adicionados dependências do confluence (padrão mavem) e/ou configurações diversas, se necessário.

    path do arquivo:

    ```bash
    RAIZ_DO_PROJETO/src/main/resources/atlassian-plugin.xml
    ```
3. codificar!

    Pode ser utilizado a estrutura criada pelo SDK para iniciar o projeto.

    ```bash
    └── src
      ├── main
      │   ├── java ...
      │   └── resources ...
      └── test
          ├── java ...
          └── resources ...
    ```

### RODANDO O PROJETO

Acessar a raíz do projeto e rodar o seguinte comando:

```bash
atlas-run
```

Esse comando baixa todas as dependências (especificadas no *pom.xml* e no *atlassian-plugin.xml*), e sobe um servidor local do confluence em uma url com o seguinte formato:

[http://NOME_DO_HOST_LOCAL:1990/confluence](http://locahost:1990/confluence)

Durante o fluxo de desenvolvimento, para *"rebuildar"* o projeto e atualizar o plugin dentro do servidor web sem a necessidade de reiniciar o serviço, abra uma nova aba/janela do terminal e execute o comando abaixo:

```bash
atlas-package
```

Para rodar testes unitários:

```bash
atlas-unit-test
```

Para rodar testes de integração:

```bash
atlas-integration-test
```

> testes de integração praticamente rodam uma instância do servidor web, logo, antes de executá-lo, encerre qualquer outra instância que possa ter sido iniciada através do comando *"atlas-run*" para evitar conflitos.

### DICAS

Após executar o *atlas-run* será criado uma estrutura de diretórios e arquivos dentro do diretório *target*.

Para melhorar a performace do ambiente durante o processo de desenvolvimento, algumas configurações são sugeridas:

1. configuração do servidor de aplicação: *confluence.cfg.xml*

    valores recomendados para ambientes de desenvolvimento:

- **hibernate.c3p0.max_size** : de 60 para 300.
- **hibernate.c3p0.min_size** : de 20 para 300.
- **synchrony.proxy.enabled** : de true para false.

    path do arquivo:

  ```bash
  RAIZ_DO_PROJETO/target/confluence/home/confluence.cfg.xml
  ```

2. arquivo *pom.xml*:

    Adicione a linha abaixo para aumentar a memória utilizada no projeto.

    ```xml
    <jvmArgs>-Xms2000M -Xmx2000M</jvmArgs>
    ```

    > esses valores devem ser especificados com base no total de memória ram disponível na estação utilizada para o desenvolvimento.

3. executando o servidor do confluence em modo stand alone:

    A utilização do servidor em modo *"stand alone"* faz mais sentido para testar mais de um plugin/módulo simultaneamente, porém o deploy dos plugins devem ser feitos manualmente (verificar o uso do comando [atlas-install-plugin](https://developer.atlassian.com/server/framework/atlassian-sdk/atlas-install-plugin/)).

    ```bash
    atlas-run-standalone --product confluence
    ```
