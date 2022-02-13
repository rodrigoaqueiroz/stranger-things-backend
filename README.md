# Stranger Things Backend - Projeto de Deploy

#### Esse é o repositório do backend do projeto, o front está [neste](https://github.com/rodrigoaqueiroz/stranger-things-frontend) repositório.

--- 

# Habilidades

Nesse projeto, você será capaz de:
  - Adaptar e configurar o `Heroku`;
  - Publicar aplicações com `Docker` através do `Heroku`;
  - Utilizar variáveis de ambiente dentro do `Heroku`;

--- 

## O que deverá ser desenvolvido

Você vai adaptar e configurar os projetos descritos nesse README para que seja feito o deploy deles. Você vai colocar os projetos frontend e backend no ar com o `Heroku`, utilizando o `Docker` em ambiente de produção. Além disso, você vai alterar alguns comportamentos aplicando os conceitos vistos no conteúdo. 

--- 

## Testes

### Backend

Todos os requisitos do projeto serão testados **automaticamente** por meio do `Jest`. Basta executar o comando abaixo:

```bash
npm test
```

---

### Backend

O Backend possui a seguinte estrutura:

```
├── README.md
├── index.js
├── data
│  ├── dataset
│  │  └── stranger-things-characters.json
│  └── repository
│     └── StrangerThings.js
├── services
│  └── StrangerThings.js
├── package-lock.json
└── package.json
```

---

#### API

A API consiste em um serviço simples com um endpoint `/` que retorna uma listagem com os personagens da série **Stranger Things**.

---

#### Resposta

A lista de personagem possui os seguintes campos:

- **name**: Nome do personagem;

- **status**: se o personagem está vivo ou não na série. Os valores possíveis são `alive`, `deceased` ou `unknown`;

- **origin**: o local de origem do personagem.

A resposta é em formato `JSON`, e o retorno é sempre um array de objetos. Abaixo, um exemplo:

```JSON
[
  {
    "name": "Will",
    "status": "Alive",
    "origin": "Byers Family"
  }
]
```

---

#### Filtros

Todos os campos da estrutura de retorno da resposta podem ser utilizados como filtros. Para isso, basta passar na `query string` o filtro desejado. No exemplo abaixo, estamos filtrando pelo _nome_ do personagem:

> localhost:3000?name=el

Os filtros são feitos utilizando uma `regex`, buscando pelo texto passado na `query string` em qualquer parte do campo, sem diferenciar maiúsculas de minúsculas.

Nesse caso o retorno seria:

```JSON
[
  {
    "name":"Russell",
    "status":"Alive",
    "origin":"Hawkings Middle School"
  },
  {
    "name":"Eleven",
    "status":"Alive",
    "origin":"Hopper Family"
  }
]
```

---

#### Modo `upside down` (dw) - Backend

Na API, no arquivo `index.js`, há a seguinte variável de controle:

```javascript
const hereIsTheUpsideDown = true;
```

Caso ela seja `true`, a API ativará o modo "Mundo Invertido", conforme a série, e começará a responder desta forma:

```JSON
[
  {
    "name":"llǝssnᴚ",
    "origin":"looɥɔS ǝlppᴉW sƃuᴉʞʍɐH",
    "status":"ǝʌᴉl∀"
  },
  {
    "name":"uǝʌǝlƎ",
    "origin":"ʎlᴉɯɐℲ ɹǝddoH",
    "status":"ǝʌᴉl∀"
  }
]
```

---

## Desenvolvimento

O código não está utilizando variáveis de ambiente. Você vai configurá-lo para utilizá-las, tornando parametrizáveis a porta e o _modo upside down_.

Em seguida, você deverá adicionar todas as configurações necessárias para executar o `Docker` juntamente do `Heroku`.

Você vai realizar o deploy do projeto (frontend e backend) no _Heroku_. Você deverá realizar o deploy do projeto, conforme requisitos, no _Heroku_. Os comandos utilizados deverão ser adicionados no README.md de cada projeto. Para mais informações revisite o Course.

Todos esses passos estão detalhados nos requisitos abaixos.


---

# Requisitos do projeto

### Backend

#### 1 - Verifica as variáveis de ambiente

