# Código Analisado: 

```
https://github.com/ESRicci26/JavaVendas/blob/master/src/br/com/projeto/dao/ClientesDAO.java
```

## DAO

`ANTES`

```JAVA
public class ClientesDAO {
    private Connection con;

    public ClientesDAO() {
        this.con = new ConnectionFactory().getConnection();
    }

    //Metodo cadastrarCliente
    public void cadastrarCliente(Clientes obj) {
        // código reduzido para apresentação
    }

    //Metodo AlterarCliente
    public void alterarCliente(Clientes obj) {
       // código reduzido para apresentação
    }

    //Metodo ExcluirCliente
    public void excluirCliente(Clientes obj) {
        // código reduzido para apresentação
    }

    //Metodo Listar Todos Clientes
    public List<Clientes> listarClientes() {
       // código reduzido para apresentação
    }

    //metodo consultaCliente por Nome
    public Clientes consultaPorNome(String nome) {
         // código reduzido para apresentação
    }

    //metodo busca Cliente  por Cpf
    public Clientes buscaporcpf(String cpf) {
        // código reduzido para apresentação
    }

    //Metodo buscarclientePorNome - retorna uma lista
    public List<Clientes> buscaClientePorNome(String nome) {
         // código reduzido para apresentação
    }

    //Busca Cep
    public Clientes buscaCep(String cep) {
         // código reduzido para apresentação
    }
}
```


### Resumo

`SOLID`

### (SRP - Princípio da Responsabilidade Única):

**Problema no Código Original:** A classe `ClientesDAO` possui responsabilidades múltiplas, incluindo interação com a GUI (`JOptionPane`) e lógica de acesso a dados.

**Melhoria:** Separação de responsabilidades, criação de métodos privados e introdução da interface `IClienteRepository` para definir claramente responsabilidades.

### (OCP - Princípio do Aberto/Fechado):

**Problema no Código Original:** Dificuldade de extensão sem modificações no código existente.

**Melhoria:** A refatoração permite que novos métodos ou funcionalidades sejam adicionados sem alterar o código existente.


### (ISP - Princípio da Segregação de Interfaces):

**Problema no Código Original:** A classe `ClientesDAO` não implementa explicitamente uma interface.

**Melhoria:** Introdução da interface `IClienteRepository` para definir apenas os métodos necessários à classe.

### (DIP - Princípio da Inversão de Dependência):

**Problema no Código Original:** Dependência direta de alto nível (`JOptionPane`).

**Melhoria:** Introdução da interface `IClienteRepository` para inverter as dependências relacionadas ao acesso a dados.

---

`GRASP`

### Information Expert (Perito da Informação):

**Melhoria:** A classe `ClientesDAO` continua sendo a especialista nas operações relacionadas ao cliente.

### Controller (Controlador):

**Problema no Código Original:** A lógica de controle e interação com a interface do usuário estava misturada com o acesso a dados.

**Melhoria:** Introdução da classe `ClienteController` para atuar como um controlador intermediário, separando as preocupações.

### Creator (Criador):

**Melhoria:** A classe `ClientesDAO` continua sendo responsável por criar instâncias de `Clientes`.


### Low Coupling (Baixo Acoplamento):

**Melhoria:** A introdução da interface `IClienteRepository` contribui para reduzir o acoplamento, isolando a lógica de acesso a dados.

`DEPOIS`

### 

------------
```IClienteRepository```

```Java
package br.com.projeto.repository;

import br.com.projeto.model.Clientes;

import java.sql.SQLException;
import java.util.List;

public interface IClienteRepository {
    void cadastrarCliente(Clientes obj) throws SQLException;
    void alterarCliente(Clientes obj) throws SQLException;
    void excluirCliente(Clientes obj) throws SQLException;
    List<Clientes> listarClientes() throws SQLException;
    Clientes consultaPorNome(String nome) throws SQLException;
    Clientes buscaporcpf(String cpf) throws SQLException;
    List<Clientes> buscaClientePorNome(String nome) throws SQLException;
    Clientes buscaCep(String cep);
}

```

