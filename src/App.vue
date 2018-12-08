<template lang="pug">
div#app
  nav.navbar.navbar-expand-lg.navbar-dark.bg-dark.shadow-sm
    span.navbar-brand
      a(href="" style="color: white;") Casting pool generator
  main.py-md-4.pl-md-5.pr-md-5(role="main")
    div.container
      div.row
        div.col-9
          //- === LEFT ===
          h2 Entry
          br
          //- Basics
          div.form-group
            label(for="spellHighestArcanum") Spell highest arcanum
            input#spellHighestArcanum.form-control(type="number" v-model.number="spellHighestArcanum" min="1" max="5")
          div.row
            div.col
              div.form-group
                label(for="casterCorrespondingArcanum") Caster corresponding arcanum
                input#casterCorrespondingArcanum.form-control(type="number" v-model.number="casterCorrespondingArcanum" min="1" max="5")
            div.col
              div.form-group
                label(for="gnosis") Caster Gnosis
                input#gnosis.form-control(type="number" v-model.number="gnosis" min="1" max="8")
          hr
          div.row
            div.col
              //- The kind of spell for the caster
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
              //- If the caster really knows this spell
              h5 Special spells
              div.form-check
                input.form-check-input(type="radio" id="castingMethodRoteGrimoire" v-model="castingMethod" value="Rote from grimoire")
                label.form-check-label(for="castingMethodRoteGrimoire") Rote from grimoire
              div.form-check
                input.form-check-input(type="radio" id="castingMethodRoteSelfDesign" v-model="castingMethod" value="Rote self-designed")
                label.form-check-label(for="castingMethodRoteSelfDesign") Rote that's self-designed
              div.form-check
                input.form-check-input(type="radio" id="castingMethodRoteMemory" v-model="castingMethod" value="Rote from memory")
                label.form-check-label(for="castingMethodRoteMemory") Rote from memory
              div.form-check
                input.form-check-input(type="radio" id="castingMethodPraxis" v-model="castingMethod" value="Praxis")
                label.form-check-label(for="castingMethodPraxis") Praxis
              div.form-check
                input.form-check-input(type="radio" id="castingMethodNormal" v-model="castingMethod" value="Normal")
                label.form-check-label(for="castingMethodNormal") Normal
          hr
          h5 Reach
          div.form-group
            //- This bit has to be entered by the user as total, because I'm not going to enter all the spells into a JSON file and load it up
            label(for="effectReaches") Reaches for spell effect changes (these are listed in the spell description)
            input#effectReaches.form-control(type="number" v-model.number="effectReaches" min="0")
          //- For the reach factors, I can list those out and allow the user to select from them
          div.row
            div.col
              div.form-check
                input.form-check-input(type="radio" id="reachCastingTimeRitual" v-model="reachCastingTime" value="Ritual")
                label.form-check-label(for="reachCastingTimeRitual") Ritual cast
              div.form-check
                input.form-check-input(type="radio" id="reachCastingTimeInstant" v-model="reachCastingTime" value="Instant" :disabled="castingMethod === 'Rote from grimoire'")
                label.form-check-label(for="reachCastingTimeInstant") Instant cast (1 reach)
            div.col.left-border
              div.form-check
                input.form-check-input(type="radio" id="reachRangeTouch" v-model="reachRange" value="Touch")
                label.form-check-label(for="reachRangeTouch") Touch range
              div.form-check
                input.form-check-input(type="radio" id="reachRangeInstant" v-model="reachRange" value="Sensory")
                label.form-check-label(for="reachRangeInstant") Sensory range (1 reach)
          //- Potency here?
          br
          div.form-group
            label(for="reachDurationSelection") Duration
            select#reachDurationSelection.form-control(v-model="reachDuration")
              option(value="0,0") 1 turn
              option(value="0,-2") 2 turns (-2 DP)
              option(value="0,-4") 3 turns (-4 DP)
              option(value="0,-6") 5 turns (-6 DP)
              option(value="0,-8") 10 turns (-8 DP)
              option(value="1,0") 1 scene or 1 hour (1 reach)
              option(value="1,-2") 1 day (1 reach and -2 DP)
              option(value="1,-4") 1 week (1 reach and -4 DP)
              option(value="1,-6") 1 month (1 reach and -6 DP)
              option(value="1,-8") 1 year (1 reach and -8 DP)
              option(value="1,-10") Indefinite (1 reach and -10 DP and 1 mana)
          div.form-group
            label(for="reachAOE") Scale
            select#reachAOE.form-control(v-model="reachAOE")
              option(value="0,0") 1 subject, largest subject size 5, arm's reach from a point
              option(value="0,-2") 2 subjects, largest 6, small room (-2 DP)
              option(value="0,-4") 4 subjects, largest 7, large room (-4 DP)
              option(value="0,-6") 8 subjects, largest 8, several rooms or house floor (-6 DP)
              option(value="0,-8") 16 subjects, largest 9, ballroom or small house (-8 DP)
              option(value="1,0") 5 subjects, largest 5, large building (1 reach)
              option(value="1,-2") 10 subjects, largest 10, small warehouse or parking lot (1 reach and -2 DP)
              option(value="1,-4") 20 subjects, largest 15, large warehouse or supermarked (1 reach and -4 DP)
              option(value="1,-6") 40 subjects, largest 20, small factory, or shopping mall (1 reach and -6 DP)
              option(value="1,-8") 80 subjects, largest 25, large factory, or city block (1 reach -8 DP)
              option(value="1,-10") 160 subjects, largest 30, campus or small neighborhood (1 reach and -10 DP)
          div.form-group.mb-0
            label(for="reachPotency") Potency
            input#reachPotency.form-control(type="number" v-model.number="reachPotency" min="1")
          div.form-check
            input#reachPotencyAdvanced.form-check-input(type="checkbox" v-model="reachPotencyAdvanced")
            label(for="reachPotencyAdvanced") +2 to withstand dispellation (1 reach)
          hr
          h5 Yantras
          div.row
            div.col
              div.form-check(v-for="yantra in yantras.slice(0, 7)")
                input.form-check-input(:id="'yantra' + yantra.name" type="checkbox" v-model="yantra.checked")
                label(:for="'yantra' + yantra.name") {{ yantra.name }}: {{ yantra.dp }}
            div.col
              div.form-check(v-for="yantra in yantras.slice(7)")
                input.form-check-input(:id="'yantra' + yantra.name" type="checkbox" v-model="yantra.checked")
                label(:for="'yantra' + yantra.name") {{ yantra.name }}: {{ yantra.dp }}
          br
          div.form-group(v-show="reachCastingTime === 'Ritual'")
            label(for="yantraRitualTime") Extend ritual interval increase (+1 DP per)
            input#yantraRitualTime.form-control(type="number" v-model.number="yantraRitualTime" min="0" max="5")
          //- TODO mana to reduce paradox dice
        div.col.left-border
          //- == RIGHT ===
          h2 Calculations
          br
          p Free reach:
            span  {{ freeReaches }}
          p Mana cost:
            span  {{ manaCost }}
          p
            strong Dice pool:
              span  {{ dicePool }}
          p
            strong Paradox pool:
              span  {{ paradoxTotal }}
          //- total potency?
          //- total casting time?
          //- mana cost
          div(v-show="additionalMessages.length > 0")
            h4 Additional notes
            ul.pl-3
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
      reachCastingTime: 'Ritual',
      reachRange: 'Touch',
      reachDuration: '0,0',
      reachAOE: '0,0',
      reachPotency: 1,
      reachPotencyAdvanced: false,
      modifyingReaches: 0,
      yantras: [
        { name: 'Demesne', dp: '+2', checked: false },
        { name: 'Environment', dp: '+1', checked: false },
        { name: 'Supernal Verge', dp: '+2', checked: false },
        { name: 'Concentration', dp: '+2', checked: false },
        { name: 'Murda', dp: '+skill dots in encoded skill', checked: false },
        { name: 'Mantra', dp: '+2', checked: false },
        { name: 'Runes', dp: '+2', checked: false },
        { name: 'Path Tool', dp: '+1', checked: false },
        { name: 'Order Tool', dp: '+1', checked: false },
        { name: 'Patrol Tool', dp: 'Special', checked: false },
        { name: 'Sympathy', dp: '+0 to +2', checked: false },
        { name: 'Sacrament', dp: '+1 to +2', checked: false },
        { name: 'Persona', dp: 'Special', checked: false }
      ],
      yantraRitualTime: 0
    }
  },
  computed: {
    freeReaches () {
      let val = 0
      if (this.castingMethod === 'Rote ...') {
        // ...
      } else {
        // this.casterCorrespondingArcanum - this.spellHighestArcanum + 1
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
      // val += this.effectReaches + this.modifyingReaches - this.freeReaches
      // ...
      return val
    },
    manaCost () {
      let val = 0
      // ...
      if (this.arcanaType !== 'Ruling') {
        val += 1
      }
      return val
    },
    additionalMessages () {
      let messages = []
      if (this.castingMethod === 'Rote from grimoire') {
        messages.push('Cannot use reach to cast the spell')
        messages.push('Cannot cast the spell instantly')
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
      // time it takes to cast (casting time * gnosis + grimoire + yantra count)
      return messages
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
    },
    castingMethod (newVal, oldVal) {
      if (newVal === 'Rote from grimoire') {
        this.reachCastingTime = 'Ritual'
      }
    },
    reachCastingTime (newVal, oldVal) {
      if (newVal === 'Instant') {
        this.yantraRitualTime = 0
      }
    }
  }
}

/*
  Dev notes
    - What's the bit about free 'primary spell factor' increase in the book?
*/
</script>

<style lang="stylus">
.left-border
  border-left: 1px solid grey
</style>
