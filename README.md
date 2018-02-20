# TREINAMENTO CONFLUENCE DEVELOPER

Referência: [github.com/joseflexa/confluence-plugin-exercises](https://github.com/joseflexa/confluence-plugin-exercises/)

### AMBIENTE DE DESENVOLVIMENTO:
  - [Oracle JDK 8](http://www.oracle.com/technetwork/pt/java/javase/downloads/jdk8-downloads-2133151.html)
  - [Atlas JDK](https://developer.atlassian.com/server/framework/atlassian-sdk/)
  - [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/)

### CRIANDO UM PROJETO

  ```bash
  atlas-create-confluence-plugin
  ```

  Ao final do download dos pacotes (mavem) será solicitado os seguintes dados:

- groupId (identificador do pacote)
- artifactId (identificador do projeto)
- versão
- package (sugere o nome do groupId automaticamente)

Após isso será criado uma estrutura conforme abaixo:

```bash
RAIZ_DO_PROJETO
├── LICENSE
├── pom.xml
├── README
└── src
    └── main
        ├── java
        │   └── br
        │       └── rnp
        │           └── br
        │               └── confluence
        │                   └── plugin
        │                       ├── api
        │                       │   └── MyPluginComponent.java
        │                       └── impl
        │                           └── MyPluginComponentImpl.java
        └── resources
            ├── atlassian-plugin.xml
            ├── bold.properties
            ├── css
            │   └── bold.css
            ├── images
            │   ├── pluginIcon.png
            │   └── pluginLogo.png
            ├── js
            │   └── bold.js
            └── META-INF
                └── spring
                    └── plugin-context.xml
```

### CONFIGURAÇÃO INICIAL DO PLUGIN

Após criação do archetype via SDK o fluxo básico de desenvolvimento é:

1. atualizar as dependências do projeto:
  ```bash
  RAIZ_DO_PROJETO\pom.xml
  ```
2. atualizar a configuração do projeto:
  ```bash
  RAIZ_DO_PROJETO/src/NOME_DO_PROJETO/resources/atlassian-plugin.xml**
  ```
3. codificar.

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
  Esse comando baixa todas as dependências (especificadas no *pom.xml* e no *atlassian-plugin.xml*), e sobe um servidor web do confluence na url abaixo:

[http://NOME_DO_HOST_LOCAL:1990/confluence](http://locahost:1990/confluence)

Durante o fluxo de desenvolvimento, para *"rebuildar"* o projeto e atualizar o artefato dentro do servidor web sem a necessidade reiniciar o servidor (*atlas-run*), deve ser utilizado o comando:

```bash
atlas-package
```

Para executar testes unitários:

```bash
atlas-unit-test
```

Para executar testes de integração:

```bash
atlas-integration-test
```

### CONFIGURANDO AMBIENTE DE APÓS SUBIR O SERVIDOR DO CONFLUENCE

Após executar o *atlas-run* será criado uma estrutura de diretórios e arquivos dentro do diretório *target*.

Para "agilizar" o processo de desenvolvimento, as configurações do servidor podem ser alteradas conforme abaixo:

Arquivo de cofiguração:

  ```bash
  RAIZ_DO_PROJETO/target/confluence/home/confluence.cfg.xml
  ```

Chaves que podem ser atualizadas:

 1. **hibernate.c3p0.max_size** : de 20 para 300.
 2. **hibernate.c3p0.min_size** : de 60 para 300.
 3. **synchrony.proxy.enabled** : de true para false.

DICAS:

O servidor do confluence pode ser executado no modo standalone (faz mais sentido para testar mais de um plugin/módulo simultaneamente):

```bash
atlas-run-standalone --product confluence
```
