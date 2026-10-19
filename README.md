# Wiki N1 - Meal Subscription Service

**Projeto acadêmico de Engenharia de Software para contratação e gerenciamento de assinaturas de marmitas contemplando o cadastro do assinante, controle de pedidos (seleção de planos de refeições, preferências alimentares, cardápio personalizado), recorrência da entrega e processamento de pagamentos.**

**04N - Profa. Ana Claudia Rossi**

## Membros do Grupo
* Caroline Lee - 10264791
* Matheus Medeiros - 10748040
* Beatriz Siqueira - 10741570

---

## 1. Lista de Classes Candidatas Identificadas

* 🔗 **Casses Candidatas:** [Classes Candidatas](/docs/classes_candidatas.md)

---

## 2. Protótipo (Sequência de Telas e Caminho Feliz)

O storyboard ilustra o fluxo ideal do usuário na plataforma: 
`Criação da conta` ➔ `Escolha do plano de refeições` ➔ `Filtro de preferências e seleção do cardápio` ➔ `Checkout` ➔ `Tela de sucesso`.

* 🔗 **Acesso ao Protótipo:** https://www.figma.com/design/tNJninqAPZxRja7Ed4Swx3/Beatriz-de-Assis-Siqueira-s-team-library?node-id=0-1&t=7E1IHlFpNNH7joOF-1
* 🔗 **Estrutura do Design:** [Design Structure](/docs/design_structure.pptx)

---

## 3. Vídeo Explicativo e Distribuição de Responsabilidades

* 🎥 **Assistir à Apresentação:** https://youtu.be/F8k4WEhj0Kg?is=SAWOn4MUsja3hRMI

### Divisão de Tarefas da Equipe:
* **Beatriz Assis:** Prototipação das telas do fluxo de cadastro via número de telefone e plano de assinatura, e design da aplicação (paleta de cores, elementos gráficos, tipografia e plataforma).
* **Matheus Medeiros:** Levantamento das classes candidatas, design da aplicação (logotipo) e prototipação das telas de endereço, pagamento, confirmação do pedido e status do pedido.
* **Caroline Lee:** Gravação/edição do vídeo, elaboração do roteiro e prototipação da sequencia de preferência alimentar, prato principal, acompanhamento, sobremesa e resumo do pedido.

## 4. Diagrama de Domínio:
<img width="2116" height="650" alt="Untitled Diagram" src="https://github.com/user-attachments/assets/07e6988e-2baa-4523-a905-8aad276301e9" />

# Wiki N2 - Meal Subscription Service

# Diagrama de Sequência 1 — Autenticação por SMS

```mermaid
sequenceDiagram
    autonumber
    actor Usuario as Assinante
    participant Tela as <<boundary>> TelaAutenticacao
    participant Ctrl as <<control>> ControladorAssinatura
    participant Repo as <<entity>> RepositorioAssinantes
    participant SMS as <<boundary>> ServicoSMS
    participant Catalogo as <<entity>> CatalogoPlanos

    Usuario->>Tela: informarCelular(celular)
    Tela->>Ctrl: informarCelular(celular): void
    Ctrl->>Repo: buscarPorCelular(celular): Assinante
    Repo-->>Ctrl: assinanteEncontrado (ou nenhum)

    alt Assinante nao encontrado
        Ctrl->>Repo: criarAssinante(celular): Assinante
        Ctrl->>Repo: salvar(assinante): void
    end

    create participant Cod as <<entity>> CodigoConfirmacao
    Ctrl->>Cod: criarCodigoConfirmacao(): CodigoConfirmacao
    Ctrl->>SMS: enviarCodigo(celular, codigoGerado): void
    Ctrl-->>Tela: solicitarCodigo(): void
    Tela-->>Usuario: exibirTelaCodigo()

    loop Ate validar ou atingir 3 tentativas
        Usuario->>Tela: informarCodigo(codigo)
        Tela->>Ctrl: informarCodigo(codigo): boolean
        Ctrl->>Cod: validar(codigo): boolean
        Cod-->>Ctrl: valido

        alt Codigo valido
            Ctrl->>Catalogo: listarPlanos(): Lista~PlanoAssinatura~
            Catalogo-->>Ctrl: planos
            Ctrl-->>Tela: apresentarPlanos(planos): void
            Tela-->>Usuario: exibirPlanos(planos)
        else Codigo invalido - FA1
            Ctrl->>Cod: registrarTentativa(): Integer
            Cod-->>Ctrl: tentativas

            alt tentativas menor que 3
                Ctrl-->>Tela: apresentarErroCodigo(): void
                Tela-->>Usuario: solicitarNovaTentativa()
            else tentativas igual a 3
                Ctrl-->>Tela: encerrarValidacao(): void
                Tela-->>Usuario: solicitarNovoCodigo()
            end
        end
    end
```

