# FÊNIX CARGO
Repositório de sistema TMS para a Fênix Cargo.

O programa será dividido em 3 ambientes, sendo eles: operacional, cliente e stakeholders.

Em operacional deve conter:
- Cadastros: clientes, agentes, usuários, funcionários, serviços
- Guarda Volume
- Tabela
- Cotação/Simulação
- Minuta
- Fatura
- Despesas (mensais, agentes)
- Documentação (certidões, minutas de embarque, declarações) -> integração com Google Drive

Em cliente deve ter:
- Meus volumes (Guarda volume)
- Cotação
- Minuta
- Fatura

Em stakeholders deve ter:
- Minhas minutas (Minuta)
- Cobranças/Pagamentos

## **OPERACIONAL**
O banco de dados `tms` será criado para manipulação dos dados presentes em todos os ambientes.

## **Status Codes**

Abaixo, listamos os possíveis **_status codes_** esperados como resposta da API.

```javascript
// 200 (OK) = requisição bem sucedida
// 201 (Created) = requisição bem sucedida e algo foi criado
// 204 (No Content) = requisição bem sucedida, sem conteúdo no corpo da resposta
// 400 (Bad Request) = o servidor não entendeu a requisição pois está com uma sintaxe/formato inválido
// 401 (Unauthorized) = o usuário não está autenticado (logado)
// 403 (Forbidden) = o usuário não tem permissão de acessar o recurso solicitado
// 404 (Not Found) = o servidor não pode encontrar o recurso solicitado
// 500 (Internal Server Error) = erro inesperado do servidor
```

<details>
<summary><b>Banco de Dados</b></summary>
<br>

Criação de tabelas e colunas conforme abaixo em PostgreSQL:

- usuarios
  - id
  - email (campo único)
  - senha
  - id_perfilAcesso (Cliente, Parceiros, Operacional)
  - nivel de acesso (administrador, financeiro, comercial)
- pessoas (clientes, agentes, usuários, funcionários, serviços)
  - id
  - id_usuario
  - cpf/cnpj
  - rg/ie (RG só não obrigatório para cliente)
  - nome/razao social
  - telefone / whatsapp
  - classificação (clientes, agentes, usuários, funcionários, serviços)
  - id_endereço (pais, cep, estado, cidade, rua, numero, complemento, infoAdicionais)
  - id_dadosBancarios (id_formaPagamento('PIX, transferencia, boleto'), codPIX, tipoPIX, id_codBanco('codBanco, nomeBanco'), agencia, conta)
- guarda_volume
- tabela
- cotação
- minuta
- fatura
- despesas

</details>

<details>
<summary><b> Usuários </b></summary>

#### `GET` `/categoria`

Essa é a rota que será chamada quando o usuário quiser listar todos os perfis cadastrados.

## **Perfil de Acesso**

- Cliente
- Parceiros
- Operacional

</details>
