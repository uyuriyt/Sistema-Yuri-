# Sistema-Yuri-
CAIXA PRA BIDINHA 
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fechamento de Caixa</title>
    <!-- Carrega o Tailwind CSS para um design responsivo e moderno -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        /* Esconde as setas dos inputs de número */
        input[type="number"]::-webkit-inner-spin-button,
        input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        input[type="number"] {
            -moz-appearance: textfield;
        }
        /* Estilo para impressão, para garantir que apenas o modal seja impresso */
        @media print {
            body > *:not(#modal) {
                display: none;
            }
            body > #modal {
                display: flex !important;
                position: static;
                width: 100%;
                height: auto;
                background-color: transparent;
            }
            #modal > div {
                width: 100%;
                box-shadow: none;
            }
            #modalTitle, #modalContent {
                color: #000;
            }
            #modal .hidden-on-print {
                display: none !important;
            }
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">

    <!-- Container principal do aplicativo -->
    <div class="w-full max-w-lg p-6 bg-white rounded-xl shadow-lg border border-gray-200">
        <h1 class="text-2xl font-bold text-gray-800 text-center mb-6">Fechamento de Caixa</h1>
        <p class="text-sm text-gray-500 text-center mb-6">
            Adicione cada venda, despesa e troco do dia para um controle completo.
        </p>

        <!-- Saldo Inicial -->
        <div class="space-y-4 mb-6">
            <div>
                <label for="saldoInicial" class="block text-gray-700 font-medium mb-1">Saldo Inicial (Dinheiro no Caixa)</label>
                <div class="relative rounded-md shadow-sm">
                    <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                        <span class="text-gray-500">R$</span>
                    </div>
                    <input type="number" id="saldoInicial" class="form-input block w-full pl-9 pr-12 py-2 rounded-md border-gray-300 focus:border-blue-500 focus:ring focus:ring-blue-200" placeholder="0,00" step="0.01" disabled>
                    <button id="btnToggleSaldo" class="absolute inset-y-0 right-0 pr-3 flex items-center text-gray-400 hover:text-gray-600 focus:outline-none focus:text-gray-600">
                        <svg id="lock-icon" xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                            <path fill-rule="evenodd" d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z" clip-rule="evenodd" />
                        </svg>
                    </button>
                </div>
            </div>
        </div>

        <!-- Seção para adicionar vendas individualmente -->
        <div class="p-4 bg-gray-50 rounded-lg border border-gray-200 mb-6">
            <h3 class="text-base font-semibold text-gray-700 mb-3">Adicionar Vendas do Dia</h3>
            <div id="vendaError" class="text-red-500 text-sm mb-2 hidden"></div>
            <div class="flex items-end space-x-2">
                <div class="flex-1">
                    <label for="inputValorVenda" class="block text-gray-600 text-sm font-medium mb-1">Valor da Venda</label>
                    <div class="relative rounded-md shadow-sm">
                        <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                            <span class="text-gray-500">R$</span>
                        </div>
                        <input type="number" id="inputValorVenda" class="form-input block w-full pl-9 pr-2 py-2 rounded-md border-gray-300 focus:border-blue-500 focus:ring focus:ring-blue-200" placeholder="0,00" step="0.01">
                    </div>
                </div>
                <div class="flex-1">
                    <label for="selectTipoPagamento" class="block text-gray-600 text-sm font-medium mb-1">Pagamento</label>
                    <select id="selectTipoPagamento" class="block w-full py-2 px-3 rounded-md border-gray-300 focus:border-blue-500 focus:ring focus:ring-blue-200 shadow-sm">
                        <option value="dinheiro">Dinheiro</option>
                        <option value="pix">Pix</option>
                        <option value="cartao">Cartão</option>
                    </select>
                </div>
                <button id="btnAdicionarVenda" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition duration-200">
                    +
                </button>
            </div>
            <!-- Campo para o valor recebido, visível apenas para pagamento em dinheiro -->
            <div id="valorRecebidoDiv" class="mt-4 hidden">
                <label for="inputValorRecebido" class="block text-gray-600 text-sm font-medium mb-1">Valor Recebido (Cliente)</label>
                <div class="relative rounded-md shadow-sm">
                    <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                        <span class="text-gray-500">R$</span>
                    </div>
                    <input type="number" id="inputValorRecebido" class="form-input block w-full pl-9 pr-2 py-2 rounded-md border-gray-300 focus:border-blue-500 focus:ring focus:ring-blue-200" placeholder="0,00" step="0.01">
                </div>
            </div>
        </div>

        <!-- Seção de Saldos e Listas -->
        <div class="space-y-4 mb-6">
            <div class="flex justify-between items-center p-3 rounded-lg bg-yellow-100 shadow-sm">
                <p class="text-sm font-semibold text-yellow-800">Saldo Fictício (Vendas do Dia):</p>
                <span id="saldoFicticio" class="text-xl font-bold text-yellow-800">R$ 0,00</span>
            </div>
            
            <div class="bg-gray-50 rounded-lg p-4 max-h-40 overflow-y-auto border border-gray-200">
                <h3 class="text-sm font-semibold text-gray-600 mb-2">Detalhes das Vendas:</h3>
                <ul id="listaVendas" class="space-y-1 text-sm text-gray-800">
                    <!-- Vendas serão adicionadas aqui via JavaScript -->
                </ul>
            </div>
        </div>
        
        <!-- Adicionar Troco Avulso -->
        <div class="p-4 bg-gray-50 rounded-lg border border-gray-200 mb-6">
            <h3 class="text-base font-semibold text-gray-700 mb-3">Adicionar Troco (Avulso)</h3>
            <div class="flex items-end space-x-2">
                <div class="flex-1">
                    <label for="inputTrocoAvulso" class="block text-gray-600 text-sm font-medium mb-1">Valor</label>
                    <div class="relative rounded-md shadow-sm">
                        <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                            <span class="text-gray-500">R$</span>
                        </div>
                        <input type="number" id="inputTrocoAvulso" class="form-input block w-full pl-9 pr-2 py-2 rounded-md border-gray-300 focus:border-blue-500 focus:ring focus:ring-blue-200" placeholder="0,00" step="0.01">
                    </div>
                </div>
                <button id="btnAdicionarTrocoAvulso" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition duration-200">
                    +
                </button>
            </div>
            <div class="bg-gray-50 rounded-lg p-4 max-h-40 overflow-y-auto mt-4 border border-gray-200">
                <h3 class="text-sm font-semibold text-gray-600 mb-2">Detalhes do Troco Avulso:</h3>
                <ul id="listaTrocoAvulso" class="space-y-1 text-sm text-gray-800">
                    <!-- Trocos avulsos serão adicionados aqui via JavaScript -->
                </ul>
            </div>
        </div>

        <!-- Total de Despesas -->
        <div class="space-y-4 mb-6">
            <div>
                <label for="totalDespesas" class="block text-gray-700 font-medium mb-1">Total de Despesas do Dia</label>
                <div class="relative rounded-md shadow-sm">
                    <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                        <span class="text-gray-500">R$</span>
                    </div>
                    <input type="number" id="totalDespesas" class="form-input block w-full pl-9 pr-2 py-2 rounded-md border-gray-300 focus:border-blue-500 focus:ring focus:ring-blue-200" placeholder="0,00" step="0.01">
                </div>
            </div>
        </div>

        <!-- Contagem Física de Dinheiro -->
        <div class="mt-4 mb-6">
            <label for="contagemDinheiro" class="block text-gray-700 font-medium mb-1">Contagem Física de Dinheiro (Final)</label>
            <div class="relative rounded-md shadow-sm">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                    <span class="text-gray-500">R$</span>
                    </div>
                    <input type="number" id="contagemDinheiro" class="form-input block w-full pl-9 pr-2 py-2 rounded-md border-gray-300 focus:border-blue-500 focus:ring focus:ring-blue-200" placeholder="0,00" step="0.01">
                </div>
            </div>

            <!-- Seção de botões de ação -->
            <div class="mt-6 flex justify-between space-x-2">
                <button id="btnSalvarDia" class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2 transition duration-200">
                    Fechar Caixa e Salvar
                </button>
            </div>

            <!-- Seção de histórico -->
            <div class="mt-6 p-4 rounded-lg bg-gray-50 border border-gray-200">
                <div class="flex justify-between items-center mb-3">
                    <h3 class="text-base font-semibold text-gray-700 text-center">Histórico de Fechamentos</h3>
                    <button id="btnLimparHistorico" class="text-sm text-red-500 hover:text-red-700 font-medium focus:outline-none">
                        Limpar Histórico
                    </button>
                </div>
                <ul id="historicoList" class="space-y-2">
                    <!-- Itens do histórico serão adicionados aqui via JavaScript -->
                </ul>
            </div>
        </div>

        <!-- Modal para senha -->
        <div id="passwordModal" class="fixed inset-0 bg-gray-900 bg-opacity-50 flex items-center justify-center p-4 hidden">
            <div class="bg-white rounded-xl shadow-lg p-6 w-full max-w-sm">
                <h3 id="passwordModalTitle" class="text-xl font-bold text-gray-800 text-center mb-4"></h3>
                <div class="space-y-4">
                    <input type="password" id="passwordInput" class="form-input w-full px-4 py-2 rounded-md border-gray-300 focus:border-blue-500 focus:ring focus:ring-blue-200" placeholder="Digite a senha">
                    <p id="passwordError" class="text-red-500 text-center hidden">Senha incorreta. Tente novamente.</p>
                    <div class="flex justify-between space-x-2">
                        <button id="passwordCancel" class="w-full py-2 px-4 rounded-lg text-gray-600 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-300">Cancelar</button>
                        <button id="passwordConfirm" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">Confirmar</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal para detalhes do histórico -->
        <div id="modal" class="fixed inset-0 bg-gray-900 bg-opacity-50 flex items-center justify-center p-4 hidden">
            <div class="bg-white rounded-xl shadow-lg p-6 w-full max-w-md">
                <div class="flex justify-between items-center mb-4 hidden-on-print">
                    <h3 id="modalTitle" class="text-xl font-bold text-gray-800"></h3>
                    <button id="closeModal" class="text-gray-500 hover:text-gray-700">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                </div>
                <div id="modalContent" class="space-y-2 text-sm text-gray-700">
                    <!-- Conteúdo do relatório vai aqui -->
                </div>
                <div class="mt-6 text-center hidden-on-print">
                    <button id="btnGerarPDF" class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2 transition duration-200">
                        Gerar Relatório em PDF
                    </button>
                </div>
            </div>
        </div>

        <script>
            document.addEventListener('DOMContentLoaded', () => {
                const saldoInicialInput = document.getElementById('saldoInicial');
                const btnToggleSaldo = document.getElementById('btnToggleSaldo');
                const lockIcon = document.getElementById('lock-icon');
                const inputValorVenda = document.getElementById('inputValorVenda');
                const inputValorRecebido = document.getElementById('inputValorRecebido');
                const valorRecebidoDiv = document.getElementById('valorRecebidoDiv');
                const selectTipoPagamento = document.getElementById('selectTipoPagamento');
                const btnAdicionarVenda = document.getElementById('btnAdicionarVenda');
                const listaVendas = document.getElementById('listaVendas');
                const vendaErrorDiv = document.getElementById('vendaError');
                
                const inputTrocoAvulso = document.getElementById('inputTrocoAvulso');
                const btnAdicionarTrocoAvulso = document.getElementById('btnAdicionarTrocoAvulso');
                const listaTrocoAvulso = document.getElementById('listaTrocoAvulso');
                
                const totalDespesasInput = document.getElementById('totalDespesas');
                const contagemDinheiroInput = document.getElementById('contagemDinheiro');

                const btnSalvarDia = document.getElementById('btnSalvarDia');
                const historicoList = document.getElementById('historicoList');
                const btnLimparHistorico = document.getElementById('btnLimparHistorico');
                
                const saldoFicticioSpan = document.getElementById('saldoFicticio');

                const modal = document.getElementById('modal');
                const modalTitle = document.getElementById('modalTitle');
                const modalContent = document.getElementById('modalContent');
                const closeModalBtn = document.getElementById('closeModal');
                const btnGerarPDF = document.getElementById('btnGerarPDF');
                
                const passwordModal = document.getElementById('passwordModal');
                const passwordModalTitle = document.getElementById('passwordModalTitle');
                const passwordInput = document.getElementById('passwordInput');
                const passwordError = document.getElementById('passwordError');
                const passwordConfirmBtn = document.getElementById('passwordConfirm');
                const passwordCancelBtn = document.getElementById('passwordCancel');

                const STORAGE_KEY_HISTORY = 'fechamento_caixa_historico';
                const STORAGE_KEY_SALDO_INICIAL = 'saldo_inicial_fixo';
                const PASSWORD = '0011';

                // Variáveis globais para o dia atual
                let estadoDiaAtual = {
                    vendasDinheiro: 0,
                    vendasPix: 0,
                    vendasCartao: 0,
                    totalTroco: 0,
                    vendas: [] // Armazena a lista de vendas detalhadas
                };

                const carregarSaldoInicial = () => {
                    const saldoSalvo = localStorage.getItem(STORAGE_KEY_SALDO_INICIAL);
                    if (saldoSalvo !== null) {
                        saldoInicialInput.value = parseFloat(saldoSalvo).toFixed(2);
                        saldoInicialInput.disabled = true;
                        lockIcon.setAttribute('fill', 'currentColor');
                    } else {
                        saldoInicialInput.disabled = false;
                        lockIcon.setAttribute('fill', 'none');
                    }
                };

                const formatarValor = (valor) => `R$ ${valor.toFixed(2).replace('.', ',')}`;

                // Alterna a visibilidade do campo de valor recebido
                selectTipoPagamento.addEventListener('change', () => {
                    if (selectTipoPagamento.value === 'dinheiro') {
                        valorRecebidoDiv.classList.remove('hidden');
                        // Removido o foco automático para evitar o fluxo que causava erro
                    } else {
                        valorRecebidoDiv.classList.add('hidden');
                        vendaErrorDiv.classList.add('hidden'); // Esconde erro se mudar o tipo
                    }
                });

                const carregarHistorico = () => {
                    const historicoSalvo = JSON.parse(localStorage.getItem(STORAGE_KEY_HISTORY) || '[]');
                    historicoList.innerHTML = '';
                    historicoSalvo.forEach((dia, index) => {
                        const li = document.createElement('li');
                        li.classList.add('flex', 'justify-between', 'items-center', 'p-3', 'rounded-lg', 'bg-white', 'shadow-sm', 'cursor-pointer', 'hover:bg-gray-100');
                        li.innerHTML = `
                            <span class="text-sm font-semibold text-gray-700">${dia.data}</span>
                            <span class="text-sm font-bold ${dia.diferenca < 0 ? 'text-red-500' : 'text-green-600'}">Diferença: ${formatarValor(dia.diferenca)}</span>
                        `;
                        li.addEventListener('click', () => abrirModal(dia));
                        historicoList.appendChild(li);
                    });
                };

                const abrirModal = (dia) => {
                    modalTitle.textContent = `Fechamento do dia ${dia.data}`;
                    
                    let content = `
                        <div class="flex justify-between items-center">
                            <span class="text-gray-600">Saldo Inicial:</span>
                            <span class="font-semibold">${formatarValor(dia.saldoInicial)}</span>
                        </div>
                        <div class="flex justify-between items-center">
                            <span class="text-gray-600">Vendas (Dinheiro):</span>
                
