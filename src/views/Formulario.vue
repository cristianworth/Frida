<template>
  <h3>
    Formulário de Avaliação de Risco em Violência Doméstica e Familiar contra a
    Mulher
  </h3>
  <el-form ref="form" :model="questions">
    <el-form-item prop="nome">
      <el-input v-model="form.nome" placeholder="Nome"></el-input>
    </el-form-item>
    <div v-bind:key="question.id" v-for="question in questions">
      <Question v-model="question.answer" :alternatives="question.alternatives" :question="question" />
    </div>
  </el-form>
  <el-button @click="onClick" type="success">Salvar</el-button>
</template>

<script>
import Question from "../components/Question";
import * as controller from "../controller/ctlFormulario";

export default {
  name: "App",
  components: {
    Question,
  },
  methods: {
    onClick() {
      var invalid = false;

      for (const q of this.questions) {
        if (q.answer == undefined) {
          this.$message.error("Questão " + q.id + " é obrigatória");
          invalid = true;
          break;
        }
        this.form.resposta["r_" + q.id] = q.answer;
      }

      if (invalid == false) {
        this.form.resultado = this.geraResultado(this.form.resposta);
        if (this.id) {
          controller.alterar(this.id, this.form).then(() => {
            this.$router.push({ path: "ListarAvaliacao" });
          });
        } else {
          controller.incluir(this.form).then(() => {
            this.$router.push({ path: "ListarAvaliacao" });
          });
        }
      }
    },
    geraResultado(respostas) {
      var resp = Object.values(respostas);
      var counts = {};
      for (var num of resp) {
          counts[num] = counts[num] ? counts[num] + 1 : 1;
      }

      var qtdeSim = counts[1];
      var qtdeNao = /*counts[2]*/ + counts[3] + counts[4];
      var risco = '';

      switch (qtdeSim) {
          case 0:
          case 1:
          case 2:
              risco = (qtdeNao > 0) ? 'M' : 'B';
              break;
          case 3:
              risco = (qtdeNao > 7) ? 'M' : 'B';
              break;
          case 4:
              risco = (qtdeNao > 3) ? 'M' : 'B';
              break;
          case 5:
              risco = (qtdeNao == 10) ? 'E' : 'M';
              break;
          case 6:
              risco = (qtdeNao > 7 && qtdeNao < 11) ? 'E' : 'M';
              break;
          case 7:
              risco = (qtdeNao > 5 && qtdeNao < 11) ? 'E' : 'M';
              break;
          case 8:
              risco = (qtdeNao > 3 && qtdeNao < 11) ? 'E' : 'M';
              break;
          case 9:
              risco = (qtdeNao > 1 && qtdeNao < 11) ? 'E' : 'M';
              break;
          default:
              risco = 'E';
              break;
      }

      return {
          qtdeSim: qtdeSim,
          qtdeNao: qtdeNao,
          risco: risco,
      };
    }
  },
  data() {
    return {
      id: null,
      form: {
        nome: "",
        data: new Date().toISOString(),
        resposta: {},
        resultado: {},
      },
      items: [
        {
          to: "/",
          text: "Home",
        },
      ],
      questions: [
        {
          text: "Questão 1 - A violência vem aumentando de gravidade e/ou de frequência no último mês?",
          id: 1,
        },
        {
          text: "Questão 2 - A senhora/você está grávida ou teve bebê nos últimos 18 meses?",
          id: 2,
        },
        {
          text:
            "Questão 3 - A senhora/você tem filhos(as) com o(a) agressor(a)? (caso não tenham filhos em comum, registre não se aplica)." +
            " Em caso afirmativo, estão vivendo algum conflito com relação à guarda dos filhos, visitas ou pagamento de pensão pelo agressor?",
          id: 3,
        },
        {
          text:
            "Questão 4 - O(A) agressor(a) persegue a senhora/você, demonstra ciúmes excessivo, tenta controlar sua vida e as coisas que você faz? " +
            " (aonde você vai, com quem conversa, o tipo de roupa que usa, etc.)",
          id: 4,
        },
        {
          text:
            "Questão 5 - A senhora/você se separou recentemente do(a) agressor(a), tentou ou tem intenção de se separar?" +
            "\n Especifique: Separou □ Tentou □ Manifestou intenção □",
          /*outrasAlternativas: [
            {
              value: 1,
              text: "Separou",
            },
            {
              value: 2,
              text: "Tentou",
            },
            {
              value: 2,
              text: "Manifestou intenção",
            },
          ],*/
          id: 5,
        },
        {
          text:
            "Questão 6 - O(A) agressor(a) também é violento com outras pessoas (familiares, amigos, colegas etc.)" +
            "\n Especifique: Crianças □ Outros familiares □ Outras pessoas □",
          id: 6,
        },
        {
          text:
            "Questão 7 - A senhora/ você possui algum animal doméstico? (caso não tenha animal doméstico, registre não se aplica)" +
            "\n Em caso afirmativo, o(a) agressor(a) maltrata ou agride o animal?",
          id: 7,
        },
        {
          text: "Questão 8 - O(A) agressor(a) já a agrediu fisicamente outras vezes?",
          id: 8,
        },
        {
          text: "Questão 9 - Alguma vez o(a) agressor(a) tentou estrangular, sufocar ou afogar a senhora/você?",
          id: 9,
        },
        {
          text: "Questão 10 - O(A) agressor(a) já fez ameças de morte ou tentou matar a senhora/você?",
          id: 10,
        },
        {
          text:
            "Questão 11 - O(A) agressor(a) já usou, ameaçou usar arma de fogo contra a senhora/você ou tem fácil acesso a uma arma?" +
            "\n Especifique: Usou □ Ameaçou usar □ Tem fácil acesso □",
          id: 11,
        },
        {
          text: "Questão 12 - O(A) agressor(a) já a ameaçou ou feriu com outro tipo de arma ou instrumento?",
          id: 12,
        },
        {
          text:
            "Questão 13 - A senhora/você necessitou de atendimento médico e/ou internação após algumas dessas agressões?" +
            "\n Especifique: Atendimento médico □ Internação",
          id: 13,
        },
        {
          text: "Questão 14 - O(A) agressor(a) é usuário de drogas e/ou bebidas alcóolicas",
          id: 14,
        },
        {
          text: "Questão 15 - O(A) agressor(a) faz uso de medicação controlada para alguma doença mental/psiquiátrica?",
          id: 15,
        },
        {
          text:
            "Questão 16 - A senhora/você já teve ou tem medida protetiva de urgência? (caso não tenha tido medidas protetivas de urgência antes, registre não se aplica)." +
            " O(A) agressor(a) já descumpriu medida protetiva de afastamento ou proibição de contato?",
          id: 16,
        },
        {
          text: "Questão 17 - O(A) agressor(a) já ameaçou ou tentou se matar alguma vez?",
          id: 17,
        },
        {
          text: "Questão 18 - O(A) agressor(a) já obrigou a senhora/você a ter relações sexuais contra a sua vontade?",
          id: 18,
        },
        {
          text: "Questão 19 - O(A) agressor(a) está com dificuldades financeiras, está desempregado ou tem dificuldade de se manter no emprego?",
          id: 19,
        },
      ],
    };
  },
  async created() {
    try {
      var id = this.$route.query.id;
      if (id) {
        var dados = await controller.bucarPorId(id);
        this.form = dados;
        this.id = id;

        for (var [key, answer] of Object.entries(dados.resposta)) {
          var q_id = key.replace('r_', '') - 1;
          this.questions[q_id].answer = answer;
        }
      }
    }
    catch (ex) {
      console.error("Erro ao Buscar Valores", ex);
    }
  },
};
</script>

<style>
.el-radio__inner::after {
  width: 0.5em;
  height: 0.5em;
}
.el-radio__inner {
  width: 1.125em;
  height: 1.125em;
}
</style>
