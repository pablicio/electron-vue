<template>
  <b-container fluid>
    <!-- User Interface controls -->
    <h1 class="text-center m-t-80">Listagem de drogas</h1>
    <b-row>
      <b-col md="4" class="my-0">
        <b-form-group label-cols-sm="6" label="Filtro" class="mb-0">
          <b-input-group>
            <b-form-input v-model="filter" placeholder="Pesquisar..."/>
            <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''">Limpar</b-button>
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
      </b-col>

      <b-col md="2" class="my-1">
        <b-form-group label-cols-sm="3" label="Por Página" class="mb-0">
          <b-form-select :options="pageOptions" v-model="perPage"/>
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
          <template slot="HEAD_selected" slot-scope="row">
            <input type="checkbox" @click.stop v-model="selectAll" @change="toggleSelectAll">
          </template>

          <template slot="selected" slot-scope="row">
            <input
              type="checkbox"
              @click.stop
              v-model="row.item.selected"
              @change="selectRow(row.item)"
            >
          </template>

          <template slot="name" slot-scope="row">{{ row.item.name }}</template>
          <template slot="cca" slot-scope="row">{{ row.item.cca }}</template>
          <template slot="modo" slot-scope="row">{{ row.item.modo }}</template>
          <template slot="diluicao" slot-scope="row">{{ row.item.diluicao }}</template>

          <template slot="actions" slot-scope="row">
            <b-button
              size="sm"
              @click="test(row.item, row.index, $event.target)"
              class="mr-1"
            >editar</b-button>
          </template>
        </b-table>
      </b-col>

      <b-col md="3" class="my-1">
        <h4>Itens da nova biblioteca</h4>

        <b-list-group v-for="itemBlio in itensBlio">
          <b-list-group-item variant="info">{{itemBlio.name }} {{itemBlio.modo }}</b-list-group-item>
        </b-list-group>
      </b-col>

      <b-col md="3" class="my-1">
        <h4></h4>
        <h4></h4>
        <h4></h4>
        <b-button size="sm" class="mr-1">Criar nova bilioteca</b-button>
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
const items = [
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
  {
    name: "Morfina",
    cca: "CAA-test",
    modo: "200ml/h",
    diluicao: "13%",
    selected: false
  }
];

export default {
  data() {
    return {
      selectAll: false,
      items: items,
      itensBlio: [],
      detail: {
        name: "",
        number: "",
        date: "2"
      },
      fields: [
        {
          key: "selected",
          sortable: false,
          label: "",
          class: "options-column"
        },
        {
          key: "name",
          label: "Nome",
          sortable: true,
          sortDirection: "desc"
        },
        {
          key: "cca",
          label: "CCA",
          sortable: true,
          class: "text-center"
        },
        {
          key: "modo",
          label: "Modo",
          sortable: true,
          class: "text-center"
        },
        {
          key: "diluicao",
          label: "Diluição",
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
      pageOptions: [5, 10, 1000],
      sortBy: null,
      sortDesc: false,
      sortDirection: "asc",
      filter: null,
      selectMode: "multi"
    };
  },
  computed: {
    hasRecords() {
      return this.items.length > 0;
    },
    totalRowss() {
      return this.items.length;
    },
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
    selectRow(item) {
      if (item.selected) {
        item._rowVariant = "success";
      } else {
        item._rowVariant = "default";
        if (this.selectAll) {
          this.selectAll = false;
        }
      }

      this.itensBlio = _.filter(this.items, function(o) {
        return o.selected;
      });
    },
    toggleSelectAll() {
      if (this.selectAll) {
        for (var i = 0; i < this.items.length; i++) {
          this.items[i].selected = true;
          this.items[i]._rowVariant = "success";
        }

        this.itensBlio = this.items;
      } else {
        for (var i = 0; i < this.items.length; i++) {
          this.items[i].selected = false;
          this.items[i]._rowVariant = "default";
        }

        this.itensBlio = [];
      }
    }
  }
};
</script>

<style scoped>
</style>
