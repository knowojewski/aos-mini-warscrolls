<template>
  <div @click="openScrollPanel" class="warscroll" ref="warscrollRef">
    <div class="warscroll-front">
        <div class="warscroll-top">
          <div class="warscroll-top-name">
              <p>{{ miniscroll.name }}</p>
              <p v-show="miniscroll.mount">ON {{ miniscroll.mount }}</p>
          </div>
          <div class="warscroll-top-weapons">
              <p> {{ weaponsTopString(miniscroll.weapons) }}</p>
          </div>
        </div>
        <div class="warscroll-stats">
          <div class="stats-box">
              <i class="fas fa-external-link-alt"></i>
              <span>{{ miniscroll.move }}"</span>
          </div>
          <div class="stats-box">
              <i class="fas fa-shield-alt"></i>
              <span>{{ miniscroll.save }}+</span>
          </div>
          <div class="stats-box">
              <i class="fas fa-skull"></i>
              <span>{{ miniscroll.wounds }}</span>
          </div>
          <div class="stats-box">
              <i class="fas fa-star"></i>
              <span>{{ miniscroll.bravery }}</span>
          </div>
        </div>
        <div class="warscroll-weapons-abilities">
          <div class="warscroll-weapons">
            <div class="weapons-stats">
              <p></p>
              <p>Range</p>
              <p>Attack</p>
              <p>To Hit</p>
              <p>To Wound</p>
              <p>Rend</p>
              <p>Dmg</p>
            </div>
            <div class="weapon" v-for="weapon in miniscroll.weapons" :key="weapon.id" :class="{'ranged': weapon.type === 'Ranged'}">
              <p> {{ createShortcut(weapon.name) }}</p>
              <p>
                  <i v-if="weapon.type === 'Melee'" class="fas fa-gavel"></i>
                  <i v-if="weapon.type === 'Ranged'" class="fas fa-expand-arrows-alt"></i>
                  {{ weapon.range }}"
              </p>
              <p>{{ weapon.attack }}</p>
              <p>{{ weapon.hit }}+</p>
              <p>{{ weapon.wound }}+</p>
              <p>-{{ weapon.rend }}</p>
              <p>{{ weapon.damage }}</p>
            </div>
          </div>
          <div v-if="!miniscroll.frontAbilities" class="warscroll-abilities">
            <div class="ability" v-for="ability in miniscroll.abilities" :key="ability.id">
              <p class="ability-type">
                  {{ createShortcut(ability.type) }}
              </p>
              <p class="ability-name">
                  {{ ability.name }}:
              </p>
              <p class="ability-desc">
                  {{ ability.description }}
              </p>
            </div> 
          </div>
          <div v-else class="warscroll-abilities">
            <div class="ability" v-for="ability in miniscroll.frontAbilities" :key="ability.id">
              <p class="ability-type">
                  {{ createShortcut(ability.type) }}
              </p>
              <p class="ability-name">
                  {{ ability.name }}:
              </p>
              <p class="ability-desc">
                  {{ ability.description }}
              </p>
            </div> 
          </div>
        </div>
        <div class="warscroll-keywords">
          <p>{{ keywordsString(miniscroll.keywords) }}</p>
        </div>
        <p class="warscroll-id">{{ miniscroll.id }}</p>
    </div>
    <div class="warscroll-panel">
      <div v-if="component === 'scrollsContent'" class="btns">
          <div class="btn-box">
              <img src="../assets/button-border.png" alt="Button border">
              <button @click="deleteScroll(miniscroll.id)" class="btn">Delete Warscroll</button>
              <div class="btn-bg"></div>
          </div>
          <div class="btn-box">
              <img src="../assets/button-border.png" alt="Button border">
              <button @click="addScrollToPrint(miniscroll)" class="btn">Add to Print Sheet</button>
              <div class="btn-bg"></div>
          </div>
          <div class="btn-box">
              <img src="../assets/button-border.png" alt="Button border">
              <button @click="editScroll(miniscroll.id)" class="btn">Edit Warscroll</button>
              <div class="btn-bg"></div>
          </div>
          <div class="arrow-btn-box">
            <button @click="changePosition({ array: getMiniscrolls, id: miniscroll.id, value: -1 })" class="btn-arrow btn-left" title="Move left">
              <img src="../assets/arrow-up.png" alt="Arrow left">
            </button> 
            <button @click="changePosition({ array: getMiniscrolls, id: miniscroll.id, value: 1 })" class="btn-arrow btn-right" title="Move right">
              <img src="../assets/arrow-up.png" alt="Arrow left">
            </button> 
          </div>
      </div> 
      <div v-else class="btns">
          <div class="btn-box">
              <img src="../assets/button-border.png" alt="Button border">
              <button class="btn">Delete from Print</button>
              <div class="btn-bg"></div>
          </div>
          <div class="arrow-btn-box">
            <button class="btn-arrow btn-left" title="Move left">
              <img src="../assets/arrow-up.png" alt="Arrow left">
            </button> 
            <button class="btn-arrow btn-right" title="Move right">
              <img src="../assets/arrow-up.png" alt="Arrow left">
            </button> 
          </div>
      </div> 
    </div>
    <div class="warscroll-back" v-show="backCard">
      <div class="warscroll-top">
        <div class="warscroll-top-name">
            <p>{{ miniscroll.name }}</p>
            <p v-show="miniscroll.mount">ON {{ miniscroll.mount }}</p>
        </div>
        <div class="warscroll-top-weapons">
            <p> {{ weaponsTopString(miniscroll.weapons) }}</p>
        </div>
      </div>
      <div class="warscroll-abilities">
        <div class="ability" v-for="ability in miniscroll.backAbilities" :key="ability.id">
            <p class="ability-type">
                {{ createShortcut(ability.type) }}
            </p>
            <p class="ability-name">
                {{ ability.name }}:
            </p>
            <p class="ability-desc">
                {{ ability.description }}
            </p>
          </div>  
      </div>
      <div class="warscroll-keywords">
        <p>{{ keywordsString(miniscroll.keywords) }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { Vue, Component, Prop } from 'vue-property-decorator';
import { Action, Getter, Mutation } from 'vuex-class';

@Component
export default class WarscrollComponent extends Vue {
    @Prop(Object) miniscroll;
    @Prop(String) component;

    @Getter getScrollsToPrint;
    @Getter getMiniscrolls;
    @Getter getScrollsToPrint;
    @Action deleteScroll;
    @Action changePosition;
    @Action editScroll;
    @Mutation addScrollToPrint;

    currentScroll = document.createElement('div');
    backCard = false;

    weaponsTopString(weapons) {
        let weaponsArray = [];
        let weaponsString = '';

        weapons.forEach(weapon => {
        weaponsArray.push(weapon.name);
        });

        weaponsString = weaponsArray.join(' / ');

        return weaponsString;
    }

    keywordsString(keywords) {
        let keywordsArray = [];
        let keywordString = '';

        keywords.forEach(keyword => {
        keywordsArray.push(keyword.keyword);
        });

        keywordString = keywordsArray.join(', ')

        return keywordString;
    }

    createShortcut(value) {
        let shortcut = value.split(' ');

        shortcut = shortcut.map(element => {
        return element.split('').splice(0, 1).join('');
        }).join('');

        return shortcut;
    }

    openScrollPanel(event) {
      this.currentScroll = event.target.closest('.warscroll').children[1];
      this.currentScroll.classList.add('turn-on');
    }

    closeScrollPanel(event) {
      if(event.target !== this.currentScroll) {
        this.currentScroll.classList.remove('turn-on');
      }
    }

    splitWarscroll() {
      const front = this.$refs.warscrollRef.firstChild;
      const frontElements = Array.from(front.children);
      let result = 0;

      frontElements.forEach( item => result += item.offsetHeight );

      if(result > 232) {
        this.backCard = true;
        this.miniscroll.frontAbilities = [...this.miniscroll.abilities];
        this.miniscroll.backAbilities = [];
        let frontAbilities = this.miniscroll.frontAbilities;
        let backAbilities = this.miniscroll.backAbilities;

        let newResult = result;
        let abilitiesOnFront = front.children[2].children[1].children;

        for(let i = abilitiesOnFront.length-1; i >= 0; i--) {
          if(newResult > 232) {
            newResult -= abilitiesOnFront[i].offsetHeight;
            backAbilities.unshift(...frontAbilities.splice(i, 1));
          } else {
            break;
          }
        }
      }
    }

    mounted() {
      window.addEventListener('mouseup', this.closeScrollPanel);
      this.miniscroll.backAbilities ? this.backCard = true : this.splitWarscroll();
    }
}
</script>

<style lang="scss" scoped>
.warscroll {
  font-family: sans-serif;
  display: flex;
  background: #fff;
  border: 1px solid #000000;
  box-shadow: none;
  transition: transform .3s, border .3s, box-shadow .3s;
  margin-right: 2px;
  margin-bottom: 2px;
  position: relative;
  overflow: hidden;
}
    
.warscroll-back { border-left: 1px solid #000000; }

.warscroll-id { display: none; }

.warscroll-panel {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  width: 100%;
  position: absolute;
  background: rgba(0, 0, 0, 0.801);
  transform: translateX(100%);
  transition: transform .3s;

  .btns {
    position: relative;
    bottom: auto;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .btn-box { margin-bottom: 10px; }

    .btn-box:last-child { margin-bottom: 0; }
  }

  .arrow-btn-box {
    width: 90%;
    display: flex;
    justify-content: space-between;

    .btn-arrow {
      border: 3px solid #000;
      background: #fff;
      transition: background .3s;
      cursor: pointer;
    }

    .btn-arrow:hover { background: rgb(204, 204, 204); }

    .btn-left { transform: rotateZ(-90deg); }
    .btn-right { transform: rotateZ(90deg); }
  }
}

.warscroll-panel.turn-on { transform: translateX(0); }

.warscroll-front, .warscroll-back {
  height: 240px;
  width: 340px;
  padding: 4px 8px;
  display: flex;
  flex-direction: column;
  cursor: pointer;

  .warscroll-top {
    border-bottom: 1px solid #000000;
    width: 100%;
    display: flex ;
    justify-content: space-between;

    .warscroll-top-name, .warscroll-top-weapons {
      display: flex;
      flex-direction: column;
      justify-content: flex-end;
      width: 48%;
    }
    
    .warscroll-top-name {
      p:first-child {
        text-transform: uppercase;
        font-weight: bold;
        font-size: 12px;
      }

      p:last-child {
        text-transform: uppercase;
        font-size: 10px;
      }
    }

    .warscroll-top-weapons {
      text-align: right;
      font-size: 9px;
    }
  }

  .warscroll-stats {
    width: 100%;
    display: flex;
    justify-content: space-evenly;
    border-bottom: 1px solid #000000;
    padding: 3px 0;

    .stats-box {
      display: flex;
      align-items: center;
        
      i {
        font-size: 23px;
        margin-right: 4px;
      }

      span { font-size: 20px; }
    }

  }
  
  .warscroll-weapons-abilities {
    width: 100%;
    border-bottom: 1px solid #000000;
    flex: 1;

    .warscroll-weapons {
      width: 95%;
      display: flex;
      flex-direction: column;
      margin: 5px auto;

      .weapons-stats, .weapon {
        width: 100%;
        display: flex;

        p {
          font-size: 11px;
          border: 1px solid #000;
          border-right: none;
          width: 14%;
          height: 16px;
          display: flex;
          justify-content: center;
          align-items: center;
        }

        p:nth-child(1) {
          width: 8%;
          padding-right: 1px;
          border: none;
        }

        p:nth-child(2) { width: 17%; }
            
        p:nth-child(5) { width: 22%; }

        p:nth-child(7) {
          border-right: 1px solid #000000;
          width: 11%;
        }
      }

      .weapon {
        // margin-top: 2px;
        order: 2;

        p {
          height: 18px;
          font-size: 14px;
          font-weight: bold;
          border-top: none;

          i { margin-right: 5px; } 
        }

        p:nth-child(1) {
          font-size: 12px;
          font-weight: normal;
          justify-content: flex-end;
        }
      }

      .weapon.ranged {
        order: 1;
      }
    }

  }

  .warscroll-abilities {
    width: 100%;

    .ability {
      width: 100%;
      font-size: 11px;

      .ability-type {
        float: left;
        font-style: normal;
        font-weight: bold;
        margin-right: 3px;
      }

      .ability-name {
        float: left;
        font-style: italic;
        margin-right: 3px;
      }

      .ability-desc {
        font-weight: bold;
      }
    }
  }

  .warscroll-keywords { font-size: 10px; }
}

.warscroll-back {
  .warscroll-abilities {
    flex: 1;
    padding: 5px 0;
  }

  .warscroll-keywords {
    border-top: 1px solid #000000;
    font-size: 10px;
  }
}

.warscroll:hover {
  transform: translateY(-3px);
  border: 1px solid #333;
  box-shadow: 0 0 10px 1px #333;
}
</style>