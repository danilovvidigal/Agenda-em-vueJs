<template>
  <div>
    <div class="d-flex flex-row justify-content-evenly">
      <div>
        <img src="../assets/ic-logo.png" alt="" class="" />
      </div>
      <button 
        v-if="contatos.length > 0"
        type="button"
        class="btn btn-primary ml-5 cores-button"
        data-toggle="modal"
        data-target="#exampleModal"
        data-whatever="@mdo"
      > 
      <i class="bi bi-plus"></i>
        Criar Contato
      </button>
    
      <div class="w-75 ml-auto">
        <input type="text"
          class="form-control cor-input "
          placeholder="Buscar..."
          aria-label="Username" 
          aria-describedby="basic-addon1" 
        />
      </div>
    </div>

    <!-- Image -->
    <div v-if="contatos.length === 0">
      <div class="agenda">
        <img src="../assets/ic-book@3x.png" alt="" class="agenda-tamanho" />
      </div>
      <p class="alinhamento-contato">Nenhum contato foi criado ainda.</p>
    </div>

    <!-- Tabela -->
    <table class="table mt-3 border border-light" v-if="contatos.length > 0">
      <thead>
        <tr class="text-secondary">
          <th scope="col">Contatos</th>
          <th scope="col">E-mail</th>
          <th scope="col">Telefone</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(contato) in contatos"
          :key="contato"
          v-bind:id="contato.id"
        >
          <th scope="col">{{ contato.nome }}</th>
          <td scope="col">{{ contato.email }}</td>
          <td scope="col">{{ contato.telefone }}</td>
          <td scope="col">
            <button class="btn btn-secondary mr-2" data-toggle="modal" data-target="#exampleModal1">
              <i class="bi bi-trash-fill"></i>
            </button>
            <button
              class="btn btn-secondary"
              @click="editar(contato)"
              data-toggle="modal"
              data-target="#editarModal"
            >
              <i class="bi bi-pencil-fill"> </i>
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Button -->
    <div class="button-alinhamento">
      <button
        v-if="contatos.length === 0"
        type="button"
        class="btn btn-primary cores-button"
        data-toggle="modal"
        data-target="#exampleModal"
        data-whatever="@mdo"
      >
      <i class="bi bi-plus"></i>
        Criar Contato
      </button>

      <!-- Modal -->
      <div
        class="modal fade"
        id="exampleModal"
        tabindex="-1"
        role="dialog"
        aria-labelledby="exampleModalLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">
                Criar novo contato
              </h5>
              <button
                type="button"
                class="close"
                data-dismiss="modal"
                aria-label="Cancelar"
              >
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <form>
                <div class="form-group">
                  <input
                    type="text"
                    class="form-control"
                    id="recipient-name"
                    placeholder="Nome: "
                    v-model="nome"
                  />
                </div>
                <div class="form-group">
                  <input
                    type="text"
                    class="form-control"
                    id="recipient-name"
                    placeholder="Email: "
                    v-model="email"
                  />
                </div>
                <div class="form-group">
                  <input
                    type="text"
                    class="form-control w-50"
                    id="message-text"
                    placeholder="Telefone: "
                    v-model="telefone"
                    @change="desabilitarButton"
                  />
                </div>
              </form>
            </div>
            <div class="modal-footer" id="app">
              <button
                type="button"
                class="btn btn text-warning cancelar"
                data-dismiss="modal"
              >
                Cancelar
              </button>
              <div data-dismiss="modal">
                <button
                  type="button"
                  class="btn btn-warning button-redondo cor-botao"
                  @click="salvarcontato"
                  :disabled="!nome && !email && !telefone"
                >
                  Salvar
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
      <!-- Modal Editar -->
    <div
      class="modal fade"
      id="editarModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="editarModal"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="editarModal">Editar contato</h5>
            <button
              type="button"
              class="close cancelar"
              data-dismiss="modal"
              aria-label="Cancelar"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          
          <div class="modal-body">
            <form>
              <div class="form-group">
                <input
                  type="text"
                  class="form-control"
                  id="recipient-name"
                  placeholder="Nome:"
                  v-model="editarContato.nome"
                />
              </div>
              <div class="form-group">
                <input
                  type="text"
                  class="form-control"
                  id="recipient-name"
                  placeholder="E-mail:"
                  v-model="editarContato.email"
                />
              </div>
              <div class="form-group">
                <input
                  type="text"
                  class="form-control"
                  id="message-text"
                  placeholder="Telefone:"
                  v-model="editarContato.telefone"
                  @change="desabilitarButton"
                />
              </div>
            </form>
          </div>
          <div class="modal-footer" id="app">
            <button
              type="button"
              class="btn btn text-warning cancelar"
              data-dismiss="modal"
            >
              Cancelar
            </button>
            <div data-dismiss="modal">
              <button
                type="button"
                class="btn btn-warning button-redondo cor-botao"
                @click="editaContato"
                :disabled="
                  !editarContato.nome &&
                  !editarContato.email &&
                  !editarContato.telefone
                "
              >
                Editar
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Modal Cancelar-->
    <div class="modal" tabindex="-1" role="dialog" id="exampleModal1">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Excluir Contato</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <p class="d-flex">Deseja realmente excluir o contato?</p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn text-warning cancelar" data-dismiss="modal">Cancelar</button>
            <button type="button" class="btn btn-warning button-redondo cor-botao" data-dismiss="modal"  @click="deletar(index)">Confirma</button>
          </div>
        </div>
      </div>
    </div>


  </div>
