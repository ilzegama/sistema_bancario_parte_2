def menu():
    menu = """\n
*********Menu***********
[1]Depositar
[2]sacar
[3]Extrato
[4]Nova conta
[5]Novo usuario
[0]Sair
   """

def sacar(*, saldo, valor, extrato, limite, numero_saques, limite_saques):
    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_saques >= limite_saques

    if excedeu_saldo:
            print("Operação falhou! Você não tem saldo suficiente.")

    elif excedeu_limite:
            print("Operação falhou! O valor do saque excede o limite.")

    elif excedeu_saques:
            print("Operação falhou! Número máximo de saques excedido.")

    elif valor > 0:
            saldo -= valor
            extrato += f"Saque:\t\t R$ {valor:.2f}\n"
            numero_saques += 1
         
    else:
            print("Operação falhou! O valor informado é inválido.")
    return saldo, extrato
  
def depositar(saldo, valor, extrato, /):
        if valor > 0:
            saldo += valor
            extrato += f"Deposito: R$ {valor:.2f}\n"
            print("\n### Deposito realizado com sucesso! ###")
        else:
            print("\n*** Operação falhou! O valor informado é inválido. ***")
            return saldo, extrato

def exibir_extrato(saldo, /, *, extrato):
        print("\n================ EXTRATO ================")
        print("Não foram realizadas movimentações." if not extrato else extrato)
        print(f"\nSaldo: R$ {saldo:.2f}")
        print("==========================================")

def criar_conta(agencia, numero_conta, usuarios):
       CPF = input("Informe o CPF do usuário: ")
       usuario = filtrar_usuario(CPF, usuarios)

       if usuario:
            print("\n### Conta Cadastrada com Sucesso! ###")
            return {"agencia": agencia, "numero_conta": numero_conta, "usuario": usuario}
       
       print("\n@@@ Usuário não encontrado, criação de conta não concluído! @@@")
     

def cadastrar_usuario(usuarios):
    CPF = input("Informe o CPF (Somente numero): ")
    usuario = filtrar_usuario (CPF, usuarios)

    if usuario:
        print("\n*** Já existe usuario com este CPF! ***")
        return
    nome= input("informe o nome completo: ")
    data_nascimento = input("Informe a data de nascimento (dd-mm-aaaa): ")
    endereco = input("Informe o endereco (logradouro, nro - bairro - cidade/sigla estado):")

    usuarios.append({"nome": nome, "data_nascimento": data_nascimento, "CPF": CPF, "endereco": endereco})

    print("=== Usuário criado com Sucesso! ===")
       
 
def filtrar_usuario(CPF, usuarios):
  usuarios_filtrados = [usuario for usuario in usuarios if usuario ["CPF"] == CPF]
  return usuarios_filtrados[0] if usuarios_filtrados else None

  
       
def main():
     LIMITE_SAQUES = 3
     Agencia = "0001"

     saldo = 0
     limite = 500
     extrato = ""
     numero_saques = 0
     usuarios = []
     contas = []

     while True:

      opcao = input(menu)

      if opcao == "1":
        valor = float(input("Informe o valor do deposito: "))

        if valor > 0:
            saldo += valor
            extrato += f"Deposito: R$ {valor:.2f}\n"

        else:
            print("Operação falhou! O valor informado é inválido.")

      elif opcao == "2":
        valor = float(input("Informe o valor do saque: "))

        saldo, extrato = sacar(
            saldo=saldo,
            valor=valor,
            extrato=extrato,
            limite=limite,
            numero_saques=numero_saques,
            limite_saques=LIMITE_SAQUES, 
         )
        
      elif opcao == "3":
       exibir_extrato(saldo, extrato= extrato)

      elif opcao == "4":
          numero_conta = len(contas) + 1
          conta = criar_conta(Agencia, numero_conta, usuarios)
           
      elif opcao == "5":
          cadastrar_usuario(usuarios)
          
      elif opcao == "0":
         break

      else:
        print("Operação inválida, por favor selecione novamente a operação desejada.")


main()
