<template>
<div class="card">
    <div class="row">
        <h2 class="col-xs-12">{{planet.name}}</h2>
    </div>
    <hr>
    <div class="row">
        <div class="col-xs-12 col-sm-4">
            <span class="description">Climate: </span><span class="value"> {{planet.climate}}</span>
        </div>
        <div class="col-xs-12 col-sm-4">
            <span class="description">Diameter: </span><span class="value"> {{planet.diameter}}</span>
        </div>
        <div class="col-xs-12 col-sm-4">
            <span class="description">Gravity: </span><span class="value"> {{planet.gravity}}</span>
        </div>
    </div>
    <div class="row">
         <div class="col-xs-12 col-sm-4">
            <span class="description">Orbital Period: </span><span class="value"> {{planet.orbital_period}}</span>
        </div>
        <div class="col-xs-12 col-sm-4">
            <span class="description">Population: </span><span class="value"> {{planet.population}}</span>
        </div>
        <div class="col-xs-12 col-sm-4">
            <span class="description">Rotation Period: </span><span class="value"> {{planet.rotation_period}}</span>
        </div>
    </div>
    <div class="row">
         <div class="col-xs-12 col-sm-4">
            <span class="description">Surface Water: </span><span class="value"> {{planet.surface_water}}</span>
        </div>
        <div class="col-xs-12 col-sm-4">
            <span class="description">Terrain: </span><span class="value"> {{planet.terrain}}</span>
        </div>
        
    </div>
    <br/>
    <div class="row residents-dropdown">
        <div class="col-xs-12">
            <a class="description" v-if="!isOpen" v-on:click="setOpen();fetchResidents();">Residents <span class='toggle-sign'>+</span></a>
            <a class="description" v-if="isOpen" v-on:click="setOpen()">Residents <span class='toggle-sign'>-</span></a>

        </div>
    </div>
    <div class="row" v-if="isOpen&&isLoading">
        <div class="col-xs-12 resident-list">
            Loading...
        </div>
    </div>
    <div class="row" v-if="isOpen&&!isLoading">
        <div class="col-xs-12 resident-list" v-for="(res, index) in residents" :key="index">
            {{res}}
        </div>
    </div>
    <div class="row" v-if="isOpen&&!isLoading&&this.planet.residents.length===0">
        <div class="col-xs-12 resident-list">
            This planet has no residents.
        </div>
    </div>
</div>
</template>

<script>

export default {
  name: 'PlanetCard',
  components: {
  },
  props: {
    planet: Object
  },
  data() {
      return {
        isOpen: false,
        residents: [],
        isLoading: true,
      }
    },
    methods: {
        setOpen: function () {
            this.isOpen = !this.isOpen;
        },
        async fetchResidents () {
            if (this.residents.length === 0 && this.planet.residents.length>0) {
                let allResidents = [];
                await Promise.allSettled(
                    this.planet.residents.map(async (url) => {
                        const response = await fetch(url)
                        const todo = await response.json()
                        allResidents.push(todo.name)
                    })
                )
                this.residents = allResidents;
                this.isLoading = false;
            }
            else {
                this.isLoading = false;
            }
        }
        
    }
}
</script>

<style>
.card {
    border: solid 1px #9f45b0;
    border-radius: 5px;
    padding: 20px;
    background-color: black
}
hr {
    height: 2px;
    background-color: #9f45b0;
    border: none;
}
h2 {
    padding-left: 10px;
    color: #597999;
}
.description {
    color: #9f45b0;
    font-weight: 600;
}
.value, a {
    color: #597999;
    text-decoration: none;
}
.residents-dropdown {
    border-bottom: 1px solid #9f45b0;
}
a:hover {
    color: #FF3131;
}
.toggle-sign {
    float: right;
}
.resident-list {
    color: #FF3131;
    padding-left: 20px;
}
</style>
