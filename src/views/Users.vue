<template>
  <div>
    <CRow>
      <CCol :md="12">
        <CCard class="mb-4">
          <CCardHeader> User </CCardHeader>
          <CCardBody>
            <CButton color="primary" style="margin-bottom: 10px;" @click="() => { showAddUserModal = true }">Add new
              user</CButton>
            <CTable align="middle" class="mb-0 border" hover responsive>
              <CTableHead color="light">
                <CTableRow>
                  <CTableHeaderCell class="text-center">#</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">Name</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">Mobile</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">User Type</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">is verifed</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">Activity</CTableHeaderCell>
                </CTableRow>
              </CTableHead>
              <CTableBody>
                <CTableRow v-for="(user, index) in users" :key="user._id">
                  <CTableDataCell class="text-center">
                    {{ index + 1 }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">{{ user.name }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    {{ user.phone_number }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    {{ user.user_type }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    {{ user.verified }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    <CButton v-if="!user.verified" color="success" variant="ghost" @click="verifyUser(user._id, index)">
                      Approve</CButton>
                    <CButton color="primary" variant="ghost" @click="viewUserDetailModal(user)">View Details</CButton>
                  </CTableDataCell>
                </CTableRow>
              </CTableBody>
            </CTable>
            <pagination v-if="users.length > 0" v-model="page" :records="total" :per-page="recordPerPage"
              @paginate="paginateUser" />
          </CCardBody>
        </CCard>
      </CCol>
    </CRow>

    <CModal scrollable :visible="showAddUserModal" backdrop="static" size="lg" @close="
      () => {
        showAddUserModal = false
      }
    ">
      <CModalHeader dismiss @close="
        () => {
          showAddUserModal = false
        }
      ">
        <CModalTitle>Add new user</CModalTitle>
      </CModalHeader>
      <CModalBody>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">Shop Image</CFormLabel>
            <CFormInput class="form-control" type="file" placeholder="Select shop image" ref="shopImage" accept="image/*"
              @change="uploadShopImage" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label"> Firm Name </CFormLabel>
            <CFormInput class="form-control" type="text" v-model="userDetail.firm_name" placeholder="Enter Firm name" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">Shop / Unit No.</CFormLabel>
            <CFormInput class="form-control" type="text" v-model="userDetail.shop_no"
              placeholder="Enter Shop / Unit No." />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">Building name</CFormLabel>
            <CFormInput class="form-control" type="text" v-model="userDetail.building_no"
              placeholder="Enter Building name" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">Street / Area</CFormLabel>
            <CFormInput class="form-control" type="text" v-model="userDetail.street" placeholder="Enter Street / Area" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">State</CFormLabel>
            <CFormSelect v-model="userDetail.state_cities" class="form-control">
              <option value="">Select state</option>
              <option v-for=" (st, i) in states" v-bind:key="st.state_name" :value="i">{{ st.state_name }}</option>
            </CFormSelect>
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">City</CFormLabel>
            <CFormSelect v-model="userDetail.city" class="form-control" placeholder="Select your city">
              <option v-for="city in userDetail.cities" v-bind:key="city" :value="city">{{ city }}</option>
            </CFormSelect>
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">Pincode</CFormLabel>
            <CFormInput class="form-control" type="text" v-model="userDetail.pin_code" placeholder="Enter pin code" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">Name</CFormLabel>
            <CFormInput class="form-control" type="text" v-model="userDetail.owner_name" placeholder="Enter Owner name" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">Primary Contact</CFormLabel>
            <CFormInput class="form-control" type="text" v-model="userDetail.primary_mobile"
              placeholder="Enter primary contact" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">Secondary Contact</CFormLabel>
            <CFormInput class="form-control" type="text" v-model="userDetail.secondary_mobile"
              placeholder="Enter Secondary contact" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">GST Registration Number</CFormLabel>
            <CFormInput class="form-control" type="text" v-model="userDetail.gst_no"
              placeholder="Enter GST Registration number" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">GST Registration</CFormLabel>
            <CFormInput class="form-control" type="file" placeholder="Select GST registration certificate"
              ref="gstRegFile" accept="image/*" @change="uploadGstTRegFile" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">FSSAI License Number</CFormLabel>
            <CFormInput class="form-control" type="text" v-model="userDetail.fssai_licence_no"
              placeholder="Enter FSSAI License Number" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">FSSAI Registration</CFormLabel>
            <CFormInput class="form-control" type="file" placeholder="Select FSSAI registration certificate"
              ref="fssaiRegFile" accept="image/*" @change="uploadFSSAITRegFile" />
          </CCol>
        </CRow>
        <CRow>
          <CCol>
            <CFormLabel class="col-form-label">FSSAI registration expiry date</CFormLabel>
            <CFormInput class="form-control" type="date" v-model="userDetail.expiry_date"
              placeholder="Select Expiry date" />
          </CCol>
        </CRow>
      </CModalBody>
      <CModalFooter>
        <CButton color="secondary" @click="() => { showAddUserModal = false }">
          Close
        </CButton>
        <CButton color="success" @click="register()">Register</CButton>
      </CModalFooter>
    </CModal>

    <CModal scrollable :visible="showUserDetailModal" backdrop="static" size="xl" @close="
      () => {
        showUserDetailModal = false
      }
    ">
      <CModalHeader dismiss @close="
        () => {
          showUserDetailModal = false
        }
      ">
        <CModalTitle>{{ user.name }} ({{ user.user_type }})</CModalTitle>
      </CModalHeader>
      <CModalBody>
        <CTable align="middle" class="mb-0 border" hover responsive>
          <CTableBody>
            <CTableRow>
              <CTableDataCell>Shop Image</CTableDataCell>
              <CTableDataCell> <a :href="user.profile_image_url" target="_blank">View</a></CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Name</CTableDataCell>
              <CTableDataCell>{{ user.name }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Primary Contact</CTableDataCell>
              <CTableDataCell> {{ user.primary_contact }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Secondary Contact</CTableDataCell>
              <CTableDataCell> {{ user.secondary_contact }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>User Type</CTableDataCell>
              <CTableDataCell> {{ user.user_type }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Firm Name</CTableDataCell>
              <CTableDataCell> {{ user.firm_name }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Building Name</CTableDataCell>
              <CTableDataCell> {{ user.building_name }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Shop Unit</CTableDataCell>
              <CTableDataCell> {{ user.shop_unit }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Street area</CTableDataCell>
              <CTableDataCell> {{ user.street_area }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>State</CTableDataCell>
              <CTableDataCell> {{ user.state }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>City</CTableDataCell>
              <CTableDataCell> {{ user.city }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Pincode</CTableDataCell>
              <CTableDataCell> {{ user.pincode }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Created at</CTableDataCell>
              <CTableDataCell> {{ $dayjs(user.createdAt).format("DD/MM/YYYY") }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>fssai registration</CTableDataCell>
              <CTableDataCell> {{ user.fssai_registration }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>fssai registration certificate</CTableDataCell>
              <CTableDataCell> <a :href="user.fssai_image_url" target="_blank">View</a></CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>GST registration</CTableDataCell>
              <CTableDataCell> {{ user.gst_registration }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>GST registration certificate</CTableDataCell>
              <CTableDataCell> <a :href="user.gst_image_url" target="_blank">View</a></CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Expiry date</CTableDataCell>
              <CTableDataCell> {{ $dayjs(user.expiry_date).format("DD/MM/YYYY") }}</CTableDataCell>
            </CTableRow>
            <CTableRow>
              <CTableDataCell>Is verified</CTableDataCell>
              <CTableDataCell> {{ user.verified }}</CTableDataCell>
            </CTableRow>
          </CTableBody>
        </CTable>
      </CModalBody>
      <CModalFooter>
        <CButton color="secondary" @click="() => { showUserDetailModal = false }">
          Close
        </CButton>
        <CButton v-if="!user.verified" color="success" @click="verifyUser(user._id, currentIndex)">Approve</CButton>
      </CModalFooter>
    </CModal>
  </div>
</template>

<script>
import Pagination from 'v-pagination-3';
import states from "../mixins/state.vue";
import upload from "../mixins/upload.vue";
export default {
  name: 'Users',
  mixins: [states, upload],
  data() {
    return {
      userDetail: {
        cities: [],
        city: null,
        state: null,
        state_cities: "",
        firm_name: null,
        shop_no: null,
        building_no: null,
        street: null,
        mobile: null,
        pin_code: null,
        owner_name: null,
        primary_mobile: null,
        secondary_mobile: null,
        gst_no: null,
        fssai_licence_no: null,
        gst_image_url: null,
        fssai_image_url: null,
        expiry_date: null,
        profile_image_url: null
      },
      visibleVerticallyCenteredDemo: true,
      users: [],
      showUserDetailModal: false,
      user: null,
      currentIndex: 0,
      page: 1,
      recordPerPage: 10,
      total: 0,
      showAddUserModal: false
    }
  },
  components: {
    Pagination
  },
  watch: {
    "userDetail.state_cities"(newVal) {
      if (newVal == "") {
        this.userDetail.state = ""
        this.userDetail.cities = ""
        this.userDetail.city = null;
      } else {
        this.userDetail.state = this.states[newVal].state_name
        this.userDetail.cities = this.states[newVal].cities
        this.userDetail.city = null;
      }
    }
  },
  methods: {
    async uploadShopImage($event) {
      const input = $event.target;
      let file = input.files && input.files[0] ? input.files[0] : null
      if (!file) {
        return;
      }
      this.userDetail.profile_image_url = await this.uploadFile(file);
    },
    async uploadFSSAITRegFile($event) {
      const input = $event.target;
      let file = input.files && input.files[0] ? input.files[0] : null
      if (!file) {
        return;
      }
      this.userDetail.fssai_image_url = await this.uploadFile(file);

    },
    async uploadGstTRegFile($event) {
      const input = $event.target;
      let file = input.files && input.files[0] ? input.files[0] : null
      if (!file) {
        return;
      }
      this.userDetail.gst_image_url = await this.uploadFile(file);

    },
    register() {
      const formData = {
        name: this.userDetail.owner_name,
        phone_number: this.userDetail.primary_mobile,
        firm_name: this.userDetail.firm_name,
        shop_unit: this.userDetail.shop_no,
        building_name: this.userDetail.building_no,
        street_area: this.userDetail.street,
        city: this.userDetail.city,
        state: this.userDetail.state,
        pincode: this.userDetail.pin_code,
        primary_contact: this.userDetail.primary_mobile,
        secondary_contact: this.userDetail.secondary_mobile,
        gst_registration: this.userDetail.gst_no,
        gst_image_url: this.userDetail.gst_image_url,
        fssai_registration: this.userDetail.fssai_licence_no,
        fssai_image_url: this.userDetail.fssai_image_url,
        expiry_date: this.userDetail.expiry_date,
        profile_image_url: this.userDetail.profile_image_url
      }
      const url = `${process.env.VUE_APP_API_URL}api/v1/user/register/b2b`;

      this.axios.post(url, formData).then(response => {
        this.$swal("User Successfully created");
        this.page = 1
        this.getUsers()
        this.showAddUserModal = false

      }).catch(error => { console.log(error); this.$swal(error.response.data.err) });
    },
    paginateUser(page) { this.page = page; this.getUsers() },
    getUsers() {
      const url = `${process.env.VUE_APP_API_URL}api/v1/admin/users`;
      this.axios.get(url, { params: { page: this.page, limit: this.recordPerPage } }).then((response) => {
        this.users = response.data.data.list
        this.total = response.data.data.count
        this.$forceUpdate()
      })
    },
    viewUserDetailModal(user, index) {
      this.user = user
      this.currentIndex = index
      this.showUserDetailModal = true
    },
    async verifyUser(userId) {
      const confirm = await this.$swal.fire({
        title: 'Are you sure want to verify the user ?',
        showCancelButton: true,
        confirmButtonText: 'Sure',
      });
      if (!confirm.isConfirmed) return false;
      const url = `${process.env.VUE_APP_API_URL}api/v1/admin/toggle-verification/${userId}`;
      this.axios.get(url).then((response) => {
        this.getUsers()
        this.user = response.data.data
        this.$forceUpdate()
      })
    }
  },
  mounted() {
    this.getUsers();
  }
}
</script>
