# QA Automation Challenge

## Descrição:

Automação (frontend e backend) dos serviços de Busca CEP.

*Frontend:* https://www.correios.com.br/

*Backend:* https://viacep.com.br/

## Cenários de testes (Api/Web):
- CEP válido com 8 dígitos;
- CEP válido menor que 8 digitos e finalizado com 0;
- CEP válido/inexistente com 8 dígitos;
- CEP válido/inexistente menor que 8 digitos;
- CEP inválido maior que 8 dígitos;
- CEP inválido com caracter alfanumério;
- CEP inválido com espaço em branco entre caracteres;

## Ferramentas 
Antes de começar, garanta que os seguintes sistemas estejam instalados em seu computador. Abaixo seguem as versões utilizadas enquanto implemento os testes:

- [git](https://git-scm.com/) (utilizada versão `2.35.2`)
- [Node.js](https://nodejs.org/en/) (utilizada a versão `v16.15.0`)
- npm (utilizada a versão `8.5.5` instalada junto com o Node.js)
- [Google Chrome](https://www.google.com/intl/pt_br/chrome/) (utilizada a versão `101.0.4951.67 (Versão oficial) 64 bits`)
- [Visual Studio Code](https://code.visualstudio.com/) (utilizada a versão `1.67.2`)
- [Allure Report](https://docs.qameta.io/allure-report/#_installing_a_commandline) (utilizada a versão `2.10.0`)


## Estratégia de testes
Os mesmos testes foram realizados utilizando algumas abordagens diferentes para gestão de massas de testes e também para implementação dos testes. Apenas para exemplificar algumas das 'n' maneiras que podemos utilizar as ferramentas a nosso favor:

**Abordagem de massas de testes:**
1) Utilizando massas de testes fixas configuradas em arquivos "json" (fixtures/cep.json)
- Uma forma mais simples de isolar a massa do teste;

2) Utilizando massas de testes fixas e "dinâmicas" em arquivos "js" (factories/CepFactory.js)
- Uma forma também de isolar a masa do teste, porém, neste caso tem uma liberdade maior para "brincar/gerar" massas de testes conforme a demanda.

*** Outra abordagem que também pode ser adotada é a utilização da biblioteca "Faker", mas, julguei que não seria tão útil neste caso onde com poucas variações é possível cobrir todos os cenários. 

**Abordagem de testes:

1) Testes utilizando massas de testes "by Fixtures" (integration/web/home.spec.js)


    

    describe('Home page correios, consulta de CEP (massas by "Fixtures")', function () {
    beforeEach(function () {
        cy.fixture('cep').then(function (c) {
            this.cep = c
        })
    })

    it('Consulta de CEP válido de 8 dígitos', function () {
        home.go()
        home.inputCep(this.cep.cepValidoCom8Digitos)
        home.submit()
        busca.messageContentShouldBe(this.cep.mensagemEsperadaParaOk)
        busca.validaCepEncontrado(this.cep.cepValidoCom8Digitos)
    })
        
    


2) Testes utilizando massas de testes "by Factories" (integration/web/homeFactory.spec.js)

`import cepFactory from '../../factories/CepFactory'



    describe('Home page correios, consulta de CEP (massas by "Factories")', ()=> {
    var mensagem = cepFactory.mensagemEsperadaParaOk()
    it('Consulta de CEP fixo e validação completa de todos os dados retornados', function ()=> {

        var cep = cepFactory.cep()
        
        home.go()
        home.inputCep(cep)
        home.submit()
        busca.messageContentShouldBe(mensagem)
        busca.validaEnderecoCompleto(cep)
    })

3) Testes utilizan comandos personalizados (integration/web/homeCommand.spec.js)

4) Testes reescritos utilizando técnica de BDD apenas com os próprios comandos do Cypress (integration/web/buscaCepBdd.spec.js)


*** Utilização do padrão "PageObject" para isolar as camadas de identificação de elemntos e ações da camada de testes e acerções.



## Como exeucar
- Para executar os testes implementados siga os passos abaixo:

1) Crie uma pasta para o projeto e inicie um repositório git `git init` ou clone o repositório em branco;
2) Faça o download do arquivo "cassio-damacena.patch" e cole nesta pasta;
3) Desempacote o patch com o comando: `git am --signoff cassio-damacena.patch`
4) Instale as dependências: `npm install`
5) Rode o projeto de testes:
`npm run cy:test` para rodar no modo interativo do Cypress
`npm run cy:run` para rodar no modo headless do Cypress
`npm run allure:generate` para gerar o relatório Allure Report

*** Obs:
- O comando `cy:run` já está configurado para sempre limpar a pasta de resultados e gerar os novos
- O comando `allure:generate` já está configura para gerar o relatório e apresentar no navegador


### Qualquer dúvida entre em contato:
cassiodamacena@hotmail.com


OBSERVAÇÃO:
- No repositório https://github.com/cassiodamacena/qa-automation-challenge-cucumber estão implementados os testes utilizando BDD com o framework Cucumber. Também estão configurado um workflows com GitHubActions para executar os testes automaticamente e gerar um relatório diretamente no [Dashboard do Cypress](https://dashboard.cypress.io/invitation/a0b0703e-ea3f-4257-bea5-846804674687).
  