- Interface IClienteRepository: Mantém a definição dos métodos relacionados ao acesso a dados.
- Agora estabelece um contrato para as operações básicas como cadastrar, alterar, excluir, listar, consultar por nome, buscar por CPF, buscar cliente por nome e buscar por CEP.

------------
```ClienteController```
```Java
// ClienteController.java
package br.com.projeto.controller;

import br.com.projeto.repository.IClienteRepository;
import br.com.projeto.model.Clientes;
import br.com.projeto.model.WebServiceCep;

import java.sql.SQLException;
import java.util.List;

public class ClienteController {

    private final IClienteRepository clienteRepository;

    public ClienteController(IClienteRepository clienteRepository) {
        this.clienteRepository = clienteRepository;
    }

    public void cadastrarCliente(Clientes obj) {
        try {
            clienteRepository.cadastrarCliente(obj);
        } catch (SQLException e) {
            // Lidar com exceções ou logar conforme necessário
        }
    }

    public void alterarCliente(Clientes obj) {
        try {
            clienteRepository.alterarCliente(obj);
        } catch (SQLException e) {
            // Lidar com exceções ou logar conforme necessário
        }
    }

    public void excluirCliente(Clientes obj) {
        try {
            clienteRepository.excluirCliente(obj);
        } catch (SQLException e) {
            // Lidar com exceções ou logar conforme necessário
        }
    }

    public List<Clientes> listarClientes() {
        try {
            return clienteRepository.listarClientes();
        } catch (SQLException e) {
            // Lidar com exceções ou logar conforme necessário
            return null;
        }
    }

    public Clientes consultaPorNome(String nome) {
        try {
            return clienteRepository.consultaPorNome(nome);
        } catch (SQLException e) {
            // Lidar com exceções ou logar conforme necessário
            return null;
        }
    }

    public Clientes buscaporcpf(String cpf) {
        try {
            return clienteRepository.buscaporcpf(cpf);
        } catch (SQLException e) {
            // Lidar com exceções ou logar conforme necessário
            return null;
        }
    }

    public List<Clientes> buscaClientePorNome(String nome) {
        try {
            return clienteRepository.buscaClientePorNome(nome);
        } catch (SQLException e) {
            // Lidar com exceções ou logar conforme necessário
            return null;
        }
    }

    public Clientes buscaCep(String cep) {
        return clienteRepository.buscaCep(cep);
    }
}

```

- Atua como um controlador intermediário entre a interface do usuário (ou outros sistemas) e o acesso a dados para clientes.

- Recebe instância de IClienteRepository no construtor, aplicando o princípio de Inversão de Dependência.

- Gerencia exceções provenientes das operações do DAO, proporcionando maior robustez.
Delega chamadas aos métodos apropriados da interface IClienteRepository


------------
```ClientesDAO.java```
```Java
// ClientesDAO.java
package br.com.projeto.dao;

import br.com.projeto.jdbc.ConnectionFactory;
import br.com.projeto.model.Clientes;
import br.com.projeto.model.WebServiceCep;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class ClientesDAO implements IClienteRepository {

    private final Connection con;

    public ClientesDAO() {
        this.con = new ConnectionFactory().getConnection();
    }

    @Override
    public void cadastrarCliente(Clientes obj) throws SQLException {
    }

    // Outros métodos da interface IClienteRepository...
}

```

- Implementa a interface  IClienteRepository, assumindo a responsabilidade de realizar operações no banco de dados relacionadas à entidade "Clientes".
- Mantém a lógica específica de acesso a dados, como cadastrar clientes, alterar, excluir, listar, consultar por nome, buscar por CPF, buscar cliente por nome e buscar por CEP.
- Utiliza PreparedStatement para prevenir SQL Injection no método cadastrarCliente.
Mantém a conexão com o banco de dados gerenciada pela ConnectionFactory.

