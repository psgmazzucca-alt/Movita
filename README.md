<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Movita - Monte Seu Kebab</title>
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
/* Estilo para o input de quantidade de bebida (AGORA SEM USO) */
.drink-input {
    @apply w-16 p-2 text-center border border-gray-300 rounded-lg;
}
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
        
        ---

        <section class="space-y-4 p-4 border border-gray-200 rounded-xl bg-gray-50">
            <h2 class="section-title text-gray-700 border-gray-200">🛒 Seu Pedido (<span id="cart-count">0</span> Pratos)</h2>
            <ul id="cart-list" class="space-y-3">
                <li id="empty-cart-message" class="text-gray-500 italic text-center">Nenhum item no carrinho.</li>
            </ul>
            <p class="text-3xl font-extrabold text-red-700 pt-3 border-t border-gray-300 text-right">
                SUBTOTAL: <span id="subtotal-display">R$ 0,00</span>
            </p>
        </section>

        ---

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
                <select id="payment-method" class="input-style" onchange="toggleTrocoField()"> <option value="Pix">Pix (Informar na mensagem)</option>
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
        whatsappNumber: "5517997381858",
        pixData: { 
            name: "SUÉLEM CRISTINA MAESTRE MAZZUCCA",
            key: "41756000867 (CPF)" 
        }
    };

    // --- ESTADO GLOBAL ---
    let cart = []; 
    let currentItemPrice = MENU.tamanhos[0].preco;
    let premiumPrice = 0; 
    let selectedAcompanhamentos = 0;
    let maxAcompanhamentos = MENU.tamanhos[0].limite;
    let selectedMolhos = 0;
    let maxMolhos = MENU.maxMolhos;

    // --- REFERÊNCIAS DOM ---
    const sizeOptionsDiv = document.getElementById('size-options');
    const molhoOptionsDiv = document.getElementById('molho-options');
    const acompOptionsDiv = document.getElementById('acomp-options');
    const acompCounter = document.getElementById('acomp-counter');
    const itemPriceDisplay = document.getElementById('item-price-display');
    const cartList = document.getElementById('cart-list');
    const checkoutSection = document.getElementById('checkout-section');
    const deliveryFeeRadios = document.querySelectorAll('input[name="deliveryFee"]');
    const addToCartBtn = document.getElementById('add-to-cart-btn');
    const checkoutBtn = document.getElementById('checkout-btn');
    const trocoInputContainer = document.getElementById('troco-input-container'); 
    const paymentMethodSelect = document.getElementById('payment-method');
    const proteinOptionsDiv = document.getElementById('protein-options');

    // --- FUNÇÕES DE LÓGICA E RENDERIZAÇÃO INICIAL ---

    function renderOptions(container, name, options, type = 'radio', checkedValue = null, isSizeOption = false) {
        container.innerHTML = options.map((option, index) => {
            const value = isSizeOption ? option.id : option;
            let labelText = isSizeOption 
                ? `${option.id} (${option.nome}) (R$ ${option.preco.toFixed(2).replace('.', ',')})`
                : option;
            
            if (isSizeOption) {
                labelText += `, Máx. ${option.limite} Acomp.`;
            }

            const isChecked = checkedValue ? (value === checkedValue) : (index === 0 && !isSizeOption);
            
            return `
                <label class="flex items-center p-3 bg-white rounded-lg shadow-md hover:bg-[#f7fcf6] transition duration-150 flex-1 cursor-pointer acomp-option">
                    <input type="${type}" name="${name}" value="${value}" 
                        class="${type === 'radio' ? 'form-radio text-green-600' : 'form-checkbox text-green-600'}"
                        ${isChecked ? 'checked' : ''}>
                    <span class="ml-2 font-medium">${labelText}</span>
                </label>
            `;
        }).join('');
    }
    
    function renderMolhos() {
        renderOptions(molhoOptionsDiv, 'molho', MENU.molhos, 'checkbox');
        molhoOptionsDiv.addEventListener('change', updateMolhoCount);
    }

    function renderAcompanhamentos() {
        MENU.acompanhamentos.sort((a, b) => a.localeCompare(b));
        renderOptions(acompOptionsDiv, 'acomp', MENU.acompanhamentos, 'checkbox');
    }
    
    function setupUI() {
        // Inicializa as opções de tamanho, proteína, molhos e acompanhamentos
        renderOptions(sizeOptionsDiv, 'size', MENU.tamanhos, 'radio', 'P', true);
        renderOptions(proteinOptionsDiv, 'proteina', MENU.proteinas, 'radio');
        renderMolhos(); 
        renderAcompanhamentos();

        // Adiciona listeners para interações que mudam o preço/limites
        sizeOptionsDiv.addEventListener('change', updateLimitAndPrice);
        acompOptionsDiv.addEventListener('change', updateAcompCount);
        deliveryFeeRadios.forEach(radio => radio.addEventListener('change', updateFinalSummary));

        updateLimitAndPrice();
        renderCart(); 
    }
    
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
        // Desmarca a proteína selecionada
        document.querySelectorAll('input[name="proteina"]').forEach(r => r.checked = false); 
        // Desmarca todos os molhos
        document.querySelectorAll('input[name="molho"]').forEach(cb => cb.checked = false); 
        // Desmarca todos os acompanhamentos
        document.querySelectorAll('input[name="acomp"]').forEach(cb => cb.checked = false);
        // Limpa a observação
        document.getElementById('obs').value = '';

        updateMolhoCount(); 
        
        // Retorna ao tamanho P
        document.querySelector('input[name="size"][value="P"]').checked = true;
        updateLimitAndPrice(); 
    }

    // --- FUNÇÕES DE CARRINHO E TOTALIZAÇÃO ---

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
                
                const molhoList = item.molhos.join(', ');
                const acompList = item.acompanhamentos.length > 0 ? item.acompanhamentos.join(', ') : 'Nenhum';
                
                li.innerHTML = `
                    <div class="flex justify-between items-center">
                        <h3 class="font-bold text-gray-800">#${index + 1} - Kebab ${item.size} (${item.proteina})</h3>
                        <button onclick="removeItem(${index})" class="text-red-500 hover:text-red-700 text-sm font-semibold transition duration-150">
                            Remover
                        </button>
                    </div>
                    <p class="text-sm text-gray-600">Molhos Selecionados: ${molhoList}</p>
                    <p class="text-sm text-gray-600">Acompanhamentos: ${acompList}</p>
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
        showModal('Kebab removido do carrinho.', 'bg-yellow-600');
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

        let message = `*PEDIDO MOVITA - MONTE SEU KEBAB*\n\n`;
        message += `====================================\n`;

        // ITENS MONTADOS (DESTACADO EM NEGRITO)
        message += `*🥙 ITENS MONTADOS (${cart.length}x):*\n`;
        cart.forEach((item, index) => {
            const molhoList = item.molhos.join(', ');
            const acompList = item.acompanhamentos.join(', ');

            // TODO o bloco do item em negrito
            message += `\n*#${index + 1} - Kebab ${item.size} (${item.proteina}) (R$ ${item.price.toFixed(2).replace('.', ',')})*\n`;
            message += `*-> Molhos: ${molhoList}*\n`;
            message += `*-> Acompanhamentos: ${acompList}*\n`;
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
    window.removeItem = removeItem;
    window.generateWhatsAppLink = generateWhatsAppLink;
    window.toggleTrocoField = toggleTrocoField;
</script>
</body>
</html>
