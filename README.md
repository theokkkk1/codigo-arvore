class No:
    def __init__(self, valor):
        self.valor = valor
        self.esquerda = None
        self.direita = None

class Contato:
    def __init__(self, nome, telefone):
        self.nome = nome
        self.telefone = telefone

class ArvoreBinaria:
    def __init__(self):
        self.raiz = None

    def inserir_contato(self, nome, telefone):
        if self.raiz is None:
            self.raiz = No(Contato(nome, telefone))
        else:
            self._inserir_recursivamente(self.raiz, Contato(nome, telefone))

    def _inserir_recursivamente(self, no, contato):
        if contato.nome < no.valor.nome:
            if no.esquerda is None:
                no.esquerda = No(contato)
            else:
                self._inserir_recursivamente(no.esquerda, contato)
        elif contato.nome > no.valor.nome:
            if no.direita is None:
                no.direita = No(contato)
            else:
                self._inserir_recursivamente(no.direita, contato)
        else:
            # Contato já existe na lista, você pode decidir o que fazer aqui
            pass

    def buscar_contato(self, nome):
        return self._buscar_recursivamente(self.raiz, nome)

    def _buscar_recursivamente(self, no, nome):
        if no is None:
            return None
        if nome == no.valor.nome:
            return no.valor.telefone
        elif nome < no.valor.nome:
            return self._buscar_recursivamente(no.esquerda, nome)
        else:
            return self._buscar_recursivamente(no.direita, nome)

    def remover_contato(self, nome):
        self.raiz = self._remover_recursivamente(self.raiz, nome)

    def _remover_recursivamente(self, no, nome):
        if no is None:
            return no

        if nome < no.valor.nome:
            no.esquerda = self._remover_recursivamente(no.esquerda, nome)
        elif nome > no.valor.nome:
            no.direita = self._remover_recursivamente(no.direita, nome)
        else:
            # Encontrou o nó a ser removido
            if no.esquerda is None:
                return no.direita
            elif no.direita is None:
                return no.esquerda
            else:
                # Nó com dois filhos: substitui pelo menor na subárvore direita
                temp = self._encontrar_minimo(no.direita)
                no.valor = temp.valor
                no.direita = self._remover_recursivamente(no.direita, temp.valor.nome)

        return no

    def _encontrar_minimo(self, no):
        while no.esquerda is not None:
            no = no.esquerda
        return no

    def listar_contatos_ordem_alfabetica(self):
        contatos = []
        self._in_order_traversal(self.raiz, contatos)
        return contatos

    def _in_order_traversal(self, no, lista_contatos):
        if no is not None:
            self._in_order_traversal(no.esquerda, lista_contatos)
            lista_contatos.append(no.valor)
            self._in_order_traversal(no.direita, lista_contatos)

    def lista_vazia(self):
        return self.raiz is None

# Exemplo de uso
arvore_contatos = ArvoreBinaria()
arvore_contatos.inserir_contato("Joao", "123456789")
arvore_contatos.inserir_contato("Maria", "987654321")
arvore_contatos.inserir_contato("Theo", "992661231")

print("Lista de contatos em ordem alfabética:")
for contato in arvore_contatos.listar_contatos_ordem_alfabetica():
    print(contato.nome, contato.telefone)

print("Buscar contato:")
print(arvore_contatos.buscar_contato("Maria"))  # Saída: 987654321

print("Remover contato:")
arvore_contatos.remover_contato("Joao")

print("Lista de contatos após remoção:")
for contato in arvore_contatos.listar_contatos_ordem_alfabetica():
    print(contato.nome, contato.telefone)

print("Verificar se a lista de contatos está vazia:")
print(arvore_contatos.lista_vazia())  # Saída: False
