<template>
  <div class="container-data">
    <form>
      <label for="name" class="form-label bold-500">Nome do cardápio</label>
      <div class="row">
        <div class="col-lg-8 d-grid gap-2">
          <input type="text" class="form-control" id="name" v-model="menuName" placeholder="Ex: Cardápio 1" />
        </div>
        <div class="col-lg-4 d-grid gap-2">
          <button @click.prevent="save()" type="submit" class="btn btn-save"><span class="add-item add-inline add-black ml-1"></span> Adicionar</button>
        </div>
        <p class="mt-2 required-alert" v-show="invalidMenuName">*Campo obrigatório</p>
        <p class="hint">Este nome não será exibido no aplicativo.</p>
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "FormMenu",
  data() {
    return {
      invalidMenuName: false,
      menuName: ""
    };
  },
  methods: {
    async save() {
      if (this.menuName.trim() === "") {
        this.invalidMenuName = true;
      } else {
        const restaurantId = this.$store.state.restaurantId;

        if (!restaurantId) {
          console.error('Erro: restaurantId não está definido!');
          return;
        }

        try {
          const response = await axios.post('https://api.prattuapp.com.br/api/menus', {
            name: this.menuName,
            restaurant_id: restaurantId
          }, {
            headers: {
              'Authorization': `Bearer ${this.$store.state.token}`
            }
          });

          const menuId = response.data.menu.id;

          this.$emit('save-menu-name', this.menuName);

          // Fecha o modal
          this.$emit('close-modal');

        } catch (error) {
          console.error('Erro ao criar o cardápio:', error);
        }
      }
    }
  }
};
</script>

<style scoped>
.modal-body .container-data {
  width: 500px;
  padding: 0 !important;
}
</style>
