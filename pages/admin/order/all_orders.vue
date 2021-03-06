<template>
  <v-row>
    <v-col>
      <client-only>
        <v-data-table
          :headers="headers"
          :items="modifiedOrders"
          :items-per-page="orderPayload.limit"
          :server-items-length="totalOrderCount"
          class="elevation-1"
          disable-sort
          @pagination="onPagination"
          :loading="loadingOrders"
          loading-text="Loading orders..."
        >
          <template v-slot:top>
            <v-toolbar flat color="white">
              <v-toolbar-title>All Orders</v-toolbar-title>
            </v-toolbar>
          </template>
          <template v-slot:item.actions="{ item }">
            <v-btn x-small tile color="accent" @click="onPressShowOrder(item.order)">Details</v-btn>
            <v-btn x-small tile color="primary">Accept</v-btn>
            <v-btn x-small tile color="warning">Reject</v-btn>

          </template>
        </v-data-table>
      </client-only>
    </v-col>
    <v-dialog v-model="orderDialog" max-width="700px">
      <SingleOrder :order="order" />
    </v-dialog>

  </v-row>
</template>

<script>
import allOrdersQuery from "@/gql/query/getOrders.gql";
import SingleOrder from "@/components/SingleOrder.vue";
export default {
  layout: "admin",
  async fetch() {
    this.fetchOrders();
  },
  components: {
    SingleOrder
  },
  data() {
    return {
      orderPayload: {
        pageNo: 1,
        limit: 15,
        status: []
      },
      orders: [],
      totalOrderCount: 0,
      headers: [
        { text: "Order By", value: "orderBy" },
        { text: "Order Date", value: "orderDate" },
        { text: "Order ID", value: "id" },
        { text: "Order Type", value: "orderType" },
        { text: "Order Status", value: "orderStatus" },
        { text: "Delivery Status", value: "deliveryStatus" },
        { text: "Actions", value: "actions", sortable: false }
      ],
      orderDialog: false,
      order: null,
      updatedStatusItemPayload: {
        id: "",
        status: ""
      },
      loadingStatusUpdate: false,
      selectedItemIndex: -1,
      selectedItem: null,
      clickedButton: "ACCEPTED",
      confirmDialog: false,
      loadingOrders: false
    };
  },
  methods: {
    async fetchOrders() {
      try {
        this.loadingOrders = true;
        const response = await this.$apollo.query({
          query: allOrdersQuery,
          variables: { input: this.orderPayload },
          fetchPolicy: "network-only"
        });
        this.loadingOrders = false;
        const orders = response.data.getOrders.orders;
        const totalOrderCount = response.data.getOrders.count;
        console.log("orders", orders);
        this.orders = orders;
        console.log("totalOrderCount", totalOrderCount);
        this.totalOrderCount = totalOrderCount;
      } catch (error) {
        this.loadingOrders = false;
        this.$notifier.showMessage({
          content: error.message,
          color: "error"
        });
      }
    },

    onPagination(val) {
      console.log("onPagination", val);
      this.orderPayload.pageNo = val.page;
      this.fetchOrders();
    },
    onPressShowOrder(val) {
      console.log("order item", val);
      this.order = val;
      this.orderDialog = true;
    },
  },
  computed: {
    modifiedOrders() {
      return this.orders.map(order => {
        return {
          id: order.id,
          orderBy: `${order.orderBy.firstName} ${order.orderBy.lastName}`,
          orderDate: `${this.$moment(order.createdAt).format("LLL")}`,
          orderItems: order.items,
          orderType: order.orderType,
          orderStatus: order.orderStatus,
          deliveryStatus: order.deliveryStatus,
          orderNote: order.orderNote,
          order: order
        };
      });
    }
  }
};
</script>

<style>
</style>