# Diagrama de Sequência 2 — Plano e Preferências

```mermaid
sequenceDiagram
    autonumber
    actor Usuario as Assinante
    participant Tela as <<boundary>> TelaPlanoPreferencias
    participant Ctrl as <<control>> ControladorAssinatura
    participant Catalogo as <<entity>> CatalogoPlanos
    participant Plano as <<entity>> PlanoAssinatura
    participant A as <<entity>> Assinante
    participant S as <<entity>> Assinatura
    participant R as <<entity>> RestricaoAlimentar
    participant C as <<entity>> Cardapio

    Usuario->>Tela: selecionarPlano(idPlano)
    Tela->>Ctrl: selecionarPlano(idPlano): void
    Ctrl->>Catalogo: buscarPlano(idPlano): PlanoAssinatura
    Catalogo-->>Ctrl: plano
    Ctrl->>S: registrarPlano(plano): void

    opt Plano permite pacote de dias
        Note over Usuario,Ctrl: Registrar pacote de dias previsto no plano
        Ctrl->>S: registrarPacoteDias(dias): void
    end

    Ctrl-->>Tela: apresentarPreferencias(): void
    Tela-->>Usuario: exibirPreferencias()

    Usuario->>Tela: informarPreferencias(restricoes)
    Tela->>Ctrl: informarPreferencias(restricoes): void

    loop Para cada restricao informada
        Ctrl->>R: criarRestricao(tipo): RestricaoAlimentar
        Ctrl->>A: adicionarRestricao(restricao): void
    end

    Ctrl->>C: listarPratosPrincipais(plano, restricoes): Lista~ItemCardapio~
    C-->>Ctrl: pratosFiltrados
    Ctrl-->>Tela: apresentarPratosPrincipais(pratosFiltrados): void
    Tela-->>Usuario: exibirPratosPrincipais(pratosFiltrados)
```
# Tabela de Estímulos

| # | Estímulo | Tela do protótipo | Passos do fluxo | Alternativo |
|---|----------|-------------------|-----------------|-------------|
| 1 | `informarCelular` | iPhone 17 - 1 | 1–2 | – |
| 2 | `informarCodigo` | iPhone 17 - 6 | 3–5 | FA1 |
| 3 | `selecionarPlano` | iPhone 17 - 3 | 6–7 | – |
| 4 | `informarPreferencias` | iPhone 17 - 4 | 8–10 | – |
| 5 | `selecionarPratosPrincipais` | iPhone 17 - 5 | 11–13 | FA2 |
| 6 | `selecionarAcompanhamentos` | iPhone 17 - 7 | 14–16 | FA2 |
| 7 | `selecionarSobremesas` | iPhone 17 - 8 (resumo na 9) | 17–20 | FA2 |
| 8 | `informarEnderecoEntrega` | iPhone 17 - 10 e 11 | 21–24 | – |
| 9 | `informarDadosCartao` | iPhone 17 - 12 e 13 | 25 | – |
| 10 | `confirmarPedido` | iPhone 17 - 17 (resultado em 18 e 19) | 26–32 | FA3 |

---

# Diagrama de Sequência 3 — Montagem do Pedido

