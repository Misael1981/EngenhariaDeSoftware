# Arquitetura Hexagonal

A **Arquitetura Hexagonal**, também conhecida como **Arquitetura de Portas e Adaptadores**, é um padrão arquitetural que busca isolar a **lógica de negócio central** de um sistema de suas **preocupações externas**, como banco de dados, interfaces de usuário (UIs), serviços externos, etc. Ela foi proposta por Alistair Cockburn e seu principal objetivo é criar aplicações **altamente testáveis, flexíveis e independentes** de tecnologias específicas.

## A Ideia Central: O Hexágono e o Domínio

A metáfora do hexágono é usada para ilustrar que o **núcleo da sua aplicação (o domínio)** está no centro e pode interagir com o "mundo exterior" através de várias "**faces**" ou "**portas**".

- **Núcleo (Domínio/Core da Aplicação)**: Esta é a parte mais importante. Contém **toda a lógica de negócio** da sua aplicação, as regras, entidades, casos de uso e serviços que definem o comportamento do sistema. O ponto crucial é que o núcleo **NÃO DEVE TER NENHUMA DEPENDÊNCIA** de tecnologias externas (como frameworks web, tipos de banco de dados, bibliotecas de UI, etc.). Ele é puro, independente e pode ser testado isoladamente.

## Portas e Adaptadores: As Pontes de Comunicação

Para que o núcleo possa interagir com o mundo externo sem depender dele diretamente, a Arquitetura Hexagonal utiliza o conceito de **Portas e Adaptadores**.

- **Portas (Interfaces)**: Pense nas portas como interfaces ou contratos. Elas são definidas dentro do núcleo da aplicação e especificam como o núcleo quer se comunicar com o mundo exterior. Existem dois tipos principais de portas:

    - **Portas Primárias (Dirigindo a Aplicação)**: São as interfaces que o mundo exterior usa para enviar comandos ou solicitações para a lógica de negócio. Por exemplo, uma interface que define um método `criarPedido(dadosDoPedido)`.

    - **Portas Secundárias (Sendo Dirigidas pela Aplicação)**: São as interfaces que o núcleo usa para solicitar algo do mundo exterior, como persistir dados, enviar e-mails ou acessar um serviço de terceiros. Por exemplo, uma interface `RepositorioDePedidos` com um método `salvar(pedido)`.

- **Adaptadores (Implementações)**: Os adaptadores são as implementações concretas das portas. Eles ficam fora do núcleo e fazem a **tradução** entre o formato que o mundo exterior entende e o formato que o núcleo espera (e vice-versa).

    - **Adaptadores Primários (para Portas Primárias)**: São responsáveis por converter as interações externas (como requisições HTTP de uma API REST, eventos de uma fila de mensagens, ou cliques em uma interface gráfica) em chamadas para as portas primárias do núcleo. Exemplo: um **Controlador REST** que recebe uma requisição HTTP e chama o método `criarPedido` no seu serviço de domínio.

    - **Adaptadores Secundários (para Portas Secundárias)**: São responsáveis por implementar as interfaces definidas pelas portas secundárias do núcleo. Eles convertem as chamadas do núcleo para a tecnologia externa específica. Exemplo: uma implementação de `RepositorioDePedidos` que usa um **ORM** para salvar o pedido em um **banco de dados relacional**, ou um cliente HTTP para enviar o pedido para um **serviço externo**.

## Como Funciona na Prática?

1. E**ntrada**: Uma requisição chega de um adaptador (ex: uma API REST).

2. **Tradução**: O adaptador traduz essa requisição para um formato que o núcleo entenda e chama uma **porta primária** no seu domínio.

3. **Lógica de Negócio**: O domínio executa sua lógica interna. Se precisar interagir com um recurso externo (ex: salvar no banco de dados), ele chama um método em uma **porta secundária**.

4. **Tradução de Saída**: O adaptador secundário, que implementa essa porta, traduz a chamada para a tecnologia específica (ex: uma query SQL) e interage com o recurso externo (banco de dados).

5. **Resposta**: O resultado da operação do domínio é devolvido ao adaptador primário, que o traduz de volta para o formato esperado pelo cliente (ex: uma resposta JSON HTTP).

## Vantagens da Arquitetura Hexagonal

- **Independência da Tecnologia**: O núcleo da sua aplicação não sabe e não se importa se você está usando Spring Boot, Node.js, um banco de dados SQL, NoSQL, ou uma fila de mensagens. Isso permite que você mude de tecnologia no futuro com muito menos impacto.

- **Testabilidade Aprimorada**: Como a lógica de negócio é isolada, você pode testá-la exaustivamente sem precisar de um banco de dados real, uma UI complexa, ou serviços externos. Basta mockar as portas, e o núcleo pode ser testado em memória de forma rápida e eficiente.

- **Flexibilidade**: É fácil adicionar novas formas de interagir com o sistema (novas UIs, novas APIs) ou integrar-se com novos sistemas externos, criando novos adaptadores.

- **Manutenibilidade**: O código é mais organizado, com responsabilidades claras, o que facilita a compreensão e a manutenção.

- **Reuso**: O núcleo da lógica de negócio pode ser facilmente reutilizado em diferentes contextos ou tipos de aplicações.

## Desvantagens da Arquitetura Hexagonal

- **Complexidade Inicial**: Para projetos muito pequenos e simples, a sobrecarga de criar portas e adaptadores pode parecer desnecessária e aumentar a quantidade de código e classes inicialmente.

- **Curva de Aprendizagem**: Desenvolvedores menos experientes podem precisar de um tempo para entender os conceitos de portas e adaptadores e a inversão de dependência.

- **Over-engineering**: Se aplicada onde não é necessária, pode levar a uma complexidade exagerada para o problema em questão.

## Quando Usar?

A Arquitetura Hexagonal é ideal para:

- **Sistemas complexos** com regras de negócio ricas.

- Aplicações que precisam ser **flexíveis a mudanças** de tecnologia.

- Sistemas onde a **testabilidade automatizada** é crucial.

- Projetos que visam a **longevidade** e a **manutenibilidade** a longo prazo.

Em resumo, a Arquitetura Hexagonal é uma abordagem poderosa para construir sistemas onde a **lógica de negócio é a estrela** e deve ser protegida das flutuações e detalhes do mundo externo, garantindo um código mais limpo, robusto e adaptável.

- ### [ALURA+ - O que é arquitetura hexagonal?](https://cursos.alura.com.br/extra/alura-mais/o-que-e-arquitetura-hexagonal--c1134)

### [Voltar ao README Principal](../README.md)