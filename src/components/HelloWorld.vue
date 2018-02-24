<template>
  <div class="hello">
    <h1>Oscars Voting 2018</h1>

    <p>Check out all the <a href='http://oscar.go.com/nominees' target='_blank'>Oscar Nominess</a></p>

    <div v-for='question in questions' :key='question.key'>
      <h3>{{question.label}}</h3>
      <div class='option' v-for='option in question.options'
        :key='option.key'
        @click='answer(question, option.key)'
        :class='{selected: userData[question.key] === option.key}'
        >
        {{option.label}}

        <div v-if='userData[question.key]'>
          <span v-if='questionStats[question.key]'>{{questionStats[question.key][option.key] || 0}}</span>
          <span v-else >0</span>
          <span>votes</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { getApp } from '../firebase'

const db = getApp().database()

export default {
  name: 'HelloWorld',
  data () {
    return {
      db: {},
      statsdb: {},
      questionStats: {},
      userData: {},
      questions: [
        {
          key: 'best_picture',
          label: 'Best Picture',
          options: [
            {
              label: 'Call Me by Your Name',
              key: 'call_my_by_your_name'
            },
            {
              label: 'Darkest Hour',
              key: 'darkest_hour'
            },
            {
              label: 'Dunkirk',
              key: 'dunkirik'
            },
            {
              label: 'Get Out',
              key: 'get_out'
            },
            {
              label: 'Lady Bird',
              key: 'lady_bird'
            },
            {
              label: 'Phantom Thread',
              key: 'phantom_thread'
            },
            {
              label: 'The Post',
              key: 'the_post'
            },
            {
              label: 'The Shape of Water',
              key: 'the_shape_of_water'
            },
            {
              label: 'Three Billboards Outside Ebbing, Missouri',
              key: 'three_billboards'
            }
          ]
        },
        {
          key: 'director',
          label: 'Director',
          options: [
            {
              label: 'Paul Thomas Anderson - Phantom Thread',
              key: 'paul_thomas_anderson'
            },
            {
              label: 'Guillermo del Toro - The Shape of Water',
              key: 'guillermo_del_toro'
            },
            {
              label: 'Greta Gerwig - Lady Bird',
              key: 'greta_gerwig'
            },
            {
              label: 'Christopher Nolan - Dunkirk',
              key: 'christopher_nolan'
            },
            {
              label: 'Jordan Peele - Jordan Peele',
              key: 'jordan_peele'
            }
          ]
        },
        {
          key: 'best_actor_leading_role',
          label: 'Actor in a Leading Role',
          options: [
            {
              label: 'Timothée Chalamet - Call Me by Your Name',
              key: 'timothee_chalamet'
            },
            {
              label: 'Daniel Day-Lewis - Phantom Thread',
              key: 'daniel_day_lewis'
            },
            {
              label: 'Daniel Kaluuya - Get Out',
              key: 'daniel_kaluuya'
            },
            {
              label: 'Gary Oldman - Darkest Hour',
              key: 'gary_oldman'
            },
            {
              label: 'Denzel Washington - Roman J. Israel, Esq.',
              key: 'denzel_washington'
            }
          ]
        },
        {
          key: 'best_actress_leading_role',
          label: 'Actress in a Leading Role',
          options: [
            {
              label: 'Sally Hawkins - The Shape of Water',
              key: 'sally_hawkins'
            },
            {
              label: 'Frances McDormand - Three Billboards Outside Ebbing, Missouri',
              key: 'frances_mcdormand'
            },
            {
              label: 'Margot Robbie - I, Tonya',
              key: 'margot_robbie'
            },
            {
              label: 'Saoirse Ronan - Lady Bird',
              key: 'saoirse_ronan'
            },
            {
              label: 'Meryl Streep - The Post',
              key: 'meryl_streep'
            }
          ]
        },
        {
          key: 'best_actor_supporting_role',
          label: 'Actor in a Supporting Role',
          options: [
            {
              label: 'Willem Dafoe - The Florida Project',
              key: 'willem_dafoe'
            },
            {
              label: 'Woody Harrelson - Three Billboards Outside Ebbing, Missouri',
              key: 'woody_harrelson'
            },
            {
              label: 'Richard Jenkins - The Shape of Water',
              key: 'richard_jenkins'
            },
            {
              label: 'Christopher Plummer - All the Money in the World',
              key: 'christopher_plummer'
            },
            {
              label: 'Sam Rockwell - Three Billboards Outside Ebbing, Missouri',
              key: 'sam_rockwell'
            }
          ]
        },
        {
          key: 'best_actress_supporting_role',
          label: 'Actress in a Supporting Role',
          options: [
            {
              label: 'Mary J. Blige - Mudbound',
              key: 'mary_j_blige'
            },
            {
              label: 'Allison Janney - I, Tonya',
              key: 'allison_janney'
            },
            {
              label: 'Lesley Manville - Phantom Thread',
              key: 'lesley_manville'
            },
            {
              label: 'Laurie Metcalf - Lady Bird',
              key: 'laurie_metcalf'
            },
            {
              label: 'Octavia Spencer - The Shape of Water',
              key: 'octavia_spencer'
            }
          ]
        },
        {
          key: 'original_screenplay',
          label: 'Original Screenplay',
          options: [
            {
              label: 'The Big Sick',
              key: 'the_big_sick'
            },
            {
              label: 'Get Out',
              key: 'get_out'
            },
            {
              label: 'Lady Bird',
              key: 'lady_bird'
            },
            {
              label: 'The Shape of Water',
              key: 'the_shape_of_water'
            },
            {
              label: 'Three Billboards Outside Ebbing, Missouri',
              key: 'three_billboards'
            }
          ]
        },
        {
          key: 'adapted_screenplay',
          label: 'Adapted Screenplay',
          options: [
            {
              label: 'Call Me by Your Name',
              key: 'call_my_by_your_name'
            },
            {
              label: 'The Disaster Artist',
              key: 'the_disaster_artist'
            },
            {
              label: 'Logan',
              key: 'logan'
            },
            {
              label: 'Molly\'s Game',
              key: 'molly_game'
            },
            {
              label: 'Mudbound',
              key: 'mudbound'
            }
          ]
        },
        {
          key: 'foreign_language_film',
          label: 'Foreign Language Film',
          options: [
            {
              label: 'A Fantastic Woman - Chile',
              key: 'fantastic_woman'
            },
            {
              label: 'The Insult - Lebanon',
              key: 'insult'
            },
            {
              label: 'Loveless - Russia',
              key: 'loveless'
            },
            {
              label: 'On Body and Soul - Hungary',
              key: 'on_body_and_soul'
            },
            {
              label: 'The Square - Sweden',
              key: 'the_square'
            }
          ]
        },
        {
          key: 'animated_feature',
          label: 'Animated Feature',
          options: [
            {
              label: 'The Boss Baby',
              key: 'boss_baby'
            },
            {
              label: 'The Breadwinner',
              key: 'the_breadwinner'
            },
            {
              label: 'Coco',
              key: 'coco'
            },
            {
              label: 'Ferdinand',
              key: 'ferdinand'
            },
            {
              label: 'Loving Vincent',
              key: 'loving_vincent'
            }
          ]
        },
        {
          key: 'sound_editing',
          label: 'Sound Editing',
          options: [
            {
              label: 'Baby Driver',
              key: 'baby_driver'
            },
            {
              label: 'Blade Runner 2049',
              key: 'blade_runner'
            },
            {
              label: 'Dunkirk',
              key: 'dunkirik'
            },
            {
              label: 'The Shape of Water',
              key: 'the_shape_of_water'
            },
            {
              label: 'Star Wars: The Last Jedi',
              key: 'last_jedi'
            }
          ]
        },
        {
          key: 'visual_effects',
          label: 'Visual Effects',
          options: [
            {
              label: 'Blade Runner 2049',
              key: 'blade_runner'
            },
            {
              label: 'Guardians of the Galaxy Vol. 2',
              key: 'guardians_galaxy'
            },
            {
              label: 'Kong: Skull Island',
              key: 'kong'
            },
            {
              label: 'Star Wars: The Last Jedi',
              key: 'last_jedi'
            },
            {
              label: 'War for the Planet of the Apes',
              key: 'war_of_apes'
            }
          ]
        },
        {
          key: 'film_editing',
          label: 'Film Editing',
          options: [
            {
              label: 'Baby Driver',
              key: 'baby_driver'
            },
            {
              label: 'dunkirik',
              key: 'guardians_galaxy'
            },
            {
              label: 'I, Tonya',
              key: 'tonya'
            },
            {
              label: 'The Shape of Water',
              key: 'the_shape_of_water'
            },
            {
              label: 'Three Billboards Outside Ebbing, Missouri',
              key: 'three_billboards'
            }
          ]
        },
        {
          key: 'short_film_animated',
          label: 'Short Film, Animated',
          options: [
            {
              label: 'Dear Basketball',
              key: 'dear_basketball'
            },
            {
              label: 'Garden Party',
              key: 'garden_party'
            },
            {
              label: 'Lou',
              key: 'lou'
            },
            {
              label: 'Negative Space',
              key: 'negative_space'
            },
            {
              label: 'Revolting Rhymes',
              key: 'revolting_rhymes'
            }
          ]
        },
        {
          key: 'short_film_live_action',
          label: 'Short Film, Live Action',
          options: [
            {
              label: 'DeKalb Elementary',
              key: 'dekalb'
            },
            {
              label: 'The Eleven O’Clock',
              key: 'eleven_oclock'
            },
            {
              label: 'My Nephew Emmett',
              key: 'newphew_emmett'
            },
            {
              label: 'The Silent Child',
              key: 'silent_child'
            },
            {
              label: 'Watu Wote/All of Us',
              key: 'watu_wote'
            }
          ]
        },
        {
          key: 'documentary_short',
          label: 'Documentary Short Subject',
          options: [
            {
              label: 'Edith+Eddie',
              key: 'edith_edie'
            },
            {
              label: 'Heaven Is a Traffic Jam on the 405',
              key: 'heave_traffic'
            },
            {
              label: 'Heroin(e)',
              key: 'heroin'
            },
            {
              label: 'Knife Skills',
              key: 'knife_skills'
            },
            {
              label: 'Traffic Stop',
              key: 'traffic_stop'
            }
          ]
        },
        {
          key: 'original_score',
          label: 'Original Score',
          options: [
            {
              label: 'Dunkirk',
              key: 'dunkirik'
            },
            {
              label: 'Phantom Thread',
              key: 'phantom_thread'
            },
            {
              label: 'The Shape of Water',
              key: 'the_shape_of_water'
            },
            {
              label: 'Star Wars: The Last Jedi',
              key: 'last_jedi'
            },
            {
              label: 'Three Billboards Outside Ebbing, Missouri',
              key: 'three_billboards'
            }
          ]
        },
        {
          key: 'original_song',
          label: 'Original Song',
          options: [
            {
              label: 'Mighty River - Mudbound',
              key: 'mighty_river'
            },
            {
              label: 'The Mystery of Love - Call Me by Your Name',
              key: 'mystery_love'
            },
            {
              label: 'Remember Me - Coco',
              key: 'remember_me'
            },
            {
              label: 'Stand Up for Something - Marshall',
              key: 'stand_up'
            },
            {
              label: 'This Is Me - The Greatest Showman',
              key: 'this_is_me'
            }
          ]
        },
        {
          key: 'production_design',
          label: 'Production Design',
          options: [
            {
              label: 'Beauty and the Beast',
              key: 'beauty_and_beast'
            },
            {
              label: 'Blade Runner 2049',
              key: 'blade_runner'
            },
            {
              label: 'Darkest Hour',
              key: 'darkest_hour'
            },
            {
              label: 'Dunkirk',
              key: 'dunkirik'
            },
            {
              label: 'The Shape of Water',
              key: 'the_shape_of_water'
            }
          ]
        },
        {
          key: 'cinematography',
          label: 'Cinematography',
          options: [
            {
              label: 'Blade Runner 2049',
              key: 'blade_runner'
            },
            {
              label: 'Darkest Hour',
              key: 'darkest_hour'
            },
            {
              label: 'Dunkirk',
              key: 'dunkirik'
            },
            {
              label: 'Mudbound',
              key: 'mudbound'
            },
            {
              label: 'The Shape of Water',
              key: 'the_shape_of_water'
            }
          ]
        },
        {
          key: 'costume_design',
          label: 'Costume Design',
          options: [
            {
              label: 'Beauty and the Beast',
              key: 'beauty_and_beast'
            },
            {
              label: 'Darkest Hour',
              key: 'darkest_hour'
            },
            {
              label: 'Phantom Thread',
              key: 'phantom_thread'
            },
            {
              label: 'The Shape of Water',
              key: 'the_shape_of_water'
            },
            {
              label: 'Victoria and Abdul',
              key: 'victoria_and_abdul'
            }
          ]
        },
        {
          key: 'makeup',
          label: 'Makeup',
          options: [
            {
              label: 'Darkest Hour',
              key: 'darkest_hour'
            },
            {
              label: 'Victoria and Abdul',
              key: 'victoria_and_abdul'
            },
            {
              label: 'Wonder',
              key: 'wonder'
            }
          ]
        },
        {
          key: 'documentary_feature',
          label: 'Documentary Feature',
          options: [
            {
              label: 'Abacus: Small Enough to Jail',
              key: 'abacus'
            },
            {
              label: 'Faces Places',
              key: 'faces_places'
            },
            {
              label: 'Icarus',
              key: 'icarus'
            },
            {
              label: 'Last Men in Aleppo',
              key: 'last_men_aleppo'
            },
            {
              label: 'Strong Island',
              key: 'strong_island'
            }
          ]
        },
        {
          key: 'sound_mixing',
          label: 'Sound Mixing',
          options: [
            {
              label: 'Baby Driver',
              key: 'baby_driver'
            },
            {
              label: 'Blade Runner 2049',
              key: 'blade_runner'
            },
            {
              label: 'Dunkirk',
              key: 'dunkirik'
            },
            {
              label: 'The Shape of Water',
              key: 'the_shape_of_water'
            },
            {
              label: 'Star Wars: The Last Jedi',
              key: 'last_jedi'
            }
          ]
        }
      ]
    }
  },
  mounted () {
    getApp()
      .auth()
      .signInAnonymously()
      .catch((error) => {
        // const errorCode = error.code;
        // const errorMessage = error.message;
        console.log(error)
      })

    getApp()
      .auth()
      .onAuthStateChanged((user) => {
        if (user) {
          // const isAnonymous = user.isAnonymous;
          const userId = user.uid
          this.db = db.ref('userVotes2018/' + userId)
          this.db.on('value', (result) => {
            this.$set(this, 'userData', result.val())
          })

          this.statsdb = db.ref('voteStats2018/')
          this.statsdb.on('value', (result) => {
            this.$set(this, 'questionStats', result.val())
          })
        }
      })
  },
  methods: {
    answer (question, answer) {
      const prevAnswer = this.userData ? this.userData[question.key] : null

      if (prevAnswer === answer) return

      // Update user current answer
      let userUpdate = {}
      userUpdate[question.key] = answer
      this.db.update(userUpdate)

      // Update question stats
      let questionUpdate = {}
      questionUpdate[question.key] = {}
      questionUpdate[question.key][answer] = 1

      if (this.questionStats && this.questionStats[question.key] && this.questionStats[question.key][answer]) {
        questionUpdate[question.key][answer] = this.questionStats[question.key][answer] + 1
      }

      if (prevAnswer && this.questionStats && this.questionStats[question.key] && this.questionStats[question.key][prevAnswer]) {
        questionUpdate[question.key][prevAnswer] = this.questionStats[question.key][prevAnswer] - 1
      }

      getApp().database().ref().child('voteStats2018/' + question.key).update(questionUpdate[question.key]);

      // this.statsdb.update(questionUpdate)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
    font-size: 40px;
    color: #bda043;
  }

  h3 {
    font-size: 28px;
    color: #bda043;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }

  .hello {
    padding-left: 2em;
    padding-right: 2em;

    margin: 0 auto;
    max-width: 960px;
  }

  .option {
    margin-bottom: .2em;
    padding: .6em;
  }

  .option:hover {
    cursor: pointer;
  }

  .selected {
    font-weight: bold;
  }
</style>
