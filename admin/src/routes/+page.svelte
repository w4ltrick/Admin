<script>
  import { goto } from "$app/navigation";

  function irParaLogin() {
    goto('/login');
  }
</script>

<div class="min-h-screen flex items-center justify-center bg-gray-100">
  <button
    on:click={irParaLogin}
    class="bg-red-400 text-white px-6 py-2 rounded-full shadow-md hover:bg-red-600 hover:shadow-lg transition duration-300 ease-in-out active:scale-95"
  >
    Ir para Login
  </button>
</div>
