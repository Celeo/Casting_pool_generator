<template lang="pug">
div#app
  nav.navbar.navbar-expand-lg.navbar-dark.bg-dark.shadow-sm
    span.navbar-brand
      a(href="" style="color: white;") Casting pool generator
  main.py-md-4.pl-md-5.pr-md-5(role="main")
    div.container-fluid
      div.row
        div.col-lg-9.col-md-12
          //- === LEFT ===
          h2.pt-2 Entry
          br
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
                input#gnosis.form-control(type="number" v-model.number="gnosis" min="1" max="10")
          hr
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
              h5 Special knowledge
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
            label(for="effectReaches") Reaches for spell effect changes (these are listed in the spell description)
            input#effectReaches.form-control(type="number" v-model.number="reach.effect" min="0")
          div.row
            div.col
              div.form-check
                input.form-check-input(type="radio" id="reachCastingTimeRitual" v-model="reach.castingTime" value="Ritual")
                label.form-check-label(for="reachCastingTimeRitual") Ritual cast
              div.form-check
                input.form-check-input(type="radio" id="reachCastingTimeInstant" v-model="reach.castingTime" value="Instant" :disabled="castingMethod === 'Rote from grimoire'")
                label.form-check-label(for="reachCastingTimeInstant") Instant cast (1 reach)
            div.col.left-border
              div.form-check
                input.form-check-input(type="radio" id="reachRangeTouch" v-model="reach.range" value="Touch")
                label.form-check-label(for="reachRangeTouch") Touch range
              div.form-check
                input.form-check-input(type="radio" id="reachRangeInstant" v-model="reach.range" value="Sensory")
                label.form-check-label(for="reachRangeInstant") Sensory range (1 reach)
          br
          div.form-group
            label(for="reachDurationSelection") Duration
            select#reachDurationSelection.form-control(v-model="reach.duration")
              //- value is [mana cost], [reach cost], [dp modifier]
              option(value="0,0,0") 1 turn
              option(value="0,0,-2") 2 turns (-2 DP)
              option(value="0,0,-4") 3 turns (-4 DP)
              option(value="0,0,-6") 5 turns (-6 DP)
              option(value="0,0,-8") 10 turns (-8 DP)
              option(value="0,1,0") 1 scene or 1 hour (1 reach)
              option(value="0,1,-2") 1 day (1 reach and -2 DP)
              option(value="0,1,-4") 1 week (1 reach and -4 DP)
              option(value="0,1,-6") 1 month (1 reach and -6 DP)
              option(value="0,1,-8") 1 year (1 reach and -8 DP)
              option(value="1,1,-10") Indefinite (1 reach and -10 DP and 1 mana)
          div.form-group
            label(for="reachAOE") Scale
            select#reachAOE.form-control(v-model="reach.aoe")
              //- value is [reach cost], [dp modifier]
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
            input#reachPotency.form-control(type="number" v-model.number="reach.potency" min="1")
          div.form-check
            input#reachPotencyAdvanced.form-check-input(type="checkbox" v-model="reach.potencyAdvanced")
            label(for="reachPotencyAdvanced") +2 DP to withstand dispellation (1 reach)
          hr
          h5 Yantras
          div.row
            div.col
              div.form-check(v-for="yantra in yantras.slice(0, 7)")
                input.form-check-input(:id="'yantra' + yantra.name" type="checkbox" v-model="yantra.checked" :disabled="yantra.name === 'Murda' && castingMethod.indexOf('Rote') === -1")
                label(:for="'yantra' + yantra.name") {{ yantra.name }}: {{ yantra.dp }} DP
            div.col
              div.form-check(v-for="yantra in yantras.slice(7)")
                input.form-check-input(:id="'yantra' + yantra.name" type="checkbox" v-model="yantra.checked" :disabled="yantra.name === 'Murda' && castingMethod.indexOf('Rote') === -1")
                label(:for="'yantra' + yantra.name") {{ yantra.name }}: {{ yantra.dp }} DP
          p.text-danger.mb-1(v-show="overYantraLimitMessage")
            strong You are using more Yantras than your Gnosis allows (using {{ yantasUsed }}, allowed {{ yantrasAllowed }})
          br
          div.form-group
            label(for="yantraRitualTime") Extend ritual interval increase (+1 DP per)
            input#yantraRitualTime.form-control(type="number" v-model.number="yantraRitualTime" min="0" max="5" :disabled="reach.castingTime === 'Instant'")
          hr
          h5 Paradox modifiers
          div.form-check(v-for="mod in paradoxModifiers")
            input.form-check-input(:id="'mod' + mod.name" type="checkbox" v-model="mod.checked")
            label(:for="'mod' + mod.name") {{ mod.name }}: {{ mod.dp }} DP
          div.form-group
            label(for="paradoxSameScene") Number of times you've caused a paradox roll this scene
            input#paradoxSameScene.form-control(type="number" v-model.number="paradoxSameScene" min="0")
          div.form-group
            label(for="manaReduceParadox") Mana to reduce paradox DP
            input#manaReduceParadox.form-control(type="number" v-model.number="manaReduceParadox" min="0")
        div.col.left-border-lg
          hr.d-lg-none.d-xl-none
          //- == RIGHT ===
          h2 Calculations
          br
          p Free reaches:
            span  {{ freeReaches }}
          p Reaches used:
            span  {{ reachesUsed }}
          hr
          p Total mana cost:
            span  {{ manaCost }}
          p Casting Time:
            span  {{ castingTime }}
          p Total potency:
            span  {{ totalPotency }}
          p Primary spell factor boost:
            span  {{ primarySpellFactorBoost }}
          hr
          p
            strong Dice pool:
              span  {{ dicePool }}
          p
            strong Paradox pool:
              span  {{ paradoxTotal }}
          div(v-show="additionalMessages.length > 0")
            h4 Additional notes
            ul.pl-3
              li(v-for="message in additionalMessages") {{ message }}
