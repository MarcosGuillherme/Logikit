<template>
  <div class="about-container">
    <div class="about-box">
      <h1 class="text-2xl font-bold mb-4">Sobre o Logikit</h1>
      <p class="mb-4">
        O <strong>Logikit</strong> é um editor de lógica projetado para
        facilitar a criação, edição e organização de lógicas personalizadas. Ele
        foi desenvolvido pensando em simplicidade, eficiência e acessibilidade,
        permitindo que você se concentre no que realmente importa: suas ideias e
        soluções.
      </p>
      <p class="mb-4">Com o Logikit, você pode:</p>
      <ul class="list-disc list-inside mb-4">
        <li>
          Destacar palavras-chave automaticamente para facilitar a leitura e
          compreensão.
        </li>
        <li>Organizar suas lógicas em um formato claro e estruturado.</li>
        <li>Exportar suas lógicas para PDF com apenas um clique.</li>
        <li>
          Personalizar palavras-chave e estilos para atender às suas
          necessidades.
        </li>
      </ul>
      <p class="mb-4">
        Este projeto foi feito com muito
        <span class="text-red-500 font-bold">amor</span> e dedicação para ajudar
        desenvolvedores, estudantes e entusiastas a trabalharem de forma mais
        produtiva e criativa.
      </p>
      <p class="mb-4">
        Esperamos que você aproveite o Logikit tanto quanto nós gostamos de
        criá-lo. Se tiver sugestões ou encontrar problemas, não hesite em entrar
        em contato!
      </p>
      <p class="text-sm text-gray-500">
        &copy; 2025 Logikit - Feito com ❤️ por Marcos.
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "AboutView",
};
</script>

<style scoped>
.about-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f9f9f9;
}

.about-box {
  background-color: #e5e5e5;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  width: 100%;
  text-align: center;
}

.about-box h1 {
  margin-bottom: 1rem;
}

.about-box p,
.about-box ul {
  margin-bottom: 1rem;
}
</style>
