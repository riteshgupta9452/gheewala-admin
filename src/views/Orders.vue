<template>
  <div>
    <CRow>
      <CCol :md="12">
        <CCard class="mb-4">
          <CCardHeader> Orders </CCardHeader>
          <CCardBody>
            <CTable align="middle" class="mb-0 border" hover responsive>
              <CTableHead color="light">
                <CTableRow>
                  <CTableHeaderCell class="text-center">#</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">Name</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">Mobile</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">status</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">No. of items</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">Order amount</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">Activity</CTableHeaderCell>
                </CTableRow>
              </CTableHead>
              <CTableBody>
                <CTableRow v-for="(order, index) in orders" :key="order._id">
                  <CTableDataCell class="text-center">
                    {{ index + 1 }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">{{ order.user.name }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    {{ order.user.phone_number }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    {{ order.status }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    {{ order.products.length }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    {{ order.total }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    <CButton color="primary" variant="ghost" @click="viewOrderDetailModal(order)">View Products</CButton>
                  </CTableDataCell>
                </CTableRow>
              </CTableBody>
            </CTable>
            <pagination v-if="orders.length > 0" v-model="page" :records="total" :per-page="recordPerPage"
              @paginate="paginateOrders" />
          </CCardBody>
        </CCard>
      </CCol>
    </CRow>
    <CModal scrollable :visible="showOrderDetailModal" backdrop="static" size="xl" @close="
      () => {
        showOrderDetailModal = false
      }
    ">
      <CModalHeader dismiss @close="
        () => {
          showOrderDetailModal = false
        }
      ">
        <CModalTitle>Items</CModalTitle>
      </CModalHeader>
      <CModalBody>
        <b>Shipping Address : </b>
        <span>
          {{ order.address.address.house_flat_no }}, {{ order.address.address.building_name }},
          {{ order.address.address.street }}, {{ order.address.address.city }} {{ order.address.address.state }}
          {{ order.address.address.pincode }}
        </span>
        <br />
        <b>Weekly Off : </b> {{ order.address.address.weekly_off }}
        <br />
        <b> Open time : </b> <span>{{ order.address.address.open_time }}</span> <b> to Close
          time
          :
        </b>
        <span>{{
          order.address.address.close_time }}</span>
        <br />
        <br>
        <CTable align="middle" class="mb-0 border" hover responsive>
          <CTableHead color="light">
            <CTableRow>
              <CTableHeaderCell class="text-center">#</CTableHeaderCell>
              <CTableHeaderCell class="text-center">Product</CTableHeaderCell>
              <CTableHeaderCell class="text-center">Quantity</CTableHeaderCell>
              <CTableHeaderCell class="text-center">Price</CTableHeaderCell>
            </CTableRow>
          </CTableHead>
          <CTableBody>
            <CTableRow v-for="(item, index) in order.items" :key="order._id">
              <CTableDataCell class="text-center">
                {{ index + 1 }}
              </CTableDataCell>
              <CTableDataCell class="text-center">{{ item.label }}
              </CTableDataCell>
              <CTableDataCell class="text-center">
                {{ order.products[index].quantity }}
              </CTableDataCell>
              <CTableDataCell class="text-center">
                {{ item.price[order.user.user_type] }}
              </CTableDataCell>
            </CTableRow>
          </CTableBody>
        </CTable>
      </CModalBody>
      <CModalFooter>
        <CButton color="secondary" @click="() => { showOrderDetailModal = false }">
          Close
        </CButton>
      </CModalFooter>
    </CModal>
  </div>
</template>

<script>
// import dayjs from 'dayjs'
import Pagination from 'v-pagination-3';
export default {
  name: 'Orders',
  data() {
    return {
      visibleVerticallyCenteredDemo: true,
      orders: [],
      showOrderDetailModal: false,
      order: null,
      currentIndex: 0,
      page: 1,
      recordPerPage: 10,
      total: 0
    }
  },
  components: {
    Pagination
  },
  methods: {
    paginateOrders(page) { this.page = page; this.getOrders() },
    getOrders() {
      const url = `${process.env.VUE_APP_API_URL}api/v1/admin/orders`;
      this.axios.get(url, { params: { page: this.page, limit: this.recordPerPage } }).then((response) => {
        this.orders = response.data.data.list
        this.total = response.data.data.count
        this.$forceUpdate()
      })
    },
    viewOrderDetailModal(order, index) {
      this.order = order
      this.currentIndex = index
      this.showOrderDetailModal = true
    },
    formatTime(time) {
      return this.$dayjs(new Date(time)).format("HH:MM A")
    }
  },
  mounted() {
    this.getOrders();
  }
}
</script>
