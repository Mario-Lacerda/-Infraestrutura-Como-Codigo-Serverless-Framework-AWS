## Desafio Dio - API Node.js com Serverless Framework em ambiente AWS

#### **CriarInfraestrutura como Código com Serverless Framework na AWS**

Neste projeto vamos criar uma infraestrutra em nuvem AWS com API Gateway, DynamoDB, AWS Lambda e AWS CloudFormation utilizando o framework Serverless para o desenvolvimento baseada em Infraestrutura as a Code.



**Objetivo:** Projetar e implementar uma infraestrutura como código usando o Serverless Framework na AWS.

#### Etapas

##### Pré requisitos: 

 - possuir uma conta na AWS e instalar Node.js na máquina.
 - Instalar o AWS CLI: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html

**Diretrizes:**

- Conceitos de Infraestrutura como Código:
  - Aprenda sobre os benefícios e práticas recomendadas da infraestrutura como código.
  - Entenda como o Serverless Framework simplifica o provisionamento e gerenciamento de infraestrutura na AWS.
- Serverless Framework:
  - Explore os recursos e funcionalidades do Serverless Framework para implantar aplicativos sem servidor na AWS.
  - Compreenda como definir funções sem servidor, gerenciar eventos e configurar recursos de infraestrutura usando arquivos YAML.
- Gerenciamento de Logs e Monitoramento:
  - Investigue estratégias para gerenciar logs e monitorar aplicativos sem servidor para garantir confiabilidade e desempenho.
  - Aprenda a usar serviços da AWS como CloudWatch Logs e CloudWatch Metrics para rastrear e analisar dados de log e métricas.

**Consultas e Resultados Práticos:**

**Consulta 1: Verificar o status da função sem servidor**

```plaintext
serverless info
```



**Resultado:** Exibe informações sobre a função, incluindo seu nome, descrição, versão e status.

**Consulta 2: Invocar a função sem servidor localmente**

```plaintext
serverless invoke local
```

**Resultado:** Invoca a função localmente e exibe o resultado. Isso é útil para testar sua função antes de implantá-la na AWS.

**Consulta 3: Implantar a função sem servidor na AWS**

```plaintext
serverless deploy
```

**Resultado:** Implanta a função na AWS e exibe o URL da API. Agora você pode acessar sua função por meio de uma solicitação HTTP.

**Consulta 4: Verificar os logs da função sem servidor**

```plaintext
serverless logs -f hello
```

**Resultado:** Exibe os logs da função especificada. Isso ajuda a depurar problemas e entender o comportamento da função.

**Consulta 5: Monitorar a função sem servidor**

```plaintext
serverless monitor
```

**Resultado:** Monitora a função em tempo real e exibe métricas como uso de memória e duração da execução. Isso permite que você monitore o desempenho e a saúde da sua função.

**Conclusão:**

Este desafio prático fornece uma compreensão abrangente da infraestrutura como código com o Serverless Framework na AWS. Ao concluir este desafio, você será capaz de:

- Projetar e implementar aplicativos sem servidor usando o Serverless Framework.
- Gerenciar logs e monitorar aplicativos sem servidor para garantir confiabilidade e desempenho.
- Compreender os benefícios e práticas recomendadas da infraestrutura como código.



#### API Node.js com Serverless Framework em ambiente AWS

Criar infraestrutra em nuvem AWS com API Gateway, DynamoDB, AWS Lambda e AWS CloudFormation utilizando o framework Serverless para o desenvolvimento baseada em Infraestrutura as a Code.



### Setup Inicial

#### Credenciais AWS

- Criar usuário: AWS Management Console -> IAM Dashboard -> Create New User -> <nome do usuário> -> Permissions "Administrator Access" -> Programmatic Access -> Dowload Keys

- No terminal: ```$ aws configure``` -> colar as credenciais geradas anteriormente

  

#### Configurar o framework Serverless

```$ npm i -g serverless```

### Desenvolvimento do projeto

```
$ serverless
Login/Register: No
Update: No
Type: Node.js REST API
Name: dio-live
```

```
$ cd dio-live
$ code .
```

- No arquivo ```serverless.yml``` adicionar a região ```region: us-east-1``` dentro do escopo de ```provider:```
- Salvar e realizar o deploy ```$ serverless deploy -v```

#### Estruturar o código

- Criar o diretório "src" e mover o arquivo "handler.js" para dentro dele
- Renomear o arquivo "handler.js" para "hello.js"
- Atualizar o código 

```
const hello = async (event) => {
/////
module.exports = {
    handler:hello
}
```

- Atualizar o arquivo "serverless.yml "

```
handler: src/hello.handler
```

```$ serverless deploy -v ```

#### DynamoDB

Atualizar o arquivo serverless.yml

```
resources:
  Resources:
    ItemTable:
      Type: AWS::DynamoDB::Table
      Properties:
          TableName: ItemTable
          BillingMode: PAY_PER_REQUEST
          AttributeDefinitions:
            - AttributeName: id
              AttributeType: S
          KeySchema:
            - AttributeName: id
              KeyType: HASH
```

#### Desenvolver funções lambda

	- Pasta /src do repositório
	- Obter arn da tabela no DynamoDB AWS Console -> DynamoDB -> Overview -> Amazon Resource Name (ARN)
	- Atualizar arquivo serverless.yml com o código a seguir, abaixo do ```region:```

  ```
	iam:
      role:
          statements:
            - Effect: Allow
              Action:
                - dynamodb:PutItem
                - dynamodb:UpdateItem
                - dynamodb:GetItem
                - dynamodb:Scan
              Resource:
                - arn:aws:dynamodb:us-east-1:167880115321:table/ItemTable
  ```

   - Instalar dependências

   ```npm init```
   ```npm i uuid aws-sdk```

  - Atualizar lista de funções no arquivo serverless.yml

  ```
functions:
hello:
  handler: src/hello.handler
  events:
    - http:
        path: /
        method: get
insertItem:
  handler: src/insertItem.handler
  events:
    - http:
        path: /item
        method: post
fetchItems:
  handler: src/fetchItems.handler
  events:
    - http:
        path: /items
        method: get
fetchItem:
  handler: src/fetchItem.handler
  events:
    - http:
        path: /items/{id}
        method: get
updateItem:
  handler: src/updateItem.handler
  events:
    - http:
        path: /items/{id}
        method: put
  ```



#### O pré requisito para esse projeto foram:

- Fazer uma conta no site da AWS: https://aws.amazon.com/pt/;
- Instalar Node.js;
- Instalar AWS CLI: https://aws.amazon.com/pt/cli/