<template>
  <div class="box add-box">
    <div class="upper-side">
        <p>Add Unit Weapon</p>
        <button @click.prevent="addWeaponForm" class="add-btn" id="addWeapon">
            <span></span>
            <span></span>
        </button>
    </div>
    <div class="lower-side">
        <div v-for="weapon in getWeapons" :key="weapon.id" class="weapon-add attribute">
            <input type="text" v-model="weapon.name" placeholder="Name">
            <select id="abilityType" v-model="weapon.type">
                <option value="Melee">Melee</option>
                <option value="Ranged">Ranged</option>
            </select>
            <input type="text" v-model="weapon.range" placeholder="Range">
            <input type="text" v-model="weapon.attack" placeholder="Attack">
            <input type="text" v-model="weapon.hit" placeholder="To Hit">
            <input type="text" v-model="weapon.wound" placeholder="To Wound">
            <input type="text" v-model="weapon.rend" placeholder="Rend">
            <input type="text" v-model="weapon.damage" placeholder="Damage">
            <button class="attributes-btn" @click="deleteForm({array: getWeapons, id: weapon.id})">
                <i class="far fa-trash-alt"></i>
            </button>
        </div>
    </div>
</div>
</template>

<script>
import { Vue, Component } from 'vue-property-decorator';
import { Action, Getter } from 'vuex-class';

@Component
export default class WeaponForm extends Vue {
    @Getter getWeapons;
    @Action deleteForm;

    addWeaponForm() {
        let idDate = new Date();
        let idNumber = idDate.valueOf();

        const weapon = {
            id: idNumber,
            name: null,
            type: "Melee",
            range: null,
            attack: null,
            hit: null,
            wound: null,
            rend: null,
            damage: null    
        };

        this.getWeapons.push(weapon);
    }
}
</script>

<style lang="scss">

</style>