<script>
    import { goto } from '$app/navigation';
  
    let abaAtual = 'pedidos';
  
    function logout() {
      goto('/login');
    }
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
          </div>
        {:else if abaAtual === 'garcom'}
          <div>
            <h1 class="text-3xl font-bold text-gray-800 mb-2">Página do Garçom</h1>
            <p class="text-gray-600">Controle os garçons por aqui.</p>
          </div>
        {:else if abaAtual === 'mesas'}
          <div>
            <h1 class="text-3xl font-bold text-gray-800 mb-2">Página de Mesas</h1>
            <p class="text-gray-600">Visualize e edite as mesas do restaurante.</p>
          </div>
        {/if}
      </div>
    </main>
  </div>
  