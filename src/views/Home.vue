<template>
  <b-container class="bv-example-row">
    <b-row align-v="center">
      <b-col md="4">
        <b-card title="BitMax">
          <b-button variant="primary" @click="fetchBitMaxData"
            >GET PRICE</b-button
          >
        </b-card>
      </b-col>
      <b-col md="4">
        <b-card title="Future Exchange">
          <b-button variant="primary" @click="fetchFuture1Data"
            >GET PRICE</b-button
          >
        </b-card>
      </b-col>
      <b-col md="4">
        <b-card title="Future Exchange">
          <b-button variant="primary" @click="fetchFuture2Data"
            >GET PRICE</b-button
          >
        </b-card>
      </b-col>
      <b-container>
        <b-row align-h="center" class="mt-5">
          <b-col cols="10">
            <b-form-checkbox-group
              v-model="selected"
              :options="options"
              class="mb-3"
              value-field="exchange"
              text-field="price"
              disabled-field="notEnabled"
            ></b-form-checkbox-group>
            <b-form inline>
              <label class="mr-sm-2" for="inline-form-custom-select-pref"
                >Preference</label
              >
              <b-form-select
                id="inline-form-custom-select-pref"
                class="mb-2 mr-sm-2 mb-sm-0"
                :options="[
                  { text: 'Choose...', value: null },
                  'Medien',
                  'Average'
                ]"
                :value="null"
              ></b-form-select>
              <label for="actor-user">Actor :</label>
              <b-input
                v-model="userId"
                :state="validation"
                id="actor-user"
              ></b-input>
              <b-button variant="primary">Create Clio Command</b-button>
              <b-form-invalid-feedback :state="validation">
                Your user ID must be 12 characters long.
              </b-form-invalid-feedback>
              <b-form-valid-feedback :state="validation">
                Looks Good.
              </b-form-valid-feedback>
              <b-col cols="15">
                <b-card class="mt-3" header="Clio Command">
                  <pre class="m-0">
./clio.sh push action -f fio.fee setfeemult '{ "multiplier": {{
                      this.multiNum.toFixed(4)
                    }}, "actor": "{{ this.userId }}" }' -p {{
                      this.userId
                    }}@active</pre
                  >
                </b-card>
              </b-col>
            </b-form>
            <span class="text-muted">fioprotocol.io</span>
          </b-col>
        </b-row>
      </b-container>
    </b-row>
  </b-container>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  components: {},
  mounted() {},
  data() {
    return {
      userId: "",
      multiNum: 0.0,
      selected: [],
      options: [],
      element: []
    };
  },
  methods: {
    async fetchBitMaxData() {
      try {
        const res = await fetch("https://bin.fioprotocol.io/fio-price");
        const val = await res.json();
        let price = val.data.close;
        let index = this.options.findIndex(item => item.exchange === "BitMax");

        if (index == -1) {
          this.element = { exchange: "BitMax", price: price };
          this.options.push(this.element);
          if (price != 0) {
            this.multiNum = this.multiNum + 1 / price;
          }
        }
      } catch (errors) {
        console.log("ERROR!!!!");
      }
    },
    async fetchFuture1Data() {
      try {
        //const res = await fetch("https://bin.fioprotocol.io/fio-price");
        //const val = await res.json();
        let price = 0.0; //val.data.close;
        let index = this.options.findIndex(item => item.exchange === "Future1");

        if (index == -1) {
          this.element = { exchange: "Future1", price: price };
          this.options.push(this.element);
          if (price != 0) {
            this.multiNum = 1 / price;
          }
        }
      } catch (errors) {
        console.log("ERROR!!!!");
      }
    },
    async fetchFuture2Data() {
      try {
        //const res = await fetch("https://bin.fioprotocol.io/fio-price");
        //const val = await res.json();
        let price = 0.0; //val.data.close;
        let index = this.options.findIndex(item => item.exchange === "Future2");

        if (index == -1) {
          this.element = { exchange: "Future2", price: price };
          this.options.push(this.element);
          if (price != 0) this.multiNum = 1 / price;
        }
      } catch (errors) {
        console.log("ERROR!!!!");
      }
    }
  },
  computed: {
    validation() {
      return this.userId.length == 12;
    }
  }
};
</script>
