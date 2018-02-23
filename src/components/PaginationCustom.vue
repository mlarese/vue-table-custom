<template>
    <div class="pagination-container">
      <uib-pagination
              :boundary-links="true"
              :previous-text="prevText"
              :next-text="nextText"
              :first-text="firstText"
              :last-text="lastText"
              v-model="pagination"
              :max-size="maxSize"
              :total-items="total"
              :items-per-page="perPage"
              :force-ellipses="true"
              @change="pageChanged()"
      />
    </div>
</template>

<script>
  export default {
    name: 'vue-good-pagination',
    props: {
      maxSize: {default: 5},
      styleClass: {default: 'table table-bordered'},
      total: {default: null},
      perPage: {},
      rtl: {default: false},
      firstText: {default: "«"},
      lastText: {default: "»"},
      // text options
      nextText: {default: '›'},
      prevText: {default: '‹'},
      rowsPerPageText: {default: 'Rows per page:'},
      ofText: {default: 'of'},
      allText: {default: 'All'}
    },

    data: () => ({
      pagination: {},
      currentPage: 1,
      currentPerPage: 10
    }),
    methods: {
      pageChanged() {
        this.$emit('page-changed', {currentPage: this.pagination.currentPage});
      }
    },
    computed: {
      paginatedInfo() {
        if (this.currentPerPage === -1) {
          return `1 - ${this.total} ${this.ofText} ${this.total}`;
        }
        const first = (this.currentPage - 1) * this.currentPerPage ? (this.currentPage - 1) * this.currentPerPage : 1;
        const last = Math.min(this.total, this.currentPerPage * this.currentPage);
        return `${first} - ${last} ${this.ofText} ${this.total}`;
      },
      nextIsPossible() {
        return (this.total > this.currentPerPage * this.currentPage);
      },
      prevIsPossible() {
        return this.currentPage > 1;
      }
    },

    mounted() {
      if (this.perPage) {
        this.currentPerPage = this.perPage;
      }
    }
  }
</script>

<style lang="css" scoped>
  /* Utility styles
  ************************************************/
  .right-align{
    text-align: right;
  }

  .left-align{
    text-align: left;
  }

  .center-align{
    text-align: center;
  }

  .pull-left{
    float:  left !important;
  }

  .pull-right{
    float:  right !important;
  }

  .clearfix::after {
    display: block;
    content: "";
    clear: both;
  }

  /* Table footer specific styles
  ************************************************/

  .table-footer{
    /* background-color: rgba(35,41,53, 0.03); */
    background-color: rgba(35,41,53,0.05);
    border: 1px solid #DDD;
    margin:  0;
    padding:  1rem;
    font-size: 14px;
    color:  rgba(0, 0, 0, 0.44);
  }

  .table-footer>div{
    display: inline-block;
  }

  .pagination-controls>*{
    display: inline-block;
  }

  .pagination-controls a{
    text-decoration: none;
    color: rgba(0, 0, 0, 0.66);
    font-size: 14px;
    font-weight: 600;
    opacity: 0.8;
  }

  .pagination-controls a.disabled,
  .pagination-controls a.disabled:hover {
    cursor: not-allowed;
    opacity: 0.4;
  }

  .pagination-controls a:hover{
    opacity: 1;
  }

  .pagination-controls a span{
    display: inline-block;
    vertical-align: middle;
  }

  .pagination-controls .info{
    margin:  0px 2px;
    font-size: 13px;
    font-weight: bold;
    color:  rgba(0, 0, 0, 0.40);
  }

  .pagination-controls a .chevron{
    width:  24px;
    height:  24px;
    border-radius: 15%;
    /* border:  1px solid rgba(35,41,53,0.2);
    background-color: #fff; */
    position:  relative;
    margin:  0px 8px;
  }

  .pagination-controls .chevron::after{
    content:  '';
    position:  absolute;
    display:  block;
    left:  50%;
    top:  50%;
    margin-top:  -6px;
    border-top: 6px solid transparent;
    border-bottom: 6px solid transparent;
  }

  .pagination-controls .chevron.left::after{
    border-right:  6px solid rgba(0, 0, 0, 0.66);
    margin-left:  -3px;
  }

  .pagination-controls .chevron.right::after{
    border-left:  6px solid rgba(0, 0, 0, 0.66);
    margin-left:  -3px;
  }

  .table-footer select {
    display: inline-block;
    background-color: transparent;
    width: auto;
    padding: 0;
    border: 0;
    border-radius: 0;
    height: auto;
    font-size: 14px;
    margin-left: 8px;
    color:  rgba(0, 0, 0, 0.55);
    font-weight: bold;
  }

  .table-footer .perpage-count{
    color:  rgba(0, 0, 0, 0.55);
    font-weight: bold;
  }

  @media only screen and (max-width: 750px) {
    /* on small screens hide the info */
    .pagination-controls .info{
      display:  none;
    }
  }
</style>
