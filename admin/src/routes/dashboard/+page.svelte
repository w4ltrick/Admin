<script>
  import { goto } from '$app/navigation';

  let abaAtual = 'pedidos';
  let mesaSelecionada = null;
  let garcomSelecionado = null;

  function logout() {
    goto('/login');
  }

  let pedidos = [
      { id: 1, item: "Pizza Margherita", preco: 32.90, observacoes: "Sem queijo extra", mesa: 1, status: "pendente", garcom: "Carlos" },
      { id: 2, item: "Hambúrguer", preco: 28.50, observacoes: "Com cebola roxa, sem alface", mesa: 2, status: "pendente", garcom: "Ana" },
      { id: 3, item: "Coca-Cola", preco: 7.50, observacoes: "Sem gelo", mesa: 3, status: "pendente", garcom: "Roberto" },
      { id: 4, item: "Salada Caesar", preco: 24.90, observacoes: "Adicionar frango grelhado", mesa: 2, status: "pendente", garcom: "Ana" },
      { id: 5, item: "Spaghetti Carbonara", preco: 34.90, observacoes: "Sem bacon", mesa: 3, status: "pendente", garcom: "Roberto" },
      { id: 6, item: "Onion Rings", preco: 16.90, observacoes: "Sem glúten. Anéis de cebola empanados e fritos até ficarem dourados e crocantes. Acompanha molho barbecue.", mesa: 3, status: "pendente", garcom: "Roberto" }
  ];

  let mesas = [
      { numero: 1, pedido: ["Pizza Margherita", "Coca-Cola"], total: 25.50, garcom: "Carlos" },
      { numero: 2, pedido: ["Hambúrguer", "Salada Caesar"], total: 30.00, garcom: "Ana" },
      { numero: 3, pedido: ["Spaghetti Carbonara", "Coca-Cola", "Onion Rings"], total: 59.30, garcom: "Roberto" },
      { numero: 4, pedido: ["Pizza Margherita", "Hambúrguer"], total: 35.00, garcom: "Juliana" }
  ];

  // Lista de garçons baseada nos dados
  let garcons = [...new Set(mesas.map(mesa => mesa.garcom))];

  function deletarPedido(id) {
    pedidos = pedidos.filter(pedido => pedido.id !== id);
    atualizarMesas();
  }

  function confirmarPedido(id) {
    // Altera o status do pedido para "confirmado" em vez de deletá-lo
    pedidos = pedidos.map(pedido => {
      if (pedido.id === id) {
        return { ...pedido, status: "confirmado" };
      }
      return pedido;
    });
    atualizarMesas();
  }

  function atualizarMesas() {
    // Atualiza os pedidos nas mesas quando o status do pedido muda ou quando um pedido é deletado
    mesas = mesas.map(mesa => {
      const pedidosDaMesa = pedidos.filter(p => p.mesa === mesa.numero);
      return {
        ...mesa,
        pedido: pedidosDaMesa.map(p => p.item),
        total: pedidosDaMesa.reduce((total, p) => total + p.preco, 0)
      };
    });
  }

  function exibirDetalhesMesa(numeroMesa) {
    mesaSelecionada = mesaSelecionada === numeroMesa ? null : numeroMesa;
    garcomSelecionado = null; // Fecha detalhes do garçom ao abrir detalhes da mesa
  }

  function exibirDetalhesGarcom(nomeGarcom) {
    garcomSelecionado = garcomSelecionado === nomeGarcom ? null : nomeGarcom;
    mesaSelecionada = null; // Fecha detalhes da mesa ao abrir detalhes do garçom
  }

  function getPedidosDaMesa(numeroMesa) {
    return pedidos.filter(pedido => pedido.mesa === numeroMesa);
  }

  function getPedidosDoGarcom(nomeGarcom) {
    return pedidos.filter(pedido => pedido.garcom === nomeGarcom);
  }

  function getMesasDoGarcom(nomeGarcom) {
    return mesas.filter(mesa => mesa.garcom === nomeGarcom);
  }
</script>

