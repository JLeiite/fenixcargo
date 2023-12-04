# FÊNIX CARGO
Repositório de sistema TMS para a Fênix Cargo.

Tempo decorrido: 2h30

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

<details>
<summary><b>Banco de Dados</b></summary>
<br>

Criação de tabelas e colunas conforme abaixo em PostgreSQL:

- usuarios
  - id
  - id_pessoas.email
  - senha
  - id_perfilAcesso (Cliente, Parceiros, Operacional)
  - id_nivelAcesso (administrador, financeiro, comercial)
- pessoas
  - id
  - cpf/cnpj
  - rg/ie (RG só não obrigatório para cliente)
  - nome/razaoSocial
  - email (campo único)
  - id_telefone (telefone + respContato)
  - id_classificação (clientes, agentes/parceiros, usuários, funcionários, serviços, companhia, motorista)
  - id_endereço (pais, cep, estado, cidade, bairro, rua, numero, complemento, infoAdicionais)
  - id_dadosBancarios{
      - id_formaPagamento('PIX, transferencia, boleto')
      - codPIX
      - tipoPIX
      - id_codBanco('codBanco, nomeBanco')
      - agencia
      - numeroConta)}
- guarda_volume
  - id
  - id_pessoas
  - dataEntrada
  - dataSaída
  - id_enderecoOrigem
  - id_minutaEntrada.numeroMinuta
  - id_enderecoDestino
  - id_minutaSaida.numeroMinuta
  - valorNF
  - id_item{
    - descricaoItem (único)
    - id_armazem (nome, id_endereço)
    - posicaoItem
    - quantidadeItem
    - alturaItem
    - larguraItem
    - comprimentoItem
    - cubagemItem
    - pesoTotalItem
    - pesoCubadoItem
    - imagemAnexo}
- tabela
  - id
  - id_tipoTabela (fenix, terceiros)
  - id_pessoa.endereço (para tabela de terceiros)
  - id_categoriaTabela (urgente, comum, exclusivo)
  - anexo
  - id_localOrigem (pais, cep, estado, cidade)
  - id_localDestino (pais, cep, estado, cidade)
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
  - id
  - descricao (Taxa interior, Seguro, Seguro Redespacho, Troca de gelo, Area de risco)
  - id_moeda (real, dolar)
  - valorTaxa
  - alcanceGeografico (pais, cep, estado, cidade)
  - condição/associado á: (valor NF, por km)
  - incluso (SEMPRE, QUANDO SELECIONADO)
- cotação
  - id_clienteOrigem
  - id_clienteDestino
  - id_localOrigem (pais, cep, estado, cidade)
  - id_localDestino (pais, cep, estado, cidade)
  - tipoDocumento (NF, declaracao, CTE)
  - valorProduto
  - id_tipoEmbalagem (caixa papelão, isopor, sem embalagem)
  - pesoTotal
  - id_item{
    - quantidadeItem
    - alturaItem
    - larguraItem
    - comprimentoItem
    - pesoCubadoItem}
  - id_TarifaAdicional (muitos pra muitos)
  - id_cotacaoFrete{
    - id_categoriaFrete (urgente, comum, exclusivo)
    - taxaMinima
    - taxaFixa
    - excedente
    - seguro
    - distanciaCapital
    - taxaInterior
    - valorFrete}
  - id_anexo
- minuta
  - id
  - produto
  - valorProduto
  - pesoTotal
  - aprovadorFrete
  - valorFrete
  - codCotacao
  - id_clienteOrigem
  - id_clienteDestino
  - id_clientePagador
  - id_endereçoOrigem
  - id_endereçoDestino
  - id_tipoDeclaracao (NF, declaracao, CTE)
  - id_tipoEmbalagem (caixa papelão, isopor, sem embalagem)
  - id_item{
    - quantidadeItem
    - alturaItem
    - larguraItem
    - comprimentoItem
    - pesoCubadoItem}
  - id_anexo
- AtualizarMinuta
  - id
  - id_minuta
  - id_statusMinuta (aguardando coleta, saiu para coleta, coleta efetuada, aguardando liberação, em trânsito, saiu pra entrega, entrega efetuada)
  - data
  - cidade
  - nomeResponsavel
  - id_tipoDocumento (RG, CPF)
  - numeroDocumento
  - id_anexo
- AssociadoMinuta
  - id
  - id_minuta
  - id_pessoa {
    - nome/razao social
    - email (campo único)
    - id_telefone (telefone + respContato)
    - classificação (clientes, agentes/parceiros, usuários, funcionários, serviços, companhia, motorista)}
  - numeroRegistro
  - custo
  - data
  - id_anexo
- fatura
  - id
  - id_minutaFatura (COUNT *)
  - id_statusPagamento (aberto, pago, atrasado)
  - id_minuta{
    - clientePagador
    - aprovadorFrete
    - valorFrete
    - codCotacao}
  - id_formaPagamento (boleto, cartão de credito, PIX)
  - incluirJuros (sim/não)
  - valorJuros
  - incluirMulta (sim/nao)
  - valorMulta
  - id_banco
  - dataVencimento
  - dataEmissao
  - codigoBarrasBoleto
  - observacaoFatura
  - valorFatura
  - numeroDocumento (nº CTE, nº NFS)
  - id_anexo (CTE / NFS)
- despesas
  - id_associadoMinuta (id_pessoa{se houver buscar daqui}, custo, id_minuta)
  - id_pessoa {
    - nome/razaoSocial
    - email
    - id_telefone
    - id_classificação
    - id_dadosBancarios}
  - id_statusPagamento (aberto, pago, atrasado)
  - id_formaPagamento (boleto, cartão de credito, PIX)
  - id_banco
  - id_anexo
  - incluirJuros (sim/não)
  - valorJuros
  - incluirMulta (sim/nao)
  - valorMulta
  - dataVencimento
  - codigoBarrasBoleto
  - valorAPagar
  - descrição (numeroIdentificação)

</details>

<details>
<summary><b> Rotas </b></summary>

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

#### `GET` `/categoria`

Essa é a rota que será chamada quando o usuário quiser listar todos os perfis cadastrados.

## **Perfil de Acesso**

- Cliente
- Parceiros
- Operacional

</details>
