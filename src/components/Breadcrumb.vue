<template>
  <div class="breacrumb-section">
    <div class="container">
      <div class="row">
        <div class="col-lg-12 text-left">
          <div class="breadcrumb-text product-more">
            <router-link to="/">
              <i class="fa fa-home"></i> Home
            </router-link>
            <span>{{ title }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'Breadcrumb',
    props: {
      title: String
    }
}
</script>