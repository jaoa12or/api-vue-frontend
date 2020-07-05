
<template>
  <div>
    <b-container fluid>
      <b-card class="card-raised">
        <div id="msg" style="display: none; padding: 1em; position: fixed; margin: 0px 5px;"></div>
        <b-card-text>
          <h1>Domain Search</h1>
          <h5>Check for any domain information in the web.</h5>
          <b-row align-v="center">
            <b-col md="8" offset-md="2" style="margin-top: 2rem; margin-bottom: 3rem">
              <b-form @submit="onSubmit">
                <b-form-group
                  id="input-group-1"
                  label-for="input-1"
                  description="Please, paste or type your domain without the protocol."
                >
                  <b-input-group
                    v-for="size in ['lg']"
                    :key="size"
                    :size="size"
                    class="mb-3"
                    prepend="https://"
                  >
                    <b-form-input
                      id="domain"
                      v-model="form.domain"
                      required
                      placeholder="google.com"
                    ></b-form-input>
                    <b-input-group-append>
                      <b-button type="submit" size="sm" text="Button" variant="success">
                        <b-icon-search></b-icon-search>
                      </b-button>
                    </b-input-group-append>
                  </b-input-group>
                </b-form-group>
              </b-form>
            </b-col>
            <b-col md="1" sm="2" style="padding-bottom:4rem; margin-left:-30px">
              <b-spinner variant="success" type="grow" v-if="show"></b-spinner>
            </b-col>
          </b-row>
          <b-collapse id="collapse-a" class="mt-2" :visible="show_info">
            <b-row align-h="center">
              <b-col md="12" sm="12" style="margin-bottom: 2rem">
                <h3>Available Servers for this Domain</h3>
              </b-col>
              <b-col
                md="4"
                sm="12"
                v-for="(server, index) in domain_info.servers"
                v-bind:key="index"
              >
                <b-card class="card-raised" title="Server Information">
                  <b-list-group flush>
                    <b-list-group-item>
                      <strong>Address:</strong>
                      {{server.ipAddress ? server.ipAddress : "N.A"}}
                    </b-list-group-item>
                    <b-list-group-item>
                      <strong>SSL Grade:</strong>
                      {{server.grade ? server.grade : "N.A"}}
                    </b-list-group-item>
                    <b-list-group-item>
                      <strong>Country:</strong>
                      {{server.country ? server.country : "N.A"}}
                    </b-list-group-item>
                    <b-list-group-item>
                      <strong>Owner:</strong>
                      {{server.owner ? server.owner : "N.A"}}
                    </b-list-group-item>
                  </b-list-group>
                </b-card>
              </b-col>
            </b-row>
          </b-collapse>
          <b-collapse id="collapse-b" class="mt-2" :visible="show_info">
            <b-row align-h="center">
              <b-col md="8">
                <b-card class="card-raised" title="General Information">
                  <b-list-group flush>
                    <b-list-group-item>
                      <strong>Server Changed:</strong>
                      {{domain_info.server_changed}}
                    </b-list-group-item>
                    <b-list-group-item>
                      <strong>SSL Minimum Grade:</strong>
                      {{domain_info.ssl_grade ? domain_info.ssl_grade: "N.A"}}
                    </b-list-group-item>
                    <b-list-group-item>
                      <strong>Previous SSL Minimum Grade:</strong>
                      {{domain_info.previous_ssl_grade ? domain_info.previous_ssl_grade: "N.A"}}
                    </b-list-group-item>
                    <b-list-group-item>
                      <strong>Logo:</strong>
                      <a :href="domain_info.logo" target="_blank">
                        <b-icon-arrow-up-right-square></b-icon-arrow-up-right-square>
                      </a>
                    </b-list-group-item>
                    <b-list-group-item>
                      <strong>Page Title:</strong>
                      {{domain_info.title ? domain_info.title: "N.A"}}
                    </b-list-group-item>
                    <b-list-group-item>
                      <strong>Reachable:</strong>
                      {{!domain_info.is_down}}
                    </b-list-group-item>
                  </b-list-group>
                </b-card>
              </b-col>
            </b-row>
          </b-collapse>
          <b-card class="card-raised" title="Consulted Domains">
            <b-container>
              <div class="overflow-auto">
                <b-table :items="domains.items" :fields="fields" striped responsive="sm">
                  <template v-slot:cell(show_details)="row">
                    <b-button
                      size="sm"
                      @click="row.toggleDetails"
                      class="mr-2"
                    >{{ row.detailsShowing ? 'Hide' : 'Show'}} Details</b-button>
                    <!-- As `row.showDetails` is one-way, we call the toggleDetails function on @change -->
                  </template>
                  <template v-slot:row-details="row">
                    <b-card>
                      <b-row align-h="center">
                        <b-col md="12" sm="12" style="margin-bottom: 2rem">
                          <h3>Available Servers for this Domain</h3>
                        </b-col>
                        <b-col
                          md="5"
                          sm="12"
                          v-for="(server, index) in row.item.data.servers"
                          v-bind:key="index"
                        >
                          <b-card class="card-raised" title="Server Information">
                            <b-list-group flush>
                              <b-list-group-item>
                                <strong>Address:</strong>
                                {{server.ipAddress ? server.ipAddress : "N.A"}}
                              </b-list-group-item>
                              <b-list-group-item>
                                <strong>SSL Grade:</strong>
                                {{server.grade ? server.grade : "N.A"}}
                              </b-list-group-item>
                              <b-list-group-item>
                                <strong>Country:</strong>
                                {{server.country ? server.country : "N.A"}}
                              </b-list-group-item>
                              <b-list-group-item>
                                <strong>Owner:</strong>
                                {{server.owner ? server.owner : "N.A"}}
                              </b-list-group-item>
                            </b-list-group>
                          </b-card>
                        </b-col>
                      </b-row>
                      <b-row align-h="center">
                        <b-col md="8">
                          <b-card class="card-raised" title="General Information">
                            <b-list-group flush>
                              <b-list-group-item>
                                <strong>Server Changed:</strong>
                                {{row.item.data.server_changed}}
                              </b-list-group-item>
                              <b-list-group-item>
                                <strong>SSL Minimum Grade:</strong>
                                {{row.item.data.ssl_grade ? row.item.data.ssl_grade: "N.A"}}
                              </b-list-group-item>
                              <b-list-group-item>
                                <strong>Previous SSL Minimum Grade:</strong>
                                {{row.item.data.previous_ssl_grade ? row.item.data.previous_ssl_grade: "N.A"}}
                              </b-list-group-item>
                              <b-list-group-item>
                                <strong>Logo:</strong>
                                <a :href="row.item.data.logo" target="_blank">
                                  <b-icon-arrow-up-right-square></b-icon-arrow-up-right-square>
                                </a>
                              </b-list-group-item>
                              <b-list-group-item>
                                <strong>Page Title:</strong>
                                {{row.item.data.title ? row.item.data.title: "N.A"}}
                              </b-list-group-item>
                              <b-list-group-item>
                                <strong>Reachable:</strong>
                                {{!row.item.data.is_down}}
                              </b-list-group-item>
                            </b-list-group>
                          </b-card>
                        </b-col>
                      </b-row>
                      <b-button size="sm" @click="row.toggleDetails">Hide Details</b-button>
                    </b-card>
                  </template>
                </b-table>
              </div>
              <b-row align-h="center" v-if="pagination">
                <b-button
                  pill
                  variant="success"
                  v-on:click="previouspage"
                  v-if="prev"
                  style="font-size:1.2rem"
                >
                  <b-icon icon="arrow-left" ></b-icon>
                </b-button>
                <p class="h2 mb-2" style="margin: 0 2rem 0 2rem">{{page}}</p>
                <b-button
                  pill
                  variant="success"
                  v-on:click="nextpage"
                  v-if="next"
                  style="font-size:1.2rem"
                >
                  <b-icon icon="arrow-right" ></b-icon>
                </b-button>
              </b-row>
            </b-container>
          </b-card>
        </b-card-text>
      </b-card>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      form: {
        domain: ""
      },
      domain_info: {
        servers: [],
        servers_changed: false,
        ssl_grade: "",
        previous_ssl_grade: "",
        logo: "",
        title: "",
        is_down: false
      },
      show: false,
      show_info: false,
      fields: ["domain", "show_details"],
      domains: {
        items: [],
        prev: 0,
        next: 0,
        rows: 0
      },
      page: 1,
      prev: false,
      next: true,
      pagination: true
    };
  },
  created: function() {
    this.getDomains(0, "");
  },

  methods: {
    onSubmit(evt) {
      this.show = true;
      this.show_info = false;
      evt.preventDefault();
      let form_data = new FormData();
      form_data.append("domain", this.form.domain);
      axios
        .post("http://localhost:9000/domains/search", form_data)
        .then(response => this.approved("Successful!", response))
        .catch(() => this.failed("Unsuccesful!"));
      // alert(JSON.stringify(this.form));
    },
    getDomains: function(cursor, action) {
      axios
        .get(
          "http://localhost:9000/domains?cursor=" + cursor + "&action=" + action
        )
        .then(res => this.updateDomains(res))
        .catch(() => this.failed("Unsuccesful!"));
    },
    updateDomains: function(res) {
      if (res.data.items != null) {
        this.domains = res.data ? res.data : [];
        if (this.page == 1 && this.domains.rows < 10) {
          this.pagination = false;
        } else {
          this.pagination = true;
        }
        if (this.page > 1 && this.domains.rows < 10) {
          this.next = false;
        } else {
          this.next = true;
        }
      } else {
        this.next = false;
        this.page -= 1;
        if (this.page == 1) {
          this.prev = false;
        }
        this.makeToast("warning", "Ups!", "There's not more data to show!");
      }
    },
    makeToast(variant = null, title = "", content = "") {
      this.$bvToast.toast(content, {
        title: title,
        variant: variant,
        solid: true
      });
    },
    approved: function(response, data) {
      this.domain_info = data.data;
      this.makeToast(
        "success",
        "Success!",
        "Now you can see the info from this domain!"
      );
      this.show = false;
      this.show_info = true;
      this.getDomains(0, "");
      this.page = 1;
      this.prev = false;
    },
    failed: function(response) {
      this.makeToast(
        "danger",
        response,
        "Something went wrong! Please, check if the domain is well spelled"
      );
      this.show = false;
    },
    nextpage: function() {
      this.getDomains(this.domains.next, "next");
      this.page += 1;
      if (this.page > 1) {
        this.prev = true;
      }
    },
    previouspage: function() {
      this.getDomains(this.domains.next, "prev");
      this.page -= 1;
      if (this.page <= 1) {
        this.prev = false;
      }
    }
  }
};
</script>