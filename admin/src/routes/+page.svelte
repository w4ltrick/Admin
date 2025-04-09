<script>
    import { goto } from "$app/navigation";
  
  
      function irParaLogin() {
        goto('/login');
      }
    </script>
    
    <button on:click={irParaLogin}>
      Ir para Login
    </button>
    