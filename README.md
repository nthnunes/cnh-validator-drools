# Drools Application - Validação de CNHs

Este projeto utiliza o Drools para definir e aplicar regras de negócio em relação à validade de Carteiras Nacionais de Habilitação (CNHs). As regras verificam se uma CNH está expirada, se o condutor possui mais pontos do que o permitido e marcam as CNHs como válidas ou inválidas com base nesses critérios.

## Estrutura do Projeto

### 1. Classes principais

#### `Cnh.java`
A classe `Cnh` contém os atributos principais relacionados a uma CNH, como:
- **Número**
- **Nome do condutor**
- **Pontos**
- **Data de expiração**
- **Status da CNH**: Pode ser `VALIDA`, `INVALIDA`, ou `NAO_PROCESSADA`.
- **Causa**: Explica o motivo pelo qual a CNH foi considerada inválida, se for o caso.

A classe possui o método `isExpirada()` que verifica se a CNH já passou da data de expiração, retornando um booleano.

#### `CnhRepository.java`
O repositório `CnhRepository` simula uma base de dados, retornando uma lista de CNHs com atributos predefinidos para fins de teste.

#### `DroolsApplication.java`
A classe `DroolsApplication` é o ponto de entrada da aplicação e responsável por:
1. Configurar o contêiner do Drools (`KieContainer`) e a sessão (`KieSession`).
2. Inserir as instâncias de CNHs na sessão.
3. Disparar as regras através do método `kieSession.fireAllRules()`.

#### `Status.java`
Define os possíveis status de uma CNH:
- `VALIDA`
- `INVALIDA`
- `NAO_PROCESSADA`

### 2. Regras de Negócio

As regras de negócio são definidas no arquivo `rules.drl`, onde são aplicadas três principais regras relacionadas ao status das CNHs.

#### `rules.drl`
Este arquivo define as regras utilizando a linguagem específica do Drools, DRL (Drools Rule Language).

##### Regras implementadas:

1. **Regra "Cnh Expirada"**
   - Condição: A CNH já está expirada (`isExpirada()`).
   - Ação: O status da CNH é alterado para `INVALIDA` e a causa é atualizada para "CNH expirada".

2. **Regra "Cnh pontos"**
   - Condição: A CNH possui mais de 20 pontos.
   - Ação: O status da CNH é alterado para `INVALIDA` e a causa é "Cnh tem mais pontos que o permitido de 20".

3. **Regra "Cnh valida"**
   - Condição: A CNH ainda não foi processada (`status == Status.NAO_PROCESSADA`).
   - Ação: O status da CNH é alterado para `VALIDA` temporariamente e a causa é "Cnh sem status, considerada valida por enquanto".

### 3. Configuração do Drools

O Drools é configurado através do contêiner de regras e a sessão:

- **KieContainer**: É responsável por carregar todas as configurações do Drools e inicializar o ambiente de execução das regras.
- **KieSession**: Gerencia as inserções de fatos (instâncias de CNHs) e dispara as regras quando necessário. A sessão utilizada no código é criada a partir da configuração definida no arquivo `kmodule.xml` (não mostrado neste exemplo).

### 4. Execução

Quando a aplicação é executada, as CNHs são carregadas do repositório e inseridas na sessão do Drools. As regras são aplicadas conforme as condições estabelecidas no arquivo de regras (`rules.drl`).

Após a execução, é possível visualizar no console o status e a causa de cada CNH, conforme as regras aplicadas.


## Considerações

- Este projeto usa uma abordagem baseada em regras com o Drools, separando a lógica de negócios da lógica de aplicação.
- O uso do Drools facilita a manutenção e atualização das regras, além de permitir a fácil adição de novas regras sem modificar diretamente o código principal.

## Requisitos

- **Java 8+**
- **Drools 7.x**

## Como executar

1. Clone o repositório.
2. Compile e execute o projeto Java.
3. Verifique o console para ver os resultados da aplicação das regras de negócio.

```bash
$ mvn clean install
$ java -jar target/drools-application.jar
