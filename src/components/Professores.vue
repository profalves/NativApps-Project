<template>
  <div>
    <q-fixed-position class="fixo" corner="bottom-right" :offset="[18, 18]">
      <q-btn 
         round
         color="primary" 
         @click="novo()">
         <q-icon name="add" />
      </q-btn>
    </q-fixed-position>
    
    <q-data-table
      style="background-color: white" 
      :data="professores"
      :config="config"
      :columns="columns"
      @refresh="refresh"
      @selection="selection"
      @rowclick="rowClick"
    >
      <template slot="col-message" slot-scope="cell">
        <span class="light-paragraph">{{cell.data}}</span>
      </template>
      
      <!--
      <template slot="col-source" slot-scope="cell">
        <div v-if="cell.data === 'Audit'" class="my-label text-white bg-primary">
          Audit
          <q-tooltip>Some data</q-tooltip>
        </div>
        <div v-else class="my-label text-white bg-negative">{{cell.data}}</div>
      </template>
      -->

      <template slot="selection" slot-scope="props">
        <q-btn flat color="primary" @click="editar(props)" v-if="selecionados<2">
          <q-icon name="edit" />
        </q-btn>
        <q-btn flat color="primary" @click="deleteRow(props)">
          <q-icon name="delete" />
        </q-btn>
      </template>
    </q-data-table>

    <q-modal ref="modal">
      <div class="layout-padding">
        <h4>{{title}}</h4>
        {{subtitle}}
        <hr>
        <div>
          <q-input v-model="professor.nome" float-label="Nome" />
          <q-input v-model="professor.sobrenome" float-label="Sobrenome" />
          <q-select
            v-model="professor.genero"
            float-label="Gênero"
            :options="[
              {label: 'Masculino', value: 'M'},
              {label: 'Feminino', value: 'F'}
            ]"
          />

        </div>
        <br>
        
        <div class="text-right">
          <q-btn color="faded" @click="$refs.modal.close()">Fechar</q-btn>
          <q-btn color="positive" @click="save">Salvar</q-btn>
        </div>
        
        
      </div>
      
    </q-modal>

  </div>
</template>

