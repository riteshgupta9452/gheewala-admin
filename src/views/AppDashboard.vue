<template>
    <div>
        <CRow>
            <CCol :md="12">
                <CCard class="mb-4">
                    <CCardHeader> Dashboard Categories </CCardHeader>
                    <CCardBody>
                        <CButton color="primary" style="margin-bottom: 10px;" @click="openDashboardCategoryModal()">Add
                            new Dashboard Category</CButton>
                        <CTable align="middle" class="mb-0 border" hover responsive>
                            <CTableHead color="light">
                                <CTableRow>
                                    <CTableHeaderCell class="text-center">#</CTableHeaderCell>
                                    <CTableHeaderCell>title</CTableHeaderCell>
                                    <CTableHeaderCell>Product count</CTableHeaderCell>
                                    <CTableHeaderCell>is viewable</CTableHeaderCell>
                                    <CTableHeaderCell>Activity</CTableHeaderCell>
                                </CTableRow>
                            </CTableHead>
                            <CTableBody>
                                <CTableRow v-for="(category, index) in categories" :key="category._id">
                                    <CTableDataCell class="text-center">
                                        {{ index + 1 }}
                                    </CTableDataCell>
                                    <CTableDataCell> {{ category.title }}</CTableDataCell>
                                    <CTableDataCell> {{ category.products.length }}</CTableDataCell>
                                    <CTableDataCell>
                                        {{ category.is_viewable }}
                                    </CTableDataCell>
                                    <CTableDataCell>
                                        <CButton :color="category.is_viewable ? 'danger' : 'success'" variant="ghost"
                                            @click="toggleStatus(category._id)">{{ category.is_viewable ? 'Hide' : 'Show' }}
                                        </CButton>
                                        <CButton color="danger" variant="ghost" @click="deleteCategory(category._id)">Delete
                                        </CButton>
                                        <CButton color="success" variant="ghost"
                                            @click="showDashboardProductsModal(category._id)">Add
                                            Products</CButton>
                                    </CTableDataCell>
                                </CTableRow>
                            </CTableBody>
                        </CTable>
                    </CCardBody>
                </CCard>
            </CCol>
        </CRow>
        <CModal scrollable :visible="addDashboardCategoryModal" backdrop="static" size="xl" @close="
            () => {
                addDashboardCategoryModal = false
            }
        ">
            <CModalHeader dismiss @close="
                () => {
                    addDashboardCategoryModal = false
                }
            ">
                <CModalTitle>Add new Banner</CModalTitle>
            </CModalHeader>
            <CModalBody>
                <CForm>
                    <div>
                        <CFormLabel for="label">Title</CFormLabel>
                        <CFormInput id="label" type="text" placeholder="Enter title (e.g. Trending)"
                            v-model="category.title" />
                        <p style="color: red;" v-for="error of v$.category.title.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </div>
                    <div>
                        <CFormLabel class="col-form-label">Is viewable</CFormLabel>
                        <CFormCheck type="radio" name="isViewable" id="isViewable1" :value="true"
                            v-model="category.is_viewable" label="true" />
                        <CFormCheck type="radio" name="isViewable" id="isViewable2" :value="false"
                            v-model="category.is_viewable" label="false" checked />
                    </div>
                </CForm>
            </CModalBody>
            <CModalFooter>
                <CButton color="secondary" @click="() => { addDashboardCategoryModal = false }">
                    Close
                </CButton>
                <CButton color="success" @click="saveCategory()">Save</CButton>
            </CModalFooter>
        </CModal>
        <CModal fullscreen :visible="addDashboardProductsModal" backdrop="static" @close="
            () => {
                addDashboardProductsModal = false
            }
        ">
            <CModalHeader dismiss @close="
                () => {
                    addDashboardProductsModal = false
                }
            ">
                <CModalTitle>Add Product</CModalTitle>
            </CModalHeader>
            <CModalBody>
                <CRow>
                    <CCol :md="6">
                        <h3>Select Products</h3>
                        <div>
                            <CFormInput id="label" type="text" placeholder="Search Products" v-model="search" @keyup="() => {
                                page = 1;
                                recordPerPage = 10;
                                getProducts()
                            }" />
                        </div>
                        <CTable align="middle" class="mb-0 border" hover responsive>
                            <CTableHead color="light">
                                <CTableRow>
                                    <CTableHeaderCell class="text-center">#</CTableHeaderCell>
                                    <CTableHeaderCell>Image</CTableHeaderCell>
                                    <CTableHeaderCell>Name</CTableHeaderCell>
                                    <CTableHeaderCell>Activity</CTableHeaderCell>
                                </CTableRow>
                            </CTableHead>
                            <CTableBody>
                                <CTableRow v-for="(product, index) in products" :key="product._id">
                                    <CTableDataCell class="text-center">
                                        {{ index + 1 }}
                                    </CTableDataCell>
                                    <CTableDataCell class="text-center"> <img :src="product.image" alt=""
                                            style="width: 100px;"> </CTableDataCell>
                                    <CTableDataCell>{{ product.label }} </CTableDataCell>
                                    <CTableDataCell>
                                        <CButton color="primary" variant="ghost"
                                            v-on:click.stop.prevent="updateProduct('add', index, product._id)">
                                            Add
                                            product </CButton>
                                    </CTableDataCell>
                                </CTableRow>
                            </CTableBody>
                        </CTable>
                        <pagination v-if="products.length > 0" v-model="page" :records="total" :per-page="recordPerPage"
                            @paginate="paginateProducts" />
                    </CCol>
                    <CCol :md="6">
                        <h3>Selected Products</h3>

                        <CTable align="middle" class="mb-0 border" hover responsive>
                            <CTableHead color="light">
                                <CTableRow>
                                    <CTableHeaderCell class="text-center">#</CTableHeaderCell>
                                    <CTableHeaderCell>Image</CTableHeaderCell>
                                    <CTableHeaderCell>Name</CTableHeaderCell>
                                    <CTableHeaderCell>Activity</CTableHeaderCell>
                                </CTableRow>
                            </CTableHead>
                            <CTableBody>
                                <CTableRow v-for="(product, index) in productDetail.products" :key="product._id">
                                    <CTableDataCell class="text-center">
                                        {{ index + 1 }}
                                    </CTableDataCell>
                                    <CTableDataCell class="text-center"> <img :src="product.image" alt=""
                                            style="width: 100px;"> </CTableDataCell>
                                    <CTableDataCell>{{ product.label }} </CTableDataCell>
                                    <CTableDataCell>
                                        <CButton color="danger" variant="ghost"
                                            @click="updateProduct('remove', index, product._id)">
                                            Remove Product
                                        </CButton>
                                    </CTableDataCell>
                                </CTableRow>
                            </CTableBody>
                        </CTable>
                    </CCol>
                </CRow>
            </CModalBody>
            <CModalFooter>
                <CButton color="secondary" @click="() => { addDashboardProductsModal = false }">
                    Close
                </CButton>
            </CModalFooter>
        </CModal>
    </div>
