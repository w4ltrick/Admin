<script>
    import { goto } from "$app/navigation";
  
    let abaAtual = "pedidos";
  
    function logout() {
      goto("/");
    }
  </script>
  
  <div class="min-h-screen bg-gray-100">
    <nav class="bg-white shadow flex justify-between items-center px-6 py-4">
      <div class="flex space-x-6 text-gray-700 font-medium">
        <button
          on:click={() => abaAtual = "pedidos"}
          class:selected={abaAtual === "pedidos"}
          class="hover:text-blue-600 focus:outline-none"
        >
          Pedidos
        </button>
        <button
          on:click={() => abaAtual = "garcom"}
          class:selected={abaAtual === "garcom"}
          class="hover:text-blue-600 focus:outline-none"
        >
          Garçom
        </button>
        <button
          on:click={() => abaAtual = "mesas"}
          class:selected={abaAtual === "mesas"}
          class="hover:text-blue-600 focus:outline-none"
        >
          Mesas
        </button>
      </div>
  
      <button
        on:click={logout}
        class="bg-red-500 text-white px-4 py-2 rounded-md hover:bg-red-600 transition"
      >
        Sair
      </button>
    </nav>
  
    <main class="p-6">
      {#if abaAtual === "pedidos"}
        <h2 class="text-xl font-semibold text-gray-800 mb-4">📦 Área de Pedidos</h2>
        <p>Aqui você pode gerenciar os pedidos em tempo real.</p>
      {:else if abaAtual === "garcom"}
        <h2 class="text-xl font-semibold text-gray-800 mb-4">🧑‍🍳 Área do Garçom</h2>
        <p>Acompanhe o status e movimentações dos garçons.</p>
      {:else if abaAtual === "mesas"}
        <h2 class="text-xl font-semibold text-gray-800 mb-4">🪑 Área de Mesas</h2>
        <p>Controle quais mesas estão ocupadas ou disponíveis.</p>
      {/if}
    </main>
  </div>
  