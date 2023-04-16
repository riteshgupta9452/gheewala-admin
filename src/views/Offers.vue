<template>
    <div>
        <CRow>
            <CCol :md="12">
                <CCard class="mb-4">
                    <CCardHeader> Banner </CCardHeader>
                    <CCardBody>
                        <CButton color="primary" style="margin-bottom: 10px;" @click="openAddOfferModal()">Add
                            new
                            Offer</CButton>
                        <CTable align="middle" class="mb-0 border" hover responsive>
                            <CTableHead color="light">
                                <CTableRow>
                                    <CTableHeaderCell class="text-center">#</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">Name</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">Description</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">Start Date</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">End Date</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">Offer Type</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">Offer Value</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">Offer Upto</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">Only for one time</CTableHeaderCell>
                                    <CTableHeaderCell class="text-center">is viewable</CTableHeaderCell>
                                    <CTableHeaderCell>Activity</CTableHeaderCell>
                                </CTableRow>
                            </CTableHead>
                            <CTableBody>
                                <CTableRow v-for="(offer, index) in offers" :key="offer._id">
                                    <CTableDataCell class="text-center">
                                        {{ index + 1 }}
                                    </CTableDataCell>
                                    <CTableDataCell>{{ offer.name }}</CTableDataCell>
                                    <CTableDataCell>{{ offer.description }}</CTableDataCell>
                                    <CTableDataCell>{{ offer.start_date ? $dayjs(offer.start_date).format("DD/MM/YYYY") :
                                        "-" }}</CTableDataCell>
                                    <CTableDataCell>{{ offer.end_date ? $dayjs(offer.end_date).format("DD/MM/YYYY") : "-" }}
                                    </CTableDataCell>
                                    <CTableDataCell>{{ offer.offer_type }}</CTableDataCell>
                                    <CTableDataCell>{{ offer.offer_value }}</CTableDataCell>
                                    <CTableDataCell>{{ offer.upto }}</CTableDataCell>
                                    <CTableDataCell>{{ offer.only_for_one_time }}</CTableDataCell>
                                    <CTableDataCell class="text-center">
                                        {{ offer.is_viewable }}
                                    </CTableDataCell>
                                    <CTableDataCell>
                                        <CButton :color="offer.is_viewable ? 'danger' : 'success'" variant="ghost"
                                            @click="toggleStatus(offer._id)">{{ offer.is_viewable ? 'Hide' : 'Show' }}
                                        </CButton>
                                        <CButton color="danger" variant="ghost" @click="deleteOffer(offer._id)">Delete
                                        </CButton>
                                    </CTableDataCell>
                                </CTableRow>
                            </CTableBody>
                        </CTable>
                    </CCardBody>
                </CCard>
            </CCol>
        </CRow>
        <CModal scrollable :visible="addOfferModal" backdrop="static" size="xl" @close="
            () => {
                addOfferModal = false
            }
        ">
            <CModalHeader dismiss @close="
                () => {
                    addOfferModal = false
                }
            ">
                <CModalTitle>Add new Offer</CModalTitle>
            </CModalHeader>
            <CModalBody>
                <CForm>
                    <div>
                        <CFormLabel class="col-form-label"> Offer Name </CFormLabel>
                        <CFormInput class="form-control" type="text" v-model="offer.name" placeholder="Enter offer name" />
                        <p style="color: red;" v-for="error of v$.offer.name.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </div>
                    <div>
                        <CFormLabel class="col-form-label"> Description </CFormLabel>
                        <CFormInput class="form-control" type="text" v-model="offer.description"
                            placeholder="Enter offer description" />
                        <p style="color: red;" v-for="error of v$.offer.description.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </div>
                    <div>
                        <CFormLabel class="col-form-label"> Offer type </CFormLabel><br>
                        <input type="radio" value="amount" name="offerType" v-model="offer.offer_type"> Amount <br>
                        <input type="radio" value="percentage" name="offerType" v-model="offer.offer_type"> Percentage
                        <!-- <CFormCheck type="radio" name="offerType" id="offerType1" :value="'amount'"
                            v-model="offer.offer_type" label="Amount" />
                        <CFormCheck type="radio" name="offerType" id="offerType2" :value="'percentage'"
                            v-model="offer.offer_type" label="Percentage" checked /> -->
                    </div>
                    <div>
                        <CFormLabel class="col-form-label"> Offer Value </CFormLabel>
                        <CFormInput class="form-control" type="number" v-model="offer.offer_value"
                            placeholder="Enter offer value" />
                        <p style="color: red;" v-for="error of v$.offer.offer_value.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </div>
                    <div>
                        <CFormLabel class="col-form-label"> Offer Value Upto </CFormLabel>
                        <CFormInput class="form-control" type="number" v-model="offer.upto"
                            placeholder="Enter offer value" />
                    </div>
                    <div>
                        <CFormLabel class="col-form-label"> Only for one time </CFormLabel>
                        <CFormCheck type="radio" name="onlyForOneTime" id="onlyForOneTime1" :value="true"
                            v-model="offer.only_for_one_time" label="true" />
                        <CFormCheck type="radio" name="onlyForOneTime" id="onlyForOneTime2" :value="false"
                            v-model="offer.only_for_one_time" label="false" checked />
                    </div>
                    <div>
                        <CFormLabel class="col-form-label"> Offer Start Date </CFormLabel>
                        <CFormInput class="form-control" type="date" v-model="offer.start_date"
                            placeholder="Enter offer value" />
                        <p style="color: red;" v-for="error of v$.offer.start_date.$errors" :key="error.$uid">
                            {{ error.$message }}
                        </p>
                    </div>
                    <div>
                        <CFormLabel class="col-form-label"> Offer End Date </CFormLabel>
                        <CFormInput class="form-control" type="date" v-model="offer.end_date"
                            placeholder="Enter offer value" />
                    </div>
                    <div>
                        <CFormLabel class="col-form-label">Is viewable</CFormLabel>
                        <CFormCheck type="radio" name="isViewable" id="isViewable1" :value="true"
                            v-model="offer.is_viewable" label="true" />
                        <CFormCheck type="radio" name="isViewable" id="isViewable2" :value="false"
                            v-model="offer.is_viewable" label="false" checked />
                    </div>
                </CForm>
            </CModalBody>
            <CModalFooter>
                <CButton color="secondary" @click="() => { addOfferModal = false }">
                    Close
                </CButton>
                <CButton color="success" @click="saveOffer()">Save</CButton>
            </CModalFooter>
        </CModal>
    </div>