</template>
  
<script>
import { ref } from 'vue'
import upload from "../mixins/upload.vue";
import Pagination from 'v-pagination-3';
import useVuelidate from '@vuelidate/core';
import { required, helpers } from '@vuelidate/validators';
export default {
    name: 'Banners',
    mixins: [upload],
    setup() {
        const categories = ref([])
        const addDashboardCategoryModal = ref(false)
        const file = ref(null);
        const addDashboardProductsModal = ref(false)
        const products = ref([])
        const page = ref(1)
        const recordPerPage = ref(10)
        const productDetail = ref({})
        return {
            v$: useVuelidate(),
            categories,
            addDashboardCategoryModal,
            category: ref({ title: null, is_viewable: false }),
            file,
            addDashboardProductsModal,
            page,
            recordPerPage,
            products,
            productDetail,
            search: null
        }
    },
    validations() {
        return {
            category: {
                title: { required: helpers.withMessage("Title is required", required) }
            }
        }
    },
    components: {
        Pagination
    },
    methods: {
        openDashboardCategoryModal() {
            this.category = { title: null, is_viewable: false }
            this.v$.$reset()
            this.addDashboardCategoryModal = true
        },
        async showDashboardProductsModal(id) {
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/app-dashboard/${id}`;
            this.axios.get(url).then((response) => {
                this.productDetail = response.data.data
                this.getProducts()
            })
        },
        async updateProduct(activity = "add", index = 0, productId = "") {
            if (activity == "add") {
                this.productDetail.products_id.push(productId)
            } else {
                this.productDetail.products_id = this.productDetail.products_id.filter(pId => pId != productId)
            }
            const params = this.productDetail.products_id
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/app-dashboard/${this.productDetail._id}`;
            this.axios.put(url, { products_id: params }).then(() => {
                if (activity == "add") {
                    let product = this.products.splice(index, 1)
                    this.productDetail.products.push(product[0])
                } else {
                    let product = this.productDetail.products.splice(index, 1)
                    this.products.push(product[0])
                }
            })
        },

        getDashboardCategory() {
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/app-dashboard`;
            this.axios.get(url).then((response) => {
                this.categories = response.data.data
                // this.$forceUpdate()
            })
        },
        getDashboardCategoryDetail(id) {
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/app-dashboard/${id}`;
            this.axios.get(url).then((response) => {
                this.productDetail = response.data.data
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
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/app-dashboard/status/${id}`;
            this.axios.put(url).then((response) => {
                this.getDashboardCategory()
            })
        },
        async deleteCategory(id) {
            const confirm = await this.$swal.fire({
                title: 'Are you sure want to delete the banner ?',
                showCancelButton: true,
                confirmButtonText: 'Delete',
            });
            if (!confirm.isConfirmed) return false;
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/app-dashboard/${id}`;
            this.axios.delete(url).then((response) => {
                this.getDashboardCategory()
            })
        },
        async saveCategory() {
            const result = await this.v$.$validate()
            if (!result) return false;
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/app-dashboard`
            this.axios.post(url, this.category).then(() => {
                this.$swal("Category successfully added");
                this.category = { title: null, is_viewable: false }
                this.getDashboardCategory()
                this.addDashboardCategoryModal = false
            })
        },
        paginateProducts(page) { this.page = page; this.getProducts() },
        getProducts() {
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/products`;
            this.axios.get(url, { params: { page: this.page, limit: this.recordPerPage, search: this.search } }).then((response) => {
                this.products = response.data.data.list.filter(p => !this.productDetail.products_id.includes(p._id))
                this.total = response.data.data.count
                this.$forceUpdate()
                if (!this.addDashboardProductsModal) {
                    this.addDashboardProductsModal = true
                }
            })
        },
    },
    mounted() {
        this.getDashboardCategory();
    }
}
</script>
  