Altere o backend para utilizar variáveis de ambiente para controlar os seguintes comportamentos:

   1. A porta que a API escutará, essa variável deve ter, nescessariamente, o nome PORT e o seu valor deve ser 3000.

   2. O modo "upsideDown". Essa variável espera um valor boleano e deverá se chamar UPSIDEDOWN_MODE. Lembre-se que as variáveis de ambiente são `strings`.

   O que será testado:
   - Se existe a variável de ambiente PORT.
   - Se a variável de ambiente UPSIDEDOWN_MODE existe e se ela é um boleano.

**Importante**: Para esse projeto, as variáveis de ambiente devem ser definidas em um arquivo .env e o arquivo deve ser enviando no seu PR(Pull Request). ISSO NÃO É UMA PRÁTICA DE MERCADO, o arquivo .env deve ser sempre incluido no .gitignore pois contém informações sensíveis, aqui será enviado apenas por motivo de avaliação.

#### 2 - Verifica se o arquivo Dockerfile está configurado da maneira correta

O teste irá verificar se o arquivo `Dockerfile` existe e está configurado da maneira correta. Ele deve construir a imagem a partir da `node:14-alpine` e usar o comando `npm start` para iniciar a aplicação via `CMD`.

  O que será testado:
  - Se a Dockerfile está utilizando a imagem `node:14-alpine`, verificando se as camadas desta imagem estão incluídas no build que essa Dockerfile faz.
  - Se a Dockerfile usa `npm start` como `CMD`.

#### 3 - Verifica se o arquivo heroku.yml está configurado na maneira correta

Adicione e configure o arquivo `heroku.yml`

  1. O arquivo deve iniciar um servidor do tipo `web`
  2. O `run` deve executar o servidor utilizando o `node`.

Execute ambos em sua máquina para validar se o comportamento é o esperado.

O que será testado:
  - Se o arquivo `heroku.yml` existe.
  - Se o serviço a ser executado é um serviço do tipo `web`.
  - Se o `node` é responsável por executar o servidor.

#### 4 - Verifica action de Linter para ser executada no GitHub

Você deverá criar uma `Action` para ser executada somente em **pull_requests** solicitados em todas as branchs de seu repositório.

**Atenção**: O arquivo de configuração da action deverá ser criado em: `./actions/` com o nome `project.yml`.

O que será testado:
- Se o arquivo `./actions/project.yml` existe.
- Se a `Action` é acionada somente em `pull_request`.
- Se o job foi nomeado como: `eslint`.
- Se o linter está sendo executado com a versão **20.04** do ubuntu.
- Se o linter está sendo executado com a versão 12 do node.

#### 5 - Verifica o Deploy no Heroku

**IMPORTANTE**: Uma variável de ambiente com o nome `GITHUB_USER` deverá ser criada em seu `.env` com o **seu nome de usuário** do GitHub.

**IMPORTANTE :warning:**: O Heroku limita o tamanho do nome de uma aplicação para ter no máximo **30 caracteres**, se o seu nome de usuário do GitHub possuir mais que 27 caracteres você não conseguirá criar uma aplicação com o nome no padrão solicitado pelo requisito. **Caso esteja nessa condição, avise nosso time de instrunção que iremos ajudar**.

1. Crie dois `apps` do Heroku a partir do mesmo código fonte (código da API). O nome do seu app no Heroku deverá conter seu nome de usuário no GitHub seguido de "-up" e "-dw". Por exemplo, se seu nome de usuário no GitHub for "student" seus apps deverão ter os nomes "student-up" e "student-dw" e as URLs abaixo:

   - https://student-up.Herokuapp.com/ - para o app hawkins

   - https://student-dw.Herokuapp.com/ - para o app upside-down

2. Configure a variável de ambiente criada para controlar o modo `UPSIDEDOWN`. Cada um dos `apps` deverá ter valores distintos, da seguinte maneira:

   - O app `hawkins` deverá ter o valor `false`;

   - O app `upside-down` deverá ter o valor `true`.

3. Utilizando o `git`, faça o deploy de ambos os `apps` no Heroku.

Acesse os URLs geradas e valide se ambas as API's estão no ar e funcionando como esperado.

**Importante**: As URLs deverão ser geradas pelo Heroku e não devem ser modificadas.

O que será testado:
  - Se ao fazer uma requisição do tipo GET para o endpoint da API hawkins serão retornadas as informações corretas.
  - Se ao fazer uma requisição do tipo GET para o endpoint da API upside-down serão retornadas as informações corretas.


