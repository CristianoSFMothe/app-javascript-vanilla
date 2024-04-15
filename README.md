# App Javascript Vanilla

Esse projeto tem com objetivo demonstra uma forma de a escrita **Gherkin** para os testes utilizando o Cypress

```gherkin
Feature: Image Registration

  Scenario: Submitting an image with invalid inputs
    Given I am on the image registration page
    When I enter "" in the title field
    Then I enter "" in the URL field
    Then I click the submit button
    Then I should see "Please type a title for the image" message above the title field
    And I should see "Please type a valid URL" message above the imageUrl field
    And I should see an exclamation icon in the title and URL fields

  Scenario: Submitting an image with valid inputs using enter key
    Given I am on the image registration page
    When I enter "Alien BR" in the title field
    Then I should see a check icon in the title field
    When I enter "https://cdn.mos.cms.futurecdn.net/eM9EvWyDxXcnQTTyH8c8p5-1200-80.jpg" in the URL field
    Then I should see a check icon in the imageUrl field
    Then I can hit enter to submit the form
    And the list of registered images should be updated with the new item
    And the new item should be stored in the localStorage
    Then The inputs should be cleared

  Scenario: Submitting an image and updating the list
    Given I am on the image registration page
    Then I have entered "BR Alien" in the title field
    Then I have entered "https://cdn.mos.cms.futurecdn.net/eM9EvWyDxXcnQTTyH8c8p5-1200-80.jpg" in the URL field
    When I click the submit button
    And the list of registered images should be updated with the new item
    And the new item should be stored in the localStorage
    Then The inputs should be cleared

  Scenario: Refreshing the page after submitting an image clicking in the submit button
    Given I am on the image registration page
    Then I have submitted an image by clicking the submit button
    When I refresh the page
    Then I should still see the submitted image in the list of registered images
```

## Estrutura do Projeto:

```

├── .github
│   ├── workflows
│   │   ├── cypress-project.yml
├── cypress-project
│   ├── cypress
│   │   ├── e2e
│   │   │    ├── app.cy.js
│   │   ├── fixtures
│   │   │    ├── example.json
│   │   ├── support
│   │   │    ├── commands.js
│   │   │    ├── e2e.js
│   ├── node_modules
│   ├── app.feature
│   ├── cypress.config.js
│   ├── cypress.env.json
│   ├── package-lock.json
│   ├── packege.json
│   ├── .gitignore
│   ├── REAME.md

```

## Integração com o GitHub Actions

Para realizar a integração com o GitHub Action e necessário primeiramente criar uma arquivo, `.yml`, dentro da seguinte estrutura:

```
├── .github
│   ├── workflows
│   │   ├── cypress-project.yml
```

Com o seguinte código:

```blash
name: Cypress Project

on:
  push:
    branches:
      - master
    # paths:
    #   - 'cypress-project/**.js'
    #   - 'cypress-project/**.json'
    #   - 'cypress-project/**.yml'

jobs:
  cypress-tests:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20.10.0

      - name: Install dependencies
        run: npm install
        working-directory: ./cypress-project

      - name: Run Cypress tests
        run: npm run cypress:headless
        working-directory: ./cypress-project
        env:
          CYPRESS_RECORD_KEY: ${{ secrets.CYPRESS_RECORD_KEY }}
```
> A linha `CYPRESS_RECORD_KEY: ${{ secrets.CYPRESS_RECORD_KEY }}` configura a key do Cypress Cloud

### Configura o Cypress Clound

1. Com o projeto em execução em modo interativo:

![2024-04-15_10h32_00](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/2d78db4c-cbeb-4cfc-856c-1a8802e7b82d)

2. Clicar no botão `Runs` e em seguinte no botão `Connect to Cypress Cloud` para realiazar login na conta do Cypress Cloud

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/59340c03-6e4b-4720-9374-53dee671f36a)

3. Na tela que abrirá clicar em `Login in` 

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/8043f18f-fb5b-4d28-bbc4-8a7efeaa1f63)

4. Abrirá uma nova página no browser para realizar login com o Cypress Cloud 

> Caso já tenha uma conta

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/ad66abc8-5506-4191-8942-505af1f963e3)


> Caso não tenha uma conta poderá realizar login com uma das opções

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/bc42d2ed-5ce0-4876-ac3a-267cc642612f)

> OBS.: Para facilitar realizar o login com a conta do GitHub

5. Selecionar uma organização, caso tenha ou criar uma nova

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/61816df2-e669-474e-beca-f71994fb909b)

6. Caso já tenha projetos criados, serão listado

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/40d43601-88ea-4a6b-8f91-086e2c08f7ee)

7. Se não poderá criar um novo, clicando no botão `New project`

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/24e4b79f-1f02-4560-a9ed-a4aa780e4f9d)

8. Nesse ponto e importante, para dizer ser o projeto será `público` ou `privado`, nesse caso será `público`

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/1268157d-9703-4412-b761-1676c98d45b5)

### Configuração no Cypress

1. Após realizar os procedimentos acima, irá apresentar a seguinte tela:

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/81a0ff05-d488-4945-9b79-e073b8911a34)

No qual contém o `projectId` que deverá ser configurado no arquivo do `cypress.config.json`

```bash
# cypress.config.json

module.exports = {
  projectId: "i5ov91",
  // ...rest of the Cypress project config
}
```

* Mrcar a opção

- [x] Ok, I added my project ID

* Clicar no botão `Next`

2. Nesse caso como está sendo realizado a integração com `GitHub Actions`, selecionar a o `GitHub Actions` e clicar no botão `Next`

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/c501e182-0b9f-4a61-beb8-a73e73ba607f)

3. Executar o comando 

```bash
# Com a key que irá ser exibida na tela

npx cypress run --record --key {key}
```

![image](https://github.com/CristianoSFMothe/app-javascript-vanilla/assets/68359459/78dccbfb-ab4c-4e72-b5cc-dfbe57cd560e)

4. Criar uma variavél de ambiente dentro do `GitHub Actiions` no repositório




