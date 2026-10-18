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

**Escopo:** telas 1 e 2, passos 1–5, alternativo FA1. **Estímulos:** `informarCelular(celular)` e `informarCodigo(codigo)`. Os retornos de apresentação são respostas do sistema, não estímulos.

```mermaid
sequenceDiagram
 autonumber
 actor Usuario as Assinante
 participant Tela as <<boundary>> TelaAutenticacao
 participant Ctrl as <<control>> ControladorAssinatura
 participant Repo as <<entity>> RepositorioAssinantes
 participant A as <<entity>> Assinante
 participant Cod as <<entity>> CodigoConfirmacao
 participant SMS as <<boundary>> ServicoSMS
 participant Catalogo as <<entity>> CatalogoPlanos
 Usuario->>Tela: informarCelular(celular)
 Tela->>Ctrl: informarCelular(celular): void
 Ctrl->>Repo: buscarPorCelular(celular): Assinante
 Repo-->>Ctrl: assinanteEncontrado (ou nenhum)
 alt Assinante nao encontrado
  create participant NovoA as <<entity>> NovoAssinante
  Ctrl->>NovoA: criarAssinante(celular)
  Ctrl->>Repo: salvar(assinante): void
 end
 create participant NovoCod as <<entity>> NovoCodigoConfirmacao
 Ctrl->>NovoCod: criarCodigoConfirmacao(): CodigoConfirmacao
 Ctrl->>SMS: enviarCodigo(celular, codigoGerado): void
 Ctrl-->>Tela: solicitarCodigo(): void
 Tela-->>Usuario: exibirTelaCodigo()
 Usuario->>Tela: informarCodigo(codigo)
 Tela->>Ctrl: informarCodigo(codigo): boolean
 Ctrl->>NovoCod: validar(codigo): boolean
 NovoCod-->>Ctrl: valido
 alt Codigo valido
  Ctrl->>Catalogo: listarPlanos(): Lista~PlanoAssinatura~
  Catalogo-->>Ctrl: planos
  Ctrl-->>Tela: apresentarPlanos(planos): void
  Tela-->>Usuario: exibirPlanos(planos)
 else Codigo invalido - FA1
  Ctrl->>NovoCod: registrarTentativa(): Integer
  NovoCod-->>Ctrl: tentativas
  alt tentativas menor que 3
   Ctrl-->>Tela: apresentarErroCodigo(): void
   Tela-->>Usuario: solicitarNovaTentativa()
   Note over Usuario,NovoCod: Repetir informarCodigo e validar a cada nova tentativa
  else tentativas igual a 3
   Ctrl-->>Tela: encerrarValidacao(): void
   Tela-->>Usuario: solicitarNovoCodigo()
  end
 end
```

**Nota de fidelidade:** `RepositorioAssinantes`, `ServicoSMS` e os métodos são elementos de projeto inferidos para concretizar os passos. O guia determina a busca/criação do assinante e o envio por SMS, mas não fixa o nome técnico do serviço ou repositório. A Pessoa 3 deve usar os mesmos nomes no diagrama de classes de projeto.



