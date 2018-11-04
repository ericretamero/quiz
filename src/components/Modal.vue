<template>
<div id="myModal" class="modal">

  <!-- Modal content -->
  <div class="modal-content">
    <div class="modal-header">
      <span class="close" @click="onClose">&times;</span>
      <h2>Result</h2>
    </div>
    <div class="modal-body">
      <p>{{msg.correct}} of 10</p>
      <p>You where better then {{ msg.total }}% of all quizer</p>
    </div>
    <!-- <div class="modal-footer">
      <h3>Modal Footer</h3>
    </div> -->
  </div>

</div>
</template>

<script>
export default {
  name: "Modal",
  props: {
    msg: Object
  },
  methods: {
    onClose: function() {
       this.$emit("close", true);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  lang="less" scoped>

  @fern:    #5cb85c; 
  @white:   #FFFFFF; 
  @black:   #000000;
  @gray:    #888888;


/* The Modal (background) */
.modal {
  display: block; 
  position: fixed; 
  z-index: 1; 
  padding-top: 100px; 
  left: 0;
  top: 0;
  width: 100%;
  height: 100%; 
  overflow: auto; 
  background-color: @black;
  background-color: fade(@black, 30%);
}

/* Modal Content */
.modal-content {
  position: relative;
  background-color: @white;
  margin: auto;
  padding: 0;
  border: 1px solid @gray;
  width: 80%;
  box-shadow: 0 4px 8px 0 fade(@black, 50%);
  text-align: center
}

/* The Close Button */
.close {
  color:  @white;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: @black;
  text-decoration: none;
  cursor: pointer;
}

.modal-header {
  padding: 2px 16px;
  background-color: @fern;
  color:  @white;
}

.modal-body {
  padding: 2px 16px;
}

.modal-footer {
  padding: 2px 16px;
  background-color: @fern;
  color:  @white;
}
</style>
