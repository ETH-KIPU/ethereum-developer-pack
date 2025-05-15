# Ataque de repetição (Replay attack)

Um ataque de repetição assinado em um contrato inteligente é um tipo de vulnerabilidade de segurança em que um invasor intercepta uma mensagem assinada válida e a reproduz no contrato. A mensagem normalmente é assinada com uma chave privada e contém instruções executáveis para o contrato inteligente. Ao reproduzir essa mensagem, o invasor pode enganar o contrato para que execute novamente as mesmas instruções, o que pode resultar em comportamentos indesejados ou até perdas financeiras.

**Exemplo:**\
Um contrato inteligente permite que os usuários retirem fundos de sua conta por meio do envio de uma mensagem autorizada e assinada. Um invasor pode interceptar uma mensagem de saque válida de um usuário legítimo e, posteriormente, reproduzir essa mensagem. Mesmo que o contexto tenha mudado, o contrato inteligente pode considerar a mensagem assinada ainda válida, por ter sido assinada digitalmente. Isso poderia permitir que o invasor retirasse fundos da conta do usuário.

**Mitigação:**\
Para prevenir ataques de repetição assinados, os desenvolvedores de contratos inteligentes podem implementar várias medidas de segurança, incluindo:

* O uso de identificadores únicos para cada mensagem (_nonces_);
* A adição de restrições baseadas em tempo para a validade das mensagens assinadas.