<div class="min-h-screen bg-gray-100 flex flex-col">
<!-- Navbar no topo -->
<nav class="bg-white shadow-md px-6 py-4 flex items-center">
  <div class="flex space-x-6 w-1/3">
    <button
      class={`font-medium transition ${abaAtual === 'pedidos' ? 'text-red-500 border-b-2 border-red-500' : 'text-gray-700 hover:text-red-500'}`}
      on:click={() => {
        abaAtual = 'pedidos';
        mesaSelecionada = null;
        garcomSelecionado = null;
      }}
    >
      Pedidos
    </button>
    <button
      class={`font-medium transition ${abaAtual === 'garcom' ? 'text-red-500 border-b-2 border-red-500' : 'text-gray-700 hover:text-red-500'}`}
      on:click={() => {
        abaAtual = 'garcom';
        mesaSelecionada = null;
        garcomSelecionado = null;
      }}
    >
      Garçom
    </button>
    <button
      class={`font-medium transition ${abaAtual === 'mesas' ? 'text-red-500 border-b-2 border-red-500' : 'text-gray-700 hover:text-red-500'}`}
      on:click={() => {
        abaAtual = 'mesas';
        mesaSelecionada = null;
        garcomSelecionado = null;
      }}
    >
      Mesas
    </button>
  </div>
  <div class="flex-1 text-center">
    <span class="text-gray-600">Você está na página: <span class="font-semibold text-red-500">{abaAtual}</span></span>
  </div>
  <div class="w-1/3 flex justify-end">
    <button
      on:click={logout}
      class="bg-red-400 text-white px-5 py-2 rounded-full hover:bg-red-600 transition shadow-md"
    >
      Deslogar
    </button>
  </div>
</nav>

