<template>
  <div class="q-pa-md" style="width: 100vw">
    <q-table
      title-class="text-pink-8 text-bold"
      title="Contacts"
      card-class="bg-pink-8 text-white"
      :data="data"
      :columns="columns"
      row-key="name"
      :filter="filter"
      grid
      hide-header
    >
      <template v-slot:top-right>
        <q-input dense debounce="300" v-model="filter" placeholder="Search">
          <template v-slot:append>
            <q-icon name="search" />
          </template>
        </q-input>
      </template>
    </q-table>
  </div>
</template>

<script>
import { api } from "boot/axios";

const getAddress = (addr) => {
  let result = "";
  ["suite", "street", "city", "zipcode"].forEach((str) => {
    result += addr[str] ? " " + addr[str].trim() : "";
  });
  return result;
};

export default {
  data() {
    return {
      filter: "",
      columns: [
        {
          name: "name",
          required: true,
          label: "Name",
          align: "left",
          field: (row) => row.name,
          format: (val) => `${val}`,
          sortable: true,
        },
        {
          name: "nickname",
          align: "left",
          label: "Nickname",
          field: "username",
          sortable: true,
        },
        {
          name: "phone",
          align: "left",
          label: "Phone",
          field: "phone",
        },
        { name: "email", label: "Email", field: "email" },
        { name: "website", label: "Website", field: "website" },
        {
          name: "company",
          label: "Company",
          field: "company",
          format: (company) => `${company.name}`,
        },
        {
          name: "address",
          label: "Address",
          field: "address",
          format: (addr) => getAddress(addr),
        },
      ],
      data: [],
    };
  },
  created() {
    this.loadData();
  },
  methods: {
    loadData() {
      api
        .get("/users")
        .then((response) => {
          console.log(response.data);
          this.$set(this, "data", response.data);
        })
        .catch(() => {
          // https://v1.quasar.dev/quasar-plugins/notify
          this.$q.notify({
            color: "negative",
            position: "top",
            message: "Loading users failed",
            icon: "report_problem",
          });
        });
    },
  },
};
</script>
