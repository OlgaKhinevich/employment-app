<template>
  <div class="applications">
      <h1>Заявки</h1>
      <div class="applications-table">
        <table>
          <tr>
            <th>Должность</th>
            <th>Компания</th>
            <th>Статус</th>
            <th>Ответ работодателя</th>
          </tr>
          <tr v-for="(item, index) in applications" :key="index" :data-index="index">
            <td>{{item.position}}</td>
            <td>{{item.company_name}}</td>
            <td><div class="status">{{item.status}}</div></td>
            <td>{{item.employer_answer}}</td>    
          </tr>
        </table>
      </div>
  </div>
</template>

<script>
import AlertError from '../../../lib/alert_error';

export default {
  data() {
    return {
      applications: []
    }       
  },
  mounted() {
    this.get_applications();
  },
  methods: {
    async get_applications() {
       try {
        const response = await fetch(`http://localhost:3000/getapplications`);
        if(!response.ok) throw new AlertError(response.statusText);
        const data = await response.json();
        this.applications = data;
      } catch (err) {
        if(err.name === "AlertError") return alert(err.message);
        console.log(err);
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  @import '../../styles/mixins';
  @import '../../styles/variables';

  .applications {
    @include container;
    padding: 0 5%;
    h1 {
      font-family: 'Montserrat', sans-serif;
      margin: 2% 0 1% 0;
      font-weight: 700;    
    }
    .applications-table {
      table {
        border-collapse: collapse;
        width: 100%;
        th, td {
          text-align: left;
          padding: 8px;
          font-size: 14px;
        }
        th {
          background-color: #E4EDF8;
          color: #224C84;
          font-family: 'Montserrat', sans-serif;
        }
        td {
          font-family: 'Raleway', sans-serif;
          font-weight: 500;
          border-top: 0.9px solid  #577eb1;
          border-bottom: 0.9px solid #577eb1;
        }
        .status {
          background-color: #74A5EE;
          width: 100px;
          height: 22px;
          text-align: center;
          border-radius: 5px;
          color: #ffffff;
          font-weight: 600;
        }
      }
    }
  }
</style>