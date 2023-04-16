<template>
    <div>
        <CRow>
            <CCol :md="12">
                <CCard class="mb-4">
                    <CCardHeader> Products </CCardHeader>
                    <CCardBody>
                        <CButton color="primary" style="margin-bottom: 10px;" @click="openAddProductModal()">
                            Add new product </CButton>
                        <br>
                        <CTable align="middle" class="mb-0 border" hover responsive bordered>
                            <CTableHead color="light">
                                <CTableRow>
                                    <CTableHeaderCell class="text-center">#</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">Image</CTableHeaderCell>
                                    <CTableHeaderCell>Name</CTableHeaderCell>
                                    <CTableHeaderCell>Description</CTableHeaderCell>
                                    <CTableHeaderCell>Category</CTableHeaderCell>
                                    <CTableHeaderCell>Inventory</CTableHeaderCell>
                                    <CTableHeaderCell>Is Viewable</CTableHeaderCell>
                                    <CTableHeaderCell>Prices</CTableHeaderCell>
                                    <CTableHeaderCell>Created At</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">Activity</CTableHeaderCell>
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
                                    <CTableDataCell>{{ product.subtext }} </CTableDataCell>
                                    <CTableDataCell>{{ product.category.label }} </CTableDataCell>
                                    <CTableDataCell>{{ product.inventory }} </CTableDataCell>
                                    <CTableDataCell> {{ product.is_viewable }} </CTableDataCell>
                                    <CTableDataCell>
                                        <template v-if="product.price" v-for="p in Object.keys(product.price)" v-key="p">
                                            <span><b>{{ p }} : </b> {{ product.price[p] }}</span><br>
                                        </template>
                                    </CTableDataCell>
                                    <CTableDataCell> {{ $dayjs(product.createdAt).format("DD/MM/YYYY")
                                    }}</CTableDataCell>
                                    <CTableDataCell>
                                        <CButton color="primary" variant="ghost" @click="getProduct(product._id)">Edit
                                            product </CButton>
                                    </CTableDataCell>
                                </CTableRow>
                            </CTableBody>
                        </CTable>
                        <pagination v-if="products.length > 0" v-model="page" :records="total" :per-page="recordPerPage"
                            @paginate="paginateProducts" />
                    </CCardBody>
                </CCard>
            </CCol>
        </CRow>

        <CModal scrollable :visible="showAddProductModal" backdrop="static" size="lg" @close="
            () => {
                showAddProductModal = false
            }
        ">
            <CModalHeader dismiss @close="
                () => {
                    showAddProductModal = false
                }
            ">
                <CModalTitle>Add new product</CModalTitle>
            </CModalHeader>
            <CModalBody>
                <CRow>
                    <CCol>
                        <file-uploader :label="'Product Image'" @upload="uploadProductImage"></file-uploader>
                        <p style="color: red;" v-for="error of v$.productDetail.image.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </CCol>
                </CRow>
                <CRow>
                    <CCol>
                        <CFormLabel class="col-form-label"> Product Name </CFormLabel>
                        <CFormInput class="form-control" type="text" v-model="productDetail.label"
                            placeholder="Enter product name" />
                        <p style="color: red;" v-for="error of v$.productDetail.label.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </CCol>
                </CRow>
                <CRow>
                    <CCol>
                        <CFormLabel class="col-form-label"> Description </CFormLabel>
                        <CFormInput class="form-control" type="text" v-model="productDetail.subtext"
                            placeholder="Enter Product description (e.g. 500 gms jar)" />
                        <p style="color: red;" v-for="error of v$.productDetail.subtext.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </CCol>
                </CRow>
                <CRow>
                    <CCol>
                        <CFormLabel class="col-form-label"> Inventory </CFormLabel>
                        <CFormInput class="form-control" type="number" v-model="productDetail.inventory"
                            placeholder="Enter avaible product count " />
                        <p style="color: red;" v-for="error of v$.productDetail.inventory.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </CCol>
                </CRow>
                <CRow>
                    <CCol>
                        <CFormLabel class="col-form-label">Select Category</CFormLabel>
                        <CFormSelect v-model="productDetail.category" class="form-control">
                            <option value="">Select category</option>
                            <option v-for=" category in categories" v-bind:key="category.label" :value="category._id">{{
                                category.label
                            }}
                            </option>
                        </CFormSelect>
                        <p style="color: red;" v-for="error of v$.productDetail.category.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </CCol>
                </CRow>
                <CRow>
                    <CCol>
                        <CFormLabel class="col-form-label">Is viewable</CFormLabel>
                        <CFormCheck type="radio" name="isViewable" id="isViewable1" :value="true"
                            v-model="productDetail.is_viewable" label="true" />
                        <CFormCheck type="radio" name="isViewable" id="isViewable2" :value="false"
                            v-model="productDetail.is_viewable" label="false" checked />
                    </CCol>
                </CRow>
                <CRow>
                    <CCol>
                        <h5>Prices</h5>
                        <hr>
                    </CCol>
                </CRow>
                <CRow v-for="userType in userTypes" :key="userType">
                    <CCol>
                        <CFormLabel class="col-form-label"> {{ userType }} </CFormLabel>
                        <CFormInput class="form-control" type="number" v-model="productDetail.price[userType]"
                            placeholder="Enter avaible product count " />
                        <p style="color: red;" v-for="error of v$.productDetail.price[userType].$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </CCol>
                </CRow>
            </CModalBody>
            <CModalFooter>
                <CButton color="secondary" @click="() => { showAddProductModal = false }">
                    Close
                </CButton>
                <CButton color="success" @click="createProduct()">{{ productDetail._id ? 'Edit' : 'Create' }}</CButton>
            </CModalFooter>
        </CModal>
    </div>