<main class="flex-1 relative">
  <div class="absolute inset-0 flex items-center justify-center text-center">
    {#if abaAtual === 'pedidos'}
      <div class="w-full max-w-4xl">
        <h1 class="text-3xl font-bold text-gray-800 mb-2 font-sans">Página de Pedidos</h1>
        <p class="text-gray-600">Aqui você gerencia os pedidos.</p>
        
        <table class="min-w-full mt-6 table-auto border-collapse shadow-lg rounded-lg overflow-hidden">
          <thead class="bg-gray-800 text-white">
            <tr>
              <th class="px-6 py-3 text-left font-semibold">Item</th>
              <th class="px-6 py-3 text-left font-semibold">Preço (R$)</th>
              <th class="px-6 py-3 text-left font-semibold">Observações</th>
              <th class="px-6 py-3 text-left font-semibold">Mesa</th>
              <th class="px-6 py-3 text-left font-semibold">Status</th>
              <th class="px-6 py-3 text-left font-semibold">Ações</th>
            </tr>
          </thead>
          <tbody class="bg-white">
            {#each pedidos as pedido}
              <tr class={`hover:bg-gray-50 ${pedido.status === 'confirmado' ? 'bg-green-50' : ''} border-b border-gray-200`}>
                <td class="px-6 py-4 font-medium">{pedido.item}</td>
                <td class="px-6 py-4">{pedido.preco.toFixed(2)}</td>
                <td class="px-6 py-4">{pedido.observacoes}</td>
                <td class="px-6 py-4">{pedido.mesa}</td>
                <td class="px-6 py-4">
                  <span class={`py-1 px-2 rounded-full text-xs font-medium ${pedido.status === 'confirmado' ? 'bg-green-100 text-green-800' : 'bg-yellow-100 text-yellow-800'}`}>
                    {pedido.status}
                  </span>
                </td>
                <td class="px-6 py-4">
                  <div class="flex space-x-2">
                    {#if pedido.status !== 'confirmado'}
                      <button 
                        class="bg-green-500 text-white px-3 py-1 rounded-md hover:bg-green-600 transition font-medium text-sm"
                        on:click={() => confirmarPedido(pedido.id)}
                      >
                        Confirmar
                      </button>
                    {:else}
                      <span class="bg-gray-200 text-gray-500 px-3 py-1 rounded-md font-medium text-sm">
                        Confirmado
                      </span>
                    {/if}
                    <button 
                      class="bg-red-500 text-white px-3 py-1 rounded-md hover:bg-red-600 transition font-medium text-sm"
                      on:click={() => deletarPedido(pedido.id)}
                    >
                      Deletar
                    </button>
                  </div>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
    {:else if abaAtual === 'garcom'}
      <div class="w-full max-w-4xl">
        <h1 class="text-3xl font-bold text-gray-800 mb-2 font-sans">Página do Garçom</h1>
        <p class="text-gray-600">Clique em um garçom para ver suas mesas e pedidos.</p>

        <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mt-6">
          {#each garcons as garcom}
            <button 
              class={`p-4 rounded-lg shadow-md transition transform hover:scale-105 ${garcomSelecionado === garcom ? 'bg-blue-100 border-2 border-blue-500' : 'bg-white'}`}
              on:click={() => exibirDetalhesGarcom(garcom)}
            >
              <div class="font-bold text-lg">{garcom}</div>
              <div class="text-sm">
                {getMesasDoGarcom(garcom).length} {getMesasDoGarcom(garcom).length === 1 ? 'mesa' : 'mesas'}
              </div>
              <div class="mt-2 font-semibold text-blue-500">
                {getPedidosDoGarcom(garcom).length} {getPedidosDoGarcom(garcom).length === 1 ? 'pedido' : 'pedidos'}
              </div>
            </button>
          {/each}
        </div>

        {#if garcomSelecionado}
          <div class="mt-8 bg-white p-6 rounded-lg shadow-md">
            <div class="flex justify-between items-center mb-4">
              <h2 class="text-2xl font-bold font-sans">Detalhes do Garçom: {garcomSelecionado}</h2>
              <button 
                class="text-gray-500 hover:text-red-500"
                on:click={() => garcomSelecionado = null}
              >
                Fechar
              </button>
            </div>

            <div class="bg-gray-50 p-4 rounded mb-4">
              <h3 class="text-xl font-semibold mb-2 font-sans">Mesas Atendidas</h3>
              <div class="grid grid-cols-2 md:grid-cols-3 gap-2">
                {#each getMesasDoGarcom(garcomSelecionado) as mesa}
                  <div class="bg-white p-3 rounded shadow">
                    <div class="font-bold">Mesa {mesa.numero}</div>
                    <div class="text-sm text-gray-600">Total: R$ {mesa.total.toFixed(2)}</div>
                  </div>
                {/each}
              </div>
            </div>

            <h3 class="text-xl font-semibold mb-2 font-sans">Pedidos em Atendimento</h3>

            {#if getPedidosDoGarcom(garcomSelecionado).length > 0}
              <table class="min-w-full table-auto border-collapse shadow-lg rounded-lg overflow-hidden">
                <thead class="bg-gray-800 text-white">
                  <tr>
                    <th class="px-6 py-3 text-left font-semibold">Item</th>
                    <th class="px-6 py-3 text-left font-semibold">Preço (R$)</th>
                    <th class="px-6 py-3 text-left font-semibold">Mesa</th>
                    <th class="px-6 py-3 text-left font-semibold">Observações</th>
                    <th class="px-6 py-3 text-left font-semibold">Status</th>
                    <th class="px-6 py-3 text-left font-semibold">Ações</th>
                  </tr>
                </thead>
                <tbody class="bg-white">
                  {#each getPedidosDoGarcom(garcomSelecionado) as pedido}
                    <tr class={`hover:bg-gray-50 ${pedido.status === 'confirmado' ? 'bg-green-50' : ''} border-b border-gray-200`}>
                      <td class="px-6 py-4 font-medium">{pedido.item}</td>
                      <td class="px-6 py-4">{pedido.preco.toFixed(2)}</td>
                      <td class="px-6 py-4">Mesa {pedido.mesa}</td>
                      <td class="px-6 py-4">{pedido.observacoes}</td>
                      <td class="px-6 py-4">
                        <span class={`py-1 px-2 rounded-full text-xs font-medium ${pedido.status === 'confirmado' ? 'bg-green-100 text-green-800' : 'bg-yellow-100 text-yellow-800'}`}>
                          {pedido.status}
                        </span>
                      </td>
                      <td class="px-6 py-4">
                        <div class="flex space-x-2">
                          {#if pedido.status !== 'confirmado'}
                            <button 
                              class="bg-green-500 text-white px-3 py-1 rounded-md hover:bg-green-600 transition font-medium text-sm"
                              on:click={() => confirmarPedido(pedido.id)}
                            >
                              Confirmar
                            </button>
                          {:else}
                            <span class="bg-gray-200 text-gray-500 px-3 py-1 rounded-md font-medium text-sm">
                              Confirmado
                            </span>
                          {/if}
                          <button 
                            class="bg-red-500 text-white px-3 py-1 rounded-md hover:bg-red-600 transition font-medium text-sm"
                            on:click={() => deletarPedido(pedido.id)}
                          >
                            Deletar
                          </button>
                        </div>
                      </td>
                    </tr>
                  {/each}
                </tbody>
              </table>
            {:else}
              <p class="text-gray-500 italic">Nenhum pedido pendente para este garçom.</p>
            {/if}
          </div>
        {/if}
      </div>
    {:else if abaAtual === 'mesas'}
      <div class="w-full max-w-4xl">
        <h1 class="text-3xl font-bold text-gray-800 mb-2 font-sans">Página de Mesas</h1>
        <p class="text-gray-600">Clique em uma mesa para ver seus pedidos detalhados.</p>

        <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mt-6">
          {#each mesas as mesa}
            <button 
              class={`p-4 rounded-lg shadow-md transition transform hover:scale-105 ${mesaSelecionada === mesa.numero ? 'bg-red-100 border-2 border-red-500' : 'bg-white'}`}
              on:click={() => exibirDetalhesMesa(mesa.numero)}
            >
              <div class="font-bold text-lg">Mesa {mesa.numero}</div>
              <div class="text-sm">Garçom: {mesa.garcom}</div>
              <div class="mt-2 font-semibold text-red-500">R$ {mesa.total.toFixed(2)}</div>
            </button>
          {/each}
        </div>

        {#if mesaSelecionada}
          <div class="mt-8 bg-white p-6 rounded-lg shadow-md">
            <div class="flex justify-between items-center mb-4">
              <h2 class="text-2xl font-bold font-sans">Detalhes da Mesa {mesaSelecionada}</h2>
              <button 
                class="text-gray-500 hover:text-red-500"
                on:click={() => mesaSelecionada = null}
              >
                Fechar
              </button>
            </div>

            <div class="bg-gray-50 p-4 rounded mb-4">
              <p><strong>Garçom:</strong> {mesas.find(m => m.numero === mesaSelecionada)?.garcom}</p>
              <p><strong>Total:</strong> R$ {mesas.find(m => m.numero === mesaSelecionada)?.total.toFixed(2)}</p>
            </div>

            <h3 class="text-xl font-semibold mb-2 font-sans">Pedidos</h3>

            {#if getPedidosDaMesa(mesaSelecionada).length > 0}
              <table class="min-w-full table-auto border-collapse shadow-lg rounded-lg overflow-hidden">
                <thead class="bg-gray-800 text-white">
                  <tr>
                    <th class="px-6 py-3 text-left font-semibold">Item</th>
                    <th class="px-6 py-3 text-left font-semibold">Preço (R$)</th>
                    <th class="px-6 py-3 text-left font-semibold">Observações</th>
                    <th class="px-6 py-3 text-left font-semibold">Status</th>
                    <th class="px-6 py-3 text-left font-semibold">Ações</th>
                  </tr>
                </thead>
                <tbody class="bg-white">
                  {#each getPedidosDaMesa(mesaSelecionada) as pedido}
                    <tr class={`hover:bg-gray-50 ${pedido.status === 'confirmado' ? 'bg-green-50' : ''} border-b border-gray-200`}>
                      <td class="px-6 py-4 font-medium">{pedido.item}</td>
                      <td class="px-6 py-4">{pedido.preco.toFixed(2)}</td>
                      <td class="px-6 py-4">{pedido.observacoes}</td>
                      <td class="px-6 py-4">
                        <span class={`py-1 px-2 rounded-full text-xs font-medium ${pedido.status === 'confirmado' ? 'bg-green-100 text-green-800' : 'bg-yellow-100 text-yellow-800'}`}>
                          {pedido.status}
                        </span>
                      </td>
                      <td class="px-6 py-4">
                        <div class="flex space-x-2">
                          {#if pedido.status !== 'confirmado'}
                            <button 
                              class="bg-green-500 text-white px-3 py-1 rounded-md hover:bg-green-600 transition font-medium text-sm"
                              on:click={() => confirmarPedido(pedido.id)}
                            >
                              Confirmar
                            </button>
                          {:else}
                            <span class="bg-gray-200 text-gray-500 px-3 py-1 rounded-md font-medium text-sm">
                              Confirmado
                            </span>
                          {/if}
                          <button 
                            class="bg-red-500 text-white px-3 py-1 rounded-md hover:bg-red-600 transition font-medium text-sm"
                            on:click={() => deletarPedido(pedido.id)}
                          >
                            Deletar
                          </button>
                        </div>
                      </td>
                    </tr>
                  {/each}
                </tbody>
              </table>
            {:else}
              <p class="text-gray-500 italic">Nenhum pedido pendente para esta mesa.</p>
            {/if}
          </div>
        {/if}
      </div>
    {/if}
  </div>
</main>
</div>