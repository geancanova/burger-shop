<template>
  <div class="table-wrapper">

    <Message :msg="msg" />

    <div v-if="burgers.length">
      <table class="orders-table">
        <thead>
          <tr>
            <th class="order-id">#</th>
            <th>Cliente</th>
            <th>Pão</th>
            <th>Carne</th>
            <th>Opcionais</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tfoot>
          <tr>
            <td colspan="6">
              <button class="delete-btn" @click="deleteAllOrders()">Limpar Pedidos</button>
            </td>
          </tr>
        </tfoot>
        <tbody>
          <tr v-for="burger in burgers" :key="burger.id">
            <td>{{ burger.id }}</td>
            <td>{{ burger.name }}</td>
            <td>{{ burger.bread }}</td>
            <td>{{ burger.meat }}</td>
            <td>
              <ul>
                <li v-for="(optional, index) in burger.optionals" :key="index">
                  {{ optional }}
                </li>
              </ul>
            </td>
            <td>
              <select name="status" @change="updateOrderStatus($event, burger.id)">
                <option value="">Selecione</option>
                <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status === s.tipo">
                  {{ s.tipo }}
                </option>
              </select>
              <button class="delete-btn" @click="deleteOrder(burger.id)">Cancelar</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-else>
      <p>Nenhum pedido cadastrado.</p>
    </div>
    
  </div>
</template>

<script>
import Message from '@/components/Message.vue';

export default {
  name: 'Dashboard',
  components: {
    Message
  },
  data() {
    return {
      burgers: [],
      burger_id: null,
      status: [],
      msg: null
    }
  }, 
  methods: {
    async getOrders() {
      const req = await fetch('//localhost:3000/burgers');
      const data = await req.json();

      if (!data) return;
      this.burgers = data;
      
      console.log(this.burgers);

      // Resgatar status
      this.getOrderStatus();

      console.log('get orders');
    },
    async getOrderStatus() {
      const req = await fetch('//localhost:3000/status');
      const data = await req.json();

      this.status = data;
    },
    async deleteOrder(id) {
      await fetch(`//localhost:3000/burgers/${id}`, {
        method: 'DELETE'
      });

      // Mensagem do sistema
      this.msg = `Pedido Nº ${id} removido com sucesso!`;
      
      // Update Orders
      this.getOrders();
    },
    async updateOrderStatus(e, id) {
      const option = e.target.value;
      const dataJson = JSON.stringify({ status: option });
      const req = await fetch(`//localhost:3000/burgers/${id}`, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: dataJson
      });
      const res = await req.json();

      // Mensagem do sistema
      this.msg = res.status ? `Pedido Nº ${res.id} atualizado para ${res.status}!` : '';
    },
    async deleteAllOrders() {
      const req = await fetch('//localhost:3000/burgers');
      const data = await req.json();
      await data.map((entry, i) => {

        // (NEEDS FIX) Prevent json server to crash
        setTimeout(() => {
          console.log(entry.id);
          fetch(`//localhost:3000/burgers/${entry.id}`, {
            method: 'DELETE'
          });
        }, 500 * i);
      });

      // (NEEDS FIX) Await all registers removal
      setTimeout(() => {
        // Mensagem do sistema
        this.msg = `Pedidos removidos com sucesso!`;

        // Update Orders
        this.getOrders();
      }, 500 * data.length);
    }
  },
  mounted() {
    this.getOrders();
  }
}
</script>

<style scoped>
  .table-wrapper {
    overflow: auto;
    max-width: 1200px;
    margin: auto;
  }
  .orders-table {
    min-width: 100%;
    border-collapse: collapse;
    table-layout: fixed;
  }

  th,
  td {
    padding: .7em 1em;
    vertical-align: top;
  }

  th {
    text-align: left;
    background-color: var(--color-black);
    color: var(--color-secondary);
    font-size: 18px;
  }

  th:first-child {
    width: 80px;
  }

  th:last-child {
    min-width: 236px;
    width: 236px;
  }

  tfoot td {
    background-color: var(--color-black);
    text-align: right;
  }

  tr:nth-child(even) {
    background-color: var(--color-gray);
  }

  ul {
    margin: 0;
    list-style-type: square;
  }

  select {
    padding: 5px 10px;
    border-radius: 5px;
    border: 2px solid var(--color-black);
  }

  .delete-btn {
    border: none;
    border-radius: 5px;
    background-color: var(--color-secondary);
    color: var(--color-black);
    cursor: pointer;
    font-size: 14px;
    font-weight: 700;
    transition: var(--transition-default);
    padding: .52em 1em;
    margin-left: 10px;
  }

  .delete-btn:hover {
    background-color: var(--color-secondary-accent);
  }
</style>