</template>
  
<script>
// import dayjs from 'dayjs'
import Pagination from 'v-pagination-3';
import upload from "../mixins/upload.vue";
import useVuelidate from "@vuelidate/core"
import { required, helpers, numeric } from "@vuelidate/validators";

export default {
    name: 'Products',
    mixins: [upload],
    data() {
        return {
            v$: useVuelidate(),
            categories: [],
            products: [],
            product: null,
            page: 1,
            recordPerPage: 10,
            total: 0,
            showAddProductModal: false,
            userTypes: [],
            productDetail: {
                label: null,
                subtext: null,
                inventory: 0,
                category: "",
                image: null,
                is_viewable: false,
                price: {}
            }
        }
    },
    validations() {
        const price = {}
        this.userTypes.forEach(userType => {
            price[userType] = { required: helpers.withMessage(`${userType} product price is required`, required), numeric }
        })
        return {
            productDetail: {
                label: { required: helpers.withMessage("Product Label is required", required) },
                subtext: { required: helpers.withMessage("Product Description is required", required) },
                inventory: { required: helpers.withMessage("Product inventory is required", required), numeric },
                category: { required: helpers.withMessage("Product Category is required", required) },
                image: { required: helpers.withMessage("Product Image is required", required) },
                price: price
            }
        }
    },
    components: {
        Pagination
    },
    methods: {
        openAddProductModal() {
            this.productDetail = {
                label: null,
                subtext: null,
                inventory: 0,
                category: "",
                image: null,
                is_viewable: false,
                price: {}
            }
            this.v$.$reset()
            this.showAddProductModal = true
        },
        async uploadProductImage(image) {
            this.productDetail.image = image;
        },
        paginateProducts(page) { this.page = page; this.getProducts() },
        getProducts() {
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/products`;
            this.axios.get(url, { params: { page: this.page, limit: this.recordPerPage } }).then((response) => {
                this.products = response.data.data.list
                this.total = response.data.data.count
                this.$forceUpdate()
            })
        },
        getCategories() {
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/categories`;
            this.axios.get(url).then((response) => {
                this.categories = response.data.data
                // this.$forceUpdate()
            })
        },
        getUserTypes() {
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/user-type`;
            this.axios.get(url).then((response) => {
                this.userTypes = response.data.data
                // this.$forceUpdate()
            })
        },
        async createProduct() {
            const result = await this.v$.$validate()
            if (!result) return

            if (this.productDetail._id) {
                const url = `${process.env.VUE_APP_API_URL}api/v1/admin/products/${this.productDetail._id}`
                delete this.productDetail._id
                delete this.productDetail.updatedAt
                delete this.__v
                delete this.updatedAt

                this.axios.post(url, this.productDetail).then(() => {
                    this.$swal("Product successfully updated");
                    this.getProducts()
                    this.showAddProductModal = false
                })
            } else {
                const url = `${process.env.VUE_APP_API_URL}api/v1/admin/products`
                this.axios.post(url, this.productDetail).then(() => {
                    this.$swal("Product successfully created");
                    this.getProducts()
                    this.showAddProductModal = false
                })
            }
        },
        getProduct(id) {
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/products/${id}`
            this.axios.get(url).then((response) => {
                this.productDetail = response.data.data
                this.v$.$reset()
                this.showAddProductModal = true
            })
        }
    },
    mounted() {
        this.getProducts();
        this.getCategories();
        this.getUserTypes()
    }
}
</script>
  