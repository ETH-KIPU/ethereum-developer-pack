# Falta de Controle de Acesso (Missing Access Control)

Essa vulnerabilidade ocorre quando funções críticas dentro de um contrato inteligente não estão devidamente protegidas por restrições de acesso. Isso permite que qualquer usuário, mesmo não autorizado, execute essas funções, o que pode resultar na manipulação de dados do contrato, apropriação indevida de fundos ou interrupção do seu funcionamento.

**Exemplo:**\
Imagine um contrato inteligente que gerencia um fundo comum, onde várias pessoas podem depositar e sacar recursos. O contrato possui uma função `withdrawAllFunds` que permite retirar todos os fundos e transferi-los para um endereço específico.

Se essa função não for protegida por um controle de acesso, qualquer usuário poderá chamá-la e indicar seu próprio endereço, retirando todos os fundos do contrato sem restrições — o que resultaria na perda de todos os recursos depositados por outros usuários.

```solidity
contract VulnerableContract {
    address public owner;
    uint public totalFunds;

    constructor() {
        owner = msg.sender;
    }

    function deposit() external payable {
        totalFunds += msg.value;
    }

    function withdrawAllFunds(address payable _to) external {
        require(totalFunds > 0, "No funds available");
        _to.transfer(totalFunds);
        totalFunds = 0;
    }
}
```

Neste exemplo, a função `withdrawAllFunds` não possui nenhum controle de acesso, portanto qualquer pessoa pode executá-la, mesmo que não seja o proprietário do contrato.

**Mitigação:**

1.  **Implementar Modificadores de Acesso:**\
    A solução mais direta é aplicar modificadores que restrinjam o acesso a funções críticas apenas a usuários autorizados, como o dono do contrato ou administradores específicos. Em Solidity, isso pode ser feito com um modificador `onlyOwner`:

    ```solidity
    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    function withdrawAllFunds(address payable _to) external onlyOwner {
        require(totalFunds > 0, "No funds available");
        _to.transfer(totalFunds);
        totalFunds = 0;
    }
    ```

    Com esse modificador, apenas o proprietário do contrato poderá chamar `withdrawAllFunds`.
2. **Usar Bibliotecas de Controle de Acesso:** Utilize bibliotecas bem testadas e auditadas, como [`OpenZeppelin AccessControl`](https://docs.openzeppelin.com/contracts/2.x/access-control), para gerenciar permissões de forma segura e estruturada. Essas bibliotecas permitem a criação de papéis (roles) e atribuição de permissões específicas a cada função.
3. **Testes e Simulações:** Realize testes rigorosos que simulem tentativas de acesso não autorizado a funções críticas. Isso garante que os mecanismos de segurança estejam funcionando corretamente e evita falhas em ambientes reais.
