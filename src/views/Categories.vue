<template>
  <div>
    <CRow>
      <CCol :md="12">
        <CCard class="mb-4">
          <CCardHeader> Categories </CCardHeader>
          <CCardBody>
            <CButton color="primary" style="margin-bottom: 10px;" @click="openAddCategoryModal()">Add new
              category</CButton>
            <CTable align="middle" class="mb-0 border" hover responsive>
              <CTableHead color="light">
                <CTableRow>
                  <CTableHeaderCell class="text-center">#</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">icon</CTableHeaderCell>
                  <CTableHeaderCell>Label</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">is viewable</CTableHeaderCell>
                  <CTableHeaderCell>Activity</CTableHeaderCell>
                </CTableRow>
              </CTableHead>
              <CTableBody>
                <CTableRow v-for="(category, index) in categories" :key="category._id">
                  <CTableDataCell class="text-center">
                    {{ index + 1 }}
                  </CTableDataCell>
                  <CTableDataCell><img :src="category.icon" alt="" style="height: 100px;">
                  </CTableDataCell>
                  <CTableDataCell>{{ category.label }}
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    {{ category.is_viewable }}
                  </CTableDataCell>
                  <CTableDataCell>
                    <CButton :color="category.is_viewable ? 'danger' : 'success'" variant="ghost"
                      @click="toggleStatus(category._id)">{{ category.is_viewable ? 'Hide' : 'Show' }}
                    </CButton>
                  </CTableDataCell>
                </CTableRow>
              </CTableBody>
            </CTable>
          </CCardBody>
        </CCard>
      </CCol>
    </CRow>
    <CModal scrollable :visible="addCategoryModal" backdrop="static" size="xl" @close="
      () => {
        addCategoryModal = false
      }
    ">
      <CModalHeader dismiss @close="
        () => {
          addCategoryModal = false
        }
      ">
        <CModalTitle>Add new Category</CModalTitle>
      </CModalHeader>
      <CModalBody>
        <CForm>
          <div class="mb-3">
            <CFormLabel for="label">Label</CFormLabel>
            <CFormInput id="label" type="text" placeholder="Enter category label (e.g. Ghee)" v-model="category.label" />
            <p style="color: red;" v-for="error of v$.category.label.$errors" :key="error.$uid">
              {{ error.$message }}
            </p>
          </div>
          <div class="mb-3">
            <file-uploader :label="'Select Icon'" @upload="uploadIconImage"></file-uploader>
            <p style="color: red;" v-for="error of v$.category.icon.$errors" :key="error.$uid">
              {{ error.$message }}
            </p>
          </div>
        </CForm>
      </CModalBody>
      <CModalFooter>
        <CButton color="secondary" @click="() => { addCategoryModal = false }">
          Close
        </CButton>
        <CButton color="success" @click="saveCategory()">Save</CButton>
      </CModalFooter>
    </CModal>
  </div>
</template>

<script>
import { ref } from 'vue'
import useVuelidate from "@vuelidate/core"
import { required, helpers } from "@vuelidate/validators";
export default {
  name: 'Categories',
  setup() {
    const categories = ref([])
    const addCategoryModal = ref(false)
    const file = ref(null);
    return {
      categories,
      addCategoryModal,
      category: ref({ label: null, icon: null }),
      file,
      v$: useVuelidate()
    }
  },
  validations() {
    return {
      category: {
        label: {
          required: helpers.withMessage("Category label is required", required)
        },
        icon: {
          required: helpers.withMessage("Category icon is required", required)
        }
      }
    }
  },
  // data() {
  //   return {
  //     categories: [],
  //     tableExample: []
  //   }
  // },
  components: {
  },
  methods: {
    openAddCategoryModal() {
      this.category = { label: null, icon: null }
      this.v$.$reset()
      this.addCategoryModal = true
    },
    async uploadFile(file) {
      const formData = new FormData();
      formData.append("file", file);
      const url = `${process.env.VUE_APP_API_URL}api/v1/media/upload`;

      try {
        const response = await this.axios.post(url, formData, {
          headers: { "Content-Type": "multipart/form-data" },
        })
        return response.data.data.Location
      } catch (error) {
        console.log(error)
      }
    },
    async uploadIconImage(imageUrl) {
      this.category.icon = imageUrl;

    },
    getCategories() {
      const url = `${process.env.VUE_APP_API_URL}api/v1/admin/categories`;
      this.axios.get(url).then((response) => {
        this.categories = response.data.data
        // this.$forceUpdate()
      })
    },
    async toggleStatus(id) {
      const confirm = await this.$swal.fire({
        title: 'Are you sure want to update the status ?',
        showCancelButton: true,
        confirmButtonText: 'Update',
      });
      if (!confirm.isConfirmed) return false;
      const url = `${process.env.VUE_APP_API_URL}api/v1/admin/category/${id}`;
      this.axios.get(url).then((response) => {
        this.getCategories()
      })
    },
    async saveCategory() {
      const result = await this.v$.$validate()
      if (!result) return
      const url = `${process.env.VUE_APP_API_URL}api/v1/admin/category`
      this.axios.post(url, this.category).then(() => {
        this.$swal("Category successfully added");
        this.category = { label: null, icon: null }
        this.getCategories()
        this.addCategoryModal = false
      })
    }
  },
  mounted() {
    this.getCategories();
  }
}
</script>
