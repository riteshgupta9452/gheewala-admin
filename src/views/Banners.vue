<template>
    <div>
        <CRow>
            <CCol :md="12">
                <CCard class="mb-4">
                    <CCardHeader> Banner </CCardHeader>
                    <CCardBody>
                        <CButton color="primary" style="margin-bottom: 10px;" @click="openAddBannerModal()">Add
                            new
                            banner</CButton>
                        <CTable align="middle" class="mb-0 border" hover responsive>
                            <CTableHead color="light">
                                <CTableRow>
                                    <CTableHeaderCell class="text-center">#</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">Banner</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">is viewable</CTableHeaderCell>
                                    <CTableHeaderCell>Activity</CTableHeaderCell>
                                </CTableRow>
                            </CTableHead>
                            <CTableBody>
                                <CTableRow v-for="(banner, index) in banners" :key="banner._id">
                                    <CTableDataCell class="text-center">
                                        {{ index + 1 }}
                                    </CTableDataCell>
                                    <CTableDataCell><img :src="banner.banner" alt="" style="height: 100px;">
                                    </CTableDataCell>
                                    <CTableDataCell class="text-center">
                                        {{ banner.is_viewable }}
                                    </CTableDataCell>
                                    <CTableDataCell>
                                        <CButton :color="banner.is_viewable ? 'danger' : 'success'" variant="ghost"
                                            @click="toggleStatus(banner._id)">{{ banner.is_viewable ? 'Hide' : 'Show' }}
                                        </CButton>
                                        <CButton color="danger" variant="ghost" @click="deleteBanner(banner._id)">Delete
                                        </CButton>
                                    </CTableDataCell>
                                </CTableRow>
                            </CTableBody>
                        </CTable>
                    </CCardBody>
                </CCard>
            </CCol>
        </CRow>
        <CModal scrollable :visible="addBannerModal" backdrop="static" size="xl" @close="
            () => {
                addBannerModal = false;
            }
        ">
            <CModalHeader dismiss @close="
                () => {
                    addBannerModal = false
                }
            ">
                <CModalTitle>Add new Banner</CModalTitle>
            </CModalHeader>
            <CModalBody>
                <CForm>
                    <div class="mb-3">
                        <file-uploader :label="'Please Select Banner Image'" @upload="uploadIconImage"></file-uploader>
                        <!-- <input type="text" v-model="banner.banner"> -->
                        <p style="color: red;" v-for="error of v$.banner.banner.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </div>
                    <div>
                        <CFormLabel class="col-form-label">Is viewable</CFormLabel>
                        <CFormCheck type="radio" name="isViewable" id="isViewable1" :value="true"
                            v-model="banner.is_viewable" label="true" />
                        <CFormCheck type="radio" name="isViewable" id="isViewable2" :value="false"
                            v-model="banner.is_viewable" label="false" checked />
                    </div>
                </CForm>
            </CModalBody>
            <CModalFooter>
                <CButton color="secondary" @click="() => { addBannerModal = false; }">
                    Close
                </CButton>
                <CButton color="success" @click="saveBanner()">Save</CButton>
            </CModalFooter>
        </CModal>
    </div>
</template>
  
<script>
import { ref } from 'vue'
import upload from "../mixins/upload.vue";
import FileUploader from "@/components/FileUploader.vue"
import useVuelidate from "@vuelidate/core"
import { required, helpers } from "@vuelidate/validators";
export default {
    name: 'Banners',
    mixins: [upload],
    setup() {
        const banners = ref([])
        const addBannerModal = ref(false)
        const file = ref(null);
        return {
            banners,
            addBannerModal,
            banner: ref({ banner: null, is_viewable: false }),
            file,
            v$: useVuelidate()
        }
    },
    validations() {
        return {
            banner: {
                banner: { required: helpers.withMessage("Banner is required", required) },
                is_viewable: { required }
            }
        }
    },

    components: {
        FileUploader
    },
    methods: {
        openAddBannerModal() {
            this.banner = { banner: null, is_viewable: false }
            this.v$.$reset()
            this.addBannerModal = true
        },
        async uploadIconImage(imageUrl) {
            // const input = $event.target;
            // let file = input.files && input.files[0] ? input.files[0] : null
            // if (!file) {
            //     return;
            // }
            this.banner.banner = imageUrl;

        },
        getBanners() {
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/banners`;
            this.axios.get(url).then((response) => {
                this.banners = response.data.data
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
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/banners/status/${id}`;
            this.axios.put(url).then((response) => {
                this.getBanners()
            })
        },
        async deleteBanner(id) {
            const confirm = await this.$swal.fire({
                title: 'Are you sure want to delete the banner ?',
                showCancelButton: true,
                confirmButtonText: 'Delete',
            });
            if (!confirm.isConfirmed) return false;
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/banners/${id}`;
            this.axios.delete(url).then((response) => {
                this.getBanners()
            })
        },
        async saveBanner() {
            const result = await this.v$.$validate()
            if (!result) { return }
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/banners`
            this.axios.post(url, this.banner).then(() => {
                this.$swal("Banner successfully added");
                this.banner = { banner: null, is_viewable: false }
                this.getBanners()
                this.addBannerModal = false
                this.v$.$reset()
            })
        }
    },
    mounted() {
        this.getBanners();
    }
}
</script>
  