</template>

<script>
export default {
  data() {
    return {
      semContato: false,
      desabilitar: false,
      poscontato: [],
      contatos: [],
      nome: null,
      email: null,
      telefone: null,
      editarContato: {
        id: null,
        nome: null,
        email: null,
        telefone: null,
      },
    };
  },
  created() {},

  methods: {
    desabilitarButton() {
      if (this.nome && this.telefone && this.email) {
        this.desabilitar = true;
      } else {
        this.desabilitar = false;
      }
    },

    salvarcontato() {
      try {
        let newContato = {
          nome: this.nome,
          email: this.email,
          telefone: this.telefone,
          id: this.contatos.length + 1,
        }
        this.contatos.push(newContato);
        this.nome = null;
        this.email = null;
        this.telefone = null;
        this.closeModal = false;
        setTimeout(function insertColor () {
          let seletor = document.querySelector("[id='" + newContato.id + "']")
          seletor.className = "inserted";
        }, 250);
        setTimeout(function removeColor () {
          let seletor = document.querySelector("[id='" + newContato.id + "']")
          seletor.classList.remove("inserted")
        }, 10000);
      } catch (error) {
        console.log(error);
      }
    },

    deletar(parametro) {
      this.contatos.splice(parametro, 1);
    },

    validaDisabled() {
      return !this.nome && !this.telefone && !this.email;
    },

    editar(contato) {
      this.editarContato = {
        nome: contato.nome,
        email: contato.email,
        telefone: contato.telefone,
        id: contato.id,
      };
    },

    editaContato() {
      this.contatos = this.contatos.map((contato) => {
        if (contato.id == this.editarContato.id) {
          contato = {
            nome: this.editarContato.nome,
            email: this.editarContato.email,
            telefone: this.editarContato.telefone,
            id: this.editarContato.id,
          };
        }

        return contato;
      });
      this.editarContato = {};
    },
  },
};
</script>

<style>
  
.agenda {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.agenda-tamanho {
  width: 300px;
  display: flex;
  align-self: center;
  padding: 50px 10px 3px 8px;
}

.alinhamento-contato {
  padding: 20px;
}

.button-alinhamento {
  padding: 20px;
}

.cores-button {
  color: #fcb411eb !important;
  background-color: #b8f71f78 !important;
  border-color: #ff000000 !important;
  border-radius: 20px;
  border-radius: 20px;
  width: 160px;
}

.button-redondo {
  border-radius: 15px;
}

.destaque {
  background-color: #eec6c2;
}

tr:hover {
  background-color: #fff3f2;
  border: solid 1px #e1e1e1;
}

.inserted {
  background-color: #fff3f2 !important;
  border: solid 1px #e1e1e1 !important;
}

.cor-botao {
  background-color: #fa7268 !important;
}

.cancelar {
  color: #fa7268 !important;
}

.cor-input {
  background-color: #f2f1f1 !important;
}
</style>