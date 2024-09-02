// SPDX-License-Identifier: GPL-3.0-or-later

pragma solidity ^0.8.0;

// Define a interface para o padrão ERC20
interface IERC20 {
    // Função para obter o total de tokens em circulação
    function totalSupply() external view returns (uint256);

    // Função para obter o saldo de uma conta
    function balanceOf(address account) external view returns (uint256);

    // Função para obter a autorização de um gastador por parte do proprietário
    function allowance(address owner, address spender) external view returns (uint256);

    // Função para transferir tokens do remetente para um destinatário
    function transfer(address recipient, uint256 amount) external returns (bool);

    // Função para aprovar um gastador a gastar uma certa quantidade de tokens em nome do remetente
    function approve(address spender, uint256 amount) external returns (bool);

    // Função para transferir tokens de uma conta para outra em nome do proprietário
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    // Evento emitido quando tokens são transferidos
    event Transfer(address indexed from, address indexed to, uint256 value);

    // Evento emitido quando uma autorização é definida
    event Approval(address indexed owner, address indexed spender, uint256 value);

    // Novo evento emitido quando tokens são queimados
    event Burn(address indexed burner, uint256 value);
}

// Implementação do token ERC20
contract DANIELcoin is IERC20 {
    string public constant name = "Daniel Coin";  // Nome do token
    string public constant symbol = "Dan";     // Símbolo do token
    uint8 public constant decimals = 18;      // Número de casas decimais

    // Mapeamento de endereços para saldos
    mapping(address => uint256) private balances;

    // Mapeamento de endereços para mapeamento de autorizados e seus saldos permitidos
    mapping(address => mapping(address => uint256)) private allowed;

    // Total de tokens em circulação
    uint256 private totalSupply_;

    // Construtor para inicializar o contrato com o total de suprimento
    constructor() {
        totalSupply_ = 50 ether;  // Define o total de suprimento
        balances[msg.sender] = totalSupply_;  // Atribui o total de suprimento ao criador do contrato
    }

    // Função para obter o total de tokens em circulação
    function totalSupply() public view override returns (uint256) {
        return totalSupply_;
    }

    // Função para obter o saldo de uma conta
    function balanceOf(address _tokenOwner) public view override returns (uint256) {
        return balances[_tokenOwner];
    }

    // Função para transferir tokens do remetente para um destinatário
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        require(amount <= balances[msg.sender], "Saldo insuficiente");  // Verifica se o remetente tem saldo suficiente
        balances[msg.sender] -= amount;  // Deduz os tokens do saldo do remetente
        balances[recipient] += amount;  // Adiciona os tokens ao saldo do destinatário
        emit Transfer(msg.sender, recipient, amount);  // Emite o evento de transferência
        return true;
    }

    // Função para aprovar um gastador a gastar uma certa quantidade de tokens em nome do remetente
    function approve(address spender, uint256 amount) public override returns (bool) {
        allowed[msg.sender][spender] = amount;  // Define o valor permitido para o gastador
        emit Approval(msg.sender, spender, amount);  // Emite o evento de aprovação
        return true;
    }

    // Função para obter a autorização de um gastador por parte do proprietário
    function allowance(address owner, address spender) public view override returns (uint256) {
        return allowed[owner][spender];  // Retorna a quantidade de tokens que o gastador está autorizado a gastar
    }

    // Função para transferir tokens de uma conta para outra em nome do proprietário
    function transferFrom(address owner, address recipient, uint256 amount) public override returns (bool) {
        require(amount <= balances[owner], "Saldo insuficiente");  // Verifica se o proprietário tem saldo suficiente
        require(amount <= allowed[owner][msg.sender], "Autorizacao excedida");  // Verifica se a autorização é suficiente

        balances[owner] -= amount;  // Deduz os tokens do saldo do proprietário
        allowed[owner][msg.sender] -= amount;  // Deduz a autorização para o gastador
        balances[recipient] += amount;  // Adiciona os tokens ao saldo do destinatário

        emit Transfer(owner, recipient, amount);  // Emite o evento de transferência
        return true;
    }

    // Função para queimar tokens
    function burn(uint256 amount) public returns (bool) {
        require(amount <= balances[msg.sender], "Saldo insuficiente para queima");  // Verifica se o remetente tem saldo suficiente para queimar
        balances[msg.sender] -= amount;  // Deduz os tokens do saldo do remetente
        totalSupply_ -= amount;  // Reduz o total de tokens em circulação
        emit Burn(msg.sender, amount);  // Emite o evento de queima
        return true;
    }
}
