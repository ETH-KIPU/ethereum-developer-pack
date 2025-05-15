# Manipulação de Recompensas (Reward Manipulation)

A manipulação de recompensas ocorre quando um invasor explora falhas na lógica de um contrato inteligente para obter ganhos indevidos ou desproporcionais. Essa vulnerabilidade é particularmente relevante em protocolos DeFi, jogos em blockchain e sistemas que distribuem recompensas com base na participação ou contribuição dos usuários.

**Exemplo:**\
Considere um contrato inteligente que distribui recompensas em tokens para usuários que fazem _staking_ em um _pool_. As recompensas são proporcionais ao tempo de permanência dos tokens no _staking_.

Um invasor pode tentar manipular esse sistema realizando depósitos e retiradas rápidas com grandes quantidades de tokens, explorando falhas na lógica de cálculo das recompensas. Se o contrato não estiver corretamente estruturado, o invasor pode acumular recompensas excessivas sem uma participação real proporcional.

```solidity
contract RewardPool {
    mapping(address => uint) public stakedAmount;
    mapping(address => uint) public rewardDebt;
    uint public totalStaked;
    uint public rewardRate;

    function stake(uint _amount) external {
        // Actualiza las recompensas pendientes antes de cambiar el estado
        updateRewards(msg.sender);
        stakedAmount[msg.sender] += _amount;
        totalStaked += _amount;
    }

    function withdraw(uint _amount) external {
        updateRewards(msg.sender);
        stakedAmount[msg.sender] -= _amount;
        totalStaked -= _amount;
    }

    function updateRewards(address _user) internal {
        uint pendingReward = stakedAmount[_user] * rewardRate;
        rewardDebt[_user] += pendingReward;
    }

    function claimRewards() external {
        updateRewards(msg.sender);
        uint reward = rewardDebt[msg.sender];
        rewardDebt[msg.sender] = 0;
        // Lógica para transferir la recompensa al usuario
    }
}
```

Neste exemplo, um atacante poderia depositar e retirar tokens rapidamente para maximizar o valor de `pendingReward` na função `updateRewards`, explorando uma vulnerabilidade na lógica de cálculo e acumulação das recompensas.

**Mitigação:**

1. **Implementar Medidas Anti-Sybil:** Uma técnica comum é limitar a frequência das interações dos usuários com o contrato, como definir um período mínimo entre depósitos e retiradas (_locking period_), ou aplicar taxas sobre transações que desencorajem comportamentos de entrada e saída repetitiva.
2.  **Cálculos Baseados em Tempo e Períodos Contínuos:** É fundamental que as recompensas sejam calculadas de forma contínua, levando em consideração o tempo em que os tokens permaneceram em _staking_, e não apenas eventos isolados de depósito ou saque. Isso evita que invasores obtenham recompensas indevidas ao realizar múltiplas transações em curto intervalo.

    ```solidity
    function updateRewards(address _user) internal {
        uint timeStaked = block.timestamp - lastUpdateTime[_user];
        uint pendingReward = stakedAmount[_user] * rewardRate * timeStaked;
        rewardDebt[_user] += pendingReward;
        lastUpdateTime[_user] = block.timestamp;
    }
    ```
3. **Aplicar um Processo de&#x20;**_**Slashing**_**:** Introduzir penalidades (_slashing_) para retiradas antes de um tempo mínimo de permanência em _staking_. Isso desincentiva ataques baseados em depósitos e saques rápidos com o objetivo de explorar o sistema de recompensas.
4. **Auditorias de Segurança:** Conduzir auditorias completas para identificar e corrigir falhas na lógica de recompensas. As auditorias devem incluir testes de estresse e simulações de uso malicioso para revelar possíveis vetores de ataque.
5. **Monitoramento e Ajuste de Parâmetros Dinâmicos:** Implantar mecanismos automáticos que monitorem o comportamento dos usuários e ajustem dinamicamente taxas de recompensa, limites de retirada, ou regras de participação em resposta a atividades suspeitas ou fora do padrão.
6. **Mecanismos de recompensa baseados no histórico do usuário:** em vez de calcular as recompensas apenas com base no saldo atual, considerar todo o histórico do usuário, incluindo quanto tempo ele esteve participando e a quantidade total de participação durante esse período.
