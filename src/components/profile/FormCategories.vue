<template>
    <form>
        <div class="row mb-2 mt-3">
            <div class="select-placeholder mb-3">
                <label v-if="selectCategory === ''">Selecione uma categoria</label>
                <select class="form-select" v-model="selectCategory">
                    <option v-for="(category, index) in allCategories" :key="index" :value="category">{{ category.name }}</option>
                </select>
            </div>
        </div>
        <div class="row mt-0">
            <div class="col-lg-6 d-grid gap-2">
                <button @click.prevent="$emit('closeModal')" type="button" class="btn btn-cancel" >Cancelar</button>
            </div>
            <div class="col-lg-6 d-grid gap-2">
                <button @click.prevent="" type="submit" class="btn btn-block" v-if="(selectCategory === '')">Salvar</button>
                <button @click.prevent="saveCategory" type="submit" class="btn btn-save" v-if="(selectCategory !== '')">Salvar</button>
            </div>
        </div>
    </form>
</template>

<script>
    import axios from 'axios';
    import store from '@/store';

    export default {
        name: "FormCategories",
        data() {
            return {
                selectCategory: "",
                allCategories: []
            } 
        },
        methods: {
            saveCategory() {
                this.$emit('saveModal', this.selectCategory);
                this.$emit('closeModal');
            }        
        },
        emits: ["closeModal", "saveModal"],
        props: {
            category: Object
        },
        async mounted() {
            this.selectCategory = this.category;
            const categoriesResponse = await axios.get(`https://api.prattuapp.com.br/api/categories-restaurant`, {
                headers: {
                    'Authorization': `Bearer ${store.state.token}`
                }
            });
            this.allCategories = categoriesResponse.data.categories;
        }
    };
</script>

<style scoped>
    .modal-container form {
        width: 450px !important;
    }

    .select-placeholder {
        label {
            left: 30px;
        }
    }
</style>