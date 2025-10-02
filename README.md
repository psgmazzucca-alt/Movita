<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Movita - Monte Seu Kebab e Saladas</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
body {
    font-family: 'Inter', sans-serif;
    background-color: #f4f7f9;
}
.section-title {
    /* Cor de título alterada para um tom de verde/oliva mais escuro */
    @apply text-xl font-bold border-b-2 pb-2 mb-4 text-[#4a5540] border-[#aec6a2];
}
.input-style {
    /* Cor de foco alterada para o verde/oliva */
    @apply w-full p-3 border border-gray-300 rounded-lg focus:ring-[#4a5540] focus:border-[#4a5540] transition duration-150;
}
/* Estilo para item desabilitado visualmente */
.acomp-option input:disabled + span,
.input-style:disabled {
    opacity: 0.6;
    cursor: not-allowed;
    background-color: #f3f4f6;
    text-decoration: line-through;
}
/* Estilo para fieldset desabilitado (opcional, para reforçar) */
fieldset:disabled {
    pointer-events: none; /* Impede cliques */
    opacity: 0.7; /* Suaviza a cor */
}

/* Adicionando sombra ao texto para que fique legível sobre o fundo */
.flag-text-shadow {
    text-shadow: 1px 1px 4px rgba(0,0,0,0.4);
}
/* Cor de fundo verde oliva claro */
.movita-bg {
    background-color: #aec6a2; /* Verde Oliva Claro */
}
/* ------------------------------------------------------------------ */
/* ESTILOS DE CARTÃO E MARCADORES (RADIO/CHECKBOX CUSTOMIZADOS) */
/* ------------------------------------------------------------------ */

/* Estilo Base para todos os itens de menu selecionáveis (Saladas, Adicionais, Molhos de Salada) */
.menu-item-card {
    @apply p-3 border border-gray-200 bg-white rounded-lg shadow-sm cursor-pointer transition duration-150 flex items-start; 
}
.menu-item-card.selected {
    @apply ring-2 ring-offset-1 ring-[#4a5540] border-[#4a5540] bg-[#f7fcf6];
}

/* Marcardor para Rádio (Saladas) */
.custom-radio-marker {
    @apply w-5 h-5 rounded-full border-2 border-gray-400 mr-3 mt-1 flex-shrink-0;
}
/* Estilo de seleção para o Rádio */
.menu-item-card.selected .custom-radio-marker {
    @apply border-[#4a5540] bg-[#4a5540] relative;
}
.menu-item-card.selected .custom-radio-marker::after {
    content: '';
    @apply absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-2 h-2 rounded-full bg-white;
}

/* NOVO: Marcador para Checkbox (Adicionais) */
.custom-checkbox-marker {
    @apply w-5 h-5 rounded border-2 border-gray-400 mr-2 flex-shrink-0 flex items-center justify-center;
}
/* Estilo de seleção para o Checkbox */
.menu-item-card.selected .custom-checkbox-marker {
    @apply border-blue-600 bg-blue-600 relative;
}
.menu-item-card.selected .custom-checkbox-marker::after {
    content: '✓';
    @apply text-white text-xs font-bold leading-none;
}
/* ------------------------------------------------------------------ */

</style>
</head>
<body class="min-h-screen p-4 flex justify-center">

<div id="app" class="w-full max-w-xl bg-white shadow-2xl rounded-xl space-y-6 md:p-0">
    <header class="text-center relative movita-bg rounded-t-xl py-6 shadow-2xl">
        <img src="https://i.postimg.cc/ht6ZFPyk/Black-White-Minimal-Modern-Simple-Bold-Business-Mag-Logo-1.png" 
             alt="Logo Movita" 
             class="w-40 h-40 object-cover mx-auto rounded-full mb-3 shadow-2xl border-4 border-white z-10 relative">
        <div class="px-2">
            <h1 class="text-4xl font-extrabold text-white sm:text-5xl flag-text-shadow leading-tight">MOVITA</h1>
            <p class="text-base text-white font-medium mt-1 flag-text-shadow">MOVIMENTO COM PROPÓSITO</p>
        </div>
    </header>

    <div class="p-6 space-y-6 md:p-8 pt-0">
        
        <section id="salad-order-section" class="space-y-6 p-4 border border-[#e1eef0] rounded-xl bg-[#f6fcf7]">
            <h2 class="section-title text-[#4a5540]">🥗 Escolha Sua Salada</h2>

            <div id="salad-options" class="space-y-3">
                </div>

            <div id="salad-details" style="display: none;" class="space-y-4 p-3 border rounded-lg bg-gray-50">
                 <h3 class="font-bold text-lg text-[#4a5540]">Personalize a <span id="selected-salad-name"></span>:</h3>
                 <p class="text-md font-semibold text-gray-700">Acompanha: <span id="selected-salad-molho" class="text-blue-600 font-bold">Molho Especial</span></p>
                 
                 <div class="space-y-2">
                    <label class="block font-semibold text-gray-700">➕ Adicionais (Opcional)</label>
                    <div id="adicional-options" class="grid grid-cols-2 gap-2">
                        </div>
                 </div>

                 <div class="space-y-2">
                    <label for="salad-obs" class="block font-semibold text-gray-700">Observações (Ex: Molho à parte, Sem cebola)</label>
                    <input type="text" id="salad-obs" class="input-style" placeholder="Digite aqui...">
                 </div>

                 <p class="text-2xl font-extrabold text-green-700 mt-4 text-right">
                    Preço da Salada: <span id="salad-price-display">R$ 0,00</span>
                 </p>

                 <button id="add-salad-to-cart-btn" onclick="addSaladToCart()" 
                    class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 rounded-lg shadow-md transition duration-300 transform hover:scale-[1.01] active:scale-[0.98]">
                    ADICIONAR SALADA AO CARRINHO
                 </button>
            </div>
        </section>
        
        <section id="custom-order-section" class="space-y-6 p-4 border border-[#e5f0e1] rounded-xl bg-[#f7fcf6]">
            <h2 class="section-title text-[#4a5540]">🥙 Monte Seu Kebab</h2>

            <div class="space-y-2">
                <label class="block font-semibold text-gray-700">📏 1. Escolha o Tamanho</label>
                <div id="size-options" class="flex space-x-4">
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div class="space-y-2">
                    <label class="block font-semibold text-gray-700">🥩 2. Escolha 1 Proteína (Obrigatório)</label>
                    <div id="protein-options" class="space-y-1"></div>
                </div>

                <div class="space-y-2">
                    <label class="block font-semibold text-gray-700">🧂 3. Escolha até 2 Molhos (Obrigatório)</label>
                    <div id="molho-options" class="space-y-1"></div>
                </div>
            </div>

            <div class="space-y-2">
                <label class="block font-semibold text-gray-700">🥗 4. Escolha até 4 Acompanhamentos (Obrigatório)</label>
                <p id="acomp-counter" class="text-sm font-medium text-[#4a5540]">Selecionados: 0/4</p>
                <div id="acomp-options" class="grid grid-cols-2 sm:grid-cols-3 gap-2"></div>
            </div>
            
            <div class="space-y-2">
                <label for="obs" class="block font-semibold text-gray-700 border-t pt-4">5. Observações (Ex: Molho a parte, Sem pimenta)</label>
                <input type="text" id="obs" class="input-style" placeholder="Digite aqui...">
                <p class="text-2xl font-extrabold text-green-700 mt-4 text-right">
                    Preço do Item: <span id="item-price-display">R$ 18,90</span>
                </p>
            </div>
            
            <button id="add-to-cart-btn" onclick="addToCart()" 
                class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 rounded-lg shadow-md transition duration-300 transform hover:scale-[1.01] active:scale-[0.98]">
                ADICIONAR PRATO AO CARRINHO E MONTAR OUTRO
            </button>
        </section>
        <section class="space-y-4 p-4 border border-gray-200 rounded-xl bg-gray-50">
            <h2 class="section-title text-gray-700 border-gray-200">🛒 Seu Pedido (<span id="cart-count">0</span> Pratos)</h2>
            <ul id="cart-list" class="space-y-3">
                <li id="empty-cart-message" class="text-gray-500 italic text-center">Nenhum item no carrinho.</li>
            </ul>
            <p class="text-3xl font-extrabold text-red-700 pt-3 border-t border-gray-300 text-right">
                SUBTOTAL: <span id="subtotal-display">R$ 0,00</span>
            </p>
        </section>

        <section id="checkout-section" class="space-y-6" style="display: none;">
            <h2 class="section-title">Dados de Entrega e Pagamento</h2>

            <div class="space-y-3">
                <label class="block font-semibold text-gray-700">Seus Dados:</label>
                <input type="text" id="nome" class="input-style" placeholder="Nome Completo *" required>
                <input type="tel" id="telefone" class="input-style" placeholder="Telefone (WhatsApp) *" required>
                <input type="text" id="endereco" class="input-style" placeholder="Endereço (Rua, Número, Bairro) *" required>
                <input type="text" id="referencia" class="input-style" placeholder="Ponto de Referência (Ex: Casa verde, ao lado da praça)">
            </div>

            <div class="space-y-2">
                <label class="block font-semibold text-gray-700">Taxa de Entrega</label>
                <div id="delivery-fee-options" class="flex flex-col space-y-2">
                    <label class="flex items-center p-2 bg-white rounded-lg shadow-sm cursor-pointer">
                        <input type="radio" name="deliveryFee" value="2.00" class="form-radio text-red-600" checked>
                        <span class="ml-2">Entrega na Cidade (R$ 2,00)</span>
                    </label>
                    <label class="flex items-center p-2 bg-white rounded-lg shadow-sm cursor-pointer">
                        <input type="radio" name="deliveryFee" value="5.00" class="form-radio text-red-600">
                        <span class="ml-2">Entrega até 5km (R$ 5,00)</span>
                    </label>
                </div>
            </div>
            
            <div class="space-y-2">
                <label class="block font-semibold text-gray-700">Forma de Pagamento</label>
                <select id="payment-method" class="input-style" onchange="toggleTrocoField()"> 
                    <option value="Pix">Pix (Informar na mensagem)</option>
                    <option value="Dinheiro">Dinheiro (Precisa de troco?)</option>
                    <option value="Cartao-Debito">Cartão de Débito</option>
                    <option value="Cartao-Credito">Cartão de Crédito</option>
                </select>
            </div>
            
            <div id="troco-input-container" class="space-y-2" style="display: none;">
                <label for="troco" class="block font-semibold text-gray-700">Precisa de Troco?</label>
                <input type="text" id="troco" class="input-style" placeholder="Troco para (Ex: R$ 50,00)">
                <p class="text-sm text-gray-500">Deixe em branco se for pagar o valor exato.</p>
            </div>


            <div class="mt-6 p-4 bg-red-100 rounded-lg shadow-inner">
                <p class="text-lg font-bold text-red-800">Subtotal Itens: <span id="final-subtotal">R$ 0,00</span></p>
                <p class="text-lg font-bold text-red-800">Taxa: <span id="final-fee">R$ 2,00</span></p>
                <p class="text-4xl font-extrabold text-red-900 mt-2">TOTAL FINAL: <span id="final-total">R$ 0,00</span></p>
            </div>

            <button id="checkout-btn" onclick="generateWhatsAppLink()" 
                class="w-full flex items-center justify-center bg-green-500 hover:bg-green-600 text-white font-extrabold py-4 rounded-lg shadow-xl transition duration-300 transform hover:scale-[1.01] active:scale-[0.98]">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor" class="mr-2">
                    <path d="M12.001 2c-5.522 0-9.999 4.477-9.999 9.999 0 3.864 2.193 7.224 5.434 8.937l-.547 1.884c-.053.183.018.384.183.454.16.07.362.003.415-.177l.568-1.859c.772.196 1.583.298 2.404.298 5.522 0 9.999-4.477 9.999-9.999s-4.477-9.999-9.999-9.999zm4.646 12.339l-.025.048c-.627 1.127-1.428 1.176-2.923 1.025-1.579-.164-3.56-1.077-5.118-2.635-1.558-1.558-2.471-3.539-2.635-5.118-.151-1.495-.102-2.296 1.025-2.923l.048-.025.109-.052c.245-.118.528-.108.763.027l1.559.866c.216.12.339.362.31.597l-.078.618c-.035.267-.168.513-.377.674l-.234.186c-.198.158-.178.431.042.616l2.365 2.365c.184.22.457.24.616.042l.186-.234c.161-.209.407-.342.674-.377l.618-.078c.235-.029.477.094.597.31l.866 1.559c.135.235.145.518.027.763l-.052.109z"/>
                </svg>
                ENVIAR PEDIDO VIA WHATSAPP
            </button>
        </section>
    </div>

</div>

<script>
    // --- DADOS DO CARDÁPIO (FONTE ÚNICA DE VERDADE) ---
    const MENU = {
        // Dados do KEBAB (Existentes)
        proteinas: ["Frango", "Carne", "Atum"], 
        molhos: ["Barbecue", "Maionese Verde", "Mostarda Amarela", "Ketchup", "Maionese de Alho", "Cream Ceese", "Molho Rosé"],
        maxMolhos: 2, 
        acompanhamentos: [
            "Alface", "Tomate", "Pepino", "Rúcula", "Cebola Roxa", 
            "Cebola Branca", "Repolho", "Beterraba", "Cenoura", "Milho", "Pimentão"
        ],
        tamanhos: [
            { id: 'P', nome: 'Pequeno', preco: 18.90, limite: 4 },
            { id: 'M', nome: 'Médio', preco: 21.90, limite: 4 },
            { id: 'G', nome: 'Grande', preco: 25.90, limite: 4 }
        ],
        
        // Dados das SALADAS (ATUALIZADO: Molho sem descrição no nome)
        saladas: [
            { id: 'origem', nome: 'ORIGEM', preco: 26.90, molho: 'Azeite & Balsâmico', detalhes: 'Macarrão penne, alface americano, tomate cereja, cebola roxa, milho, cenoura ralada, salsinha. Proteína: tiras de frango ou atum.' },
            { id: 'crockfit', nome: 'CROCK FIT', preco: 26.90, molho: 'Molho Caesar', detalhes: 'Alface americano, pepino, tomate cereja, parmesão ralado, croutons. Proteína: tiras de frango.' },
            { id: 'terramar', nome: 'TERRA E MAR', preco: 29.90, molho: 'Molho de Maracujá', detalhes: 'Camarão, manga, alface crespa, rúcula, pepino, tomate cereja, milho. Proteína: camarão.' },
            { id: 'tropicalia', nome: 'TROPICÁLIA (VEGETARIANA)', preco: 22.90, molho: 'Azeite & Limão', detalhes: 'Alface crespa, rúcula, tomate cereja, cebola roxa, morango.' },
            { id: 'chefefit', nome: 'CHEFE FIT', preco: 26.90, molho: 'Molho Pesto', detalhes: 'Mix de folhas, tomate cereja, brócolis grelhado ao alho, parmesão. Proteína: tiras de frango ou carne.' },
            { id: 'arcoiris', nome: 'ARCO-ÍRIS', preco: 23.90, molho: 'Maionese Temperada', detalhes: 'Alface crespa, tomate, repolho, beterraba ralada, cenoura ralada, milho. Proteína: ovos de codorna.' },
        ],
        adicionais: [
            { id: 'ad_arroz', nome: 'Arroz integral', preco: 4.00 },
            { id: 'ad_ovo', nome: 'Ovo cozido', preco: 3.00 },
            { id: 'ad_carne', nome: 'Tiras de carne', preco: 5.00 },
            { id: 'ad_frango', nome: 'Tiras de frango', preco: 5.00 },
            { id: 'ad_camarao', nome: 'Camarão', preco: 12.00 },
        ],
        
        // Dados de Contato (Existentes)
        whatsappNumber: "5517997381858",
        pixData: { 
            name: "SUÉLEM CRISTINA MAESTRE MAZZUCCA",
            key: "41756000867 (CPF)" 
        }
    };

    // --- ESTADO GLOBAL ---
    let cart = []; 
    // Variáveis Kebab (Existentes)
    let currentItemPrice = MENU.tamanhos[0].preco;
    let premiumPrice = 0; 
    let selectedAcompanhamentos = 0;
    let maxAcompanhamentos = MENU.tamanhos[0].limite;
    let selectedMolhos = 0;
    let maxMolhos = MENU.maxMolhos;
    
    // Variáveis Salada (NOVAS)
    let selectedSalad = null;
    let currentSaladPrice = 0;

    // --- REFERÊNCIAS DOM ---
    // Referências Kebab (Existentes)
    const sizeOptionsDiv = document.getElementById('size-options');
    const molhoOptionsDiv = document.getElementById('molho-options');
    const acompOptionsDiv = document.getElementById('acomp-options');
    const acompCounter = document.getElementById('acomp-counter');
    const itemPriceDisplay = document.getElementById('item-price-display');
    const proteinOptionsDiv = document.getElementById('protein-options');
    const addToCartBtn = document.getElementById('add-to-cart-btn');
    
    // Referências Salada (NOVAS)
    const saladOptionsDiv = document.getElementById('salad-options');
    const saladDetailsDiv = document.getElementById('salad-details');
    const selectedSaladName = document.getElementById('selected-salad-name');
    const selectedSaladMolho = document.getElementById('selected-salad-molho'); 
    const adicionalOptionsDiv = document.getElementById('adicional-options');
    const saladPriceDisplay = document.getElementById('salad-price-display');
    const addSaladToCartBtn = document.getElementById('add-salad-to-cart-btn');

    // Referências Gerais (Existentes)
    const cartList = document.getElementById('cart-list');
    const checkoutSection = document.getElementById('checkout-section');
    const deliveryFeeRadios = document.querySelectorAll('input[name="deliveryFee"]');
    const checkoutBtn = document.getElementById('checkout-btn');
    const trocoInputContainer = document.getElementById('troco-input-container'); 
    const paymentMethodSelect = document.getElementById('payment-method');

    // --- FUNÇÕES DE LÓGICA E RENDERIZAÇÃO INICIAL ---

    // Função auxiliar para renderizar opções (Radio ou Checkbox)
    function renderOptions(container, name, options, type = 'radio', checkedValue = null, isSizeOption = false) {
        container.innerHTML = options.map((option, index) => {
            const value = isSizeOption ? option.id : option;
            let labelText = isSizeOption 
                ? `${option.id} (${option.nome}) (R$ ${option.preco.toFixed(2).replace('.', ',')})`
                : option;
            
            if (isSizeOption) {
                labelText += `, Máx. ${option.limite} Acomp.`;
            }

            const isChecked = checkedValue ? (value === checkedValue) : (index === 0 && !isSizeOption && type === 'radio');
            
            // Layout mais simples para Molhos e Acompanhamentos
            const labelClasses = type === 'checkbox' && name !== 'adicional' ? 'flex items-center p-3 bg-white rounded-lg shadow-md hover:bg-[#f7fcf6] transition duration-150 flex-1 cursor-pointer acomp-option' : 'flex items-center p-3 bg-white rounded-lg shadow-md hover:bg-[#f7fcf6] transition duration-150 flex-1 cursor-pointer';

            return `
                <label class="${labelClasses}">
                    <input type="${type}" name="${name}" value="${value}" 
                        class="${type === 'radio' ? 'form-radio text-green-600' : 'form-checkbox text-green-600'}"
                        ${isChecked ? 'checked' : ''}>
                    <span class="ml-2 font-medium">${labelText}</span>
                </label>
            `;
        }).join('');
    }
    
    // Renderiza as opções de Salada (SEM O DETALHE DO MOLHO NA OPÇÃO)
    function renderSaladOptions() {
        saladOptionsDiv.innerHTML = MENU.saladas.map(salad => `
            <div class="menu-item-card" data-salad-id="${salad.id}" onclick="selectSalad('${salad.id}')">
                <div class="custom-radio-marker"></div>
                <div class="flex-1">
                    <div class="flex justify-between items-start">
                        <h3 class="font-bold text-gray-800">${salad.nome}</h3>
                        <span class="font-extrabold text-lg text-green-700">R$ ${salad.preco.toFixed(2).replace('.', ',')}</span>
                    </div>
                    <p class="text-sm text-gray-500 mt-1">${salad.detalhes}</p>
                    <p class="text-xs font-semibold text-blue-500 mt-1">Acompanha Molho Especial.</p>
                </div>
            </div>
        `).join('');
    }

    // Renderiza as opções de Adicionais (Mantida)
    function renderAdicionalOptions() {
        const adicionalHtml = MENU.adicionais.map(adicional => `
            <div class="menu-item-card adicional-card" data-adicional-id="${adicional.id}" data-price="${adicional.preco}" onclick="toggleAdicional('${adicional.id}')">
                <div class="custom-checkbox-marker"></div>
                <div class="flex-1">
                    <p class="text-sm font-medium text-gray-700">${adicional.nome}</p>
                    <span class="text-xs text-blue-600 font-bold">(+ R$ ${adicional.preco.toFixed(2).replace('.', ',')})</span>
                </div>
                <input type="checkbox" name="adicional" value="${adicional.nome}" data-price="${adicional.preco}" data-id="${adicional.id}" class="hidden">
            </div>
        `).join('');
        adicionalOptionsDiv.innerHTML = adicionalHtml;
    }
    
    // Função para alternar o adicional e atualizar o preço (Mantida)
    function toggleAdicional(adicionalId) {
        const card = document.querySelector(`.adicional-card[data-adicional-id="${adicionalId}"]`);
        const checkbox = card.querySelector(`input[data-id="${adicionalId}"]`);
        
        checkbox.checked = !checkbox.checked;

        card.classList.toggle('selected', checkbox.checked);

        updateSaladPrice();
    }
    
    // Função de Inicialização (Mantida)
    function setupUI() {
        // Kebab Setup
        renderOptions(sizeOptionsDiv, 'size', MENU.tamanhos, 'radio', 'P', true);
        renderOptions(proteinOptionsDiv, 'proteina', MENU.proteinas, 'radio');
        renderOptions(molhoOptionsDiv, 'molho', MENU.molhos, 'checkbox'); 
        renderOptions(acompOptionsDiv, 'acomp', MENU.acompanhamentos, 'checkbox');

        sizeOptionsDiv.addEventListener('change', updateLimitAndPrice);
        molhoOptionsDiv.addEventListener('change', updateMolhoCount); 
        acompOptionsDiv.addEventListener('change', updateAcompCount);

        // Salada Setup
        renderSaladOptions();
        renderAdicionalOptions(); 

        deliveryFeeRadios.forEach(radio => radio.addEventListener('change', updateFinalSummary));

        updateLimitAndPrice();
        renderCart(); 
        toggleTrocoField(); 
    }
    
    // Lógica de Seleção de Salada (ATUALIZADA: Não exibe o nome do molho nos detalhes)
    function selectSalad(saladId) {
        const newSalad = MENU.saladas.find(s => s.id === saladId);

        // Remove a seleção anterior
        document.querySelectorAll('.menu-item-card').forEach(card => card.classList.remove('selected'));
        
        // Adiciona a seleção ao card clicado
        const selectedCard = document.querySelector(`[data-salad-id="${saladId}"]`);
        if (selectedCard) {
            selectedCard.classList.add('selected');
        }

        selectedSalad = newSalad;
        
        // Atualiza a interface
        selectedSaladName.textContent = newSalad.nome;
        selectedSaladMolho.textContent = newSalad.molho; // Apenas armazena, mas o texto do HTML já é "Molho Especial"
        saladDetailsDiv.style.display = 'block';
        
        // Reinicia os adicionais 
        document.querySelectorAll('.adicional-card').forEach(card => card.classList.remove('selected'));
        document.querySelectorAll('input[name="adicional"]').forEach(cb => cb.checked = false);
        
        document.getElementById('salad-obs').value = '';
        
        updateSaladPrice();
    }
    
    // Lógica de Cálculo de Preço da Salada (Mantida)
    function updateSaladPrice() {
        if (!selectedSalad) {
            currentSaladPrice = 0;
            return;
        }

        let basePrice = selectedSalad.preco;
        let adicionalPrice = 0;

        // Busca o preço pelos checkboxes hidden
        document.querySelectorAll('input[name="adicional"]:checked').forEach(cb => {
            adicionalPrice += parseFloat(cb.dataset.price);
        });

        currentSaladPrice = basePrice + adicionalPrice;
        saladPriceDisplay.textContent = `R$ ${currentSaladPrice.toFixed(2).replace('.', ',')}`;
    }

    // Lógica para adicionar Salada ao Carrinho (Mantida)
    function addSaladToCart() {
        addSaladToCartBtn.disabled = true; 
        addSaladToCartBtn.textContent = 'Adicionando...';
        
        if (!selectedSalad) {
            showModal('Por favor, selecione uma salada para adicionar ao carrinho.', 'bg-red-500');
            addSaladToCartBtn.disabled = false;
            addSaladToCartBtn.textContent = 'ADICIONAR SALADA AO CARRINHO';
            return;
        }

        const saladMolho = selectedSalad.molho; 

        const adicionais = Array.from(document.querySelectorAll('input[name="adicional"]:checked'))
                                .map(cb => `${cb.value} (+R$ ${parseFloat(cb.dataset.price).toFixed(2).replace('.', ',')})`);

        const saladItem = {
            type: 'salad',
            nome: selectedSalad.nome,
            price: currentSaladPrice,
            molho: saladMolho, 
            adicionais: adicionais,
            obs: document.getElementById('salad-obs').value.trim()
        };

        cart.push(saladItem);
        showModal(`Salada ${selectedSalad.nome} adicionada ao carrinho!`, 'bg-blue-500');
        renderCart();
        
        // Reset da Salada
        selectedSalad = null;
        saladDetailsDiv.style.display = 'none';
        document.querySelectorAll('.menu-item-card').forEach(card => card.classList.remove('selected'));
        
        document.getElementById('checkout-section').scrollIntoView({ behavior: 'smooth', block: 'start' });

        setTimeout(() => {
            addSaladToCartBtn.disabled = false;
            addSaladToCartBtn.textContent = 'ADICIONAR SALADA AO CARRINHO';
        }, 300); 
    }

    // Funções Kebab (Mantidas)
    function toggleTrocoField() {
        const isMoney = paymentMethodSelect.value === 'Dinheiro';
        trocoInputContainer.style.display = isMoney ? 'block' : 'none';
        if (!isMoney) {
            document.getElementById('troco').value = '';
        }
    }

    function updateLimitAndPrice() {
        const selectedSizeId = document.querySelector('input[name="size"]:checked').value;
        const selectedSize = MENU.tamanhos.find(t => t.id === selectedSizeId);
        
        if (selectedSize) {
            maxAcompanhamentos = selectedSize.limite;
        }
        
        updateItemPrice();
        updateAcompCount();
    }
    
    function updateMolhoCount() {
        const checkboxes = document.querySelectorAll('input[name="molho"]');
        const selected = document.querySelectorAll('input[name="molho"]:checked');
        selectedMolhos = selected.length;
        
        checkboxes.forEach(cb => {
            if (selectedMolhos >= maxMolhos && !cb.checked) {
                cb.disabled = true;
                cb.closest('label').classList.add('opacity-50');
            } else {
                cb.disabled = false;
                cb.closest('label').classList.remove('opacity-50');
            }
        });
    }

    function updateItemPrice() {
        const selectedSizeId = document.querySelector('input[name="size"]:checked').value;
        const basePrice = MENU.tamanhos.find(t => t.id === selectedSizeId)?.preco || 0;
        
        premiumPrice = 0;
        
        currentItemPrice = basePrice + premiumPrice;
        itemPriceDisplay.textContent = `R$ ${currentItemPrice.toFixed(2).replace('.', ',')}`;
    }

    function updateAcompCount() {
        const checkboxes = document.querySelectorAll('input[name="acomp"]:checked');
        selectedAcompanhamentos = checkboxes.length;
        const selectedSizeId = document.querySelector('input[name="size"]:checked').value;

        acompCounter.textContent = `Selecionados: ${selectedAcompanhamentos}/${maxAcompanhamentos} (Tamanho ${selectedSizeId})`;
        acompCounter.classList.toggle('font-bold', selectedAcompanhamentos === maxAcompanhamentos);

        document.querySelectorAll('input[name="acomp"]').forEach(cb => {
            if (selectedAcompanhamentos >= maxAcompanhamentos && !cb.checked) {
                cb.disabled = true;
                cb.closest('label').classList.add('opacity-50');
            } else {
                cb.disabled = false;
                cb.closest('label').classList.remove('opacity-50');
            }
        });
    }


    function getFormData() {
        const sizeRadio = document.querySelector('input[name="size"]:checked');
        const proteinaRadio = document.querySelector('input[name="proteina"]:checked'); 
        const molhoCheckboxes = document.querySelectorAll('input[name="molho"]:checked'); 
        const acompCheckboxes = document.querySelectorAll('input[name="acomp"]:checked');
        
        if (!sizeRadio || !proteinaRadio || molhoCheckboxes.length === 0 || acompCheckboxes.length === 0) {
            showModal('Por favor, selecione o **Tamanho**, **1 Proteína**, pelo menos **1 Molho** e **1 Acompanhamento**.', 'bg-red-500');
            return null;
        }
        if (molhoCheckboxes.length > MENU.maxMolhos) {
            showModal(`Você pode selecionar no máximo ${MENU.maxMolhos} Molhos.`, 'bg-red-500');
            return null;
        }

        const customItem = {
            type: 'kebab', // Adicionando tipo para diferenciar no carrinho
            size: sizeRadio.value,
            price: currentItemPrice,
            proteina: proteinaRadio.value, 
            molhos: Array.from(molhoCheckboxes).map(cb => cb.value), 
            acompanhamentos: Array.from(acompCheckboxes).map(cb => cb.value),
            obs: document.getElementById('obs').value.trim()
        };

        return customItem;
    }

    function addToCart() {
        addToCartBtn.disabled = true; 
        addToCartBtn.textContent = 'Adicionando...';
        
        const item = getFormData();
        if (item) {
            cart.push(item);
            showModal('Kebab adicionado ao carrinho! Você pode montar outro.', 'bg-green-500');
            renderCart();
            resetForm();
            
            document.getElementById('checkout-section').scrollIntoView({ behavior: 'smooth', block: 'start' });
        }

        setTimeout(() => {
            addToCartBtn.disabled = false;
            addToCartBtn.textContent = 'ADICIONAR PRATO AO CARRINHO E MONTAR OUTRO';
        } , 300); 
    }
    
    function resetForm() {
        document.querySelectorAll('input[name="proteina"]').forEach(r => r.checked = false); 
        document.querySelectorAll('input[name="molho"]').forEach(cb => cb.checked = false); 
        document.querySelectorAll('input[name="acomp"]').forEach(cb => cb.checked = false);
        document.getElementById('obs').value = '';

        updateMolhoCount(); 
        
        document.querySelector('input[name="size"][value="P"]').checked = true;
        updateLimitAndPrice(); 
    }

    // --- FUNÇÕES DE CARRINHO E TOTALIZAÇÃO (Mantidas) ---

    function renderCart() {
        cartList.innerHTML = '';
        let subtotalPratos = 0;
        
        if (cart.length === 0) {
            cartList.innerHTML = `<li id="empty-cart-message" class="text-gray-500 italic text-center">Nenhum item no carrinho.</li>`;
        } else {
            cart.forEach((item, index) => {
                subtotalPratos += item.price;
                const li = document.createElement('li');
                li.className = 'p-3 border border-gray-100 bg-white rounded-lg shadow-sm space-y-1';
                
                let itemDetails;

                if (item.type === 'kebab') {
                    const molhoList = item.molhos.join(', ');
                    const acompList = item.acompanhamentos.length > 0 ? item.acompanhamentos.join(', ') : 'Nenhum';
                    
                    itemDetails = `
                        <h3 class="font-bold text-gray-800">#${index + 1} - Kebab ${item.size} (${item.proteina})</h3>
                        <p class="text-sm text-gray-600">Molhos Selecionados: ${molhoList}</p>
                        <p class="text-sm text-gray-600">Acompanhamentos: ${acompList}</p>
                    `;
                } else if (item.type === 'salad') {
                    const adicionaisList = item.adicionais.length > 0 ? item.adicionais.join(' | ') : 'Nenhum Adicional';
                    
                    itemDetails = `
                        <h3 class="font-bold text-gray-800">#${index + 1} - Salada ${item.nome}</h3>
                        <p class="text-sm text-gray-600">Molho Especial: ${item.molho}</p>
                        <p class="text-sm text-gray-600">Adicionais: ${adicionaisList}</p>
                    `;
                }
                
                li.innerHTML = `
                    <div class="flex justify-between items-start">
                        ${itemDetails}
                        <button onclick="removeItem(${index})" class="text-red-500 hover:text-red-700 text-sm font-semibold transition duration-150">
                            Remover
                        </button>
                    </div>
                    <p class="text-sm text-gray-600 italic">Obs: ${item.obs || 'Nenhuma'}</p>
                    <p class="text-lg font-extrabold text-green-700 text-right">R$ ${item.price.toFixed(2).replace('.', ',')}</p>
                `;
                cartList.appendChild(li);
            });
        }
        
        document.getElementById('cart-count').textContent = cart.length;
        document.getElementById('subtotal-display').textContent = `R$ ${subtotalPratos.toFixed(2).replace('.', ',')}`;
        
        if (cart.length > 0) {
            checkoutSection.style.display = 'block';
        } else {
            checkoutSection.style.display = 'none';
        }
        
        updateFinalSummary();
    }

    function removeItem(index) {
        cart.splice(index, 1); 
        renderCart();
        showModal('Item removido do carrinho.', 'bg-yellow-600');
    }

    function getDeliveryFee() {
        const selectedFee = document.querySelector('input[name="deliveryFee"]:checked');
        return parseFloat(selectedFee ? selectedFee.value : '0.00');
    }

    function updateFinalSummary() {
        let subtotalGeral = cart.reduce((sum, item) => sum + item.price, 0);
        
        const deliveryFee = getDeliveryFee();
        const total = subtotalGeral + deliveryFee;

        document.getElementById('final-subtotal').textContent = `R$ ${subtotalGeral.toFixed(2).replace('.', ',')}`;
        document.getElementById('final-fee').textContent = `R$ ${deliveryFee.toFixed(2).replace('.', ',')}`;
        document.getElementById('final-total').textContent = `R$ ${total.toFixed(2).replace('.', ',')}`;
    }

    // Função de Geração de Link (Mantida)
    function generateWhatsAppLink() {
        const nome = document.getElementById('nome').value.trim();
        const telefone = document.getElementById('telefone').value.trim();
        const endereco = document.getElementById('endereco').value.trim();

        if (cart.length === 0) {
            showModal('Seu carrinho está vazio!', 'bg-red-500');
            return;
        }

        if (!nome || !telefone || !endereco) {
            showModal('Por favor, preencha seu Nome, Telefone e Endereço para finalizar o pedido.', 'bg-red-500');
            return;
        }
        
        checkoutBtn.disabled = true;
        checkoutBtn.textContent = 'Gerando Link...';

        const deliveryFee = getDeliveryFee();
        const total = parseFloat(document.getElementById('final-total').textContent.replace('R$ ', '').replace(',', '.'));
        const subtotalGeral = parseFloat(document.getElementById('final-subtotal').textContent.replace('R$ ', '').replace(',', '.'));
        const paymentMethod = paymentMethodSelect.value;
        const troco = document.getElementById('troco').value.trim();
        const referencia = document.getElementById('referencia').value.trim();

        let message = `*PEDIDO MOVITA - KEBAB & SALADAS*\n\n`;
        message += `====================================\n`;

        // ITENS MONTADOS (DESTACADO EM NEGRITO)
        message += `*🍲 ITENS DO PEDIDO (${cart.length}x):*\n`;
        cart.forEach((item, index) => {
            message += `\n*#${index + 1} - ${item.type === 'kebab' ? 'Kebab' : 'Salada'} ${item.type === 'kebab' ? item.size : item.nome} (R$ ${item.price.toFixed(2).replace('.', ',')})*\n`;

            if (item.type === 'kebab') {
                const molhoList = item.molhos.join(', ');
                const acompList = item.acompanhamentos.join(', ');
                message += `*-> Proteína: ${item.proteina}*\n`;
                message += `*-> Molhos: ${molhoList}*\n`;
                message += `*-> Acompanhamentos: ${acompList}*\n`;
            } else if (item.type === 'salad') {
                const adicionaisList = item.adicionais.length > 0 ? item.adicionais.join(' | ') : 'Nenhum';
                // Molho Especial SEM descrição detalhada
                message += `*-> Molho Especial: ${item.molho}*\n`;
                message += `*-> Adicionais: ${adicionaisList}*\n`;
            }

            if (item.obs) {
                message += `*-> Obs: ${item.obs}*\n`;
            }
        });

        // Resumo de Valores
        message += `\n====================================\n`;
        message += `*💸 RESUMO FINANCEIRO*\n`;
        message += `Subtotal Itens: R$ ${subtotalGeral.toFixed(2).replace('.', ',')}\n`;
        message += `Taxa de Entrega: R$ ${deliveryFee.toFixed(2).replace('.', ',')}\n`;
        message += `*TOTAL FINAL: R$ ${total.toFixed(2).replace('.', ',')}*\n`;

        // Dados do Cliente
        message += `\n====================================\n`;
        message += `*👤 DADOS PARA ENTREGA E PAGAMENTO*\n`;
        message += `Nome: ${nome}\n`;
        message += `Telefone: ${telefone}\n`;
        message += `Endereço: ${endereco}\n`;
        if (referencia) {
            message += `Ponto de Ref.: ${referencia}\n`;
        }
        message += `\n*Forma de Pagamento: ${paymentMethod}*\n`;
        
        // Adiciona informações de Troco/PIX
        if (paymentMethod === 'Dinheiro' && troco) {
            message += `*Precisa de Troco para: ${troco}*\n`;
        } else if (paymentMethod === 'Dinheiro') {
             message += `Pagamento em Dinheiro (Valor Exato)\n`;
        } else if (paymentMethod === 'Pix') {
            message += `*Dados PIX:* ${MENU.pixData.key} (Nome: ${MENU.pixData.name})\n`;
        }
        
        message += `====================================\n`;
        message += `*AGRADECEMOS A PREFERÊNCIA!*`;

        const encodedMessage = encodeURIComponent(message);
        const whatsappUrl = `https://api.whatsapp.com/send?phone=${MENU.whatsappNumber}&text=${encodedMessage}`;

        window.open(whatsappUrl, '_blank');
        
        setTimeout(() => {
            checkoutBtn.disabled = false;
            checkoutBtn.textContent = 'ENVIAR PEDIDO VIA WHATSAPP';
        }, 1000);
    }
    
    // --- FUNÇÕES AUXILIARES (Modal Simples) ---
    function showModal(message, bgColor) {
        const modal = document.createElement('div');
        modal.className = `fixed top-0 left-0 right-0 z-50 p-4 text-center text-white font-bold ${bgColor} shadow-lg`;
        modal.innerHTML = message;
        document.body.appendChild(modal);
        setTimeout(() => {
            modal.remove();
        }, 3000);
    }

    // Inicialização
    window.onload = setupUI;
    // Exporta funções para uso global (inline onclick, onchange)
    window.addToCart = addToCart;
    window.addSaladToCart = addSaladToCart; 
    window.selectSalad = selectSalad; 
    window.toggleAdicional = toggleAdicional; 
    window.removeItem = removeItem;
    window.generateWhatsAppLink = generateWhatsAppLink;
    window.toggleTrocoField = toggleTrocoField;
</script>
</body>
</html>