```mermaid
sequenceDiagram
    autonumber
    actor Usuario as Assinante
    participant Tela as <<boundary>> TelaMontagemPedido
    participant Ctrl as <<control>> ControladorAssinatura
    participant A as <<entity>> Assinante
    participant S as <<entity>> Assinatura
    participant Plano as <<entity>> PlanoAssinatura
    participant C as <<entity>> Cardapio
    participant IP as <<entity>> ItemPedido

    Note over Usuario,Ctrl: Estimulo 1 - Pratos principais
    Usuario->>Tela: selecionarPratosPrincipais(itens)
    Tela->>Ctrl: selecionarPratosPrincipais(itens): void

    Ctrl->>A: gerarPedido(): Pedido
    create participant P as <<entity>> Pedido
    A->>P: criarPedido(): Pedido
    P-->>A: pedido
    A-->>Ctrl: pedido

    Ctrl->>S: validarQuantidade(tipoRefeicao, quantidade): boolean
    S->>Plano: getQuantidadeRefeicoes(): Integer
    Plano-->>S: limite
    S-->>Ctrl: valido

    alt Quantidade dentro do limite do plano
        loop Para cada prato selecionado
            Ctrl->>C: buscarItem(idItem): ItemCardapio
            C-->>Ctrl: itemCardapio
            Ctrl->>P: adicionarItem(itemCardapio, quantidade): void
            P->>IP: criarItemPedido(itemCardapio, quantidade): ItemPedido
        end
        Ctrl->>A: getRestricoes(): Lista~RestricaoAlimentar~
        A-->>Ctrl: restricoes
        Ctrl->>C: listarAcompanhamentos(restricoes): Lista~ItemCardapio~
        C-->>Ctrl: acompanhamentos
        Ctrl-->>Tela: apresentarAcompanhamentos(acompanhamentos): void
        Tela-->>Usuario: exibirAcompanhamentos(acompanhamentos)
    else Quantidade acima do limite - FA2
        Ctrl->>S: getLimiteRefeicoes(): Integer
        S-->>Ctrl: limite
        Ctrl-->>Tela: apresentarErroLimite(limite): void
        Tela-->>Usuario: solicitarAjusteSelecao(limite)
        Note over Usuario,Ctrl: Apos o ajuste, o fluxo retorna a selecao de pratos principais
    end

    Note over Usuario,Ctrl: Estimulo 2 - Acompanhamentos
    Usuario->>Tela: selecionarAcompanhamentos(itens)
    Tela->>Ctrl: selecionarAcompanhamentos(itens): void
    Ctrl->>S: validarQuantidade(tipoRefeicao, quantidade): boolean
    S-->>Ctrl: valido

    alt Quantidade dentro do limite do plano
        loop Para cada acompanhamento selecionado
            Ctrl->>C: buscarItem(idItem): ItemCardapio
            C-->>Ctrl: itemCardapio
            Ctrl->>P: adicionarItem(itemCardapio, quantidade): void
            P->>IP: criarItemPedido(itemCardapio, quantidade): ItemPedido
        end
        Ctrl->>A: getRestricoes(): Lista~RestricaoAlimentar~
        A-->>Ctrl: restricoes
        Ctrl->>C: listarSobremesas(restricoes): Lista~ItemCardapio~
        C-->>Ctrl: sobremesas
        Ctrl-->>Tela: apresentarSobremesas(sobremesas): void
        Tela-->>Usuario: exibirSobremesas(sobremesas)
    else Quantidade acima do limite - FA2
        Ctrl->>S: getLimiteRefeicoes(): Integer
        S-->>Ctrl: limite
        Ctrl-->>Tela: apresentarErroLimite(limite): void
        Tela-->>Usuario: solicitarAjusteSelecao(limite)
        Note over Usuario,Ctrl: Apos o ajuste, o fluxo retorna a selecao de acompanhamentos
    end

    Note over Usuario,Ctrl: Estimulo 3 - Sobremesas
    Usuario->>Tela: selecionarSobremesas(itens)
    Tela->>Ctrl: selecionarSobremesas(itens): void
    Ctrl->>S: validarQuantidade(tipoRefeicao, quantidade): boolean
    S-->>Ctrl: valido

    alt Quantidade dentro do limite do plano
        loop Para cada sobremesa selecionada
            Ctrl->>C: buscarItem(idItem): ItemCardapio
            C-->>Ctrl: itemCardapio
            Ctrl->>P: adicionarItem(itemCardapio, quantidade): void
            P->>IP: criarItemPedido(itemCardapio, quantidade): ItemPedido
        end
        Ctrl->>P: obterResumo(): Lista~ItemPedido~
        P-->>Ctrl: itensPedido
        Ctrl-->>Tela: apresentarResumoPedido(itensPedido): void
        Tela-->>Usuario: exibirResumoPedido(itensPedido)
        Ctrl->>A: getEnderecos(): Lista~Endereco~
        A-->>Ctrl: enderecos
        Ctrl-->>Tela: solicitarEnderecoEntrega(enderecos): void
        Tela-->>Usuario: exibirEnderecos(enderecos)
    else Quantidade acima do limite - FA2
        Ctrl->>S: getLimiteRefeicoes(): Integer
        S-->>Ctrl: limite
        Ctrl-->>Tela: apresentarErroLimite(limite): void
        Tela-->>Usuario: solicitarAjusteSelecao(limite)
        Note over Usuario,Ctrl: Apos o ajuste, o fluxo retorna a selecao de sobremesas
    end
```

---

# Diagrama de Sequência 4 — Endereço de Entrega

