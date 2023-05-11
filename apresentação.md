# Introdução

Sei que já devem saber, mas o Storybook serve para:

- Documentar;
- Modularizar;
- Padronizar;
- Testar.

Mas agora como assim ele ajuda a modularizar? Documentar faz sentido, ele é para isso mesmo, também faz sentido ser para padronizar e testar se você ta falando beleza, mas modularizar? 

Bem, o storybook ele foi pensado para documentação de projetos orientados a componentes, normalmente quando você vai construir componentes você os constroi visualizando direto onde você vai usá-los, como por exemplo:

'Vou criar um botão aqui no formulário'

O que isso implica? Implica que eu vou codar esse botão, mas enquanto to codando preciso ver como ele está ficando pra ter certeza que ta tudo indo certo? Onde eu vejo essa construção? No formulário que já ta lá na minha página, supondo que estou fazendo em react eu vou ter lá o meu componente button que vai ser chamado no meu formulário e esse formulário vai ser chamado na página... Ou seja, eu estou criando um componente já onde eu quero que ele esteja, o que isso implica? Implica numa dificuldade maior de abstrair para outros locais, como por exemplo, meu formulário vai ter esse botão aqui ocupando todo o espaço de onde ele está, mas em outro local da minha aplicação ele vai ocupar só uma parte, como você está criando o botão no próprio formulário pode ser que ocupar todo o espaço de onde ele esteja acabe virando o padrão desse botão e no outro local que você vai colocar que ele precise ocupar só uma parte você coloca só uma div lá e já está, foi resolvido.

Mas será que foi mesmo? Beleza que nesse caso é só uma div a mais ali e tals, mas poderia ter feito meu componente do botão com algo do tipo "fit" "full" para ocupar só o tamanho do seu conteúdo ou o tamanho de onde ele estiver, dai não vou precisar de mais uma div para isso.

Com o storybook você tem um lugar a parte para visualizar o que está acontecendo com o seu botão enquanto está construindo isso, você está fazendo ele de fora para colocar dentro e não de dentro pensando em como ele pode ficar fora, dá uma ajudada nessa abstração.

Dai pra quem quiser essa imagem foi print de uma página, dai é só escanear o qr que tu consegue acessar lá pra saber mais a fundo desses textos.

## Introduzindo no código
O storybook foi feito para ser introduzido em um projeto que já exista, na sua documentação tem uma parte que fala sobre isso, primeiro você precisa criar um projeto, seja ele em angular, react, vue... e dai depois adicionar o storybook no projeto.
Criando seu projeto, utilize o comando:
```bash
  npx storybook init
```
Que irá instalar todas as dependências necessárias no seu projeto, normalmente ele identifica qual é a tecnologia do projeto mas se não coseguir ele irá mostrar uma interface no bash ali pra ti para que escolha qual está sendo usada, no final ele vai perguntar se quer fazer uma migração para o npm7, não precisa aceitar, ele vai rodar de qualquer forma, a questão é só que o storybook foi criado a partir dessa versão do npm.

Esse comando também vai adicionar algumas histórias padronizadas para que você consiga começar a visualizar como se é feito, depois que ele rodar utilize:

```bash
  npm run storybook
```
E ele vai te mostrar tudo lindo maravilhoso lá, ou não.
Quando fui adicionar ele no projeto levei uma pequena surra porque começou a me apitar um erro que acabei esquecendo de pegar, mas era algo relacionado com o node, dai tive que adicionar o cross-env no projeto:
```bash
  npm install --save-dev cross-env
```
Ir até o package.json e mudar o script do storybook para:
```json
  "storybook": "cross-env NODE_OPTIONS=--openssl-legacy-provider start-storybook -p 6006",
```
E ai sim rodar o run storybook que ele abriu tudo certinho pra mim. Demorei um pouco pra conseguir resolver isso a primeira vez, fiquei meio triste me perguntei porque fui inventar de fazer isso e quando rodou fiquei feliz e falei ainda bem que deu isso comigo e eu resolvi, agora posso repassar.


### 1
Bom, beleza, instalei, rodou e agora??

Agora você começa o trabalho mesmo, se você estiver usando o Typescript vai te facilitar na parte da documentação do projeto, se não, sinto muito.

Mas a ideia é simples, você vai montar seus componentes normalmente, e adicionar um outro arquivo que você vai fazer a documentação, esse arquivo por padrão vai receber o NomeDoComponente.stories.tsx(ou jsx, js, ts....) vai depender de como está seu projeto, e nele você vai importar o componente (e a interface se tiver com o TS), e ainda se estiver com o TS vai importar o Meta e StoryObj da lib do storybook.

### 2
Depois disso você vai montar algo semelhante a essa estrutura aqui, porém, para quem tem o TS e usa as interfaces, a parte de 'options' é uma opção se colocar aqui, porque o próprio storybook vai atrás das props através da interface e identifica qual é o melhor modo de exibi-las na história(que é como o storybook chama cada componente documentado nele.) Então nesse caso eu poderia só colocar a parte dos args, que são os valores que serão colocados como default nas histórias e no argType eu poderia só colocar a description para colocar uma descrição naquele argumento, mais pra frente vou mostrar como fica. E parte do trabalho se vai por usar o TS.

