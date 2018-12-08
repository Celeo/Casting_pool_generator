<template lang="pug">
div#app
  nav.navbar.navbar-expand-lg.navbar-dark.bg-dark.shadow-sm
    span.navbar-brand
      a(href="" style="color: white;") Casting pool generator
  main.py-md-4.pl-md-5.pr-md-5(role="main")
    div.container
      div.row
        div.col-8
          h2 Entry
          //- Basics
          div.form-group
            label(for="spellHighestArcanum") Spell highest arcanum
            input#spellHighestArcanum.form-control(type="number" v-model.number="spellHighestArcanum" min="1" max="5")
          div.form-group
            label(for="casterCorrespondingArcanum") Caster corresponding arcanum
            input#casterCorrespondingArcanum.form-control(type="number" v-model.number="casterCorrespondingArcanum" min="1" max="5")
          div.form-group
            label(for="gnosis") Caster Gnosis
            input#gnosis.form-control(type="number" v-model.number="gnosis" min="1" max="8")
          div.row
            div.col
              //- What kind of spell is this for the caster
              h5 Arcana type
              div.form-check
                input.form-check-input(type="radio" id="arcanaTypeRuling" v-model="arcanaType" value="Ruling")
                label.form-check-label(for="arcanaTypeRuling") Ruling
              div.form-check
                input.form-check-input(type="radio" id="arcanaTypeCommon" v-model="arcanaType" value="Common")
                label.form-check-label(for="arcanaTypeCommon") Common
              div.form-check
                input.form-check-input(type="radio" id="arcanaTypeInferior" v-model="arcanaType" value="Inferior")
                label.form-check-label(for="arcanaTypeInferior") Inferior
            div.col.left-border
              //- Does the caster really know this spell
              h5 Special spells
              div.form-check
                input.form-check-input(type="radio" id="castingMethodRoteGrimoire" v-model="castingMethod" value="Rote from grimoire")
                label.form-check-label(for="castingMethodRoteGrimoire") Rote from grimoire
              div.form-check
                input.form-check-input(type="radio" id="castingMethodRoteSelfDesign" v-model="castingMethod" value="Rote self-designed")
                label.form-check-label(for="castingMethodRoteSelfDesign") Rote self-designed
              div.form-check
                input.form-check-input(type="radio" id="castingMethodRoteMemory" v-model="castingMethod" value="Rote from memory")
                label.form-check-label(for="castingMethodRoteMemory") Rote from memory
              div.form-check
                input.form-check-input(type="radio" id="castingMethodPraxis" v-model="castingMethod" value="Praxis")
                label.form-check-label(for="castingMethodPraxis") Praxis
              div.form-check
                input.form-check-input(type="radio" id="castingMethodNormal" v-model="castingMethod" value="Normal")
                label.form-check-label(for="castingMethodNormal") Normal
          //- Reach
          div.form-group
            label(for="effectReaches") Reaches for spell effect changes
            input#effectReaches.form-control(type="number" v-model.number="effectReaches" min="0")
          //- Spell factors
          //- Yantras
          //- Extensions
        div.col.left-border
          h2 Calculations
          p Free reaches:
            span  {{ freeReaches }}
          p
            strong Dice pool:
              span  {{ dicePool }}
          p
            strong Paradox pool:
              span  {{ paradoxTotal }}
          div(v-show="additionalMessages.length > 0")
            h4 Additional notes
            ul.pl-1
              li(v-for="message in additionalMessages") {{ message }}
</template>

<script>
export default {
  data () {
    return {
      gnosis: 1,
      spellHighestArcanum: 1,
      casterCorrespondingArcanum: 1,
      arcanaType: 'Ruling',
      castingMethod: 'Normal',
      effectReaches: 0,
      modifyingReaches: 0,
      yantras: [],
      manaSpent: 0
    }
  },
  computed: {
    freeReaches () {
      let val = this.casterCorrespondingArcanum - this.spellHighestArcanum + 1
      if (this.castingMethod === 'Rote from grimoire') {
        // ...
      }
      return val
    },
    dicePool () {
      let val = 0
      val += this.gnosis
      val += this.casterCorrespondingArcanum
      // yantras
      return val
    },
    paradoxTotal () {
      let val = 0
      val += this.effectReaches + this.modifyingReaches - this.freeReaches
      // ...
      return val >= 0 ? val : 0
    },
    additionalMessages () {
      let messages = []
      if (this.castingMethod === 'Rote from grimoire') {
        messages.push('Cannot use reach to cast the spell')
        messages.push('Ritual casting time is doubled')
        messages.push('Dice pool gets the rote quality')
      }
      if (this.castingMethod === 'Rote self-designed') {
        messages.push('Can use dots in the associated skill as a Yantra')
        messages.push('Dice pool gets the rote quality')
      }
      if (this.castingMethod === 'Rote from memory') {
        messages.push('Can use dots in the associated skill as a Yantra')
      }
      if (this.castingMethod === 'Praxis') {
        messages.push('3 successes means an exceptional success')
      }
      return messages
    }
  },
  watch: {
    // do not allow the caster's corresponding arcanum be lower than the highest arcanum of the spell
    spellHighestArcanum (newVal, oldVal) {
      if (newVal > this.casterCorrespondingArcanum) {
        this.spellHighestArcanum = oldVal
      }
    },
    casterCorrespondingArcanum (newVal, oldVal) {
      if (newVal < this.spellHighestArcanum) {
        this.spellHighestArcanum = newVal
      }
    }
  }
}
</script>

<style lang="stylus">
.left-border
  border-left: 1px solid grey
</style>