</template>
  
<script>
import { ref } from 'vue'
import useVuelidate from '@vuelidate/core'
import { required, helpers, numeric } from '@vuelidate/validators'
export default {
    name: 'Offers',
    setup() {
        const offers = ref([])
        const addOfferModal = ref(false)
        let month = new Date().getMonth() + 1
        month = month > 9 ? month : `0${month}`
        const startDate = `${new Date().getFullYear()}-${month}-${new Date().getDate()}`;
        return {
            v$: useVuelidate(),
            offers,
            addOfferModal,
            startDate,
            offer: ref({ name: null, description: null, offer_type: "percentage", upto: null, offer_value: null, only_for_one_time: false, start_date: startDate, end_date: null, is_viewable: false }),
        }
    },
    validations() {
        return {
            offer: {
                name: { required: helpers.withMessage("Offer name/code is required", required) },
                description: { required: helpers.withMessage("Offer description is required", required) },
                offer_type: { required: helpers.withMessage("Offer type is required", required) },
                offer_value: { required: helpers.withMessage("Offer value is required", required), numeric },
                start_date: { required: helpers.withMessage("Offer name/code is required", required) },
            }
        }
    },

    methods: {
        openAddOfferModal() {
            this.offer = { name: null, description: null, offer_type: "percentage", upto: null, offer_value: null, only_for_one_time: false, start_date: this.startDate, end_date: null, is_viewable: false }
            this.v$.$reset()
            this.addOfferModal = true
        },
        getOffers() {
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/offers`;
            this.axios.get(url).then((response) => {
                this.offers = response.data.data
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
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/offers/status/${id}`;
            this.axios.put(url).then((response) => {
                this.getOffers()
            })
        },
        async deleteOffer(id) {
            const confirm = await this.$swal.fire({
                title: 'Are you sure want to delete the offer ?',
                showCancelButton: true,
                confirmButtonText: 'Delete',
            });
            if (!confirm.isConfirmed) return false;
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/offers/${id}`;
            this.axios.delete(url).then((response) => {
                this.getOffers()
            })
        },
        async saveOffer() {
            const result = await this.v$.$validate()
            if (!result) return false
            const url = `${process.env.VUE_APP_API_URL}api/v1/admin/offers`
            this.axios.post(url, this.offer).then(() => {
                this.$swal("Offer successfully added");
                this.offer = { name: null, description: null, offer_type: "percentage", upto: null, offer_value: null, only_for_one_time: false, start_date: this.startDate, end_date: null, is_viewable: false }
                this.getOffers()
                this.addOfferModal = false
            })
        }
    },
    mounted() {
        this.getOffers();
    }
}
</script>
  