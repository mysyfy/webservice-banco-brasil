# Integração Boletos via webservice com o Banco do Brasil
Componente destinado a facilitar a integração com o sistema de cobrança do banco do brasil.

A partir de 2017, a rede bancária brasileira traz uma nova plataforma de geração Boletos de Cobrança Registrada, buscando uma maior agilidade e segurança para toda sociedade.

Conforme divulgado pela Febraban, a implantação deve ser totalmente concluída a partir de dezembro/2017.

A solução desenvolvida pelo Banco do Brasil é baseada em Web Services e utiliza o protocolo OAuth 2.0 para autenticação e autorização das requisições.

O código-fonte proposto foi desenvolvido em PHP segundo a especificação para consumir o Web Service fornecido pelo Banco do Brasil.

Começando...
Você precisa ter as seguintes bibliotecas do PHP instaladas para usar esta implementação.

## Requirements

Para que este pacote possa funcionar são necessários os seguintes requisitos do PHP e outros pacotes dos quais esse depende.

- PHP 7.x (recomendável PHP 7.2) 
- ext-curl
- ext-json
- ext-xml

## Intanciar a classe

```
//sandbox = HOMOLOGAÇÃO E production = PRODUÇÃO
$bb = new BancoBrasil('CLIENTID', 'CLIENTSECRET', 'DEVELOPERKEY', 'AMBIENTE');
```

## Gerar Token

```
$token = $bb->getTokenAcess();
```

## Registrar Boleto

```
$registro['numeroConvenio'] = '';
$registro['numeroCarteira'] = '17';
$registro['numeroVariacaoCarteira'] = '35';
$registro['codigoModalidade'] = '01'; //Identifica a característica dos boletos dentro das modalidades de cobrança existentes no banco. Domínio: 01 - SIMPLES; 04 - VINCULADA
$registro['dataEmissao'] = '30.03.2021'; //Data de emissão do boleto (formato "dd.mm.aaaaa").
$registro['dataVencimento'] = '31.03.2021'; //Data de vencimento do boleto (formato "dd.mm.aaaaa").
$registro['valorOriginal'] = '10'; //Valor de cobrança > 0.00, emitido em Real (formato decimal separado por "."). Valor do boleto no registro. Deve ser maior que a soma dos campos “VALOR DO DESCONTO DO TÍTULO” e “VALOR DO ABATIMENTO DO TÍTULO”, se informados. Informação não passível de alteração após a criação. No caso de emissão com valor equivocado, sugerimos cancelar e emitir novo boleto.
$registro['valorAbatimento'] = '0';
$registro['quantidadeDiasProtesto'] = '';
$registro['quantidadeDiasNegativacao'] = '';
$registro['orgaoNegativador'] = '';
$registro['indicadorAceiteTituloVencido'] = 'S';
$registro['numeroDiasLimiteRecebimento'] = '90';
$registro['codigoAceite'] = 'A';
$registro['codigoTipoTitulo'] = '2';
$registro['descricaoTipoTitulo'] = 'DUPLICATA MERCANTIL';
$registro['indicadorPermissaoRecebimentoParcial'] = 'N';
$registro['numeroTituloBeneficiario'] = '1';
$registro['campoUtilizacaoBeneficiario'] = 'UM TEXTO';
$registro['numeroTituloCliente'] = 'nossonumero';
$registro['mensagemBloquetoOcorrencia'] = '';
$registro['desconto']['tipo'] = '';
$registro['desconto']['dataExpiracao'] = '';
$registro['desconto']['porcentagem'] = '';
$registro['desconto']['valor'] = '';
$registro['segundoDesconto']['dataExpiracao'] = '';
$registro['segundoDesconto']['porcentagem'] = '';
$registro['segundoDesconto']['valor'] = '';
$registro['terceiroDesconto']['dataExpiracao'] = '';
$registro['terceiroDesconto']['porcentagem'] = '';
$registro['terceiroDesconto']['valor'] = '';
$registro['jurosMora']['tipo'] = '';
$registro['jurosMora']['porcentagem'] = '';
$registro['jurosMora']['valor'] = '';
$registro['multa']['tipo'] = '';
$registro['multa']['data'] = '';
$registro['multa']['porcentagem'] = '';
$registro['multa']['valor'] = '';
$registro['pagador']['tipoInscricao'] = '1';
$registro['pagador']['numeroInscricao'] = '';
$registro['pagador']['nome'] = '';
$registro['pagador']['endereco'] = '';
$registro['pagador']['cep'] = '';
$registro['pagador']['cidade'] = '';
$registro['pagador']['bairro'] = '';
$registro['pagador']['uf'] = '';
$registro['pagador']['telefone'] = '';
$registro['beneficiarioFinal']['tipoInscricao'] = '1';
$registro['beneficiarioFinal']['numeroInscricao'] = '';
$registro['beneficiarioFinal']['nome'] = '';
$registro['indicadorPix'] = 'S';

$registro_info = $bb->registerBoleto($registro);
```

## Consultar Boleto

```
$id_boleto = 'nossonumero';
$numeroConvenio = '';
$read = $bb->readBoleto($id_boleto, $numeroConvenio);
```

## Editar Boleto

