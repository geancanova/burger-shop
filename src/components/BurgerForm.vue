<template>
  <div>

    <Message :msg="msg" />

    <form id="burger-form" @submit="createBurger">
      <div class="input-container">
        <label class="form-label" for="name">Nome do cliente:</label>
        <input type="text" id="name" name="name" v-model="name" placeholder="Digite o seu nome" required>
      </div>
      <div class="input-container">
        <label class="form-label" for="bread">Escolha o pão:</label>
        <select name="bread" id="bread" v-model="bread">
          <option value="">Selecione seu pão</option>
          <option v-for="bread in breads" :key="bread.id" :value="bread.tipo" required>{{ bread.tipo }}</option>
        </select>
      </div>
      <div class="input-container">
        <label class="form-label" for="meat">Escolha a carne:</label>
        <select name="meat" id="meat" v-model="meat">
          <option value="">Selecione sua carne</option>
          <option v-for="meat in meats" :key="meat.id" :value="meat.tipo" required>{{ meat.tipo }}</option>
        </select>
      </div>
      <div class="input-container">
        <label class="form-label">Escolha os opcionais:</label>
        <div class="checkbox-wrapper">
          <label class="checkbox-container" v-for="optional in optionals_data" :key="optional.id">
            <input type="checkbox" name="optionals" v-model="optionals" :value="optional.tipo">
            <span>{{ optional.tipo }}</span>
          </label>
        </div>
      </div>
      <div class="input-container">
        <input type="submit" class="submit-btn" value="Criar meu Burger">
      </div>
    </form>

  </div>
</template>

<script>
import Message from './Message.vue'
export default {
  name: 'BurgerForm',
  components: {
    Message
  },
  data() {
    return {
      breads: null,
      meats: null,
      optionals_data: null,
      name: null,
      bread: null,
      meat: null,
      optionals: [],
      msg: null
    }
  },
  methods: {
    async getIngredients() {
      const req = await fetch('//localhost:3000/ingredientes');
      const data = await req.json();

      this.breads = data.paes;
      this.meats = data.carnes;
      this.optionals_data = data.opcionais;
    },
    async createBurger(e) {
      e.preventDefault();

      const data = {
        name: this.name,
        meat: this.meat,
        bread: this.bread,
        optionals: Array.from(this.optionals),
        status: 'Solicitado'
      }
      const dataJson = JSON.stringify(data);
      const req = await fetch('//localhost:3000/burgers', {
        method: 'POST',
        headers: { 'Content-type': 'application/json' },
        body: dataJson
      });
      const res = await req.json();

      // Mensagem do sistema
      this.msg = `Pedido Nº ${res.id} realizado com sucesso! =)`;

      // Limpar campos
      this.name = '';
      this.meat = '';
      this.bread = '';
      this.optionals = [];
    }
  },
  mounted() {
    this.getIngredients();
  }
}
</script>

<style scoped>
  #burger-form {
    max-width: 400px;
    margin: 0 auto;
  }

  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }

  .form-label {
    border-left: 4px solid var(--color-secondary);
    font-weight: 700;
    margin-bottom: 10px;
    padding: 5px 10px;
  }

  input[type=text],
  select {
    border-radius: 5px;
    border: 2px solid var(--color-black);
    font-size: 16px;
    padding: 10px;
  }

  .checkbox-wrapper {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 15px;
  }

  .checkbox-container {
    display: flex;
    position: relative;
    align-items: center;
  }

  .checkbox-container span {
    position: relative;
    cursor: pointer;
    font-weight: 700;
  }

  .checkbox-container span::before {
    content: '';
    display: inline-block;
    vertical-align: top;
    height: 15px;
    width: 15px;
    border: 2px solid var(--color-black);
    border-radius: 4px;
    margin-right: 5px;
  }

  .checkbox-container span::after {
    content: '';
    position: absolute;
    top: 4px;
    left: 4px;
    display: inline-block;
    height: 11px;
    width: 11px;
    background-color: var(--color-primary);
    border-radius: 2px;
    margin-right: 5px;
    opacity: 0;
    transition: var(--transition-default);
  }

  .checkbox-container input {
    position: absolute;
    opacity: 0;
  }

  .checkbox-container input:checked + span::after {
    opacity: 1;
  }

  .submit-btn {
    border: none;
    border-radius: 10px;
    background-color: var(--color-primary);
    color: var(--color-white);
    cursor: pointer;
    font-size: 20px;
    font-weight: 700;
    transition: var(--transition-default);
    padding: 1em 2em;
  }

  .submit-btn:hover {
    background-color: var(--color-primary-accent);
  }
</style>