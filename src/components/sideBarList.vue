<template>
<div class="sideBarList">
  <div class="ui visible left vertical sidebar menu">
    <a class="item" v-for="hockeyTeam in hockeyTeams.data.teams" :key="hockeyTeam.id"  @click="HockeyTeamSelected(hockeyTeam.id,hockeyTeam.name)">
      <img class="ui avatar image" :src="'https://www-league.nhlstatic.com/images/logos/teams-current-primary-light/'+ hockeyTeam.id + '.svg'" alt="">
      {{ hockeyTeam.name }}
    </a>
  </div>
  <div class="pusher">
    <div class="ui center aligned page grid">
      <img class="ui small circular image" :src="'https://www-league.nhlstatic.com/images/logos/teams-current-primary-light/'+ hockeyTeamId + '.svg'" alt="">
      <h1 class="ui header center aligned page grid">
        {{ hockeyTeamName }}
      </h1>
      <table class="ui selectable celled table column twelve wide" v-if="hockeyTeamSelectedList">
        <thead>
          <tr>
            <th class="collapsing">
              <a href="#" @click="sortbyJerseyNumber('jerseyNumber', ascDesc())">Jersey Number</a>
            </th>
            <th class="ten wide">
              <a href="#" @click="sortbyFullName('person.fullName', ascDesc())">Player Name</a>
            </th>
            <th class="six wide">Position</th>
          </tr>
        </thead>
        <tbody v-for="hockeyTeamSelected in doubles" :key="hockeyTeamSelected.id">
          <tr>          
            <td><div class="content">
              <h4 class="ui image header">{{ hockeyTeamSelected.jerseyNumber }} </h4>
            </div></td>
            <td>{{ hockeyTeamSelected.person.fullName }}</td>
            <td>{{ hockeyTeamSelected.position.type }}</td>
          </tr>
        </tbody>
      </table>
      <div class="backgroundNHL"  v-else>
        <img src="../assets/NHLWallpaper.jpeg" alt="">
      </div>
    </div>
  </div>
</div>
</template>

<script>
import _ from 'lodash';
import axios from "axios";

export default {
  mounted () {
    axios.get('https://statsapi.web.nhl.com/api/v1/teams')
      .then(response => (this.hockeyTeams = response))
  },
  data() {
    return {
      hockeyTeamId: null,
      hockeyTeamName: null,
      hockeyTeams: null,
      hockeyTeamsLogos: null,
      hockeyTeamSelectedList: null,
      asc : true,
      double: null
    }
  },
  methods: {
    HockeyTeamSelected(hockeyTeamId, hockeyTeamName) {
      axios.get('https://statsapi.web.nhl.com/api/v1/teams/'+hockeyTeamId+'/roster')
      .then(response => (this.hockeyTeamSelectedList = response))
      .then(() => {      
        this.hockeyTeamId = hockeyTeamId;
        this.hockeyTeamName = hockeyTeamName;
        this.doubles = this.hockeyTeamSelectedList.data.roster.map(function(hockeyTeamSelected) {
          return hockeyTeamSelected; 
        });
      })
    },
    sortbyJerseyNumber(prop,asc) {
      this.hockeyTeamSelectedList.data.roster = this.hockeyTeamSelectedList.data.roster.sort(function(a, b) {
        if (asc) {
          return (parseFloat(a[prop]) > parseFloat(b[prop])) ? 1 : ((parseFloat(a[prop]) < parseFloat(b[prop])) ? -1 : 0);
        } else {
          return (parseFloat(b[prop]) > parseFloat(a[prop])) ? 1 : ((parseFloat(b[prop]) < parseFloat(a[prop])) ? -1 : 0);
        }
      });
    },
    fullNameList() {
      doubles = this.hockeyTeamSelectedList.data.roster.map(function(hockeyTeamSelected) {
        return hockeyTeamSelected.person.fullName; 
      });
    },
    sortbyFullName(prop,asc) {
      doubles = this.hockeyTeamSelectedList.data.roster.map(function(hockeyTeamSelected) {
        return hockeyTeamSelected.person.fullName; 
      });
      doubles = doubles.sort();
    },
    ascDesc() {
      return this.asc = !this.asc;
    }
  }
}
</script>


<style>

.backgroundNHL img{
  padding-top: 100px; 
}

h1.ui.header {
  font-size: 50px;
}

.backgroundNHL {
  padding: 0;
  margin: 0;
  position: relative;
}

.pusher {
  padding-bottom: 5%;
  position: relative;
} 

.sideBarList {
  margin-top: 50px;
}

.ui.celled.table.column.twelve.wide {
  margin-top: 50px;
}

.ui.grid {
  align-items: center;
}

.ui.sidebar {
  top: 93px;
}

.ui.page.grid {
  padding-left: 0;
}

.ui.header.center.aligned.page.grid {
  padding-right: 10%;
}
</style>