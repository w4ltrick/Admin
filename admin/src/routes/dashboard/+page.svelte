<script>
  import { goto } from '$app/navigation';

  let abaAtual = 'pedidos';

  function logout() {
    goto('/login');
  }

  let pedidos = [
      { item: "Pizza Margherita", observacoes: "Sem queijo extra" },
      { item: "Hambúrguer", observacoes: "Com cebola roxa, sem alface" },
      { item: "Coca-Cola", observacoes: "Sem gelo" },
      { item: "Salada Caesar", observacoes: "Adicionar frango grelhado" },
      { item: "Spaghetti Carbonara", observacoes: "Sem bacon" }
  ];

  let mesas = [
      { numero: 1, pedido: ["Pizza Margherita", "Coca-Cola"], total: 25.50, garcom: "Carlos" },
      { numero: 2, pedido: ["Hambúrguer", "Salada Caesar"], total: 30.00, garcom: "Ana" },
      { numero: 3, pedido: ["Spaghetti Carbonara", "Coca-Cola"], total: 28.00, garcom: "Roberto" },
      { numero: 4, pedido: ["Pizza Margherita", "Hambúrguer"], total: 35.00, garcom: "Juliana" }
  ];

  let mostrarTabelaPedidos = false;
  let mostrarTabelaGarcom = false;
  let mostrarTabelaMesas = false;
</script>

<div class="min-h-screen bg-gray-100 flex flex-col">
<!-- Navbar no topo -->
<nav class="bg-white shadow-md px-6 py-4 flex justify-between items-center">
  <div class="flex space-x-6">
    <button
      class="text-gray-700 hover:text-red-500 font-medium transition"
      on:click={() => abaAtual = 'pedidos'}
    >
      Pedidos
    </button>
    <button
      class="text-gray-700 hover:text-red-500 font-medium transition"
      on:click={() => abaAtual = 'garcom'}
    >
      Garçom
    </button>
    <button
      class="text-gray-700 hover:text-red-500 font-medium transition"
      on:click={() => abaAtual = 'mesas'}
    >
      Mesas
    </button>
  </div>
  <button
    on:click={logout}
    class="bg-red-400 text-white px-5 py-2 rounded-full hover:bg-red-600 transition shadow-md"
  >
    Deslogar
  </button>
</nav>

<main class="flex-1 relative">
  <div class="absolute inset-0 flex items-center justify-center text-center">
    {#if abaAtual === 'pedidos'}
      <div>
        <h1 class="text-3xl font-bold text-gray-800 mb-2">Página de Pedidos</h1>
        <p class="text-gray-600">Aqui você gerencia os pedidos.</p>
        
        <button
          class="bg-red-400 text-white px-4 py-2 rounded mt-4 hover:bg-red-600 transition"
          on:click={() => mostrarTabelaPedidos = !mostrarTabelaPedidos}
        >
          {mostrarTabelaPedidos ? 'Ocultar Tabela de Pedidos' : 'Mostrar Tabela de Pedidos'}
        </button>

        {#if mostrarTabelaPedidos}
          <table class="min-w-full mt-6 table-auto border-collapse border border-gray-300">
            <thead>
              <tr>
                <th class="px-4 py-2 border border-gray-300 text-left">Item</th>
                <th class="px-4 py-2 border border-gray-300 text-left">Observações</th>
              </tr>
            </thead>
            <tbody>
              {#each pedidos as pedido}
                <tr>
                  <td class="px-4 py-2 border border-gray-300">{pedido.item}</td>
                  <td class="px-4 py-2 border border-gray-300">{pedido.observacoes}</td>
                </tr>
              {/each}
            </tbody>
          </table>
        {/if}
      </div>
    {:else if abaAtual === 'garcom'}
      <div>
        <h1 class="text-3xl font-bold text-gray-800 mb-2">Página do Garçom</h1>
        <p class="text-gray-600">Controle os garçons e as mesas por aqui.</p>

        <button
          class="bg-red-400 text-white px-4 py-2 rounded mt-4 hover:bg-red-600 transition"
          on:click={() => mostrarTabelaGarcom = !mostrarTabelaGarcom}
        >
          {mostrarTabelaGarcom ? 'Ocultar Tabela de Garçom' : 'Mostrar Tabela de Garçom'}
        </button>

        {#if mostrarTabelaGarcom}
          <table class="min-w-full mt-6 table-auto border-collapse border border-gray-300">
            <thead>
              <tr>
                <th class="px-4 py-2 border border-gray-300 text-left">Mesa</th>
                <th class="px-4 py-2 border border-gray-300 text-left">Garçom</th>
              </tr>
            </thead>
            <tbody>
              {#each mesas as mesa}
                <tr>
                  <td class="px-4 py-2 border border-gray-300">Mesa {mesa.numero}</td>
                  <td class="px-4 py-2 border border-gray-300">{mesa.garcom}</td>
                </tr>
              {/each}
            </tbody>
          </table>
        {/if}
      </div>
    {:else if abaAtual === 'mesas'}
      <div>
        <h1 class="text-3xl font-bold text-gray-800 mb-2">Página de Mesas</h1>
        <p class="text-gray-600">Veja os pedidos e os totais de cada mesa.</p>

        <button
          class="bg-red-400 text-white px-4 py-2 rounded mt-4 hover:bg-red-600 transition"
          on:click={() => mostrarTabelaMesas = !mostrarTabelaMesas}
        >
          {mostrarTabelaMesas ? 'Ocultar Tabela de Mesas' : 'Mostrar Tabela de Mesas'}
        </button>

        {#if mostrarTabelaMesas}
          <table class="min-w-full mt-6 table-auto border-collapse border border-gray-300">
            <thead>
              <tr>
                <th class="px-4 py-2 border border-gray-300 text-left">Mesa</th>
                <th class="px-4 py-2 border border-gray-300 text-left">Pedidos</th>
                <th class="px-4 py-2 border border-gray-300 text-left">Total</th>
              </tr>
            </thead>
            <tbody>
              {#each mesas as mesa}
                <tr>
                  <td class="px-4 py-2 border border-gray-300">Mesa {mesa.numero}</td>
                  <td class="px-4 py-2 border border-gray-300">
                    {#each mesa.pedido as item}
                      <span>{item}</span>{#if item !== mesa.pedido[mesa.pedido.length - 1]} , {/if}
                    {/each}
                  </td>
                  <td class="px-4 py-2 border border-gray-300">{mesa.total.toFixed(2)} R$</td>
                </tr>
              {/each}
            </tbody>
          </table>
        {/if}
      </div>
    {/if}
  </div>
</main>
</div>
