# Clean Architecture (Arquitetura Limpa)

A **Clean Architecture**, ou Arquitetura Limpa, é um conceito de design de software popularizado por Robert C. Martin (o famoso "Uncle Bob"). Ela é, na verdade, uma tentativa de unificar várias arquiteturas de software existentes (como a Arquitetura Hexagonal, Arquitetura Onion, DDD - Domain Driven Design, etc.) em uma ideia central com princípios claros.

## O Coração da Clean Architecture: A Regra de Dependência

A ideia principal da Clean Architecture é a **separação de preocupações** em camadas concêntricas (como uma cebola), onde cada camada tem responsabilidades específicas e, o mais importante, a **Regra de Dependência** é estritamente aplicada:

### As dependências do código-fonte só podem apontar para dentro.

Isso significa que as camadas internas (as mais próximas do centro) **nunca devem saber nada** sobre as camadas externas. Elas devem ser independentes de frameworks, bancos de dados, UIs, e qualquer outra coisa que esteja nas camadas mais externas. As camadas externas, por outro lado, podem depender das camadas internas.

## As Camadas da Clean Architecture (Círculos Concêntricos)

Embora o diagrama possa variar um pouco, a Clean Architecture geralmente é visualizada com quatro camadas principais, do centro para fora:

1. **Entities (Entidades)**

- **O que são**: São as **regras de negócio da empresa** de mais alto nível e mais gerais. Elas encapsulam os dados e o comportamento fundamental do domínio que não mudariam mesmo se toda a aplicação fosse usada em um contexto diferente (ex: uma aplicação web, mobile, desktop).

- **Características**: São puras, sem dependência de absolutamente nada externo. Representam o coração do seu sistema.

- **Exemplos**: Uma classe `Cliente`, `Produto`, `Pedido` com seus atributos e métodos que expressam as regras de negócio intrínsecas (ex: `calcularDesconto()`, `adicionarItem()`).

2. **Use Cases (Casos de Uso) ou Interactors**

- **O que são**: Contêm as regras de negócio específicas da aplicação. Eles orquestram o fluxo de dados para e das Entidades, implementando os casos de uso do sistema. Um caso de uso é uma funcionalidade específica que o usuário (ou outro sistema) quer realizar.

- **Características**: Dependem apenas das Entities. Eles definem **interfaces** (contratos) que serão implementadas pelas camadas externas, para interagir com bancos de dados, serviços externos, etc.

- **Exemplos**: Um `CriarPedidoUseCase`, `RealizarLoginUseCase`, `ListarProdutosUseCase`. Eles recebem dados de entrada, aplicam a lógica de negócio (envolvendo Entities) e fornecem uma saída.

3. **Interface Adapters (Adaptadores de Interface)**

- **O que são**: Esta camada traduz dados das camadas mais externas (como UI ou banco de dados) para o formato que as Use Cases e Entities entendem, e vice-versa. É aqui que frameworks e bibliotecas começam a aparecer.

- **Características**: Dependem das Use Cases e Entities. Contém **Controladores** (para APIs web), **Presenters** (para UIs), e **Gateways/Repositories** (para bancos de dados).

- **Exemplos**:

    - **Controllers**: Recebem requisições HTTP e chamam os Use Cases.

    - **Presenters**: Preparam dados para exibição na UI.

    - **Implementações de Repositório**: Implementam as interfaces definidas nas Use Cases para acessar bancos de dados (usando ORMs, drivers JDBC, etc.).

4. **Frameworks and Drivers (Frameworks e Drivers) ou Infrastructure**

- **O que são**: A camada mais externa. Contém todos os detalhes de implementação e ferramentas específicas: frameworks web (Spring Boot, Express.js, ASP.NET Core), bancos de dados (SQL Server, MongoDB, PostgreSQL), UI (React, Angular, Vue), serviços de e-mail, etc.

- **Características**: Dependem de todas as camadas internas. Esta é a camada onde as tecnologias "sujas" vivem.

## Os Benefícios Chave da Clean Architecture

Assim como a Arquitetura Hexagonal, a Clean Architecture traz vantagens significativas:

- **Independência de Frameworks**: Seu código de negócio central é completamente isolado de frameworks web ou de banco de dados. Se você quiser trocar de Spring Boot para Quarkus, ou de PostgreSQL para Cassandra, as mudanças ficam concentradas nas camadas externas.

- **Testabilidade Aprimorada**: As regras de negócio (Entities e Use Cases) podem ser testadas unitariamente sem a necessidade de levantar um banco de dados, um servidor web ou uma interface gráfica. Isso torna os testes mais rápidos e confiáveis.

- **Independência de UI/Banco de Dados/Serviços Externos**: Você pode trocar a forma como a interface é apresentada ou como os dados são persistidos sem impactar a lógica de negócio principal.

- **Manutenibilidade e Escalabilidade**: Com responsabilidades claras e dependências bem definidas, o código se torna mais fácil de entender, manter e evoluir ao longo do tempo.

- **Separação de Preocupações**: Cada parte do sistema tem uma única responsabilidade, tornando-o mais modular.

## Clean Architecture vs. Arquitetura Hexagonal

Você pode ter notado muitas semelhanças entre a Clean Architecture e a Arquitetura Hexagonal, e isso não é coincidência!

- A **Clean Architecture** é uma evolução e uma generalização de outras arquiteturas centradas no domínio, incluindo a Arquitetura Hexagonal e a Arquitetura Onion.

- Ambas buscam o **isolamento da lógica de negócio** das preocupações externas.

- Ambas usam o conceito de **inversão de dependência**, onde as camadas internas definem interfaces que são implementadas por camadas externas.

A principal diferença é que a **Clean Architecture** apresenta uma estrutura de camadas mais explícita e detalhada (as quatro camadas concêntricas), enquanto a **Arquitetura Hexagonal** foca mais nos conceitos de "Portas e Adaptadores" para descrever a fronteira entre o domínio e o mundo exterior, sem necessariamente definir um número fixo de camadas internas. Em essência, a Arquitetura Hexagonal é uma das formas de implementar os princípios da Clean Architecture.

Pense na Clean Architecture como um conjunto de princípios mais abrangente sobre como organizar seu software para máxima independência, testabilidade e manutenibilidade, enquanto a Arquitetura Hexagonal é uma das várias "táticas" ou "diagramas" que podem ser usados para alcançar esses objetivos.

- ### [ALURA+ - Clean Architecture (Arquitetura Limpa) - O que é?](https://cursos.alura.com.br/extra/alura-mais/clean-architecture-arquitetura-limpa-o-que-e--c204)

### [Voltar ao README Principal](../README.md)