Agora, para criar as variantes eu faço dessa forma e passo o arg que quero que aquela variante tenha diferente do default que passei anteriormente. E com isso eu já tenho esses componentes sendo mostrados.

Agora, além disso, posso trabalhar com markdown também nessa documentação, tem uma outra forma de se documentar com storybook que vi em um curso do alura, achei interessante mas já havia começado a fazer dessa forma. Mas a ideia é que tem como documentar tudo do storybook adicionando os componentes com MarkDown ao invés de criar um .stories e fazer tudo isso que estou mostrando aqui, ainda vai precisar fazer essas equivalências que mostrei nessa outra forma, mas eu vejo mais as pessoas utilizando a forma com js do que com o markdown, até os criadores de conteúdo do storybook usam a forma com js, não cheguei a ver um vídeo deles ainda desde que comecei a acompanhar usando a forma com markdown. MAAAS para adicionar o markdown aqui tem essa forma para ficar algo mais descritivo.

### 3
Agora um pouco mais dessa forma de adicionar o markdown porque tem uma peculiaridade também que é: para o componente principal, você coloca:
```js
export const Primary: StoryObj<ButtonProps> = {
  parameters: {
    docs: {
      description: {
        component: StorieMarkdown.Primary,
      },
    },
  },
}
```
E para os outros você põe:
```js
export const Secondary: StoryObj<ButtonProps> = {
  args: {
    styleType: 'secondary',
  },
  parameters: {
    docs: {
      description: {
        story: StorieMarkdown.Secondary,
      },
    },
  },
}
```
Ou seja, o componente principal a descrição vai ser adicionada no `component` e nas variações `story`

## Configurando o Canva
Agora seu componente já ta mostrando lá, com essas descrições e tals, mas agora nesses códigos que coloquei o que é o que realmente? Na parte de argTypes, o que estou dizendo ali é que a descrição do styleType é que é o estilo do botão, e o seu controlador é um select que tem aquelas opções de escolha, para os outros eu não coloquei que eram string ou algo do gênero por conta de dois fatores:
- O typescript, que já faz essa definição pra mim na parte do `Meta<ButtonProps>`;
- O storybook também interpreta de acordo com o que foi colocado nos args aqui em cima.

Dai também tem esses com radio-inline, boolean para true/false.

E dai começa a parte mais divertida, que são as ações no storybook, essa parte eu vou começar a falar mais de como elas podem ser porque estou aprendendo também e to levando um pouco de surra para implementar no projeto, mas o que eu sei sobre isso:
Tem como você criar interações que serão mostradas ao clicar/passar por cima, enviar algo e chamar da forma que você quiser e descrever o que ter clicado vai fazer com que aquele componente faça. 

Dai já podemos pular também para parte de testes que são as interactions, dentro daquele código do .stories você pode adicionar algumas interações entre um componente e outro e definir certos comportamentos que precisam ser seguidos e criar os testes, como isso? Preferi trazer esse vídeo aqui de 7 minutos, não vou mostrar por inteiro, vou pular aqui pra 1:04 e deixar ele falar um pouco para poderem ver que é algo que não trás muito trabalho para se fazer.

## Deploy
Para o deploy no github pages sei que precisa instalar esse carinha aqui:
```bash
  npm i @storybook/storybook-deployer --save-dev
```
Com ele podemos usar 2 scripts em nosso packagejson para o deploy:
```json
  {
    "scripts": {
      "deploy-storybook": "storybook-to-aws-s3"
    }
  }
  // or
  {
    "scripts": {
      "deploy-storybook": "storybook-to-ghpages"
    }
  }
```
O primeiro é para publicação na aws (não sei como funciona) e o segundo é para o ghpages, que foi o que fiz. 

Dai o passo a passo depois de instalar a lib de deploy:
- adicionar o script de deploy no packagejson;
- criar o deploy-docs.yml para rodar as coisas com o actions do github; 
```yml
name: Deploy Storybook

on: 
  push:
    branches: 
      - master
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Chekcout
        uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 16
      - name: Install dependencies
        run: npm ci

      - name: Build Storybook
        run: npm run build-storybook

      - name: Deploy Storybook
        run: npm run deploy-storybook -- --ci --existing-output-dir=storybook-static
        env:
          GH_TOKEN: ${{ github.actor }}:${{ secrets.GITHUB_TOKEN }}
```
- adicionar no final do arquivo main.cjs do storybook o seguinte código:
```cjs
  viteFinal: (config, {configType})=>{
    if (configType == 'PRODUCTION'){
      config.base = '/IgniteLabStorybook'
    }
    return config
  }
```
Depois envia pro github, espera as ações rodarem e publica ele no github pages e tcharam, sua doc ta online pelo github kk.

## Addons
Mais umas coisas do sotrybook são os plugins, como o plugin de acessibilidade, esse plugin retorna informações sobre acessibilidade de nossos componentes, instalando e adicionando ele ao arquivo main.cjs você já tem algumas informações sobre constraste, ID, interações, uma avaliação sobre essas coisas e informações se tem alguma violação ou algo incompleto, e isso só instalando algo e utilizando.
