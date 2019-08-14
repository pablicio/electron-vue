<template>
  <b-container fluid>
    <!-- User Interface controls -->
    <h1 class="text-center m-t-80">Listagem de biliotecas</h1>
    <b-row>
      <b-col md="6" class="my-0">
        <b-form-group label-cols-sm="6" label class="mb-0">
          <b-input-group>
            <b-form-input v-model="filter" placeholder="Pesquisar..."/>
            <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''">Limpar</b-button>
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
      </b-col>
    </b-row>

    <b-row>
      <b-col md="6" class="my-1">
        <!-- Main table element -->
        <b-table
          head-variant="light"
          hover
          small
          bordered
          show-empty
          stacked="md"
          :items="items"
          :fields="fields"
          :current-page="currentPage"
          :per-page="perPage"
          :filter="filter"
          :sort-by.sync="sortBy"
          :sort-desc.sync="sortDesc"
          :sort-direction="sortDirection"
          @filtered="onFiltered"
        >
          <template slot="name" slot-scope="row">{{ row.item.name }}</template>
          <template slot="number" slot-scope="row">{{ row.item.number }}</template>
          <template slot="date" slot-scope="row">{{ row.item.date }}</template>

          <template slot="actions" slot-scope="row">
            <b-button
              size="sm"
              @click="test(row.item, row.index, $event.target)"
              class="mr-1"
            >itens</b-button>
            <b-button size="sm" class="mr-1">clonar</b-button>
          </template>
        </b-table>
      </b-col>

      <b-col md="6" class="my-1">
        <h3>Detalhes da biblioteca</h3>
        <h4>Nome: {{ detail.name }}</h4>
        <h4>Número: {{ detail.number }}</h4>
        <h4>Data: {{ detail.date }}</h4>

        <hr>
        <h4>Drogas associadas:</h4>
        <b-list-group v-for="itemBlio in detail.bibli">
          <b-list-group-item variant="info">{{itemBlio.name }} {{itemBlio.modo }}</b-list-group-item>
        </b-list-group>
      </b-col>
    </b-row>

    <b-row>
      <b-col md="6" class="my-1">
        <b-pagination
          size="sm"
          :total-rows="totalRows"
          :per-page="perPage"
          v-model="currentPage"
          class="my-0"
        />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>

const bibli1 = [
  {
    name: "Propofol",
    cca: "CAA-test",
    modo: "13ml/h",
    diluicao: "13%",
    selected: false
  },
  {
    name: "Sibazol",
    cca: "CAA-test",
    modo: "5ml/h",
    diluicao: "1%",
    selected: false
  },
  {
    name: "Dorflex",
    cca: "CAA-test",
    modo: "9ml/h",
    diluicao: "22%",
    selected: false
  },
]

const bibli2 = [
    {
    name: "Demerol",
    cca: "CAA-test",
    modo: "2ml/h",
    diluicao: "20%",
    selected: false
  },
  {
    name: "Insulina",
    cca: "CAA-test",
    modo: "4ml/h",
    diluicao: "5%",
    selected: false
  },
  {
    name: "Denil",
    cca: "CAA-test",
    modo: "1ml/h",
    diluicao: "8%",
    selected: false
  },
]

const bibli3 = [
  {
    name: "Denil",
    cca: "CAA-test",
    modo: "1ml/h",
    diluicao: "8%",
    selected: false
  },
  {
    name: "As",
    cca: "CAA-test",
    modo: "6ml/h",
    diluicao: "15%",
    selected: false
  },
  {
    name: "Tilenol",
    cca: "CAA-test",
    modo: "500ml/h",
    diluicao: "6%",
    selected: false
  },
]


const bibli4 = [
  {
    name: "Aspirina",
    cca: "CAA-test",
    modo: "9ml/h",
    diluicao: "11%",
    selected: false
  },
  {
    name: "Morfina",
    cca: "CAA-test",
    modo: "200ml/h",
    diluicao: "13%",
    selected: false
  }
]

const bibli5 = [
    {
    name: "Insulina",
    cca: "CAA-test",
    modo: "4ml/h",
    diluicao: "5%",
    selected: false
  },
  {
    name: "Denil",
    cca: "CAA-test",
    modo: "1ml/h",
    diluicao: "8%",
    selected: false
  },
  {
    name: "As",
    cca: "CAA-test",
    modo: "6ml/h",
    diluicao: "15%",
    selected: false
  },
  {
    name: "Tilenol",
    cca: "CAA-test",
    modo: "500ml/h",
    diluicao: "6%",
    selected: false
  },
  {
    name: "Aspirina",
    cca: "CAA-test",
    modo: "9ml/h",
    diluicao: "11%",
    selected: false
  },
]



const items = [
  { name: "Bliotest001", number: "201901001", date: "20/02/2019", bibli: bibli1  },
  { name: "Bliotest002", number: "201901002", date: "20/02/2019", bibli: bibli2  },
  { name: "Bliotest003", number: "201901003", date: "20/02/2019", bibli: bibli3  },
  { name: "Bliotest004", number: "201901004", date: "20/02/2019", bibli: bibli4  },
  { name: "Bliotest005", number: "201901005", date: "20/02/2019", bibli: bibli5  },
  { name: "Bliotest006", number: "201901006", date: "20/02/2019", bibli: bibli1  },
  { name: "Bliotest007", number: "201901007", date: "20/02/2019", bibli: bibli2  },
  { name: "Bliotest008", number: "201901008", date: "20/02/2019", bibli: bibli3  },
  { name: "Bliotest009", number: "201901009", date: "20/02/2019", bibli: bibli4  },
  { name: "Bliotest010", number: "201901010", date: "20/02/2019", bibli: bibli5  },

];

export default {
  data() {
    return {
      items: items,
      detail: {
        name: "",
        number: "",
        date: "",
        bibli: []
      },
      fields: [
        {
          key: "name",
          label: "Nome",
          sortable: true,
          sortDirection: "desc"
        },
        {
          key: "number",
          label: "Número",
          sortable: true,
          class: "text-center"
        },
        {
          key: "date",
          label: "Data",
          sortable: true,
          class: "text-center"
        },
        {
          key: "actions",
          label: "Ações"
        }
      ],
      currentPage: 1,
      perPage: 5,
      totalRows: items.length,
      pageOptions: [5, 10, 15],
      sortBy: null,
      sortDesc: false,
      sortDirection: "asc",
      filter: null,
      selectMode: "multi"
    };
  },
  computed: {
    sortOptions() {
      // Create an options list from our fields
      return this.fields
        .filter(f => f.sortable)
        .map(f => {
          return { text: f.label, value: f.key };
        });
    }
  },
  methods: {
    info(item, index, button) {
      this.$root.$emit("bv::show::modal", "modalInfo", button);
    },
    test(item, index, button) {
      this.detail = item;
      // console.log(item, index, button);
    },
    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length;
      this.currentPage = 1;
    },
    rowSelected(items) {
      this.selected = items;
    }
  }
};
</script>

<style scoped>
</style>
