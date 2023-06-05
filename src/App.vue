<script setup>
import { ref } from 'vue'
const produtos = ref([
  {
    id: 1,
    nome: 'Camiseta',
    preco: 49.9,
    quantidade: 1
  },
  {
    id: 2,
    nome: 'Calça',
    preco: 99.9,
    quantidade: 1
  },
  {
    id: 3,
    nome: 'Meia',
    preco: 9.9,
    quantidade: 1
  },
  {
    id: 4,
    nome: 'Sapato',
    preco: 199.9,
    quantidade: 1
  },
  {
    id: 5,
    nome: 'Boné',
    preco: 29.9,
    quantidade: 1
  },
  {
    id: 6,
    nome: 'Óculos',
    preco: 99.9,
    quantidade: 1
  },
  {
    id: 7,
    nome: 'Relógio',
    preco: 299.9,
    quantidade: 1
  },
  {
    id: 8,
    nome: 'Bermuda',
    preco: 79.9,
    quantidade: 1
  },
  {
    id: 9,
    nome: 'Cueca',
    preco: 19.9,
    quantidade: 1
  },
  {
    id: 10,
    nome: 'Meia',
    preco: 9.9,
    quantidade: 1
  }
])
const carrinho = ref([])
let exibirModal = ref(false)
let valorTotal = ref(0)

function incrementar(cod) {
  produtos.value[cod].quantidade++
}
function decrementar(cod) {
  if (produtos.value[cod].quantidade > 1) {
    produtos.value[cod].quantidade--
  }
}

function addToCart(cod, name, quant, price) {
  // carrinho.value.find(prod => prod.id === 1);
  if (carrinho.value.findIndex((prod) => prod.cod === cod) > -1) {
    alert(`Item já consta na lista!`)
  } else {
    // const newItem = { codNI: cod, nameNI: name, quantNI: quant, priceNI: price }
    // const newItem = { cod: cod, name: name, quant: quant, price: price }
    carrinho.value.push({ cod, name, quant, price })
  }
  inCart()
  total()
}

function inCart() {
  if (carrinho.value.length > 0) {
    exibirModal.value = true
  } else {
    exibirModal.value = false
  }
}

function deleteItems() {
  carrinho.value = []
  total()
}

function removeItem(cod) {
  carrinho.value.splice(
    carrinho.value.findIndex((prod) => prod.cod == cod),
    1
  )
  total()
}

function total() {
  valorTotal.value = 0
  valorTotal.value = carrinho.value.reduce(getTotal, 0)
}
function getTotal(total, item) {
  return Number(total) + item.quant * item.price
}
</script>

<template>
  <div class="container text-left">
    <div class="row">
      <div class="col-5">
        <!-- Button trigger modal -->
        <button
          type="button"
          class="btn btn-md"
          :class="valorTotal > 0 ? 'btn-primary' : 'btn-secondary'"
          data-bs-toggle="modal"
          data-bs-target="#carrinho"
        >
          <i class="bi bi-cart-fill"></i> Ver Carrinho
        </button>
      </div>
      <!-- Modal -->
      <div
        class="modal fade"
        id="carrinho"
        tabindex="-1"
        aria-labelledby="carrinhoLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog modal-dialog-centered">
          <div class="modal-content">
            <div class="modal-header">
              <h1 class="modal-title fs-5" id="carrinhoLabel">Itens Adicionados</h1>
              <button
                type="button"
                class="btn-close"
                data-bs-dismiss="modal"
                aria-label="Close"
              ></button>
            </div>
            <div class="modal-body">
              <div class="row">
                <div class="col-12">
                  <ul class="list-group">
                    <li v-for="item in carrinho" :key="item.cod" class="list-group-item">
                      <b>{{ item.cod }} - {{ item.name }}</b> <br />Preço:
                      {{
                        item.price.toLocaleString('pt-br', { style: 'currency', currency: 'BRL' })
                      }}
                      | Quant: {{ item.quant }} (Total:
                      {{
                        (item.price * item.quant).toLocaleString('pt-br', {
                          style: 'currency',
                          currency: 'BRL'
                        })
                      }})
                      <br />
                      <button @click="removeItem(item.cod)">
                        <i class="bi bi-trash-fill"></i>Remover
                      </button>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
            <div class="modal-footer">
              Total a Pagar:
              {{ valorTotal.toLocaleString('pt-br', { style: 'currency', currency: 'BRL' }) }}
              <button @click="deleteItems()" type="button" class="btn btn-warning">
                Limpar Carrinho
              </button>
              <button type="button" class="btn btn-dark" data-bs-dismiss="modal">Fechar</button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <hr />
    <div class="row">
      <div v-for="item in produtos" :key="item.id" class="col-3 prodItem">
        <b>{{ item.id }} - {{ item.nome }}</b> <br />Preço (em Reais):
        {{ item.preco.toLocaleString('pt-br', { style: 'currency', currency: 'BRL' }) }}
        <br />
        <label for="">Quantidade: {{ item.quantidade }}</label>
        <br />
        <button @click="decrementar(item.id - 1)">
          <i class="bi bi-dash-lg"></i>
        </button>
        <button @click="incrementar(item.id - 1)">
          <i class="bi bi-plus-lg"></i>
        </button>

        <button
          @click="addToCart(item.id, item.nome, item.quantidade, item.preco)"
          class="buttonAdd"
        >
          <i class="bi bi-check-square-fill"></i>Adicionar
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.prodItem {
  margin-bottom: 10px;
  border-bottom: 1px solid black;
}
.buttonAdd {
  margin-bottom: 5px;
}
</style>
