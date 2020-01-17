
<script>
export default {
  name: "Domain",
  props: ["domains"],
  data() {
    return {};
  }
};
</script>


<template>
  <div>
    <br />
    <h5>
      Domains
      <span class="badge badge-info">{{domains.length}}</span>
    </h5>
    <div class="card">
      <div class="card-body">
        <ul class="list-group">
          <li class="list-group-item" v-for="domain in domains" v-bind:key="domain.name">
            <div class="row">
              <div class="col-md">{{domain.name}}</div>
              <div class="col-md text-right">
                <a class="btn btn-info" v-bind:href="domain.link" target="_blank">
                  <span class="fa fa-shopping-cart"></span>
                </a>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>



<style>
</style>
