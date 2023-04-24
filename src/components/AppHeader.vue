<template>
  <CHeader position="sticky" class="mb-4">
    <CContainer fluid>
      <CHeaderToggler class="ps-1" @click="$store.commit('toggleSidebar')">
        <CIcon icon="cil-menu" size="lg" />
      </CHeaderToggler>
      <CHeaderBrand class="mx-auto d-lg-none" to="/">
        <CIcon :icon="logo" height="48" alt="Logo" />
      </CHeaderBrand>
      <CHeaderNav>
        <CNavItem>
          <CNavLink @click="logout">Logout</CNavLink>
        </CNavItem>
      </CHeaderNav>
    </CContainer>
    <CHeaderDivider />
    <CContainer fluid>
      <AppBreadcrumb />
    </CContainer>
  </CHeader>
</template>

<script>
import AppBreadcrumb from './AppBreadcrumb'
import AppHeaderDropdownAccnt from './AppHeaderDropdownAccnt'
import { logo } from '@/assets/brand/logo'
export default {
  name: 'AppHeader',
  components: {
    AppBreadcrumb,
    AppHeaderDropdownAccnt,
  },
  setup() {
    return {
      logo,
    }
  },
  methods: {
    async logout() {
      const confirm = await this.$swal.fire({
        title: 'Are you sure want to logout ?',
        showCancelButton: true,
        confirmButtonText: 'Yes',
        cancelButtonText: "No"
      });
      if (!confirm.isConfirmed) return false;
      localStorage.removeItem("token")
      this.$router.replace("/")
    }
  }
}
</script>
