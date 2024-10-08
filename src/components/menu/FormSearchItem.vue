<template>
  <div class="container-data small-data">
    <form>
      <p class="hint">Encontre um produto já cadastrado na plataforma.</p>
      <div class="row">
        <div class="col-lg-10 d-grid gap-2">
          <div class="w-full search-input">
            <input 
              type="text" 
              class="form-control" 
              placeholder="Buscar itens do cardápio" 
              v-model="productTerm" 
              @input="searchProduct"
            >
            <!-- Exibir a lista de resultados apenas se houver um termo de pesquisa e resultados -->
            <ul v-if="productTerm.length > 0 && searchProducts.length > 0" class="w-full rounded search-list" role="list">
              <li v-for="(product, index) in searchProducts" :key="product.id">
                <span>{{ product.name }}</span> 
                <span 
                  role="button" 
                  class="add-item add-right add-on" 
                  @click="selectProduct(product)"
                ></span>
              </li>
            </ul>
          </div>
        </div>
        <div class="col-lg-2 d-grid gap-2 mb-2">
          <button @click.prevent="" type="submit" class="btn btn-save">
            <span class="add-item add-inline icon-search"></span>
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
import { ref, computed } from 'vue';
import { useStore } from 'vuex';

export default {
  name: "FormSearchItem",
  props: {
    categoryId: {  // Aceitar um único ID de categoria
      type: Number,  // Pode ser número único
      required: true
    }
  },
  setup(props, { emit }) {
    const store = useStore();
    const productTerm = ref(''); // Termo de busca do produto
    const searchProducts = ref([]); // Resultados da busca de produtos

    const restaurantId = computed(() => store.state.restaurantId);
    const token = computed(() => store.state.token);

    // Função para buscar produtos com base no termo digitado
    const searchProduct = async () => {
      if (productTerm.value.trim().length === 0) {
        // Limpar a lista se o campo de busca estiver vazio
        searchProducts.value = [];
        return;
      }

      if (!restaurantId.value || !token.value) {
        console.error("restaurantId ou token está indefinido");
        return;
      }

      try {
        const response = await axios.get(`https://api.prattuapp.com.br/api/products`, {
          params: {
            name: productTerm.value,
            restaurant_id: restaurantId.value
          },
          headers: {
            Authorization: `Bearer ${token.value}`
          }
        });
        searchProducts.value = response.data.data;
      } catch (error) {
        console.error('Erro ao buscar produtos:', error);
      }
    };

    // Função para selecionar um produto e adicionar à categoria
    const selectProduct = async (product) => {
      const categoryIds = props.categoryId ? [props.categoryId] : [];

      if (!categoryIds.length) {
        console.error("Nenhuma categoria foi selecionada");
        return;
      }

      try {
        console.log('Adicionando produto à(s) categoria(s):', categoryIds);
        const response = await axios.post(`https://api.prattuapp.com.br/api/category/add-product`, {
          product_id: product.id,
          categories: categoryIds  // Enviamos como array, mesmo sendo um único ID
        }, {
          headers: {
            Authorization: `Bearer ${token.value}`
          }
        });

        if (response.status === 200) {
          store.dispatch('fetchMenusAndItems');
          store.dispatch('fetchFormCategoriesAndComplements');
          emit('saveItem', product);
          emit('closeModal');
        }
      } catch (error) {
        console.error('Erro ao adicionar produto à(s) categoria(s):', error);
      }
    };

    return {
      productTerm,
      searchProducts,
      searchProduct,
      selectProduct
    };
  }
};
</script>

<style lang="scss" scoped>
.modal-body .small-data {
  width: 400px !important;
  padding: 0 !important;
}

.search-list {
  position: relative !important;
  top: 5px !important;
}
</style>

