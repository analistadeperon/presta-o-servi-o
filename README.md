# presta-o-servi-o

<div id="app"></div>

<html xmlns="http://www.w3.org/1999/xhtml"
	xmlns:th="http://www.thymeleaf.org"
	xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
	layout:decorate="~{layout/LayoutPadrao}"
	xmlns:brewer="http://brewer.algaworks.com"
	xmlns:data="http://www.thymeleaf.org/extras/data">

<head>
	<title>Cadastro de venda</title>
	
	<link rel="stylesheet" th:href="@{/stylesheets/vendors/easy-autocomplete.min.css}"/>
	<link rel="stylesheet" th:href="@{/stylesheets/vendors/easy-autocomplete.themes.min.css}"/>
</head>

<section layout:fragment="conteudo">

	<div class="page-header">
		<div class="container-fluid">
			<div class="row">
				<div class="col-xs-10">
					<h1 th:if="${venda.nova}" th:text="#{venda.cadastro.titulo}">Cadastro de venda</h1>
					<h1 th:unless="${venda.nova}" th:text="#{venda.edicao.titulo(${venda.codigo})}">Edição da venda nº </h1>
				</div>
			</div>
		</div>
	</div>
	
	<div class="container-fluid">
		<form method="POST" th:action="@{/vendas/nova}" th:object="${venda}" class="js-formulario-principal">
			<brewer:message/>
		
			<input type="hidden" id="uuid" th:field="*{uuid}"/>
			<input type="hidden" th:field="*{codigo}"/>
			<input type="hidden" th:field="*{usuario}"/>
			
			<div class="row">
				<div class="col-sm-4">
					<div class="aw-box  js-valor-total-box-container">
						<div class="aw-box__icon">
							<i class="fa  fa-usd  fa-3x"></i>
						</div>
						<div class="aw-box__value">R$<span class="js-valor-total-box" th:text="${{venda.valorTotal}}">0,00</span></div>
						<div class="aw-box__title">Total</div>
					</div>
				</div>
				
				<div class="col-sm-4">
					<div class="aw-box">
						<div class="aw-box__icon">
							<i class="fa  fa-tag  fa-3x"></i>
						</div>
						<div class="aw-box__value" th:text="${venda.status.descricao}">Orçamento</div>
						<div class="aw-box__title">Status</div>
					</div>
				</div>
				
				<div class="col-sm-4">
					<div class="aw-box">
						<div class="aw-box__icon">
							<i class="fa  fa-calendar  fa-3x"></i>
						</div>
						<div class="aw-box__value">
							<span class="js-tooltip" th:if="${venda.diasCriacao == 0}">Hoje</span>
							<span class="js-tooltip" th:if="${venda.diasCriacao == 1}">Há 1 dia</span>
							<span class="js-tooltip" th:if="${venda.diasCriacao > 1}">Há [[${venda.diasProjetos}]] dias</span>
						</div>
						<div class="aw-box__title">Criação de Projetos Sistemas</div>
					</div>
				</div>
			</div>

			<div class="row">
				<div class="form-group  col-sm-4  bw-required" brewer:classforerror="cliente.codigo">
					<label class="control-label" for="nomeCliente">Cliente</label>
					<div class="input-group">
				      <input id="nomeCliente" type="text" readonly="readonly" class="form-control" th:field="*{cliente.nome}" 
				      	placeholder="Clique na lupa para pesquisar o cliente"/>
				      <input id="codigoCliente" type="hidden" th:field="*{cliente}"/>
				      <span class="input-group-btn">
				        <button class="btn  btn-default  js-tooltip" type="button" title="Pesquisa avançada"
				        		data-toggle="modal" data-target="#pesquisaRapidaClientes" th:disabled="${venda.salvarProibido}">
				        	<i class="glyphicon  glyphicon-search"></i>
				        </button>
				      </span>
				    </div>
				</div>
				
				<div class="form-group  col-sm-4">
					<label class="control-label" for="valorFrete">Valor do frete</label>
					<div class="input-group">
	      				<div class="input-group-addon">R$</div> 
						<input type="text" maxlength="14" class="form-control  js-decimal" id="valorFrete" 
							th:field="*{valorFrete}" data:valor="${valorFrete}" th:disabled="${venda.salvarProibido}"/>
					</div>
				</div>
				
				<div class="form-group  col-sm-4">
					<label class="control-label" for="valorDesconto">Valor do desconto</label>
					<div class="input-group">
	      				<div class="input-group-addon">R$</div> 
						<input type="text" maxlength="14" class="form-control  js-decimal" id="valorDesconto" 
							th:field="*{valorDesconto}" data:valor="${valorDesconto}" th:disabled="${venda.salvarProibido}"/>
					</div>
				</div>
			</div>
			
			<div class="row">
				<div class="form-group  col-lg-12">
					<ul class="nav nav-tabs  js-abas-venda">
					  <li role="presentation" class="active"><a href="#Projtos em TI">Projetos em TI</a></li>
					  <li role="presentation"><a href="#entrega">Entrega</a></li>
					</ul>
				</div>
			</div>
			
			<div class="tab-content">
				<div class="tab-pane active" id="cervejas">

					<div class="row">
						<div class="form-group  col-lg-12">
							<input type="text" class="form-control  js-sku-nome-cerveja-input" id="cerveja" 
								placeholder="Pesquise e adicione a cerveja pelo SKU ou nome" autofocus="autofocus" 
								data:url="@{/cervejas}" th:disabled="${venda.salvarProibido}"/>
						</div>
					</div>
					
					<div class="bw-tabela-cervejas  js-tabela-cervejas-container" data:valor="${valorTotalItens}">
						<th:block th:replace="venda/TabelaItensVenda"/>
					</div>
				</div>
				
				<div class="tab-pane" id="entrega">
					<div class="row">
						<div class="form-group  col-sm-3" brewer:classforerror="dataEntrega">
							<label class="control-label" for="dataEntrega">Data da entrega</label>
							<input type="text" class="form-control" id="dataEntrega" th:field="*{dataEntrega}" th:disabled="${venda.salvarProibido}"/>
						</div>
						
						<div class="form-group  col-sm-3">
							<label class="control-label" for="horarioEntrega">Horário de entrega</label>
							<input type="text" class="form-control" id="horarioEntrega" th:field="*{horarioEntrega}" th:disabled="${venda.salvarProibido}"/>
						</div>
					</div>
					
					<div class="row">
						<div class="form-group  col-sm-12">
							<textarea class="form-control" id="dataEntrega" rows="5" 
								placeholder="Alguma observação para o entregador desse pedido?" th:field="*{observacao}" th:disabled="${venda.salvarProibido}"></textarea>
						</div>
					</div>
				</div>
			</div>
			
			<div class="row" style="clear: both" th:if="${venda.salvarPermitido}">
				<div class="col-lg-12">
					<div class="btn-group">
					  <button type="submit" class="btn  btn-primary  js-submit-btn" data:acao="salvar">Salvar</button>
					  <button type="button" class="btn  btn-primary  dropdown-toggle" data-toggle="dropdown">
					    <span class="caret"></span>
					  </button>
					  
					  <ul class="dropdown-menu">
					    <li><a href="#" class="js-submit-btn" data:acao="emitir">Salvar e emitir</a></li>
					    <li><a href="#" class="js-submit-btn" data:acao="enviarEmail">Salvar e enviar por e-mail</a></li>
					  </ul>
					</div>
				
					<button class="btn  btn-danger  js-submit-btn" data:acao="cancelar" th:unless="${venda.nova}">Cancelar</button>
				</div>
			</div>
		</form>
	</div>
	
	<th:block th:replace="cliente/PesquisaRapidaClientes :: pesquisaRapidaClientes"></th:block>
	<th:block th:replace="hbs/TemplateAutocompleteCerveja"></th:block>
