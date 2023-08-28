---
title: Ativar o Forms adaptável headless no AEM Forms as a Cloud Service
seo-title: Step-by-Step Guide for enabling Headless Adaptive Forms on AEM Forms as a Cloud Service
description: Saiba como ativar formulários adaptáveis headless no AEM Forms as a Cloud Service com nosso guia passo a passo. Nosso tutorial o orienta pelo processo, facilitando a ativação desse recurso poderoso para seu ambiente AEM Forms.
seo-description: Learn how to enable headless adaptive forms on AEM Forms as a Cloud Service with our step-by-step guide. Our tutorial walks you through the process, making it easy to enable this powerful feature for your AEM Forms environment.
solution: Experience Manager Forms
feature: Adaptive Forms
topic: Headless
role: Admin
level: Beginner, Intermediate
contentOwner: Khushwant Singh
docset: CloudService
hide: true
hidefromtoc: true
exl-id: 7c545ca6-cb2d-4d28-b9e8-b6efe3faee00
source-git-commit: 47ac7d03c8c4fa18ac3bdcef04352fdd1cad1b16
workflow-type: tm+mt
source-wordcount: '923'
ht-degree: 2%

---

# Ativar o Forms adaptável headless no AEM Forms as a Cloud Service {#enable-headless-adaptive-forms-on-aem-forms-cloud-service}

A ativação do headless Adaptive Forms no AEM Forms as a Cloud Service permite começar a criar, publicar e fornecer Forms headless usando as instâncias do AEM Forms Cloud Service para vários canais. Você precisa do ambiente habilitado dos Componentes principais adaptáveis do Forms para usar o Forms adaptável headless.

## Considerações

