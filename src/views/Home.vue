<template>
  <div class="main-flex card" v-if="!$store.getters.getLogged">

    <el-card>
      <div class="grid top-grid">
        <h2 class="title">NÃO SE CALE!</h2>
        <h4 class="subtitle">PEÇA AJUDA!</h4>
        <el-button class="ajuda" type="danger" size="big" @click="helpme">
          ME AJUDE!
        </el-button>

      </div>
    </el-card>
    <el-card>
      <div class="info">

        <h2>O QUE É VIOLÊNCIA DOMÉSTICA ?</h2>
        <p class="violencia">
          Violência doméstica é todo tipo de violência que é praticada entre os membros que habitam um ambiente familiar em comum.
        </p>
        <el-divider>
          <font-awesome-icon class="icon" icon="heart" />
        </el-divider>
        <h2 class="existe-lei">EXISTE LEI ?</h2>

        <p class="title-lei">
          A Lei Maria da Penha, em vigor desde 2006. ela cria mecanismos para prevenir e coibir a violência doméstica e familiar contra a mulher
        </p>

        <p class="lei">A Lei Maria da Penha (11.340/16) é o instrumento principal no
          enfrentamento à violência doméstica e familiar contra mulheres no
          Brasil. Mais do que física, ela abrange abusos sexuais, psicológicos,
          morais e patrimoniais entre a vítima e seu agressor – que não precisa
          necessariamente ser cônjuge, basta que tenha algum tipo de relação
          afetiva.</p>

        <el-divider>
          <font-awesome-icon class="icon" icon="heart" />
        </el-divider>
      </div>

    </el-card>
    <el-card>
      <div class="ciclos">
        <CicloItem :key="ciclo.title" :icon="ciclo.icon" :text="ciclo.text" :title="ciclo.title" :index="index+1" v-for="(ciclo,index) in clicos">
          <el-divider>
          </el-divider>
        </CicloItem>
      </div>
    </el-card>
  </div>
</template>

<script>
import CicloItem from "../components/CicloItem.vue";
export default {
  name: "Home",
  components: {
    CicloItem,
  },
  data() {
    return {
      clicos: [
        {
          title: "AUMENTO DA TENSÃO",
          text: "Nesse primeiro momento, o agressor mostra-se tenso e irritado por coisas insignificantes, chegando a ter acessos de raiva. Ele também humilha a vítima, faz ameaças e destrói objetos.",
          icon: "thermometer-half",
        },
        {
          title: "ATO DE VIOLÊNCIA",
          text: "Esta fase corresponde à explosão do agressor, ou seja, a falta de controle chega ao limite e leva ao ato violento. Aqui, toda a tensão acumulada na Fase 1 se materializa em violência verbal, física, psicológica, moral ou patrimonial.",
          icon: "fist-raised",
        },
        {
          title: "ARREPENDIMENTO E COMPORTAMENTO CARINHOSO",
          text: "Também conhecida como “lua de mel”, esta fase se caracteriza pelo arrependimento do agressor, que se torna amável para conseguir a reconciliação.<br><br> A mulher se sente confusa e pressionada a manter o seu relacionamento diante da sociedade, sobretudo quando o casal tem filhos. Um misto de medo, confusão, culpa e ilusão fazem parte dos sentimentos da mulher.<br><br> Por fim, a tensão volta e, com ela, as agressões da Fase 1.",
          icon: "hand-holding-heart",
        },
      ],
    };
  },
  methods: {
    helpme() {
      this.$router.push({ name: "Pedir Ajuda!" });
    },
  },
};
</script>

<style scoped>
p,
h2 {
  text-align: center;
}
.card {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}
.main-flex {
  flex-direction: column;
  align-items: stretch;
  gap: 2em;
}
.grid {
  display: grid;
  min-height: 500px;
  grid-gap: 20px;
  grid-row-gap: 55px;
}
.info {
  display: flex;
  flex-direction: column;
  gap: 25px;
  padding: 0 20%;
}
.ciclos {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  gap: 50px;
}
.top-grid {
  display: flex;
  flex-direction: column;
  padding: 10px 15em;
  gap: 0px;
  min-height: unset;
}

@media (max-width: 800px) {
  .main-grid {
    display: grid;
    min-height: 500px;
    grid-gap: 20px;
    align-items: stretch;
    grid-template-columns: 1fr;
    grid-template-areas:
      "paralax"
      "denuncia"
      "user";
  }
  .title {
    display: none;
  }
  .acesso-rapido {
    display: none;
  }
}
.ajuda {
  grid-area: ajuda;
  height: 80px;
}
.title {
  grid-area: title;
  font-size: 36pt;
  margin: 0;
}
.subtitle {
  grid-area: subtitle;
  font-size: 16pt;
}
.lei {
  grid-area: lei;
}
.title-lei {
  grid-area: title-lei;
}
.existe-lei {
  grid-area: existe-lei;
}
.violencia {
  grid-area: oque;
}
</style>