```mermaid
sequenceDiagram
    autonumber
    actor Usuario as Assinante
    participant Tela as <<boundary>> TelaEndereco
    participant Ctrl as <<control>> ControladorAssinatura
    participant A as <<entity>> Assinante
    participant E as <<entity>> Endereco
    participant P as <<entity>> Pedido
    participant S as <<entity>> Assinatura

    Usuario->>Tela: informarEnderecoEntrega(endereco)
    Tela->>Ctrl: informarEnderecoEntrega(endereco): void

    alt Endereco novo
        Ctrl->>A: adicionarEndereco(dadosEndereco): Endereco
        A->>E: criarEndereco(dadosEndereco): Endereco
        E-->>A: endereco
        A-->>Ctrl: endereco
    else Endereco ja cadastrado
        Ctrl->>A: buscarEndereco(idEndereco): Endereco
        A-->>Ctrl: endereco
    end

    Ctrl->>P: definirEnderecoEntrega(endereco): void
    Ctrl->>S: alterarStatus(AGUARDANDO_PGMT): void
    Note over S: status = AGUARDANDO_PGMT

    Ctrl->>P: calcularValorTotal(): Float
    P-->>Ctrl: valorTotal
    Ctrl-->>Tela: solicitarDadosPagamento(valorTotal): void
    Tela-->>Usuario: exibirTelaPagamento(valorTotal)
```

## Diagrama de Domínio Corrigido — N2

```mermaid
classDiagram
    direction LR

    class Endereco {
        -logradouro: String
        -numero: String
        -bairro: String
        -cidade: String
        -uf: char[2]
        -cep: String
    }

    class Assinante {
        -nome: String
        -sobrenome: String
        -dataNascimento: Date
        -celular: String
    }

    class Assinatura {
        -valorCobrado: Float
        -dataInicio: Date
        -dataFim: Date
        -status: StatusAssinatura
        -protocolo: String
        -previsaoPrimeiraEntrega: Date
    }

    class PlanoAssinatura {
        -nome: String
        -quantidadeRefeicoes: Integer
        -periodicidade: String
        -valor: Float
    }

    class Pedido {
        -dataPedido: Date
        -status: StatusPedido
    }

    class Refeicao {
        -id: Integer
    }

    class ItemCardapio {
        -nomeItem: String
        -tipoRefeicao: TipoRefeicao
        -restritoPreferenciaAlimentar: TipoRestricaoAlimentar[]
    }

    class CartaoCredito {
        -nomeTitular: String
        -numero: String
        -codigoSeguranca: String
        -opcaoPgmt: OpcaoPgmt
    }

    class Pagamento {
        -dataHora: Date
        -valorPago: Float
        -codigoAutorizacao: String
        -statusPgmt: StatusPgmt
    }

    class CodigoConfirmacao {
        -codigo: String
        -tentativas: Integer
        -validado: Boolean
    }

    class Cardapio
    class RestricaoAlimentar
    class OperadoraCartaoCredito

    class ItemPedido {
        -quantidade: Integer
    }

    class TipoRestricaoAlimentar {
        <<enumeration>>
        NA
        VEGETARIANO
        VEGANO
        S_LACTOSE
        S_GLUTEM
    }

    class TipoRefeicao {
        <<enumeration>>
        PRATO_PRINCIPAL
        ACOMPANHAMENTO
        SOBREMESA
    }

    class StatusAssinatura {
        <<enumeration>>
        AGUARDANDO_PGMT
        ERRO_PGMT
        CONFIRMADO_PGMT
        ATIVO
        INATIVO
        CANCELADO
    }

    class StatusPedido {
        <<enumeration>>
        EM_PREPARO
        ENVIADO_ENTREGA
        EM_ROTA
        ENTREGUE
        TENTATIVA_ENTREGA_MAL_S
    }

    class OpcaoPgmt {
        <<enumeration>>
        VR
        VA
        CREDITO
    }

    class StatusPgmt {
        <<enumeration>>
        PENDENTE
        APROVADO
        RECUSADO
    }

    Assinante "1" -- "0..*" Endereco : possui
    Assinante "1" -- "0..1" Assinatura : possui
    Assinante "1" -- "0..*" CartaoCredito : possui
    Assinante "1" -- "1..*" Pedido : gera

    Assinatura "0..*" --> "1" PlanoAssinatura : contrato de
    Endereco "1" -- "0..*" Pedido : entrega

    Pedido "1" *-- "1..*" Refeicao : possui
    Refeicao "0..*" o-- "2..3" ItemCardapio : possui

    Assinatura -- Pagamento : pagamento independente
    Pagamento --> OperadoraCartaoCredito : autorizacao

    Assinante -- CodigoConfirmacao : codigo SMS
    Assinante -- RestricaoAlimentar : preferencias

    Cardapio -- ItemCardapio : apresenta

    Pedido -- ItemPedido : inclui
    ItemPedido --> ItemCardapio : referencia

    Pedido --> Endereco : endereco de entrega
    CartaoCredito --> Endereco : endereco de cobranca
```