</template>

<script>
const paradoxFromGnosis = {
  1: 1,
  2: 1,
  3: 2,
  4: 2,
  5: 2,
  6: 3,
  7: 3,
  8: 3,
  9: 4,
  10: 4
}

// in minutes
const ritualTimeFromGnosis = {
  1: 180,
  2: 180,
  3: 60,
  4: 60,
  5: 30,
  6: 30,
  7: 10,
  8: 10,
  9: 10,
  10: 10
}

const yantraLimitFromGnosis = {
  1: 2,
  2: 2,
  3: 3,
  4: 3,
  5: 4,
  6: 4,
  7: 5,
  8: 5,
  9: 6,
  10: 6
}

const manaPerRoundFromGnosis = {
  1: 1,
  2: 2,
  3: 3,
  4: 4,
  5: 5,
  6: 6,
  7: 7,
  8: 8,
  9: 10,
  10: 15
}

export default {
  data () {
    return {
      gnosis: 1,
      spellHighestArcanum: 1,
      casterCorrespondingArcanum: 1,
      arcanaType: 'Ruling',
      castingMethod: 'Normal',
      reach: {
        effect: 0,
        castingTime: 'Ritual',
        range: 'Touch',
        duration: '0,0,0',
        aoe: '0,0',
        potency: 1,
        potencyAdvanced: false
      },
      yantras: [
        { name: 'Demesne', dp: '2', checked: false },
        { name: 'Environment', dp: '1', checked: false },
        { name: 'Supernal Verge', dp: '2', checked: false },
        { name: 'Concentration', dp: '2', checked: false },
        { name: 'Murda', dp: '(see book)', checked: false },
        { name: 'Mantra', dp: '2', checked: false },
        { name: 'Runes', dp: '2', checked: false },
        { name: 'Path Tool', dp: '1', checked: false },
        { name: 'Order Tool', dp: '1', checked: false },
        { name: 'Patrol Tool', dp: '(see book)', checked: false },
        { name: 'Sympathy', dp: '(see book)', checked: false },
        { name: 'Sacrament', dp: '(see book)', checked: false },
        { name: 'Persona', dp: '(see book)', checked: false }
      ],
      yantraRitualTime: 0,
      paradoxModifiers: [
        { name: 'Inured to spell', dp: '2', checked: false },
        { name: 'Sleepers witness obvious casting', dp: '1', checked: false },
        { name: 'Dedicated tool as a yantra', dp: '-2', checked: false }
      ],
      paradoxSameScene: 0,
      manaReduceParadox: 0
    }
  },
  computed: {
    freeReaches () {
      if (this.castingMethod.indexOf('Rote') !== -1) {
        return 5 - this.spellHighestArcanum + 1
      } else {
        return this.casterCorrespondingArcanum - this.spellHighestArcanum + 1
      }
    },
    dicePool () {
      let val = 0
      val += this.gnosis
      val += this.casterCorrespondingArcanum
      for (let yanta of this.yantras) {
        if (yanta.checked) {
          if (yanta.dp !== '(see book)') {
            val += parseInt(yanta.dp)
          }
        }
      }
      val += parseInt(this.reach.duration.split(',')[2])
      val += parseInt(this.reach.aoe.split(',')[1])
      if (this.reach.potency > 1) {
        val -= this.reach.potency * 2
      }
      if (val > 0) {
        return `Roll ${val} ${val > 1 ? 'dice' : 'die'}`
      }
      if (val > -6) {
        return 'Roll a chance die'
      }
      return 'You cannot cast the spell like this'
    },
    paradoxTotal () {
      let val = 0
      let any = false
      const fromReaches = this.reachesUsed - this.freeReaches
      val += fromReaches < 0 ? 0 : fromReaches * paradoxFromGnosis[this.gnosis]
      if (val > 0) {
        any = true
      }
      for (let mod of this.paradoxModifiers) {
        if (mod.checked) {
          val += parseInt(mod.dp)
          any = true
        }
      }
      if (this.paradoxSameScene > 0) {
        val += this.paradoxSameScene
        any = true
      }
      val -= this.manaReduceParadox
      val = val > 0 ? val : 0
      if (any && val === 0) {
        val = '1 chance die'
      }
      return val
    },
    manaCost () {
      let val = 0
      if (this.arcanaType !== 'Ruling') {
        val += 1
      }
      val += this.manaReduceParadox
      if (this.reach.duration.split(',')[0] === '1') {
        val += 1
      }
      return val
    },
    reachesUsed () {
      let val = 0
      val += this.reach.effect
      val += parseInt(this.reach.duration.split(',')[1])
      val += parseInt(this.reach.aoe.split(',')[0])
      if (this.reach.castingTime === 'Instant') {
        val += 1
      }
      if (this.reach.range === 'Sensory') {
        val += 1
      }
      if (this.reach.potencyAdvanced) {
        val += 1
      }
      return val
    },
    castingTime () {
      if (this.reach.castingTime === 'Ritual') {
        // time based on gnosis * additional multiplier for bonus
        let minutes = ritualTimeFromGnosis[this.gnosis]
        if (this.yantraRitualTime > 0) {
          minutes *= (this.yantraRitualTime + 1)
        }
        return `${minutes} minute${minutes > 1 ? 's' : ''}`
      } else {
        // 1 round + number of rounds to use all yantras + number of rounds to spend all mana
        let rounds = 1
        const yantasUsed = this.yantras.filter(e => e.checked).length
        if (yantasUsed > 1) {
          rounds += yantasUsed - 1
        }
        if (this.manaCost > manaPerRoundFromGnosis[this.gnosis]) {
          rounds += Math.ceil(this.manaCost / manaPerRoundFromGnosis[this.gnosis])
        }
        return `${rounds} round${rounds > 1 ? 's' : ''}`
      }
    },
    totalPotency () {
      return 1 + this.reach.potency - 1
    },
    primarySpellFactorBoost () {
      return this.casterCorrespondingArcanum - 1
    },
    overYantraLimitMessage () {
      return this.yantras.filter(e => e.checked).length > yantraLimitFromGnosis[this.gnosis]
    },
    yantasUsed () {
      return this.yantras.filter(e => e.checked).length
    },
    yantrasAllowed () {
      return yantraLimitFromGnosis[this.gnosis]
    },
    additionalMessages () {
      let messages = []
      if (this.castingMethod === 'Rote from grimoire') {
        messages.push('Cannot use reach to cast the spell')
        messages.push('Cannot cast the spell instantly')
        messages.push('Ritual cast time is doubled')
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
        this.reach.castingTime = 'Ritual'
        this.reach.range = 'Touch'
        this.reach.duration = '0,0,0'
        this.reach.aoe = '0,0'
        this.reach.potency = 1
      }
      if (newVal.indexOf('Rote') === -1) {
        this.yantras[4].checked = false
      }
    },
    'reach.CastingTime' (newVal, oldVal) {
      if (newVal === 'Instant') {
        this.yantraRitualTime = 0
      }
    }
  }
}
</script>

<style lang="stylus">
.left-border
  border-left: 1px solid grey

.left-border-lg
  @media (min-width: 992px)
    border-left: 1px solid grey
</style>