-------------------------------
------------------------------


## Model

2º
- [x] **Análise Inicial**
    - [x] Identificação de violações dos padrões GRASP e princípios SOLID.
    - [x] Código original:
        ```java
            package br.com.projeto.model;
            /** Classe Clientes **/ 
            public class Clientes {
            private int id;
            private String nome;
            private String rg;
            private String cpf;
            private String email;
            private String telefone;
            private String celular;
            private String cep;
            private String endereco;
            private int numero;
            private String complemento;
            private String bairro;
            private String cidade;
            private String uf;   

            }
            /** Classe Fornecedores **/ 
            public class Fornecedores extends  Clientes {
                private String cnpj;
            }
            /** Classe Funcionarios **/ 
            public class Funcionarios extends Clientes {
                private String senha;
                private String cargo;
                private String nivel_acesso;
            }
        ```
   - [x] **Violações**
        - [x] Violação do Princípio GRASP: A violação do princípio de Informação Expert ocorre porque a classe `Clientes` está sendo usada como uma superclasse para `Fornecedores` e `Funcionarios`, contendo atributos que são específicos para essasa subclasses. Isso viola o princípio, pois `Clientes` não é especialista na informação relacionada aos fornecedores ou funcionários, resultando em uma classe que acumula responsabilidades além do necessário.

        - [x] Violação do Princípio SOLID: Single Responsibility: As classes `Clientes`, `Fornecedores` e `Funcionarios` possuem mais de uma responsabilidade. Isso torna o código menos flexível e difícil de testar.

        - [x] Violação do Princípio SOLID: Open-Closed: As classes Clientes, `Fornecedores` e `Funcionarios` estão fechadas para modificações e extensões. Isso torna o código menos adaptável a novas necessidades.
 [x] 
- [x] **Propostas de Ajustes**
- [x] Sugestões de refatoração e melhorias no código e na arquitetura.
    - Criar classes específicas para cada tipo de entidade:

        `Pessoa`: Responsável por informações básicas como nome, RG, CPF, email, telefone, celular e complemento. Também contém uma referência para a classe `Endereco`.
        `Endereco`: Responsável por informações relacionadas ao endereço, como CEP, rua, número, complemento, bairro, cidade e UF.
        `Cliente`: Herda de Pessoa e inclui informações específicas como número, bairro, cidade e UF.
        `Fornecedor`: Herda de Pessoa e inclui informações específicas como CNPJ.
        `Funcionario`: Herda de Pessoa e inclui informações específicas como senha, cargo e nível de acesso.

        - Mover responsabilidades para as classes específicas:
            Métodos relacionados à manipulação de dados específicos de cada tipo de entidade devem ser movidos para suas classes respectivas.
        Ex: Métodos para validação de CNPJ na classe Fornecedor.

        - Implementar interfaces para funcionalidades comuns:
            Se necessário, interfaces podem ser utilizadas para definir funcionalidades comuns entre as classes, como Autenticavel para login de Funcionários e Clientes.

- [x] **Impacto da Aplicação dos Padrões e Princípios**
    - [x] Avaliação do impacto das mudanças propostas na qualidade do sistema.

        -   Melhor coesão e acoplamento: As classes serão mais coesas, com responsabilidades bem definidas, e menos acopladas entre si, tornando o código mais modular e fácil de entender.
        -   Maior flexibilidade e extensibilidade: O código será mais flexível e adaptável a novas necessidades, facilitando a adição de novas funcionalidades e a modificação de funcionalidades existentes.
        -   Maior testabilidade: As classes com responsabilidades únicas serão mais fáceis de testar, aumentando a confiabilidade do código.
        -   Maior qualidade do código: A aplicação dos padrões e princípios resultará em um código mais limpo, organizado e fácil de manter, aumentando a qualidade geral do software.
    ---

### Link para o diagrama
 - https://drive.google.com/file/d/12MWWS1LrXjfmnduQxeVPBKHIpb6f8NdO/view?usp=drivesdk
