<template>
  <div class="container">
    <navbar/>
      <div class="icon">
        <audio controls id="myVideo" class="player">
          <source src="../assets/OpenCajasss.mp3" type="audio/wav">
          Your browser does not support the audio element.
        </audio>
      </div>
    </div>
</template>

<script>
  require('howler');
  export default {

    name: "CallEmployee",
    data() {
      return {}
    }
  }

</script>

<style>
  .container {
    background: #fff;
    border-radius: 4px;
    height: 80vh;
    padding: 0;
    width: 40%;
  }

  table.container {
    height: calc(100% - 66px);
    margin: 0;
    width: 100%;
  }

  .scrolleable {
    overflow: auto;
    max-height: calc(100% - 66px);
  }

  th {
    text-align: center;
  }
  .player{
    margin-top: 150px;
  }
</style>