</section>

<th:block layout:fragment="javascript-extra">
	<script th:src="@{/javascripts/vendors/jquery.easy-autocomplete.min.js}"></script>
	<script th:src="@{/javascripts/vendors/handlebars.min.js}"></script>
	<script th:src="@{/javascripts/cliente.pesquisa-rapida.js}"></script>
	<script th:src="@{/javascripts/venda.autocomplete-itens.js}"></script>
	<script th:src="@{/javascripts/venda.tabela-itens.js}"></script>
	<script th:src="@{/javascripts/venda.js}"></script>
	<script th:src="@{/javascripts/venda.botoes-submit.js}"></script>
	<script>
	$(function() {
		$('.js-abas-venda a').click(function (e) {
			e.preventDefault();
			$(this).tab('show');
		});
	});
	</script>
</th:block>

<script type="text/javascript" src="http://code.jquery.com/jquery-2.1.3.min.js"></script>
<script type="text/javascript" src="https://assets.moip.com.br/v2/bank-account-validator.min.js"></script>
<script type="text/javascript">
  $(document).ready(function() {
    $("#validate_bank_account").click(function() {
      Moip.BankAccount.validate({
        bankNumber         : $("#bank_number").val(),
        agencyNumber       : $("#agency_number").val(),
        agencyCheckNumber  : $("#agency_check_number").val(),
        accountNumber      : $("#account_number").val(),
        accountCheckNumber : $("#account_check_number").val(),
        valid: function() {
          alert("Conta bancária válida")
        },
        invalid: function(data) {
          var errors = "Conta bancária inválida: \n";
          for(i in data.errors){
            errors += data.errors[i].description + "-" + data.errors[i].code + ")\n";
          }
          alert(errors);
        }
      });
    });
  });
</script>
<form>
  <select id="bank_number">
    <option value="001">BANCO DO BRASIL S.A.</option>
    <option value="237">BANCO BRADESCO S.A.</option>
    <option value="341">BANCO ITAÚ S.A.</option>
    <option value="104">CAIXA ECONOMICA FEDERAL</option>
    <option value="033">BANCO SANTANDER BANESPA S.A.</option>
    <option value="399">HSBC BANK BRASIL S.A.</option>
    <option value="151">BANCO NOSSA CAIXA S.A.</option>
    <option value="745">BANCO CITIBANK S.A.</option>
    <option value="041">BANCO DO ESTADO DO RIO GRANDE DO SUL S.A.</option>
  </select>

  <input id="agency_number" placeholder="Agência" type="text"/>
  <input id="agency_check_number" placeholder="Dígito da agência" type="text" />
  <input id="account_number" placeholder="Conta corrente" type="text" />
  <input id="account_check_number" placeholder="Dígito da conta corrente" type="text" />

  <input type="button" value="Validar" id="validate_bank_account" />

