![Desafio de Programação 1](https://github.com/user-attachments/assets/7bcaa2f8-9fcf-433b-96e5-1dc2c476ed86)

![Desafio de Programação 2](https://github.com/user-attachments/assets/59bca363-0ae0-4c7f-b51a-a0043e521739)

![Desafio de Programação 3](https://github.com/user-attachments/assets/51aaea02-eefd-4077-90ff-3b589073809d)

![Desafio de Programação 4](https://github.com/user-attachments/assets/ca8858f2-fe43-4538-9310-e30d33942a13)

![Desafio de Programação 5](https://github.com/user-attachments/assets/e3da765e-8304-4ac7-971a-29817e8f66e3)


Código utilizado para a realização do trabalho:
import csv

def menu():
    print("=== Menu ===")
    print("1. Buscar clientes pelo nome")
    print("2. Listar casos de um cliente")
    print("3. Detalhes de um caso específico")
    print("4. Despesa total")
    print("5. Receita total")
    print("6. Caso com a maior despesa")
    print("7. Caso com a maior receita")
    print("8. Exportar dados de um cliente")
    print("9. Sair")

def ler_registros():
    registros = []
    with open('registro.txt', 'r') as file:
        leitor = csv.reader(file)
        next(leitor)  # Pula o cabeçalho
        for linha in leitor:
            registros.append({
                'nome': linha[0],
                'numero_caso': linha[1].strip(),
                'despesa': float(linha[2]),
                'receita': float(linha[3])
            })
    return registros

def buscar_clientes(nome_parcial, registros):
    nomes = {registro['nome'] for registro in registros if nome_parcial.lower() in registro['nome'].lower()}
    return list(nomes)

def listar_casos_cliente(nome, registros):
    casos = [(r['numero_caso'], r['despesa'], r['receita']) for r in registros if r['nome'] == nome]
    return casos

def detalhes_caso(numero_caso, registros):
    for r in registros:
        if r['numero_caso'] == numero_caso:
            return r
    return None

def calcular_despesa_total(registros):
    return sum(r['despesa'] for r in registros)

def calcular_receita_total(registros):
    return sum(r['receita'] for r in registros)

def caso_maior_despesa(registros):
    return max(registros, key=lambda r: r['despesa'])

def caso_maior_receita(registros):
    return max(registros, key=lambda r: r['receita'])

def exportar_dados_cliente(nome, registros):
    casos_cliente = listar_casos_cliente(nome, registros)
    if not casos_cliente:
        return None

    total_despesas = sum(c[1] for c in casos_cliente)
    total_receitas = sum(c[2] for c in casos_cliente)
    diferenca = total_receitas - total_despesas

    with open(f'{nome.replace(" ", "_")}_dados.txt', 'w') as file:
        file.write(f'Nome: {nome}\n')
        file.write('Casos:\n')
        for numero, despesa, receita in casos_cliente:
            file.write(f'Caso {numero}: Despesa = {despesa}, Receita = {receita}\n')
        file.write(f'Total de Despesas: {total_despesas}\n')
        file.write(f'Total de Receitas: {total_receitas}\n')
        file.write(f'Diferença: {diferenca}\n')

def main():
    registros = ler_registros()

    while True:
        menu()
        opcao = input("Escolha uma opção: ")

        if opcao == '1':
            nome_parcial = input("Digite parte do nome: ")
            clientes = buscar_clientes(nome_parcial, registros)
            print("Clientes encontrados:", clientes)
        
        elif opcao == '2':
            nome = input("Digite o nome completo do cliente: ")
            casos = listar_casos_cliente(nome, registros)
            print("Casos encontrados:", casos)

        elif opcao == '3':
            numero_caso = input("Digite o número do caso: ")
            detalhes = detalhes_caso(numero_caso, registros)
            if detalhes:
                print(f"Detalhes do caso {numero_caso}: {detalhes}")
            else:
                print("Caso não encontrado.")

        elif opcao == '4':
            total_despesa = calcular_despesa_total(registros)
            print(f"Despesa total: {total_despesa}")

        elif opcao == '5':
            total_receita = calcular_receita_total(registros)
            print(f"Receita total: {total_receita}")

        elif opcao == '6':
            maior_despesa = caso_maior_despesa(registros)
            print(f"Maior despesa: {maior_despesa}")

        elif opcao == '7':
            maior_receita = caso_maior_receita(registros)
            print(f"Maior receita: {maior_receita}")

        elif opcao == '8':
            nome = input("Digite o nome completo do cliente: ")
            exportar_dados_cliente(nome, registros)
            print("Dados exportados com sucesso.")

        elif opcao == '9':
            print("Saindo do programa...")
            break

        else:
            print("Opção inválida! Tente novamente.")

if __name__ == "__main__":
    main()

