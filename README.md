# FÊNIX CARGO
Repositório de sistema TMS para a Fênix Cargo.

O programa será dividido em 3 módulos, sendo eles EXECUTIVO, CLIENTES e PARCEIROS.

<details>
  <summary><b>Executivo/Operacional</b></summary>
  <br>
  Esse módulo é direcionado aos funcionários da empresa, sendo responsável pelo gerenciamento dos dados em sistema. Dividido em:
  
  - Cadastros: clientes, agentes, usuários, funcionários, serviços
  - Guarda Volume
  - Tabela
  - Cotação/Simulação
  - Minuta
  - Fatura
  - Despesas (mensais, agentes)
  - Documentação (certidões, minutas de embarque, declarações) -> integração com Google Drive
</details>

<details>
  <summary><b>Clientes</b></summary>
  <br>
  Esse módulo é direcionado aos clientes da empresa. Além de visualizar os dados associados a eles, os clientes também podem enviar solicitações pelo sistema. Dividido em:

  - Meus volumes (visualizar itens do guarda volume)
  - Cotação (envio de cotação)
  - Minuta (emissão e acompanhamento)
  - Fatura (pagamento online, extrato de pendencias e pagamentos)
  - Dados cadastrais (editar endereço, email, telefone)
</details>

<details>
  <summary><b>Parceiros</b></summary>
  <br>
  Esse módulo é direcionado para funcionários terceiros, como motoristas ou agentes para atualização do status da encomenda e acessarem seus relatórios de pagamento. Dividido em:
  
  - Minhas minutas (atualizar minuta, acompanhamento do histórico)
  - Cobranças/Pagamentos (pagamentos recebidos e pendentes)
  - Dados cadastrais (editar endereço, email, telefone, tabela de preço e dados de pagamento)
</details>


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
  - id_telefone (telefone + respContato)
  - classificação (clientes, agentes, usuários, funcionários, serviços)
  - id_endereço (pais, cep, estado, cidade, bairro, rua, numero, complemento, infoAdicionais)
  - id_dadosBancarios (id_formaPagamento('PIX, transferencia, boleto'), codPIX, tipoPIX, id_codBanco('codBanco, nomeBanco'), agencia, conta)
- guarda_volume
- tabela
  - id_tipoTabela (fenix, terceiros)
  - id_categoriaTabela (urgente, comum, exclusivo)
  - anexo
  - id_endereço_origem (pais, cep, estado, cidade)
  - id_endereço_destino (pais, cep, estado, cidade)
  - id_tarifas {
    - id_tipoTarifa (taxaMinima, excedente, taxaFixaKg)
    - id_moeda (real, dolar)
    - valorFrete
    - pesoInicial
    - pesoFinal
    - id_tipoPrazo (dias / horas)
    - prazoMinimo
    - prazoMaximo
    }
- taxas adicionais
  - descricao (Taxa interior, Seguro, Seguro Redespacho, Troca de gelo)
  - id_moeda (real, dolar)
  - valorTaxa
  - alcanceGeografico
  - condição/associado á: (valor NF, por km)
  - incluso (SEMPRE, QUANDO SELECIONADO)
- cotação
- minuta
- fatura
- despesas

</details>

<details>
<summary><b> Rotas </b></summary>

#### `GET` `/categoria`

Essa é a rota que será chamada quando o usuário quiser listar todos os perfis cadastrados.

## **Perfil de Acesso**

- Cliente
- Parceiros
- Operacional

</details>
