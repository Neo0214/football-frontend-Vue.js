<template>
  <div>
    <subHeader :prop-active-index="1" />
    </div>
    <div class="main">
      <div class="title">检阅您的球队！</div>
        <div>
            <div class="one-title">门将</div>
            <div v-for="(group, index) in keeper" :key="index" class="row">
                <PlayerCardOnly v-for="(player, index) in group" :key="index" :card-info="player" />
            </div>

        </div>
        <div>
            <div class="one-title">前场</div>
            <div v-for="(group, groupIndex) in forward" :key="`group-${groupIndex}`" class="row">
                <PlayerCardOnly v-for="(player, playerIndex) in group" :key="`player-${groupIndex}-${playerIndex}`"
                    :card-info="player" />
            </div>
        </div>
        <div>
            <div class="one-title">中场</div>
            <div v-for="(group, index) in midfielder" :key="index" class="row">
                <PlayerCardOnly v-for="(player, index) in group" :key="index" :card-info="player" />
            </div>
        </div>
        <div>
            <div class="one-title">后防</div>
            <div v-for="(group, index) in defender" :key="index" class="row">
                <PlayerCardOnly v-for="(player, index) in group" :key="index" :card-info="player" />
            </div>
        </div>
  </div>

</template>

<script>
import subHeader from '@/components/sub-components/Header.vue';
import PlayerCardOnly from '@/components/sub-components/PlayerCardOnly.vue';

export default {
  name: 'PageHome',
  components: {
    subHeader,
    PlayerCardOnly
  },
  data() {
        return {
            keeper: [],
            forward: [],
            midfielder: [],
            defender: [],
        }
    },
    mounted() {
        this.$axios.get('/api/Transfer/getPlayerByPosition', {
            params: {
                clubId: localStorage.getItem('clubId'),
            }
        }).then(res => {
            console.log(res.data);
            this.keeper = res.data.keeper;
            this.forward = res.data.forward;
            this.midfielder = res.data.midfielder;
            this.defender = res.data.defender;
        }).catch(err => {
            this.$message.error('获取球员信息失败');
            console.log(err);

        })
    }
}

</script>


<style scoped>
.main {
  width: 95%;
    height: auto;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    padding: 2% 0 5% 5%;
}

.one-title {
    font-size: 40px;
    font-weight: bold;
    margin-bottom: 2%;
}

.row {
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 10px;
}
.title{
  font-size: 50px;
  font-weight: bold;
  margin-bottom: 20px;
  width:100%;
  text-align:center;
}
</style>