```
$id_boleto = 'nossonumero';
$alter['numeroConvenio'] = '3128557';
$alter['indicadorNovaDataVencimento'] = 'S'; //Indica a intenção de atribuir nova data de vencimento ao boleto. Valores a informar: "S" -> Sim, desejo alterar "N" -> Não, não desejo alterar
$alter['alteracaoData']['novaDataVencimento'] = '01.04.2021'; //Data de vencimento do boleto (formato "dd.mm.aaaaa").
$alter['indicadorAtribuirDesconto'] = 'N'; //Indica a intenção de atribuir desconto ao boleto. Valores a informar: "S" -> Sim, desejo alterar "N" -> Não, não desejo alterar
$alter['desconto']['tipoPrimeiroDesconto'] = '';
$alter['desconto']['valorPrimeiroDesconto'] = '';
$alter['desconto']['percentualPrimeiroDesconto'] = '';
$alter['desconto']['dataPrimeiroDesconto'] = '';
$alter['desconto']['tipoSegundoDesconto'] = '';
$alter['desconto']['valorSegundoDesconto'] = '';
$alter['desconto']['percentualSegundoDesconto'] = '';
$alter['desconto']['dataSegundoDesconto'] = '';
$alter['desconto']['tipoTerceiroDesconto'] = '';
$alter['desconto']['valorTerceiroDesconto'] = '';
$alter['desconto']['percentualTerceiroDesconto'] = '';
$alter['desconto']['dataTerceiroDesconto'] = '';
$alter['indicadorAlterarDesconto'] = 'N';
$alter['alteracaoDesconto']['tipoPrimeiroDesconto'] = '';
$alter['alteracaoDesconto']['novoValorPrimeiroDesconto'] = '';
$alter['alteracaoDesconto']['novoPercentualPrimeiroDesconto'] = '';
$alter['alteracaoDesconto']['novaDataLimitePrimeiroDesconto'] = '';
$alter['alteracaoDesconto']['tipoSegundoDesconto'] = '';
$alter['alteracaoDesconto']['novoValorSegundoDesconto'] = '';
$alter['alteracaoDesconto']['novoPercentualSegundoDesconto'] = '';
$alter['alteracaoDesconto']['novaDataLimiteSegundoDesconto'] = '';
$alter['alteracaoDesconto']['tipoTerceiroDesconto'] = '';
$alter['alteracaoDesconto']['novoValorTerceiroDesconto'] = '';
$alter['alteracaoDesconto']['novoPercentualTerceiroDesconto'] = '';
$alter['alteracaoDesconto']['novaDataLimiteTerceiroDesconto'] = '';
$alter['indicadorAlterarDataDesconto'] = 'N';
$alter['alteracaoDataDesconto']['novaDataLimitePrimeiroDesconto'] = '';
$alter['alteracaoDataDesconto']['novaDataLimiteSegundoDesconto'] = '';
$alter['alteracaoDataDesconto']['novaDataLimiteTerceiroDesconto'] = '';
$alter['indicadorProtestar'] = 'N';
$alter['protesto']['quantidadeDiasProtesto'] = '';
$alter['indicadorSustacaoProtesto'] = 'N';
$alter['indicadorCancelarProtesto'] = 'N';
$alter['indicadorIncluirAbatimento'] = 'N';
$alter['abatimento']['valorAbatimento'] = '';
$alter['indicadorCancelarAbatimento'] = 'N';
$alter['alteracaoAbatimento']['novoValorAbatimento'] = '';
$alter['indicadorCobrarJuros'] = 'N';
$alter['juros']['tipoJuros'] = '';
$alter['juros']['valorJuros'] = '';
$alter['juros']['taxaJuros'] = '';
$alter['indicadorDispensarJuros'] = 'N';
$alter['indicadorCobrarMulta'] = 'N';
$alter['multa']['tipoMulta'] = '';
$alter['multa']['valorMulta'] = '';
$alter['multa']['dataInicioMulta'] = '';
$alter['multa']['taxaMulta'] = '';
$alter['indicadorDispensarMulta'] = 'N';
$alter['indicadorNegativar'] = 'N';
$alter['negativacao']['quantidadeDiasNegativacao'] = '';
$alter['negativacao']['tipoNegativacao'] = '';
$alter['indicadorAlterarSeuNumero'] = 'N';
$alter['alteracaoSeuNumero']['codigoSeuNumero'] = '';
$alter['indicadorAlterarEnderecoPagador'] = 'N';
$alter['alteracaoEndereco']['enderecoPagador'] = '';
$alter['alteracaoEndereco']['bairroPagador'] = '';
$alter['alteracaoEndereco']['cidadePagador'] = '';
$alter['alteracaoEndereco']['UFPagador'] = '';
$alter['alteracaoEndereco']['CEPPagador'] = '';
$alter['indicadorAlterarPrazoBoletoVencido'] = 'N';
$alter['alteracaoPrazo']['quantidadeDiasAceite'] = '';

$update = $bb->alterBoleto($id_boleto, $alter);
```

## Baixar/Cancelar Boleto

```
$baixar['numeroConvenio'] = 'convenio';
$id_boleto = 'nossonumero';
$down = $bb->baixaBoleto($id_boleto, $baixar);
```
