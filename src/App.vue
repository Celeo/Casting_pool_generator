<template lang="pug">
div#app
  nav.navbar.navbar-expand-lg.navbar-dark.bg-dark.shadow-sm
    span.navbar-brand
      a(href="" style="color: white;") Casting pool generator
    button.navbar-toggler(type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation")
      span.navbar-toggler-icon
    div#navbarSupportedContent.collapse.navbar-collapse
      ul.navbar-nav.mr-auto
      ul.navbar-nav
        li.nav-item
          a.nav-link(href="") Home
  main.py-md-4.pl-md-5.pr-md-5(role="main")
    div.container
      div.row
        div.col-9
          h2 Entry
          div.form-group
            label(for="spellHighestArcanum") Spell highest arcanum
            input#spellHighestArcanum.form-control(type="number" v-model="spellHighestArcanum" min="1" max="5")
          div.form-group
            label(for="casterCorrespondingArcanum") Caster corresponding arcanum
            input#casterCorrespondingArcanum.form-control(type="number" v-model="casterCorrespondingArcanum" min="1" max="5")
          div.form-group
            label(for="gnosis") Caster Gnosis
            input#gnosis.form-control(type="number" v-model="gnosis" min="1" max="8")
          div.row
            div.col
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
              h5 Special spells
              div.form-check
                input.form-check-input(type="radio" id="castingMethodRote" v-model="castingMethod" value="Rote")
                label.form-check-label(for="castingMethodRote") Rote
              div.form-check
                input.form-check-input(type="radio" id="castingMethodPraxis" v-model="castingMethod" value="Praxis")
                label.form-check-label(for="castingMethodPraxis") Praxis
              div.form-check
                input.form-check-input(type="radio" id="castingMethodNormal" v-model="castingMethod" value="Normal")
                label.form-check-label(for="castingMethodNormal") Normal
          //- Spending reach
          //- Spell factors
          //- Yantras
          //- Extensions
        div.col-3.left-border
          h2 Calculations
          p Free reaches:
            span  {{ freeReaches }}
          p
            strong Dice pool:
              span  {{ dicePool }}
          p
            strong Paradox pool:
              span  {{ paradoxTotal }}
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
      reaches: [],
      factors: [],
      yantras: [],
      manaSpent: 0,
      paradox: []
    }
  },
  computed: {
    freeReaches () {
      return this.casterCorrespondingArcanum - this.spellHighestArcanum + 1
    },
    dicePool () {
      let val = 0
      val += this.gnosis
      val += this.casterCorrespondingArcanum
      // yantras
      return val
    },
    paradoxTotal () {
      // ...
      return 0
    }
  },
  watch: {
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
