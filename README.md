# POOexercice Python
basic banking system

from abc import ABC, abstractmethod

class Pessoa:
    def __init__(self, nome, idade):
        self._nome = nome
        self._idade = idade

        @property
        def nome(self):
            return self._nome

        @property
        def idade(self):
            return self._idade

class Cliente(Pessoa):
    def __init__(self, nome, idade):
        super().__init__(nome, idade)
        self.conta = None

    def inserir_conta(self, conta):
            self.conta = conta

class Conta(ABC):
    def __init__(self, agencia, conta, saldo):
        self.agencia = agencia
        self.conta = conta
        self. saldo = saldo

    def depositar(self, valor):
            self.saldo += valor
            self.detalhes()

    def detalhes(self):
            print(f'Agência: {self.agencia} '
                  f'Conta: {self.conta} '
                  f'Saldo: {self.saldo} ')

    @abstractmethod
    def sacar(self, valor): pass

class ContaPoupanca(Conta):
    def sacar(self, valor):
        if self.saldo < valor:
            print ('Saldo Insuficiente')
            return

        self.saldo -= valor
        self.detalhes()

class ContaCorrente(Conta):
    def __init__(self, agencia, conta, saldo, limite=100):
        super().__init__(agencia, conta, saldo)
        self.limite = limite

    def sacar(self, valor):
        if (self.saldo + self.limite) < valor:
            print('Saldo Insuficiente')
            return

        self.saldo -= valor
        self.detalhes()

class Banco:
    def __init__(self):
        self.agencias = [1111, 2222, 3333]
        self.clientes = []
        self.contas = []

    def inserir_clientes(self, cliente):
        self.clientes.append(cliente)

    def inserir_conta(self, conta):
        self.contas.append(conta)

    def autenticar(self, cliente):
        if cliente not in self.clientes:
            return False

        if cliente.conta not in self.contas:
            return False

        if cliente.conta.agencia not in self.agencias:
            return False

        return True

###########
banco = Banco()

cliente1 = Cliente ('Maira', 35)
cliente2 = Cliente ('Tiago', 50)
cliente3 = Cliente ('Sofia', 18)

conta1 = ContaCorrente(1111, 1052030, 600)
conta2 = ContaCorrente(2222, 1053078, 100)
conta3 = ContaPoupanca(3333, 1052089, 300)

cliente1.inserir_conta(conta1)
cliente2.inserir_conta(conta2)
cliente3.inserir_conta(conta3)

###########

banco.inserir_clientes(cliente1)
banco.inserir_conta(conta1)

banco.inserir_clientes(cliente2)
banco.inserir_conta(conta2)

if banco.autenticar(cliente1):
    cliente1.conta.depositar(40)
    cliente1.conta.sacar(60)
else:
    print('Cliente não autenticado.')

print('##############')

if banco.autenticar(cliente2):
    cliente2.conta.depositar(0)
    cliente2.conta.sacar(20)
else:
    print('Cliente não autenticado.')
