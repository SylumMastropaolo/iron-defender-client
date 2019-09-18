<template>
  <v-container>
    <br />
    <v-layout align-center wrap>
      <v-flex xs12 md6>
        <v-slider
          v-model="charIntMod"
          label="Intelligence Mod"
          max="5"
          min="-5"
          hide-details
          ticks
          thumb-label
        >
          <template v-slot:append>
            <v-text-field
              v-model="charIntMod"
              class="mt-0 pt-0"
              hide-details
              single-line
              type="number"
              style="width: 60px"
            ></v-text-field>
          </template>
        </v-slider>
      </v-flex>
      <v-flex xs12 md6>
        <v-slider
          v-model="artificerLevel"
          label="Artificer Levels"
          :max="ArtificerLevelMax"
          :min="ArtificerLevelMin"
          hide-details
          ticks
          thumb-label
        >
          <template v-slot:append>
            <v-text-field
              v-model="artificerLevel"
              class="mt-0 pt-0"
              hide-details
              single-line
              type="number"
              style="width: 60px"
            ></v-text-field>
          </template>
        </v-slider>
      </v-flex>
      <v-flex xs1>
        <v-switch v-model="multiClass" label="Multiclass"></v-switch>
      </v-flex>
      <v-flex xs11 v-if="multiClass">
        <v-slider
          v-model="totalLevel"
          label="Total Levels"
          max="20"
          :min="artificerLevel"
          hide-details
          ticks
          thumb-label
        >
          <template v-slot:append>
            <v-text-field
              v-model="totalLevel"
              class="mt-0 pt-0"
              hide-details
              single-line
              type="number"
              style="width: 60px"
            ></v-text-field>
          </template>
        </v-slider>
      </v-flex>
      <v-flex xs12>
        <br /><v-divider></v-divider><br />
        <h1>Iron Defender</h1>
        <h3>Medium construct, neutral</h3>
        <br /><v-divider></v-divider><br />
        <h2>Armor Class: {{ armorClass }} (natural armor)</h2>
        <h2>Hit Points: {{ hitPoints }}</h2>
        <h2>Speed: {{ speed }}ft.</h2>
        <br /><v-divider></v-divider><br />
        <v-layout row text-center>
          <v-flex v-for="score in scores" xs2 :key="score">
            <h2>{{ score.name }}</h2>
            <h2>
              {{ score.score }}
              (<span v-if="score.mod >= 0">+</span>{{ score.mod }})
            </h2>
          </v-flex>
        </v-layout>
        <br /><v-divider></v-divider><br />
        <h3>Perception: +{{ perception }}</h3>
        <h3>Damage Immunities: poison</h3>
        <h3>Condition Immunities: charmed, exhaustion, poisoned</h3>
        <h3>Senses: darkvision 60 ft., passive Perception 10</h3>
        <h3>Languages: understands languages you speak</h3>
        <h3>Challenge: 0 (10XP)</h3>
        <br /><v-divider></v-divider><br />
        <h4>
          <span class="font-weight-bold font-italic">
            Might of the Master:
          </span>
          <span class="font-weight-regular">
            The following numbers increase by 1 when your proficiency bonus
            increases by 1: the iron defender’s skill bonuses, the bonuses to
            hit and damage of its bite, and the number of hit points restored by
            its Repair action.
          </span>
        </h4>
        <h4>
          <span class="font-weight-bold font-italic">Vigilant:</span>
          <span class="font-weight-regular">
            The iron defender can’t be surprised.
          </span>
        </h4>
        <h4>
          <span class="font-weight-bold font-italic">Iron Defender:</span>
          <span class="font-weight-regular">
            In combat, the iron defender shares your initiative count, but it
            takes its turn immediately after yours. It can move and use its
            reaction on its own, but the only action it takes on its turn is the
            Dodge action, unless you take a bonus action on your turn to command
            it to take one of the actions in its stat block or the Dash,
            Disengage, or Help action.
          </span>
        </h4>
        <h4>
          <span class="font-weight-regular">
            If the mending spell is cast on it, it regains 2d6 hit points. If it
            has died within the last hour, you can use your smith’s tools as an
            action to revive it, provided you are within 5 feet of it and you
            expend a spell slot of 1st level or higher. The iron defender
            returns to life after 1 minute with all its hit points restored.
          </span>
        </h4>
        <h4 v-if="this.artificerLevel >= 6">
          <span class="font-weight-bold font-italic">Arcane Jolt:</span>
          <span class="font-weight-regular">
            Your Iron Defender's bite attack is now considered magical for the
            purposes of bypassing resistances and immunities, thanks to
            enhancements you've made to the creature in your free time.
          </span>
        </h4>
        <h4 v-if="this.artificerLevel >= 6">
          <span class="font-weight-regular">
            Also, when you or your iron defender hits a target with a magic
            weapon attack, you can channel magical energy through the strike to
            create one of the following effects.
            <ul>
              <li>The target takes an extra {{ joltDice }}d4 force damage.</li>
              <li>
                Choose one creature or object you can see within 30 feet of the
                target. Healing energy flows into the chosen recipient,
                restoring {{ joltDice }}d4 hit points to it.
              </li>
            </ul>
          </span>
        </h4>
        <br />
        <h1 class="font-weight-bold">Actions</h1>
        <v-divider></v-divider><br />
        <h4>
          <span class="font-weight-bold font-italic">Bite.</span>
          <span class="font-italic">
            Melee Weapon Attack:
          </span>
          <span class="font-weight-regular">
            +{{ toHit }} to hit, reach 5 ft., one target you can see. Hit:
            1d8+{{ hitDmg }} piercing damage.
          </span>
        </h4>
        <h4>
          <span class="font-weight-bold font-italic">Repair (3/Day).</span>
          <span class="font-weight-regular">
            The magical mechanisms inside the iron defender restore 2d8 +
            {{ repair }} hit points to itself or to one construct or object
            within 5 feet of it.
          </span>
        </h4>
        <br />
        <h1 class="font-weight-bold">Reaction</h1>
        <v-divider></v-divider><br />
        <h4>
          <span class="font-weight-bold font-italic">Defensive Pounce.</span>
          <span class="font-weight-regular">
            The iron defender imposes disadvantage on the attack roll of one
            creature it can see that is within 5 feet of it, provided the attack
            roll is against a creature other than the iron defender.
          </span>
          <span class="font-weight-regular" v-if="artificerLevel >= 14">
            Whenever your iron defender uses its Defensive Pounce, the attacker
            takes 1d4 + {{ pounceDmg }} force damage.
          </span>
        </h4>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    artificerLevel: 16,
    totalLevel: 3,
    multiClass: false,
    armorClass: 15,
    charIntMod: 5,
    speed: 40,
    scores: [
      {
        name: "STR",
        score: 14,
        mod: 2
      },
      {
        name: "DEX",
        score: 12,
        mod: 1
      },
      {
        name: "CON",
        score: 14,
        mod: 2
      },
      {
        name: "INT",
        score: 4,
        mod: -4
      },
      {
        name: "WIS",
        score: 10,
        mod: 0
      },
      {
        name: "CHA",
        score: 6,
        mod: -2
      }
    ]
  }),
  computed: {
    ArtificerLevelMax: function() {
      return 20;
    },
    ArtificerLevelMin: function() {
      return 3;
    },
    hitPoints: function() {
      return 5 * this.artificerLevel + this.charIntMod + this.scores[2].mod;
    },
    might: function() {
      return Math.floor(
        ((this.multiClass ? this.totalLevel : this.artificerLevel) - 1) / 4
      );
    },
    perception: function() {
      return 4 + this.might;
    },
    toHit: function() {
      return 4 + this.might;
    },
    hitDmg: function() {
      return 2 + this.might;
    },
    repair: function() {
      return 2 + this.might;
    },
    pounceDmg: function() {
      return this.charIntMod;
    },
    joltDice: function() {
      return this.artificerLevel >= 16 ? 4 : 2;
    }
  },
  methods: {}
};
</script>
