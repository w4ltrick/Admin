<script>
  import { goto } from "$app/navigation";

  let email = "";
  let password = "";

  function handleLogin() {
    goto("/dashboard");
  }

 
</script>

<div class="min-h-screen flex flex-col justify-center items-center bg-gray-100 px-4">

  <div class="bg-white shadow-2xl rounded-2xl p-8 w-full max-w-md">
    <h2 class="text-2xl font-semibold text-gray-800 mb-6 text-center">Acesse sua conta</h2>
    <form on:submit|preventDefault={handleLogin} class="space-y-4">
      <div>
        <label for="email" class="block text-sm font-medium text-gray-700 mb-1">Seu e-mail</label>
        <input
          bind:value={email}
          id="email"
          type="email"
          placeholder="exemplo@dominio.com"
          class="w-full px-4 py-2 rounded-xl border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:outline-none"
        />
      </div>
      <div>
        <label for="password" class="block text-sm font-medium text-gray-700 mb-1">Sua senha</label>
        <input
          bind:value={password}
          id="password"
          type="password"
          placeholder="••••••••"
          class="w-full px-4 py-2 rounded-xl border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:outline-none"
        />
      </div>
      <button
        type="submit"
        class="w-full bg-red-400 text-white py-2 rounded-xl hover:bg-red-600 transition duration-200"
      >
        Entrar
      </button>
    </form>
  </div>
</div>
