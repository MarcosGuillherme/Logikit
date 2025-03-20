<template>
  <div class="p-4 max-w-xl mx-auto flex">
    <!-- Quadro de palavras-chave flutuante -->
    <div
      class="fixed left-4 top-16 p-4 bg-white shadow-lg rounded-lg border border-gray-300 w-48"
    >
      <h2 class="text-md font-bold mb-2">Palavras-chave</h2>
      <ul class="kw-list">
        <li
          v-for="(color, keyword) in keywords"
          :key="keyword"
          :class="color"
          class="flex justify-between kw-item"
        >
          <span>{{ keyword }}</span>
          <span
            @click="removeKeyword(keyword)"
            class="pd_del cursor-pointer fa-solid fa-trash"
          ></span>
        </li>
      </ul>
      <!-- Adicionar nova palavra-chave -->
      <div class="mb-4 flex gap-2">
        <input
          v-model="newKeyword"
          placeholder="Palavra-chave"
          class="border p-2 w-full"
        />
        <button
          @click="addKeyword"
          class="p-2 bg-yellow-500 text-white rounded"
        >
          Adicionar
        </button>
      </div>
    </div>

    <!-- Editor de lógica -->
    <div class="flex-1 ml-56">
      <h1 class="text-xl font-bold mb-2">Editor de Lógica</h1>
      <div v-for="(logic, index) in logics" :key="index" class="mb-4">
        <input
          v-model="logic.name"
          placeholder="Nome da lógica"
          class="mb-2 border p-2 w-full"
        />
        <textarea
          v-model="logic.content"
          placeholder="Digite a lógica..."
          class="mb-2 border p-2 w-full"
        ></textarea>
      </div>
      <button @click="addLogic" class="p-2 bg-blue-500 text-white rounded mb-2">
        Adicionar Nova Lógica
      </button>
      <button
        @click="exportToPDF"
        class="p-2 bg-green-500 text-white rounded mb-4"
      >
        Exportar para PDF
      </button>

      <!-- Exibir lógica formatada -->
      <div>
        <div v-for="(logic, index) in logics" :key="index" class="mb-4">
          <h2 class="text-lg font-bold">
            {{ logic.name || `Lógica ${index + 1}` }}
          </h2>
          <div
            v-html="formatLogic(logic.content)"
            class="p-2 border-l-4 border-blue-500"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import jsPDF from "jspdf";
import { createClient } from "@supabase/supabase-js";

const supabaseUrl = "https://yucvlughhxedsnifuxip.supabase.co";
const supabaseAnonKey =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Inl1Y3ZsdWdoaHhlZHNuaWZ1eGlwIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDE4OTY2NTYsImV4cCI6MjA1NzQ3MjY1Nn0.UlT0oSGlsEBkM6CxJRu2LPmqdJpbZZDAVKOqiQLY7k8";
const supabase = createClient(supabaseUrl, supabaseAnonKey);

export default {
  setup() {
    const logics = ref([{ name: "", content: "" }]);
    const newKeyword = ref("");
    const keywords = ref({});

    const fetchKeywords = async () => {
      const { data, error } = await supabase.from("tb_keywords").select("*");
      if (data) {
        const keywordsData = {};
        data.forEach((item) => {
          keywordsData[item.keyword] = item.color;
        });
        keywords.value = keywordsData;
      } else {
        console.error("Erro ao buscar palavras-chave:", error);
      }
    };

    onMounted(fetchKeywords);

    const addKeyword = async () => {
      if (newKeyword.value.trim() && !keywords.value[newKeyword.value]) {
        const { error } = await supabase
          .from("tb_keywords")
          .insert([
            { keyword: newKeyword.value, color: "text-blue-600 font-bold" },
          ]);
        if (!error) {
          newKeyword.value = "";
          fetchKeywords();
        } else {
          console.error("Erro ao adicionar palavra-chave:", error);
        }
      }
    };

    const removeKeyword = async (keyword) => {
      const { error } = await supabase
        .from("tb_keywords")
        .delete()
        .eq("keyword", keyword);
      if (!error) {
        fetchKeywords();
      } else {
        console.error("Erro ao remover palavra-chave:", error);
      }
    };

    const addLogic = () => {
      if (
        logics.value.length === 0 ||
        logics.value[logics.value.length - 1].content.trim() !== ""
      ) {
        logics.value.push({ name: "", content: "" });
      }
    };

    const formatLogic = (content) => {
      let formatted = content;
      Object.keys(keywords.value).forEach((keyword) => {
        const regex = new RegExp(`\\b${keyword}\\b`, "gi");
        formatted = formatted.replace(
          regex,
          `<span class='${
            keywords.value[keyword]
          }'>${keyword.toUpperCase()}</span>`
        );
      });
      formatted = formatted.replace(
        /\bv_[a-zA-Z_]+\b/g,
        "<span class='text-red-600 font-bold'>$&</span>"
      );
      return formatted;
    };

    const exportToPDF = () => {
      const doc = new jsPDF();
      logics.value.forEach((logic, index) => {
        if (index > 0) doc.addPage();
        doc.text(logic.name || `Lógica ${index + 1}`, 10, 10);
        doc.text(logic.content, 10, 20);
      });
      doc.save("logicas.pdf");
    };

    return {
      logics,
      newKeyword,
      keywords,
      addKeyword,
      addLogic,
      formatLogic,
      exportToPDF,
      removeKeyword,
    };
  },
};
</script>

<style src="../style.css"></style>
