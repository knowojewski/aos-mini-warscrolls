<template>
    <div class="box add-box">
        <div class="upper-side">
            <p>Add Unit Ability</p>
            <button @click.prevent="addAbilityForm" class="add-btn" id="addAbility">
                <span></span>
                <span></span>
            </button>
        </div>
        <div class="lower-side">
            <div v-for="ability in getAbilities" :key="ability.id" class="ability-add attribute">
                <select id="abilityType" v-model="ability.type">
                    <option value="Ability">Ability</option>
                    <option value="Command Ability">Command Ability</option>
                    <option value="Spell">Spell</option>
                </select>
                <input type="text" v-model="ability.name" placeholder="Name">
                <input type="text" v-model="ability.description" placeholder="Description">
                <button class="attributes-btn" @click="deleteForm({array: getAbilities, id: ability.id})">
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
export default class AbilityForm extends Vue {
    @Getter getAbilities;
    @Action deleteForm;

    addAbilityForm() {
        let idDate = new Date();
        let idNumber = idDate.valueOf();

        const abilityForm = {
            id: idNumber,
            type: "Ability",
            name: null,
            description: null  
        };

        this.getAbilities.push(abilityForm);
    }
}
</script>

<style lang="scss">

</style>