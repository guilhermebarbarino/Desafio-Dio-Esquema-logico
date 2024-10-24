---

### Passo 1: Mapeamento do Esquema ER para Relacional
1. **Identificar Entidades e Relacionamentos**: A partir do esquema ER, identifique todas as entidades e seus relacionamentos. Exemplo:
   - **Entidades**: Cliente, Carro, Serviço.
   - **Relacionamentos**: Cliente possui Carro, Carro recebe Serviço.

2. **Definição de Chaves Primárias e Estrangeiras**:
   - Cada entidade deve ter uma chave primária.
   - Os relacionamentos se transformam em chaves estrangeiras ou tabelas associativas, se necessário.

---

### Passo 2: Criação do Script SQL para o Esquema de Banco de Dados
Abaixo, um exemplo de script para criar as tabelas baseadas no modelo relacional:

```sql
CREATE TABLE Cliente (
    cliente_id INT PRIMARY KEY,
    nome VARCHAR(100),
    telefone VARCHAR(15)
);

CREATE TABLE Carro (
    carro_id INT PRIMARY KEY,
    modelo VARCHAR(100),
    ano INT,
    cliente_id INT,
    FOREIGN KEY (cliente_id) REFERENCES Cliente(cliente_id)
);

CREATE TABLE Servico (
    servico_id INT PRIMARY KEY,
    descricao VARCHAR(255),
    preco DECIMAL(10, 2),
    carro_id INT,
    FOREIGN KEY (carro_id) REFERENCES Carro(carro_id)
);
```

---

### Passo 3: Persistência de Dados para Testes
Inserção de dados fictícios para testar as consultas:

```sql
INSERT INTO Cliente (cliente_id, nome, telefone) VALUES 
(1, 'João Silva', '11999999999'),
(2, 'Maria Oliveira', '21988888888');

INSERT INTO Carro (carro_id, modelo, ano, cliente_id) VALUES 
(1, 'Honda Civic', 2018, 1),
(2, 'Toyota Corolla', 2020, 2);

INSERT INTO Servico (servico_id, descricao, preco, carro_id) VALUES 
(1, 'Troca de óleo', 150.00, 1),
(2, 'Alinhamento e balanceamento', 200.00, 2);
```

---

### Passo 4: Queries SQL para Recuperação de Informações
Abaixo estão exemplos de queries SQL que cobrem todas as cláusulas solicitadas:

1. **Recuperação Simples com SELECT**:
   ```sql
   SELECT nome, telefone FROM Cliente;
   ```

2. **Filtro com WHERE**:
   ```sql
   SELECT * FROM Carro WHERE ano > 2018;
   ```

3. **Atributo Derivado**:
   ```sql
   SELECT descricao, preco, preco * 1.1 AS preco_com_imposto FROM Servico;
   ```

4. **Ordenação com ORDER BY**:
   ```sql
   SELECT * FROM Servico ORDER BY preco DESC;
   ```

5. **Filtro em Grupos com HAVING**:
   ```sql
   SELECT carro_id, SUM(preco) AS total_gasto
   FROM Servico
   GROUP BY carro_id
   HAVING total_gasto > 150;
   ```

6. **Junção entre Tabelas**:
   ```sql
   SELECT Cliente.nome, Carro.modelo, Servico.descricao, Servico.preco
   FROM Cliente
   JOIN Carro ON Cliente.cliente_id = Carro.cliente_id
   JOIN Servico ON Carro.carro_id = Servico.carro_id;
   ```

---

### Passo 5: Perguntas Respondidas pelas Consultas
1. **Quais clientes possuem carros mais novos que 2018?**
2. **Qual o valor total gasto em serviços por carro?**
3. **Quais serviços foram realizados e qual o valor final com imposto?**
4. **Qual é o cliente que mais gastou em serviços?**

---

**queries SQL específicas para responder às perguntas definidas no Passo 5**:

---

### 1. **Quais clientes possuem carros mais novos que 2018?**

```sql
SELECT Cliente.nome, Carro.modelo, Carro.ano
FROM Cliente
JOIN Carro ON Cliente.cliente_id = Carro.cliente_id
WHERE Carro.ano > 2018;
```

---

### 2. **Qual o valor total gasto em serviços por carro?**

```sql
SELECT Carro.modelo, SUM(Servico.preco) AS total_gasto
FROM Carro
JOIN Servico ON Carro.carro_id = Servico.carro_id
GROUP BY Carro.modelo;
```

---

### 3. **Quais serviços foram realizados e qual o valor final com imposto?**

```sql
SELECT Servico.descricao, Servico.preco, 
       Servico.preco * 1.1 AS preco_com_imposto
FROM Servico;
```

---

### 4. **Qual é o cliente que mais gastou em serviços?**

```sql
SELECT Cliente.nome, SUM(Servico.preco) AS total_gasto
FROM Cliente
JOIN Carro ON Cliente.cliente_id = Carro.cliente_id
JOIN Servico ON Carro.carro_id = Servico.carro_id
GROUP BY Cliente.nome
ORDER BY total_gasto DESC
LIMIT 1;
```

---

### 5. **Exemplo Adicional: Quais carros têm mais de um serviço realizado?**

```sql
SELECT Carro.modelo, COUNT(Servico.servico_id) AS qtd_servicos
FROM Carro
JOIN Servico ON Carro.carro_id = Servico.carro_id
GROUP BY Carro.modelo
HAVING COUNT(Servico.servico_id) > 1;
```

---