* Ao criar um novo programa AEM Forms as a Cloud Service, [O headless Adaptive Forms já está ativado para seus ambientes](#are-adaptive-forms-core-components-enabled-for-my-environment).

* Se você tiver um programa as a Cloud Service mais antigo do Forms, em que os Componentes principais estão [não ativado](#enable-components), você pode [adicionar dependências dos Componentes principais do Forms adaptável](#enable-headless-adaptive-forms-for-an-aem-forms-as-a-cloud-service-environment) no repositório do AEM as a Cloud Service e implante o repositório nos ambientes Cloud Service para ativar o Forms adaptável headless.

* Se o ambiente de Cloud Service existente fornecer a opção de [criar Forms adaptável com base em Componentes principais](create-a-headless-adaptive-form.md), o Headless Adaptive Forms já estão habilitados para o seu ambiente e você pode fornecer formulários adaptáveis baseados no componente principal do Adaptive Forms como formulários headless para canais como dispositivos móveis, Web, aplicativos nativos e serviços que exigem uma representação headless do Adaptive Forms.


>[!NOTE]
>
>
> Adobe fornece Forms adaptável [kit inicial (React App)](create-and-publish-a-headless-form.md) para ajudar os desenvolvedores a começar rapidamente com o desenvolvimento headless adaptável do Forms, sem ativar o ambiente headless adaptável do Forms no AEM Forms as a Cloud Service. Você pode ativar o Forms adaptável headless em um ambiente as a Cloud Service do Forms posteriormente após um [práticas rápidas com o desenvolvimento de formulários headless](create-and-publish-a-headless-form.md).

## Ativar o Forms adaptável headless para um ambiente as a Cloud Service do AEM Forms

Execute as seguintes etapas, na ordem listada, para ativar o Headless Adaptive Forms para um ambiente as a Cloud Service do AEM Forms


![](/help/assets/enable-headless-adaptive-forms-on-aem-forms-cloud-service.png)


## 1. Clonar o repositório Git as a Cloud Service do AEM Forms {#clone-git-repository}

1. Efetue logon no [Cloud Manager](https://my.cloudmanager.adobe.com/) e selecione sua organização e programa.

1. Navegue até a **Pipelines** do seu **Visão geral do programa** clique no link **Acessar informações do repositório** botão para acessar e gerenciar o Repositório Git. A página inclui as seguintes informações:

   * URL do Repositório Git do Cloud Manager.
   * Credenciais do nome de usuário do Git do Repositório Git (nome de usuário e senha).

   Clique em **Gerar senha** para exibir ou gerar a senha.

1. Abra o terminal ou o prompt de comando no computador local e execute o seguinte comando:

   ```Shell
   git clone [Git Repository URL]
   ```

   Quando solicitado, forneça as credenciais. O repositório é clonado no computador local.


## 2. Adicione as dependências dos Componentes principais do Forms adaptável ao Repositório Git {#add-adaptive-forms-core-components-dependencies}

1. Abra a pasta Repositório Git em um editor de código de texto simples. Por exemplo, Código VS.
1. Abra o `[AEM Repository Folder]\pom.xml` arquivo para edição.
1. Substituir versões do `core.forms.components.version`, `core.forms.components.af.version` e `core.wcm.components.version` componentes com versões especificadas em [documentação dos Componentes principais](https://github.com/adobe/aem-core-forms-components). Se o componente não existir, adicione esses componentes.

   ```XML
   <!-- Replace the version with the latest released version at https://github.com/adobe/aem-core-forms-components/tags -->
   
   <properties>
       <core.forms.components.version>2.0.14</core.formscomponents.version>
       <core.forms.components.af.version>2.0.14</core.forms.components.af.version>  
       <core.wcm.components.version>2.21.2</core.wcmcomponents.version>
   </properties>
   ```

   ![Mencione a versão mais recente dos Componentes principais do Forms](/help/assets/latest-forms-component-version.png)

1. Na seção de dependências do `[AEM Repository Folder]\pom.xml` adicione as seguintes dependências e salve o arquivo.

   ```XML
       <!-- WCM Core Component Examples Dependencies -->
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.apps</artifactId>
               <type>zip</type>
               <version>${core.wcm.components.version}</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.content</artifactId>
               <type>zip</type>
               <version>${core.wcm.components.version}</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.config</artifactId>
               <version>${core.wcm.components.version}</version>
               <type>zip</type>
           </dependency>    
           <!-- End of WCM Core Component Examples Dependencies -->
           <!-- Forms Core Component Dependencies -->
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-core</artifactId>
               <version>${core.forms.components.version}</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-apps</artifactId>
               <version>${core.forms.components.version}</version>
               <type>zip</type>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-af-core</artifactId>
               <version>${core.forms.components.version}</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-af-apps</artifactId>
               <version>${core.forms.components.version}</version>
               <type>zip</type>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-examples-apps</artifactId>
               <type>zip</type>
               <version>${core.forms.components.version}</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-examples-content</artifactId>
               <type>zip</type>
               <version>${core.forms.components.version}</version>
           </dependency>
   <!-- End of AEM Forms Core Component Dependencies -->
   ```

1. Abra o `[AEM Repository Folder]/all/pom.xml` arquivo para edição. Adicione as seguintes dependências no `<embeddeds>` e salve o arquivo.

   ```XML
   <!-- WCM Core Component Examples Dependencies -->
   
   <!-- inside plugin config of filevault-package-maven-plugin -->  
   <!-- embed wcm core components examples artifacts -->
   
   <embedded>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples.ui.apps</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/content/install</target>
   </embedded>
   <embedded>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples.ui.content</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/content/install</target>
   </embedded>
   <embedded>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples.ui.config</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/content/install</target>
   </embedded>
   <!-- embed forms core components artifacts -->
   <embedded>
       <groupId>com.adobe.aem</groupId>
       <artifactId>core-forms-components-af-apps</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/application/install</target>
   </embedded>
   <embedded>
       <groupId>com.adobe.aem</groupId>
       <artifactId>core-forms-components-af-core</artifactId>
       <target>/apps/${appId}-vendor-packages/application/install</target>
   </embedded>
   <embedded>
       <groupId>com.adobe.aem</groupId>
       <artifactId>core-forms-components-examples-apps</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/content/install</target>
   </embedded>
   <embedded>
       <groupId>com.adobe.aem</groupId>
       <artifactId>core-forms-components-examples-content</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/content/install</target>
   </embedded>
   ```

   >[!NOTE]
   >
   >
   >  Substituir `${appId}` com sua appId.
   >
   >  Para encontrar o `${appId}`, no `[AEM Repository Folder]/all/pom.xml` arquivo, pesquise o `-packages/application/install` termo. O texto antes da `-packages/application/install` o termo é seu `${appId}`. Por exemplo, o código a seguir, `myheadlessform` é `${appId}`.
   >
   >   ```
   >             <embedded>
   >                     <groupId>com.myheadlessform</groupId>
   >                     <artifactId>myheadlessform.ui.apps<artifactId>
   >                     <type>zip</type>
   >                   <target>/apps/myheadlessform-packages/application install</target>
   >             </embedded>
   >   ```

1. No `<dependencies>` seção do `[AEM Repository Folder]/all/pom.xml` adicione as seguintes dependências e salve o arquivo:

   ```XML
           <!-- Other existing dependencies -->
           <!-- wcm core components examples dependencies -->
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.apps</artifactId>
               <type>zip</type>
           </dependency>
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.config</artifactId>
               <type>zip</type>
               </dependency>
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.content</artifactId>
               <type>zip</type>
           </dependency>
               <!-- forms core components dependencies -->
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-af-apps</artifactId>
               <type>zip</type>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-examples-apps</artifactId>
               <type>zip</type>
           </dependency>
               <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-examples-content</artifactId>
               <type>zip</type>
           </dependency>
   ```

1. Abra o `[AEM Repository Folder]/ui.apps/pom.xml` para edição. Adicione o `af-core bundle` e salve o arquivo.

   ```XML
       <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>core-forms-components-af-core</artifactId>
       </dependency>
   ```

   >[!NOTE]
   >
   >Verifique se os seguintes artefatos dos Componentes principais adaptáveis do Forms não estão incluídos em seu projeto.
   >
   > `<dependency>`
   >
   >   `<groupId>com.adobe.aem</groupId>`
   >   `<artifactId>core-forms-components-apps</artifactId>`
   >
   > `</dependency>`
   >
   > e
   >
   > `<dependency>`
   >
   >   `<groupId>com.adobe.aem</groupId>`
   >   `<artifactId>core-forms-components-core</artifactId>`
   >
   > `</dependency>`


1. Salvar e fechar o arquivo.

## 3. Atualize o projeto para incluir a versão mais recente dos Componentes principais do Forms:

1. Abra o [Pasta de projeto do arquétipo AEM]/pom.xml para edição.


1. Salvar e fechar o arquivo.

## 4. Confirme as atualizações no Repositório Git e execute o pipeline para implantar o repositório {#Commit-the-updates-to-your-git-repository}

1. Para confirmar o código no Repositório Git:
   1. Abra o terminal ou o prompt de comando.
   1. Navegue até o `[AEM Repository Folder]` e execute os seguintes comandos na ordem listada

      ```Shell
      git add pom.xml
      git add all/pom.xml
      git add ui.apps/pom.xml
      git commit -m "Added dependencies for Adaptive Forms Core Components"
      git push origin
      ```

1. Depois que os arquivos forem confirmados no Repositório Git, [Executar o pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/deploying-code.html?lang=pt-BR).

   Depois que a execução do pipeline for bem-sucedida, os Componentes principais adaptáveis do Forms serão ativados para o ambiente correspondente. Além disso, um modelo de Forms adaptável (Componentes principais) e o tema do Canvas 3.0 são adicionados ao ambiente as a Cloud Service do Forms, fornecendo opções para personalizar e criar Componentes principais com base no Forms adaptável.


## Perguntas frequentes {#faq}

### Quais são os componentes principais? {#core-components}

A variável [Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR) são um conjunto de componentes padronizados de Gerenciamento de Conteúdo na Web (WCM, na sigla em inglês) para o AEM a fim de acelerar o tempo de desenvolvimento e reduzir o custo de manutenção de seus sites.

### Quais são todos os recursos adicionados na ativação dos componentes principais? {#core-components-capabilities}

Quando os Componentes principais do Forms adaptável estiverem ativados para seu ambiente, um modelo de Formulário adaptável baseado em Componentes principais em branco e o tema do Canvas 3.0 serão adicionados ao seu ambiente. Depois de ativar os Componentes principais adaptáveis do Forms no seu ambiente, você pode:

* Crie Componentes principais com base no Forms adaptável.
* Crie Componentes principais com base em modelos de formulário adaptável.
* Crie temas personalizados para os Componentes principais com base em modelos de Formulário adaptável.
* Servir representações JSON do Formulário adaptável com base no Componente principal para canais como dispositivos móveis, Web, aplicativos nativos e serviços que exigem a representação headless de um formulário.

### Os Componentes principais adaptáveis do Forms estão habilitados para meu ambiente? {#enable-components}

Para verificar se os Componentes principais do Adaptive Forms estão ativados para o seu ambiente:

1. [Clonar o repositório as a Cloud Service do AEM Forms](#1-clone-your-aem-forms-as-a-cloud-service-git-repository).

1. Abra o `[AEM Repository Folder]/all/pom.xml` arquivo do Repositório Git do AEM Forms Cloud Service.

1. Procure as seguintes dependências:

   * core-forms-components-af-core
   * core-forms-components-core
   * core-forms-components-apps
   * core-forms-components-af-apps
   * core-forms-components-examples-apps
   * core-forms-components-examples-content


   ![localize o artefato core-forms-components-af-core em all/pom.xml](/help/assets/enable-headless-adaptive-forms-on-aem-forms-cloud-service-locate-core-af-artifact.png)

   Se as dependências existirem, os Componentes principais adaptáveis do Forms serão ativados para o seu ambiente.
