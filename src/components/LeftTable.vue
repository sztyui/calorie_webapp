
<script>
export default {
    props: {
        items: Array,
        bodyweight: String(),
        dailykcal: String(),
    },
    emits: ['update-body-data'],
    data(){
        return {
            inputName: "", inputProtein: "", inputCarb: "", inputFat: "", inputCalorie: "",
        }
    },
    computed: {
        weightLossKcal() {
            return this.dailykcal * 0.8
        },
        sum_calorie() {
            if(this.items.length == 0) {
                return 0
            }
            return this.items.reduce((a, b) => a + (parseFloat(b.calorie / 100) * b.quantity), 0);
        },
        calRemaining() {
            return (this.dailykcal * 0.8 - this.sum_calorie).toFixed(2);
        },
    },
    methods: {
        sumByProp(prop) {
            if(this.items.length === 0) return 0;
            return this.items.reduce((a, b) => a + (parseFloat(b[prop])/100 * b['quantity']), 0).toFixed(2);
        },
        propByBodyWeight(prop) {
            if(this.items.length === 0) {
                return 0
            }
            return (this.sumByProp(prop) / this.bodyweight).toFixed(2);
        },
        updateBodyWeight(event) {
            this.$emit('update:bodyweight', event.target.value);
        },
        updateCalorie(event){
            this.$emit('update:dailykcal', event.target.value);
        },
        updateBodyData(event){
            this.$emit('update-body-data', parseInt(event.target.value))
            document.getElementById('bodyWeightInput').focus()
        }
    }
}
</script>

<template>
    <div class="row">
        <div class="col-4"><p class="h3 p-2">Alapadatok</p></div>
        <div class="col-8 mt-3">
            <div class="form-check form-check-inline">
                <input class="form-check-input" type="radio" name="genderRadioButton" id="inlineRadio1" value="0" checked v-on:change="updateBodyData">
                <label class="form-check-label" for="inlineRadio1">NÅ ðââïž</label>
                </div>
            <div class="form-check form-check-inline">
                <input class="form-check-input" type="radio" name="genderRadioButton" id="inlineRadio2" value="1" v-on:change="updateBodyData">
                <label class="form-check-label" for="inlineRadio2">FÃ©rfi ðââïž</label>
                </div>
            <div class="form-check form-check-inline">
                <input class="form-check-input" type="radio" name="genderRadioButton" id="inlineRadio3" value="2" v-on:change="updateBodyData">
                <label class="form-check-label" for="inlineRadio3">LMBTQ+ ð</label>
            </div>
        </div>
    </div>
    
    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">TestsÃºly: </span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" id="bodyWeightInput" :value="bodyweight" @input="updateBodyWeight">
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">Napi ajÃ¡nlott kalÃ³ria bevitel: </span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="dailykcal" placeholder="2000 Kcal" @input="updateCalorie" id="dailyKcalInput">
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">FogyÃ¡shoz szÃŒksÃ©ges kalÃ³ria bevitel: </span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="weightLossKcal" readonly>
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">AjÃ¡nlott fehÃ©rje testsÃºly-kilÃ³nkÃ©nt:</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" value="1.5">
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">AjÃ¡nlott szÃ©nhidrÃ¡t kilÃ³nkÃ©nt:</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" value="2">
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">Ãsszes kalÃ³ria</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="sum_calorie.toFixed(2)" readonly>
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">FennmaradÃ³ kalÃ³ria</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="calRemaining" readonly>
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">Ãsszes fehÃ©rje (edzÃ©s elÅttâ)</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="sumByProp('protein')" readonly>
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">Ãsszes protein (edzÃ©s utÃ¡nâ)</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="sumByProp('protein')" readonly>
    </div>
    
    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">FehÃ©rje /tskg</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="propByBodyWeight('protein')" readonly>
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">Ãsszes szÃ©nhidrÃ¡t</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="sumByProp('carb')" readonly>
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">SzÃ©nhidrÃ¡t /tskg</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="propByBodyWeight('carb')" readonly>
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">Ãsszes zsÃ­r</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="sumByProp('fat')" readonly>
    </div>

    <div class="input-group input-group-sm mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">ZsÃ­r /tskg</span>
        </div>
        <input type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm" :value="propByBodyWeight('fat')" readonly>
    </div>
</template>

<style scoped>
#foods {
    font-family: Arial, Helvetica, sans-serif;
    border-collapse: collapse;
    width: 100%;
}

#foods td, #foods th {
    border: 1px solid #ddd;
    padding: 8px;
}

#foods tr:nth-child(even){background-color: #f2f2f2;}

#foods tr:hover {background-color: #ddd;}

#foods th {
    padding-top: 12px;
    padding-bottom: 12px;
    text-align: left;
    background-color: #04AA6D;
    color: white;
}

table {
    border-collapse: collapse;
    width: 90%;
}

th, td {
    text-align: left;
    padding: 8px;
}

td:nth-child(2) {
    text-align: right;
    padding: 8px;
}

tr:nth-child(even){background-color: #f2f2f2}

th {
    background-color: #04AA6D;
    color: white;
}

* {
    box-sizing: border-box;
}

.row {
    margin-left:-5px;
    margin-right:-5px;
}

.column {
    float: left;
    width: 50%;
    padding: 5px;
}
body {
    border: 30px solid white;
}
.header {
    font-weight: bold;
}
</style>