<script>
/* eslint-disable */
import {
  QDataTable,
  QField,
  QInput,
  QCheckbox,
  QSelect,
  QSlider,
  QBtn,
  QIcon,
  QTooltip,
  QCollapsible,
  QModal,
  QModalLayout,
  QFixedPosition,
  clone,
  Loading,
  Toast
} from 'quasar'
import axios from 'axios'
import API from './api.js'
export default {
  components: {
    QDataTable,
    QField,
    QInput,
    QCheckbox,
    QSelect,
    QSlider,
    QBtn,
    QIcon,
    QTooltip,
    QCollapsible,
    QModal,
    QModalLayout,
    QFixedPosition
  },
  data () {
    return {
      title: '',
      subtitle: '',
      API,
      professor: {},
      professores: [],
      editando: false,
      selecionados: 0,
      config: {
        title: '<h5>Professores Cadastrados</h5>Selecione um registro para editar ou excluir',
        refresh: true,
        noHeader: false,
        columnPicker: true,
        leftStickyColumns: 0,
        rightStickyColumns: 2,
        bodyStyle: {
          maxHeight: '500px'
        },
        rowHeight: '50px',
        responsive: true,
        pagination: {
          rowsPerPage: 15,
          options: [5, 10, 15, 30, 50, 500]
        },
        selection: 'multiple',
        messages: {
          noData: '<i class="material-icons">warning</i> Não há dados para exibir.',
          noDataAfterFiltering: '<i class="material-icons">warning</i> Sem resultados.'
        },
        labels: {
          columns: 'Colunas',
          allCols: 'Todos',
          rows: 'Linhas',
          selected: {
            singular: 'item selecionado.',
            plural: 'itens selecionados.'
          },
          clear: 'Limpar',
          search: 'Procurar',
          all: 'Todos'
        }
      },
      columns: [
        {
          label: 'Nome',
          field: 'nome',
          width: '200px',
          classes: 'bg-grey-5',
          filter: true,
          sort: true,
        },
        {
          label: 'Sobrenome',
          field: 'sobrenome',
          width: '200px',
          sort: true,
        },
        {
          label: 'Genero',
          field: 'genero',
          width: '100px',
          sort: true,
          type: 'number'
        },
        {
          label: 'ID',
          field: '_id',
          filter: true,
          sort: true,
          type: 'string',
          width: '200px'
        }
      ],
      pagination: true,
      rowHeight: 50,
      bodyHeightProp: 'maxHeight',
      bodyHeight: 500
    }
  },
  methods: {
    listaProfessores(){
      Loading.show()
      axios.get(this.API + 'professor')
      .then((res) => {
        Loading.hide()
        //console.log(res.data)
        this.professores = res.data
      })
      .catch((e) => {
        Loading.hide()
        console.log(e.response)
        Toast.create('A lista de Professores não foi obtida!')
      })
    },
    editar (props) {
      this.title = 'Editar Cadastro'
      this.editando = true
      console.log(props)
      this.professor = props.rows[0].data
      this.subtitle = 'Professor: ' + this.professor.nome
      this.$refs.modal.open()
    },
    deleteRow (props) {
      props.rows.forEach(row => {
        console.info(row.data)
        this.excluir(row.data)
      })
    },
    excluir(item){
      Loading.show()
      axios.delete(this.API + 'professor/' + item._id)
      .then((res) => {
        Loading.hide()
        console.log(res.data)
        this.listaProfessores()
      })
      .catch((e) => {
        Loading.hide()
        console.log(e.response)
        Toast.create(e.responde.data.message)
      })     
    },
    refresh (done) {
      this.timeout = setTimeout(() => {
        this.listaProfessores()
        done()
      }, 3000)
    },
    selection (number, rows) {
      console.log(`selected ${number}: ${rows}`)
      this.selecionados = number
    },
    rowClick (row) {
      console.log('clicked on a row', row)
      this.editando = true
      this.title = 'Editar Cadastro'
      this.professor = row
      this.subtitle = 'Professor: ' + this.professor.nome + ' ' + this.professor.sobrenome
      this.$refs.modal.open()
    },
    novo(){
      this.title = 'Novo Cadastro'
      this.subtitle = ''
      this.professor = {
        nome: '',
        sobrenome: '',
        genero: '',
      },
      this.$refs.modal.open()
    },
    save(){
      if(this.editando === true){
        console.log(this.professor._id)
        let data = JSON.stringify(this.professor);
        let xhr = new XMLHttpRequest();
        
        xhr.withCredentials = true;
        xhr.addEventListener("readystatechange", function () {
          if (this.readyState === 4) {
            console.log(this.responseText);
          }
        });

        xhr.open("PUT", this.API + 'professor/' + this.professor._id);
        xhr.setRequestHeader("Content-Type", "application/json");

        xhr.send(data);
        this.$refs.modal.close()
         
      }
      else{
        Loading.show()
        axios.post(this.API + 'professor', this.professor)
        .then((res) => {
          Loading.hide()
          // console.log(res.data)
          this.$refs.modal.close()
          this.listaProfessores()
        })
        .catch((e) => {
          Loading.hide()
          console.log(e.response)
          Toast.create({
            html: 'O registro não foi enviado, certifique-se que todos os dados foram preenchidos. Caso contrário, a conexão com o servidor não foi estabelecida',
            icon: 'warning',
            timeout: 7000,
          })
        })
      }
    }
  },
  beforeDestroy () {
    clearTimeout(this.timeout)
  },
  watch: {
    pagination (value) {
      if (!value) {
        this.oldPagination = clone(this.config.pagination)
        this.config.pagination = false
        return
      }
      this.config.pagination = this.oldPagination
    },
    rowHeight (value) {
      this.config.rowHeight = value + 'px'
    },
    bodyHeight (value) {
      let style = {}
      if (this.bodyHeightProp !== 'auto') {
        style[this.bodyHeightProp] = value + 'px'
      }
      this.config.bodyStyle = style
    },
    bodyHeightProp (value) {
      let style = {}
      if (value !== 'auto') {
        style[value] = this.bodyHeight + 'px'
      }
      this.config.bodyStyle = style
    }
  },
  mounted(){
    this.listaProfessores()
  }
}
</script>

<style lang="stylus">
.my-label
  padding 5px
  border-radius 3px
  display inline-block
</style>