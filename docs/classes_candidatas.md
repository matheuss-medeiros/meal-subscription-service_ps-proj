# Classes Candidatas - Plataforma Minha Refeição

* **Assinante:** O usuário que utiliza a plataforma ("Minha Refeição") para contratar um plano de assinatura, escolher itens do cardápio e realizar pagamentos; resumidamente, interage ativamente com a plataforma.

* **Plano de Assinatura:** O plano escolhido pelo assinante, que determinará a quantidade de refeições e a periodicidade das entregas.

* **Preferência Alimentar:** A preferência informada pelo assinante que define o seu padrão de consumo, classificando a alimentação como tradicional, vegetariana ou sem lactose.

* **Refeição:** Definida na especificação do projeto como um conjunto que contém prato principal, acompanhamento e sobremesa. O limite da quantidade de refeições que podem ser selecionadas é definido pelo plano contratado.

* **Prato Principal:** Item central da refeição, no qual o usuário escolherá a quantidade desejada para a entrega periódica. As opções apresentadas são filtradas de acordo com as preferências alimentares previamente informadas.

* **Acompanhamento:** Item que compõe uma refeição, oferecendo opções secundárias para acompanhar o prato principal. O usuário também escolherá a quantidade desejada desse item para as entregas periódicas.

* **Sobremesa:** Item complementar que compõe a refeição. O usuário poderá optar por incluí-la ou não, selecionando a quantidade desejada para a entrega periódica.

* **Endereço:** Registra as informações de localização do usuário (como moradia, trabalho ou casa dos pais). Será utilizado para garantir que a entrega aconteça no local escolhido.

* **Cartão de Crédito:** Entidade que armazena as informações do cartão de crédito do usuário, utilizadas para efetuar o processamento do pagamento.

* **Pedido:** Um registro do sistema, cujo status é atualizado ao longo do fluxo (ex: Aguardando Pagamento, Aprovado). Centraliza todas as escolhas feitas pelo usuário para aquela janela de tempo definida na periodicidade do plano de assinatura.